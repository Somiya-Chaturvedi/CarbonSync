# CarbonSync - Architecture Documentation

This directory contains comprehensive architectural diagrams and documentation for the CarbonSync Carbon Credit Management Platform.

## 📊 Diagram Index

### 01. High-Level Design (HLD)
**File**: `01-high-level-design.md`

System architecture overview showing:
- 3-tier architecture (Frontend, Backend, Database)
- Technology stack
- System layers and components
- Design principles and non-functional requirements

**Use this when**: You need to understand the overall system structure and technology choices.

---

### 02. Low-Level Design (LLD)
**File**: `02-low-level-design.md`

Detailed component architecture showing:
- All classes, methods, and their interactions
- Controllers, Services, Repositories structure
- DTOs and Model definitions
- Design patterns used
- API response format

**Use this when**: You need to understand implementation details or add new features.

---

### 03. Entity-Relationship Diagram (ERD)
**File**: `03-entity-relationship-diagram.md`

Database schema design showing:
- All database tables and columns
- Relationships and foreign keys
- Constraints and indexes
- Data types and sizes
- Storage estimates

**Use this when**: You need to understand the data model or make database changes.

---

### 04. Data Flow Diagram (DFD)
**File**: `04-data-flow-diagram.md`

Data movement through the system showing:
- Level 0: Context diagram
- Level 1: High-level processes
- Level 2: Detailed process flows
- Data transformations
- Data stores

**Use this when**: You need to trace how data moves through the system.

---

### 05. Sequence Diagrams
**File**: `05-sequence-diagrams.md`

User interaction flows showing:
- Authentication flow
- Emission submission flow
- Auditor verification flow
- Government compliance monitoring
- Report export flow
- Error handling flow

**Use this when**: You need to understand the step-by-step interactions for specific features.

---

### 06. Component Diagram
**File**: `06-component-diagram.md`

System components and dependencies showing:
- Frontend components (pages, scripts, styles)
- Backend components (controllers, services, repositories)
- Security components
- External integrations
- Component lifecycle

**Use this when**: You need to understand component boundaries and dependencies.

---

### 07. Deployment Diagram
**File**: `07-deployment-diagram.md`

Infrastructure and deployment architecture showing:
- Production deployment setup
- Network architecture
- CI/CD pipeline
- Monitoring and observability
- Disaster recovery strategy
- Scaling strategy
- Security measures

**Use this when**: You need to deploy, scale, or troubleshoot infrastructure.

---

### 08. Use Case Diagram
**File**: `08-use-case-diagram.md`

User interactions and system use cases showing:
- All user roles and their capabilities
- Detailed use case descriptions
- Use case relationships (include, extend)
- Actor permissions matrix
- Complexity estimates

**Use this when**: You need to understand user requirements or plan feature development.

---

## 🎯 Quick Navigation Guide

### For New Developers
1. Start with **01-high-level-design.md** to understand the big picture
2. Read **08-use-case-diagram.md** to understand user requirements
3. Review **02-low-level-design.md** for implementation details
4. Check **03-entity-relationship-diagram.md** for database structure

### For Frontend Developers
- **06-component-diagram.md** → Frontend components section
- **05-sequence-diagrams.md** → User flows
- **04-data-flow-diagram.md** → API data flows

### For Backend Developers
- **02-low-level-design.md** → Classes and methods
- **03-entity-relationship-diagram.md** → Database schema
- **05-sequence-diagrams.md** → Service interactions

### For DevOps Engineers
- **07-deployment-diagram.md** → Complete infrastructure guide
- **01-high-level-design.md** → Technology stack

### For Product Managers
- **08-use-case-diagram.md** → Feature requirements
- **01-high-level-design.md** → System capabilities
- **04-data-flow-diagram.md** → Business processes

### For QA Engineers
- **08-use-case-diagram.md** → Test scenarios
- **05-sequence-diagrams.md** → Expected behaviors
- **04-data-flow-diagram.md** → Data validation points

---

## 📝 Diagram Format

All diagrams are written in **Mermaid** format, which renders natively on GitHub and in most modern markdown viewers.

### Viewing Diagrams
1. **On GitHub**: Diagrams render automatically when viewing `.md` files
2. **In VS Code**: Install "Markdown Preview Mermaid Support" extension
3. **Online**: Copy-paste Mermaid code to https://mermaid.live/

### Updating Diagrams
1. Edit the Mermaid code in the respective `.md` file
2. Preview changes before committing
3. Keep diagrams in sync with code changes
4. Update the "Last Updated" date in each file

---

## 🛠️ Tools Used

- **Diagram Language**: Mermaid.js
- **Version Control**: Git
- **Documentation**: Markdown
- **Rendering**: GitHub, VS Code, mermaid.live

---

## 📅 Document Maintenance

| Diagram | Last Updated | Next Review |
|---------|-------------|-------------|
| 01-HLD | 2025-03-31 | Before major releases |
| 02-LLD | 2025-03-31 | When adding new features |
| 03-ERD | 2025-03-31 | When schema changes |
| 04-DFD | 2025-03-31 | When workflows change |
| 05-Sequence | 2025-03-31 | When flows change |
| 06-Component | 2025-03-31 | When architecture changes |
| 07-Deployment | 2025-03-31 | Before infrastructure changes |
| 08-Use-Case | 2025-03-31 | When requirements change |

---

## 🤝 Contributing

When updating diagrams:
1. Create a feature branch
2. Update relevant diagram(s)
3. Test Mermaid rendering
4. Update "Last Updated" date
5. Submit pull request with description of changes

---

## 📞 Questions?

- **Architecture**: Contact backend team
- **Database**: Contact database admin
- **Infrastructure**: Contact DevOps team
- **Requirements**: Contact product manager

---

## 🔗 Related Documentation

- [Main README](../../README.md)
- [Backend README](../../backend/README.md)
- [API Documentation](../api/)
- [Development Guide](../development/)

---

*These diagrams are living documents and should be updated as the system evolves.*
