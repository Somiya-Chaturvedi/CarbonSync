# Sequence Diagrams - CarbonSync User Flows

## 1. User Authentication Flow

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant AuthController
    participant AuthService
    participant Database
    participant JWTUtil
    
    User->>Frontend: Enter email & password
    Frontend->>Frontend: Validate input
    Frontend->>AuthController: POST /api/auth/login
    
    AuthController->>AuthService: login(LoginRequest)
    AuthService->>Database: Query user by email
    Database-->>AuthService: User record
    
    alt User not found
        AuthService-->>AuthController: Throw ResourceNotFoundException
        AuthController-->>Frontend: 404 Not Found
        Frontend-->>User: Display error
    else User found
        AuthService->>AuthService: Verify password hash
        alt Password incorrect
            AuthService-->>AuthController: Throw AuthenticationException
            AuthController-->>Frontend: 401 Unauthorized
            Frontend-->>User: Display error
        else Password correct
            AuthService->>JWTUtil: generateToken(user)
            JWTUtil-->>AuthService: JWT token
            AuthService-->>AuthController: AuthResponse(token, email, role)
            AuthController-->>Frontend: 200 OK + ResponseWrapper
            Frontend->>Frontend: Store token in localStorage
            Frontend-->>User: Redirect to dashboard
        end
    end
```

## 2. Company Emission Submission Flow

```mermaid
sequenceDiagram
    actor Company
    participant Dashboard
    participant CarbonController
    participant CarbonService
    participant CompanyService
    participant Database
    participant NotificationService
    
    Company->>Dashboard: Fill emission form
    Dashboard->>Dashboard: Validate inputs
    Company->>Dashboard: Click Submit
    
    Dashboard->>CarbonController: POST /api/carbon
    Note over Dashboard,CarbonController: Authorization: Bearer <token>
    
    CarbonController->>CarbonController: Validate @RequestBody
    CarbonController->>CarbonService: create(CarbonEntry)
    
    CarbonService->>CarbonService: Calculate total emissions
    Note over CarbonService: total = scope1 + scope2 + scope3
    
    CarbonService->>Database: INSERT INTO carbon_entry
    Database-->>CarbonService: Entry ID
    
    CarbonService->>CompanyService: updateEmissions(companyId, total)
    CompanyService->>Database: UPDATE company SET current_emissions
    Database-->>CompanyService: Success
    
    CarbonService->>NotificationService: notifyAuditors(entryId)
    NotificationService-->>CarbonService: Queued
    
    CarbonService-->>CarbonController: CarbonEntry
    CarbonController-->>Dashboard: 200 OK + ResponseWrapper
    Dashboard-->>Company: Display confirmation
```

## 3. Auditor Verification Flow

```mermaid
sequenceDiagram
    actor Auditor
    participant AuditorPortal
    participant CarbonController
    participant AuditorController
    participant CarbonService
    participant AuditService
    participant Database
    participant NotificationService
    
    Auditor->>AuditorPortal: Open review queue
    AuditorPortal->>CarbonController: GET /api/carbon?status=PENDING
    CarbonController->>CarbonService: getAllByStatus("PENDING")
    CarbonService->>Database: SELECT * WHERE status='PENDING'
    Database-->>CarbonService: List<CarbonEntry>
    CarbonService-->>CarbonController: Pending entries
    CarbonController-->>AuditorPortal: 200 OK + entries
    AuditorPortal-->>Auditor: Display pending list
    
    Auditor->>AuditorPortal: Click entry to review
    AuditorPortal->>CarbonController: GET /api/carbon/{id}
    CarbonController->>CarbonService: getById(id)
    CarbonService->>Database: SELECT * WHERE id=?
    Database-->>CarbonService: CarbonEntry + Company details
    CarbonService-->>CarbonController: Entry details
    CarbonController-->>AuditorPortal: 200 OK + details
    AuditorPortal-->>Auditor: Display full details
    
    Auditor->>AuditorPortal: Verify/Flag entry + notes
    AuditorPortal->>AuditorController: POST /api/auditors/verify
    
    AuditorController->>AuditService: verifyEntry(entryId, action, notes)
    AuditService->>Database: BEGIN TRANSACTION
    
    AuditService->>Database: UPDATE carbon_entry SET status, verified_by
    Database-->>AuditService: Updated
    
    AuditService->>Database: INSERT INTO audit_record
    Database-->>AuditService: Audit ID
    
    AuditService->>Database: COMMIT TRANSACTION
    
    AuditService->>NotificationService: notifyCompany(companyId, status)
    NotificationService-->>AuditService: Queued
    
    AuditService-->>AuditorController: Success
    AuditorController-->>AuditorPortal: 200 OK
    AuditorPortal-->>Auditor: Display confirmation
