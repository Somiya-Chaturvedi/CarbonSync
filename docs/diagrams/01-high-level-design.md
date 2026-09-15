# High-Level Design (HLD) - CarbonSync Platform

## System Architecture Overview

```mermaid
graph TB
    subgraph "Client Layer"
        WEB[Web Browser]
        MOBILE[Mobile Browser]
    end
    
    subgraph "Frontend Layer - Netlify"
        LANDING[Landing Page]
        COMPANY_UI[Company Dashboard]
        AUDITOR_UI[Auditor Portal]
        GOVT_UI[Government Monitor]
        LOGIN[Login Pages]
    end
    
    subgraph "API Gateway Layer"
        NGINX[NGINX/Load Balancer]
    end
    
    subgraph "Backend Layer - Spring Boot"
        AUTH[Auth Service]
        COMPANY_SVC[Company Service]
        AUDITOR_SVC[Auditor Service]
        GOVT_SVC[Government Service]
        CARBON_SVC[Carbon Service]
        
        subgraph "Cross-Cutting Concerns"
            SECURITY[Security Config]
            EXCEPTION[Exception Handler]
            CORS[CORS Config]
        end
    end
    
    subgraph "Data Layer"
        POSTGRES[(PostgreSQL Database)]
        CACHE[(Redis Cache - Future)]
    end
    
    subgraph "External Services"
        JWT[JWT Token Service]
        EMAIL[Email Service - Future]
        STORAGE[File Storage - Future]
    end
    
    WEB --> LANDING
    MOBILE --> LANDING
    LANDING --> LOGIN
    LOGIN --> COMPANY_UI
    LOGIN --> AUDITOR_UI
    LOGIN --> GOVT_UI
    
    COMPANY_UI --> NGINX
    AUDITOR_UI --> NGINX
    GOVT_UI --> NGINX
    
    NGINX --> AUTH
    NGINX --> COMPANY_SVC
    NGINX --> AUDITOR_SVC
    NGINX --> GOVT_SVC
    NGINX --> CARBON_SVC
    
    AUTH --> JWT
    AUTH --> POSTGRES
    COMPANY_SVC --> POSTGRES
    AUDITOR_SVC --> POSTGRES
    GOVT_SVC --> POSTGRES
    CARBON_SVC --> POSTGRES
    
    SECURITY -.-> AUTH
    EXCEPTION -.-> AUTH
    EXCEPTION -.-> COMPANY_SVC
    EXCEPTION -.-> AUDITOR_SVC
    EXCEPTION -.-> GOVT_SVC
    EXCEPTION -.-> CARBON_SVC
    CORS -.-> NGINX
    
    style POSTGRES fill:#4A90E2
    style LANDING fill:#FFD700
    style AUTH fill:#E74C3C
    style SECURITY fill:#E67E22
```

## Technology Stack

### Frontend
- **Framework**: Vanilla HTML5, CSS3, JavaScript
- **Styling**: Custom CSS with variables, components, animations
- **3D Graphics**: Spline for globe animation
- **Deployment**: Netlify (Static hosting)

### Backend
- **Framework**: Spring Boot 3.2.4
- **Language**: Java 17
- **Build Tool**: Maven
- **Security**: Spring Security + JWT (Planned)
- **Validation**: Jakarta Validation
- **Deployment**: Railway/Heroku (Planned)

### Database
- **Primary DB**: PostgreSQL (Planned - Currently using stubs)
- **Caching**: Redis (Future enhancement)
- **ORM**: Spring Data JPA (To be wired)

### DevOps
- **Version Control**: Git + GitHub
- **CI/CD**: GitHub Actions (Planned)
- **Monitoring**: Spring Actuator + Prometheus (Planned)
- **Logging**: SLF4J + Logback

## System Layers

### 1. Presentation Layer (Frontend)
- **Responsibility**: User interface and user experience
- **Components**: Landing, Company Dashboard, Auditor Portal, Government Monitor
- **Communication**: REST API calls to backend

### 2. API Layer (Controllers)
- **Responsibility**: HTTP request handling, routing
- **Components**: AuthController, CompanyController, AuditorController, GovernmentController, CarbonController
- **Pattern**: RESTful architecture

### 3. Business Logic Layer (Services)
- **Responsibility**: Core business logic, validation, processing
- **Components**: AuthService, CompanyService, AuditorService, GovernmentService, CarbonService
- **Pattern**: Service layer pattern

### 4. Data Access Layer (Repositories)
- **Responsibility**: Database operations
- **Components**: Spring Data JPA repositories (Planned)
- **Pattern**: Repository pattern

### 5. Data Layer (Database)
- **Responsibility**: Data persistence
- **Components**: PostgreSQL with normalized schema
- **Pattern**: Relational database design

## Design Principles

1. **Separation of Concerns**: Clear layer boundaries
2. **Single Responsibility**: Each component has one job
3. **Dependency Injection**: Spring manages object lifecycle
4. **RESTful API Design**: Standard HTTP methods and status codes
5. **Security First**: Authentication and authorization at every layer
6. **Scalability**: Stateless backend, horizontal scaling ready
7. **Maintainability**: Clean code, clear naming conventions

## Non-Functional Requirements

- **Performance**: API response time < 200ms
- **Availability**: 99.9% uptime target
- **Scalability**: Handle 10,000+ concurrent users
- **Security**: HTTPS, JWT tokens, input validation
- **Reliability**: Automated backups, error recovery
- **Maintainability**: Clean code, comprehensive documentation
