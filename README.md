# 🌱 CarbonSync - Carbon Data Management Platform

> **🔴 Live Demo:** [https://carbon-sync.netlify.app](https://carbon-sync.netlify.app)

CarbonSync is a full-stack web application designed to simplify the process of tracking, managing, and verifying carbon emissions. It provides a centralized platform where companies, auditors, and authorities can collaborate efficiently.

---

## 🌐 Live Portals

| Portal | Link |
|--------|------|
| Landing Page | [carbon-sync.netlify.app](https://carbon-sync.netlify.app) |
| Company Dashboard | [carbon-sync.netlify.app/pages/dashboard.html](https://carbon-sync.netlify.app/pages/dashboard.html) |
| Auditor Portal | [carbon-sync.netlify.app/pages/auditor.html](https://carbon-sync.netlify.app/pages/auditor.html) |
| Government Monitor | [carbon-sync.netlify.app/pages/government.html](https://carbon-sync.netlify.app/pages/government.html) |

---

## 🚀 Problem Statement

Carbon emission tracking in industries is often manual, scattered, and inefficient.  
Organizations face challenges in:
- Managing emission data
- Generating structured reports
- Ensuring transparency and compliance

At the same time, auditors and authorities lack a unified system to verify and monitor this data effectively.

---

## 💡 Solution

CarbonSync addresses these challenges by providing a centralized digital platform that:

- Enables companies to track and manage carbon emissions
- Allows auditors to verify and validate data
- Helps authorities monitor compliance and activities

The platform connects all stakeholders into a single, streamlined workflow.

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML, CSS, JavaScript |
| Backend | Java Spring Boot 3, Maven |
| Database | PostgreSQL (wiring in progress) |
| Deployment | Netlify (frontend) · Railway (backend — upcoming) |
| Tools | Git, GitHub, Postman |

---

## 📁 Project Structure

```
CarbonSync/
├── frontend/
│   ├── index.html              # Landing page
│   ├── pages/
│   │   ├── dashboard.html      # Company portal
│   │   ├── auditor.html        # Auditor portal
│   │   ├── government.html     # Government monitor
│   │   ├── login-company.html
│   │   ├── login-auditor.html
│   │   └── login-govt.html
│   ├── styles/                 # Design system (variables, base, layout, components)
│   ├── scripts/                # Globe, animations, API wrappers
│   └── assets/                 # Icons, fonts, images
└── backend/
    ├── pom.xml
    └── src/main/java/com/carbonsync/
        ├── auth/               # JWT auth (stub)
        ├── company/            # Company CRUD
        ├── auditor/            # Auditor CRUD
        ├── government/         # Government body CRUD
        ├── carbon/             # Carbon entry CRUD
        ├── config/             # CORS, Security
        ├── exception/          # Global error handling
        └── utils/              # Response wrapper
```

---

## ⚙️ Features (Built / In Progress)

- Monochromatic black & gold design system
- Interactive Spline 3D hero on landing page
- Three role-based portals — Company, Auditor, Government
- Animated dashboards with live counters, charts, and terminal widgets
- Auditor review queue with expand / verify / flag toggle
- Government compliance table with real CSV, JSON, and PDF export
- Onboarding tour on each portal for new users
- Fully responsive — mobile hamburger nav, stacked layouts
- User authentication and role-based access
- Emission data tracking and management
- Report generation and structured data handling
- Auditor verification workflow
- Centralized dashboard for monitoring

---

## 📐 Architecture & Design Documentation

Comprehensive architectural diagrams are available in the [`docs/diagrams/`](docs/diagrams/) directory:

| Diagram | Description | Link |
|---------|-------------|------|
| **High-Level Design (HLD)** | System architecture overview, tech stack, layers | [View](docs/diagrams/01-high-level-design.md) |
| **Low-Level Design (LLD)** | Classes, methods, design patterns, API format | [View](docs/diagrams/02-low-level-design.md) |
| **Entity-Relationship (ERD)** | Database schema, tables, relationships, constraints | [View](docs/diagrams/03-entity-relationship-diagram.md) |
| **Data Flow Diagram (DFD)** | Data movement, processes, transformations | [View](docs/diagrams/04-data-flow-diagram.md) |
| **Sequence Diagrams** | User flows, authentication, verification, monitoring | [View](docs/diagrams/05-sequence-diagrams.md) |
| **Component Diagram** | Components, dependencies, integrations | [View](docs/diagrams/06-component-diagram.md) |
| **Deployment Diagram** | Infrastructure, CI/CD, monitoring, scaling | [View](docs/diagrams/07-deployment-diagram.md) |
| **Use Case Diagram** | User roles, features, requirements | [View](docs/diagrams/08-use-case-diagram.md) |

> 📖 **Quick Start**: New to the project? Start with [HLD](docs/diagrams/01-high-level-design.md) → [Use Cases](docs/diagrams/08-use-case-diagram.md) → [LLD](docs/diagrams/02-low-level-design.md)

---

## 🧠 System Design Highlights

- **Architecture**: 3-tier (Frontend, Backend, Database)
- **Role-based Access**: Company / Auditor / Government
- **Data Model**: Normalized relational database (PostgreSQL)
- **API Design**: RESTful with standard response wrapper
- **Security**: JWT authentication, HTTPS, input validation
- **Scalability**: Stateless backend, horizontal scaling ready

---

## ⚡ Run Locally

**Frontend**
```bash
cd frontend
python3 -m http.server 5500
# Open http://localhost:5500
```

**Backend**
```bash
cd backend
mvn spring-boot:run
# API at http://localhost:8080/api
```

---

## 🔌 API Endpoints

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/auth/signup` | Register a new user |
| POST | `/api/auth/login` | Authenticate and receive token |
| GET | `/api/companies` | List all companies |
| GET | `/api/auditors` | List all auditors |
| GET | `/api/government` | List all government bodies |
| GET | `/api/carbon` | List all carbon entries |
| GET | `/api/carbon/company/{id}` | Get entries for a company |

---

## 📌 Current Status

🚧 The frontend prototype is fully built and live. The Spring Boot backend compiles and runs with stub data. Database wiring and JWT authentication are the next milestones.

---

## 🎯 Future Scope

- Wire PostgreSQL via Railway
- Implement JWT authentication
- Connect frontend API calls to live backend
- Advanced analytics and insights
- AI-assisted anomaly detection for auditors
- Carbon credit marketplace module
- Integration with external systems
- Scalable architecture for larger datasets

---

## 🤝 Contributors

- Yash Tripathi
- Somiya Chaturvedi
- Tarang Nemani
- Utkarsh Tiwari

---

## 📬 Contact

For any queries or collaboration, feel free to connect.
