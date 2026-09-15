# Data Flow Diagram (DFD) - CarbonSync Platform

## Level 0 - Context Diagram

```mermaid
graph LR
    COMPANY[Company User]
    AUDITOR[Auditor User]
    GOVT[Government User]
    
    SYSTEM[CarbonSync Platform]
    
    DB[(Database)]
    EMAIL[Email Service]
    
    COMPANY -->|Submit Emissions Data| SYSTEM
    SYSTEM -->|Emission Reports| COMPANY
    
    AUDITOR -->|Verify/Flag Data| SYSTEM
    SYSTEM -->|Pending Reviews| AUDITOR
    
    GOVT -->|Monitor Compliance| SYSTEM
    SYSTEM -->|Compliance Reports| GOVT
    
    SYSTEM -->|Store/Retrieve Data| DB
    SYSTEM -->|Send Notifications| EMAIL
    
    style SYSTEM fill:#FFD700
    style DB fill:#4A90E2
```

## Level 1 - High-Level DFD

```mermaid
graph TB
    subgraph "External Entities"
        COMPANY[Company]
        AUDITOR[Auditor]
        GOVT[Government]
    end
    
    subgraph "Processes"
        P1[1.0<br/>User Authentication]
        P2[2.0<br/>Emission Data Management]
        P3[3.0<br/>Audit & Verification]
        P4[4.0<br/>Compliance Monitoring]
        P5[5.0<br/>Report Generation]
    end
    
    subgraph "Data Stores"
        D1[(D1: User Database)]
        D2[(D2: Company Database)]
        D3[(D3: Carbon Entries)]
        D4[(D4: Audit Records)]
        D5[(D5: Reports)]
    end
    
    COMPANY -->|Login Credentials| P1
    AUDITOR -->|Login Credentials| P1
    GOVT -->|Login Credentials| P1
    P1 -->|Auth Token| COMPANY
    P1 -->|Auth Token| AUDITOR
    P1 -->|Auth Token| GOVT
    P1 -->|User Details| D1
    
    COMPANY -->|Emission Data| P2
    P2 -->|Emission Report| COMPANY
    P2 -->|Store Entry| D3
    P2 -->|Company Info| D2
    
    AUDITOR -->|Verification Action| P3
    P3 -->|Review Queue| AUDITOR
    P3 -->|Read Entry| D3
    P3 -->|Update Status| D3
    P3 -->|Audit Trail| D4
    
    GOVT -->|Query Request| P4
    P4 -->|Compliance Data| GOVT
    P4 -->|Read Entries| D3
    P4 -->|Read Companies| D2
    
    P5 -->|Read Data| D3
    P5 -->|Read Data| D2
    P5 -->|Read Data| D4
    P5 -->|Store Report| D5
    GOVT -->|Generate Report| P5
    P5 -->|Report File| GOVT
    
    style P1 fill:#E74C3C
    style P2 fill:#3498DB
    style P3 fill:#2ECC71
    style P4 fill:#F39C12
    style P5 fill:#9B59B6
```

## Level 2 - Detailed DFD

### 2.1 User Authentication Process

```mermaid
graph TB
    USER[User]
    
    P1_1[1.1<br/>Validate Credentials]
    P1_2[1.2<br/>Generate Token]
    P1_3[1.3<br/>Create Session]
    
    D1[(User Database)]
    D6[(Session Store)]
    
    USER -->|Email + Password| P1_1
    P1_1 -->|Query User| D1
    D1 -->|User Record| P1_1
    P1_1 -->|Valid User| P1_2
    P1_2 -->|JWT Token| P1_3
    P1_3 -->|Store Session| D6
    P1_3 -->|Token + User Info| USER
```

### 2.2 Emission Data Management Process

```mermaid
graph TB
    COMPANY[Company]
    
    P2_1[2.1<br/>Validate Input]
    P2_2[2.2<br/>Calculate Totals]
    P2_3[2.3<br/>Store Entry]
    P2_4[2.4<br/>Trigger Notification]
    
    D2[(Company Database)]
    D3[(Carbon Entries)]
    
    COMPANY -->|Emission Data| P2_1
    P2_1 -->|Validated Data| P2_2
    P2_2 -->|Total Emissions| P2_3
    P2_3 -->|Store| D3
    P2_3 -->|Update Balance| D2
    P2_3 -->|Entry ID| P2_4
    P2_4 -->|Confirmation| COMPANY
```

### 2.3 Audit & Verification Process