```

## 4. Government Compliance Monitoring Flow

```mermaid
sequenceDiagram
    actor Government
    participant GovtPortal
    participant GovernmentController
    participant ComplianceService
    participant CarbonService
    participant CompanyService
    participant Database
    participant ReportGenerator
    
    Government->>GovtPortal: Open compliance dashboard
    GovtPortal->>GovernmentController: GET /api/government/compliance
    
    GovernmentController->>ComplianceService: getComplianceOverview()
    
    par Fetch Companies
        ComplianceService->>CompanyService: getAll()
        CompanyService->>Database: SELECT * FROM company
        Database-->>CompanyService: List<Company>
        CompanyService-->>ComplianceService: Companies
    and Fetch Emissions
        ComplianceService->>CarbonService: getAllVerified()
        CarbonService->>Database: SELECT * WHERE status='VERIFIED'
        Database-->>CarbonService: List<CarbonEntry>
        CarbonService-->>ComplianceService: Verified entries
    end
    
    ComplianceService->>ComplianceService: Calculate compliance for each company
    Note over ComplianceService: compliance = (target / actual) * 100
    
    ComplianceService->>ComplianceService: Identify violations
    ComplianceService-->>GovernmentController: ComplianceReport
    GovernmentController-->>GovtPortal: 200 OK + report
    GovtPortal-->>Government: Display dashboard
    
    Government->>GovtPortal: Click Export CSV
    GovtPortal->>GovernmentController: GET /api/government/export?format=csv
    
    GovernmentController->>ReportGenerator: generateReport("CSV", data)
    ReportGenerator->>ReportGenerator: Format as CSV
    ReportGenerator->>Database: INSERT INTO reports
    Database-->>ReportGenerator: Report ID
    ReportGenerator-->>GovernmentController: CSV file + metadata
    
    GovernmentController-->>GovtPortal: 200 OK + file download
    GovtPortal-->>Government: Download CSV
```

## 5. Company Dashboard Load Flow

```mermaid
sequenceDiagram
    actor Company
    participant Browser
    participant Dashboard
    participant AuthController
    participant CompanyController
    participant CarbonController
    participant Database
    
    Company->>Browser: Navigate to dashboard.html
    Browser->>Dashboard: Load page
    Dashboard->>Dashboard: Check localStorage for token
    
    alt No token found
        Dashboard-->>Company: Redirect to login
    else Token found
        Dashboard->>AuthController: GET /api/auth/verify-token
        AuthController-->>Dashboard: Token valid + user info
        
        par Fetch Company Details
            Dashboard->>CompanyController: GET /api/companies/{id}
            CompanyController->>Database: SELECT company
            Database-->>CompanyController: Company data
            CompanyController-->>Dashboard: Company info
        and Fetch Emission History
            Dashboard->>CarbonController: GET /api/carbon/company/{id}
            CarbonController->>Database: SELECT carbon entries
            Database-->>CarbonController: Entry list
            CarbonController-->>Dashboard: Emission data
        end
        
        Dashboard->>Dashboard: Render charts
        Dashboard->>Dashboard: Calculate statistics
        Dashboard->>Dashboard: Display notifications
        Dashboard-->>Company: Show dashboard
    end
```

## 6. Error Handling Flow

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Controller
    participant Service
    participant GlobalExceptionHandler
    participant Database
    
    User->>Frontend: Submit request
    Frontend->>Controller: API call
    Controller->>Service: Business logic
    Service->>Database: Query
    
    alt Database error
        Database-->>Service: SQLException
        Service-->>Controller: Throw RuntimeException
        Controller->>GlobalExceptionHandler: Exception caught
        GlobalExceptionHandler->>GlobalExceptionHandler: Log error
        GlobalExceptionHandler-->>Frontend: 500 Internal Server Error
        Frontend-->>User: Display error message
    else Resource not found
        Database-->>Service: Empty result
        Service-->>Controller: Throw ResourceNotFoundException
        Controller->>GlobalExceptionHandler: Exception caught
        GlobalExceptionHandler-->>Frontend: 404 Not Found
        Frontend-->>User: Display "not found" message
    else Validation error
        Controller->>Controller: @Valid fails
        Controller->>GlobalExceptionHandler: MethodArgumentNotValidException
        GlobalExceptionHandler->>GlobalExceptionHandler: Extract field errors
        GlobalExceptionHandler-->>Frontend: 400 Bad Request + errors
        Frontend-->>User: Highlight invalid fields
    else Success
        Database-->>Service: Result
        Service-->>Controller: Data
        Controller-->>Frontend: 200 OK
        Frontend-->>User: Display success
    end
```

## 7. Session Management Flow

```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Backend
    participant JWTUtil
    participant Database
    
    User->>Frontend: Login
    Frontend->>Backend: POST /api/auth/login
    Backend->>JWTUtil: generateToken(user, expiry=24h)
    JWTUtil-->>Backend: JWT token
    Backend-->>Frontend: Token + user info
    Frontend->>Frontend: Store in localStorage
    
    loop Every API call
        Frontend->>Backend: Request + Authorization header
        Backend->>JWTUtil: validateToken(token)
        
        alt Token expired
            JWTUtil-->>Backend: TokenExpiredException
            Backend-->>Frontend: 401 Unauthorized
            Frontend->>Frontend: Clear localStorage
            Frontend-->>User: Redirect to login
        else Token valid
            JWTUtil-->>Backend: User claims
            Backend->>Backend: Process request
            Backend-->>Frontend: Response
        end
    end
    
    User->>Frontend: Logout
    Frontend->>Frontend: Clear localStorage
    Frontend->>Backend: POST /api/auth/logout (optional)
    Backend->>Database: Blacklist token
    Frontend-->>User: Redirect to login
```

## Key Interactions Summary

| Flow | Primary Actors | Key Operations | Duration |
|------|---------------|----------------|----------|
| Authentication | User, AuthService | Login, token generation | < 1 sec |
| Emission Submission | Company, CarbonService | Validate, store, notify | < 2 sec |
| Verification | Auditor, AuditService | Review, update status, log | < 3 sec |
| Compliance Monitoring | Government, ComplianceService | Aggregate, calculate, flag | < 5 sec |
| Dashboard Load | Company, Multiple services | Fetch data, render UI | < 2 sec |
| Report Export | Government, ReportGenerator | Query, format, download | < 10 sec |
