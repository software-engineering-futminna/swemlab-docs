# SWE-MLAB Technical Documentation & Architecture Report 📚

<div align="center">

![FUTMinna Department of Software Engineering](https://img.shields.io/badge/FUTMinna-Department%20of%20Software%20Engineering-0B132B?style=for-the-badge&logo=target&logoColor=3B7DD8)
![SWE-MLAB](https://img.shields.io/badge/Platform-SWE--MLAB%20Cloud-3B7DD8?style=for-the-badge)
![Curriculum](https://img.shields.io/badge/Course-FUTM--SWE--221-10B981?style=for-the-badge)
![Session](https://img.shields.io/badge/Academic%20Session-2025%2F2026-blue?style=for-the-badge)

**Federal University of Technology Minna (FUTMinna)**  
*School of Information and Communication Technology (SICT)*  
*Department of Software Engineering*

---

### 🌐 [Live Academic Showcase & Project Telemetry](https://software-engineering-futminna.github.io/showcase/)

</div>

---

## 📖 Table of Contents

- [1. Executive Summary](#1-executive-summary)
- [2. System Topology & Architecture](#2-system-topology--architecture)
- [3. Documentation Directory](#3-documentation-directory)
- [4. Role-Based User Manuals](#4-role-based-user-manuals)
- [5. Repository Migration Pipeline](#5-repository-migration-pipeline)
- [6. Development & Local Setup](#6-development--local-setup)

---

## 1. Executive Summary

**SWE-MLAB (Software Engineering Machine Laboratory)** is an integrated pedagogical cloud platform engineered for the Department of Software Engineering at the **Federal University of Technology Minna (FUTMinna)**.

The platform automates the software engineering educational lifecycle by providing:
1. **Automated GitHub Organization Management:** All coursework and student teams are provisioned and governed under the institutional organization (`software-engineering-futminna`).
2. **Institutional Identity Enforcement:** Verification of student emails under the `@st.futminna.edu.ng` domain.
3. **Continuous Telemetry & Activity Tracking:** Tracking individual commit contributions, branch hygiene, and milestone progress.
4. **Instantaneous Repository Migration (`TransferToOrg`):** Transferring personal repositories into the institutional organization in milliseconds with preserved commit histories.
5. **Interactive Public Showcase:** Live exhibition of student project deliverables, API integrations, and team rosters.

---

## 2. System Topology & Architecture

```
+-----------------------------------------------------------------------------------+
|                              SWE-MLAB SYSTEM TOPOLOGY                             |
+-----------------------------------------------------------------------------------+

  [CLIENT TIER]
  React 18 SPA + Vite 5 + Swiss Design Tokens + Vanilla CSS Custom Properties
          │
          │  HTTPS / JSON REST API
          ▼
  [INGRESS & REVERSE PROXY]
  Nginx Alpine (Gzip Compression, Security Headers, SPA Fallback Routing)
          │
          ▼
  [BACKEND APPLICATION SERVER (Golang 1.22+)]
  ├── Gin HTTP Router & Middleware (RBAC, Rate Limiting, Auth Context)
  ├── GORM ORM Layer (Transactions, Auto-Migrations, Model Associations)
  ├── GitHub API Client (OAuth2 Token Source, Org Repo Transfers, Webhooks)
  └── Sentry Go SDK (Telemetry, Panic Recovery, Error Tracking)
          │
          ├──▶ [PERSISTENCE TIER] Neon Serverless PostgreSQL Cloud (SSL Pooled)
          └──▶ [ORGANIZATION TIER] GitHub Org: software-engineering-futminna
```

---

## 3. Documentation Directory

Detailed documentation is organized into modular chapters within the [`docs/`](./docs/) directory:

| Chapter | Document | Description |
|---|---|---|
| **01** | [**Pedagogical Framework**](./docs/01-pedagogical-framework.md) | Course curriculum, 4-sprint lifecycle, learning milestones, and evaluation rubrics. |
| **02** | [**System Architecture**](./docs/02-system-architecture.md) | Technical stack breakdown (React, Go, Gin, GORM, Neon PostgreSQL, Nginx, Docker). |
| **03** | [**GitHub Migration Pipeline**](./docs/03-github-org-migration-pipeline.md) | Deep dive into `TransferToOrg`, asynchronous 202 handling, and collaborator sync. |
| **04** | [**REST API Specification**](./docs/04-api-specification.md) | Endpoints, authentication headers, request payloads, and response schemas. |
| **05** | [**Database Schema & ERD**](./docs/05-database-schema.md) | PostgreSQL data models, relationships, and Entity-Relationship Diagram. |
| **06** | [**User Operation Manuals**](./docs/06-user-guides.md) | Step-by-step guides for Students, Instructors, and Super Administrators. |
| **07** | [**DevOps & Deployment**](./docs/07-deployment-devops.md) | Docker Compose configurations, environment variables, Sentry, and cloud deployment. |
| **08** | [**Operational Troubleshooting**](./docs/08-troubleshooting-runbook.md) | Runbooks for rate limits, stale migration records, and database maintenance. |
| **Full** | [**Complete Technical Report**](./FULL_REPORT.md) | Consolidated, publication-grade academic report and architecture dossier. |

---

## 4. Role-Based User Manuals

- **Students (`@st.futminna.edu.ng`):** Sign in with GitHub, verify institutional email, join course `FUTM-SWE-221`, create/join project teams, push code, deploy to cloud staging, and submit project URLs.
- **Instructors:** Provision courses, define project rubrics, monitor real-time student team telemetry, execute 1-click repository migrations into `software-engineering-futminna`, grade submissions with markdown feedback, and update public showcases.
- **Super Administrators:** Manage GitHub Organization OAuth applications, configure Neon Serverless PostgreSQL connection pooling, monitor Sentry error traces, and execute disaster recovery runbooks.

---

## 5. Repository Migration Pipeline

SWE-MLAB eliminates manual file copying by transferring personal repositories directly into `software-engineering-futminna`:

```
[Instructor Dashboard]
        │  Click "Migrate to Org"
        ▼
[SWE-MLAB Backend (/api/v1/teams/:id/migrate-repo)]
        │
        ├── 1. Load team members & GitHub usernames from DB
        ├── 2. Clean up any stale placeholder repository in the organization
        ├── 3. Execute GitHub Transfer API: POST /repos/{source_owner}/{source_repo}/transfer
        │      └── Handle HTTP 202 Accepted via go-github (*github.AcceptedError)
        ├── 4. Set repository visibility to Public: PATCH /repos/software-engineering-futminna/{repo}
        ├── 5. Add all team members as collaborators: PUT /repos/{org}/{repo}/collaborators/{username}
        └── 6. Save updated organization repo URL in PostgreSQL
```

---

## 6. Development & Local Setup

```bash
# Clone this documentation repository
git clone https://github.com/software-engineering-futminna/docs.git
cd docs

# View documentation locally
open index.html   # or browse markdown files directly in docs/
```

---

## 🏛️ Institutional Governance

- **Institution:** Federal University of Technology Minna (FUTMinna)
- **Faculty:** School of Information and Communication Technology (SICT)
- **Department:** Department of Software Engineering
- **GitHub Organization:** [`software-engineering-futminna`](https://github.com/software-engineering-futminna)
- **Showcase Repository:** [`software-engineering-futminna/showcase`](https://github.com/software-engineering-futminna/showcase)