```mermaid
graph TB
    AUDITOR[Auditor]
    
    P3_1[3.1<br/>Fetch Pending]
    P3_2[3.2<br/>Review Entry]
    P3_3[3.3<br/>Update Status]
    P3_4[3.4<br/>Log Audit Action]
    
    D3[(Carbon Entries)]
    D4[(Audit Records)]
    
    AUDITOR -->|Request| P3_1
    P3_1 -->|Query Pending| D3
    D3 -->|Entry List| P3_1
    P3_1 -->|Display| AUDITOR
    
    AUDITOR -->|Verify/Flag| P3_2
    P3_2 -->|Entry Details| D3
    P3_2 -->|Action| P3_3
    P3_3 -->|Update| D3
    P3_3 -->|Create Log| P3_4
    P3_4 -->|Store| D4
    P3_4 -->|Confirmation| AUDITOR
```

### 2.4 Compliance Monitoring Process

```mermaid
graph TB
    GOVT[Government]
    
    P4_1[4.1<br/>Query Data]
    P4_2[4.2<br/>Calculate Compliance]
    P4_3[4.3<br/>Flag Violations]
    P4_4[4.4<br/>Generate Alert]
    
    D2[(Company Database)]
    D3[(Carbon Entries)]
    D5[(Reports)]
    
    GOVT -->|Filter Criteria| P4_1
    P4_1 -->|Query Entries| D3
    P4_1 -->|Query Companies| D2
    D3 -->|Emission Data| P4_2
    D2 -->|Target Data| P4_2
    P4_2 -->|Compliance %| P4_3
    P4_3 -->|Violations| P4_4
    P4_4 -->|Store| D5
    P4_4 -->|Dashboard Data| GOVT
```

### 2.5 Report Generation Process

```mermaid
graph TB
    GOVT[Government]
    
    P5_1[5.1<br/>Select Report Type]
    P5_2[5.2<br/>Aggregate Data]
    P5_3[5.3<br/>Format Output]
    P5_4[5.4<br/>Export File]
    
    D2[(Company Database)]
    D3[(Carbon Entries)]
    D4[(Audit Records)]
    D5[(Reports)]
    
    GOVT -->|Report Parameters| P5_1
    P5_1 -->|Fetch| D3
    P5_1 -->|Fetch| D2
    P5_1 -->|Fetch| D4
    D3 -->|Data| P5_2
    D2 -->|Data| P5_2
    D4 -->|Data| P5_2
    P5_2 -->|Aggregated| P5_3
    P5_3 -->|CSV/PDF/JSON| P5_4
    P5_4 -->|Store| D5
    P5_4 -->|Download| GOVT
```

## Data Flow Details

### Authentication Flow
1. User submits email + password
2. System validates credentials against database
3. On success, generates JWT token with user role
4. Token returned to client for subsequent requests
5. Token included in Authorization header for all API calls

### Emission Submission Flow
1. Company submits emission data (Scope 1, 2, 3)
2. System validates input (positive numbers, valid date)
3. Calculates total emissions = Scope1 + Scope2 + Scope3
4. Stores entry in database with status = PENDING
5. Updates company's current_emissions total
6. Returns entry ID and confirmation

### Verification Flow
1. Auditor requests pending entries
2. System fetches all entries with status = PENDING
3. Auditor reviews entry and takes action (VERIFY/FLAG/REJECT)
4. System updates entry status
5. Creates audit record with action details
6. Notifies company of status change

### Compliance Monitoring Flow
1. Government user sets filter criteria (date range, industry, etc.)
2. System queries carbon entries and company targets
3. Calculates compliance % for each company
4. Identifies violations (emissions > target)
5. Generates dashboard with compliance overview
6. Flags non-compliant companies

### Report Export Flow
1. Government user selects report type (CSV/PDF/JSON)
2. System aggregates data from multiple tables
3. Formats data according to selected type
4. Generates downloadable file
5. Stores report metadata in database
6. Returns file to user

## Data Transformations

### Emission Calculation
```
Input: scope1, scope2, scope3
Transform: totalEmissions = scope1 + scope2 + scope3
Output: emission_value
```

### Compliance Calculation
```
Input: total_emissions, emission_target
Transform: compliance_pct = (emission_target / total_emissions) * 100
Output: compliance_percentage
Status: 
  - compliance_pct >= 100 → COMPLIANT
  - compliance_pct >= 80 → WARNING
  - compliance_pct < 80 → NON_COMPLIANT
```

### Credit Balance Update
```
Input: current_balance, emissions, credits_purchased
Transform: new_balance = current_balance - (emissions * 0.01) + credits_purchased
Output: credit_balance (must be >= 0)
```

## Data Stores

| Store | Description | Read Operations | Write Operations |
|-------|-------------|-----------------|------------------|
| D1: User Database | Auth & profile data | Login, profile view | Signup, profile update |
| D2: Company Database | Company metadata | Company list, details | Company creation, update |
| D3: Carbon Entries | Emission records | Query, filter, export | Submit, verify, update |
| D4: Audit Records | Audit trail | Audit history | Log action (append-only) |
| D5: Reports | Generated reports | Report list, download | Generate report |
| D6: Session Store | Active sessions (Redis) | Token validation | Create/destroy session |
