# SWE-MLAB: Software Engineering Machine Laboratory
## Comprehensive Technical Report, Pedagogical Architecture, and System Documentation

**Department of Software Engineering**  
**School of Information and Communication Technology (SICT)**  
**Federal University of Technology Minna (FUTMinna)**  
*Academic Session: 2025/2026*  
**Interactive Landing & Handover Portal:** [`docs/index.html`](./docs/index.html)  

---

```
  ____  _      __ _____       __  __ _        _     ____  
 / ___|| | /\ / /| ____|     |  \/  | |      / \   | __ ) 
 \___ \| |/  / / |  _|  _____| |\/| | |     / _ \  |  _ \ 
  ___) |   /  \  | |___|_____| |  | | |___ / ___ \ | |_) |
 |____/ \_/    \_\_____|     |_|  |_|_____/_/   \_\|____/ 
                                                           
 Automated Pedagogical Cloud for Software Engineering Education
```

---

## Table of Contents
1. [Executive Summary & Institutional Context](#1-executive-summary--institutional-context)
2. [Pedagogical Framework & Academic Curriculum](#2-pedagogical-framework--academic-curriculum)
3. [System Architecture & Technology Stack](#3-system-architecture--technology-stack)
4. [Core Subsystems & Technical Deep Dives](#4-core-subsystems--technical-deep-dives)
   - 4.1 [Institutional Authentication & RBAC](#41-institutional-authentication--rbac)
   - 4.2 [Course & Activity Management](#42-course--activity-management)
   - 4.3 [Team Formation & Collaboration Engine](#43-team-formation--collaboration-engine)
   - 4.4 [GitHub Organization Provisioning & Migration Pipeline](#44-github-organization-provisioning--migration-pipeline)
   - 4.5 [Evaluation, Submission & Automated Feedback Engine](#45-evaluation-submission--automated-feedback-engine)
   - 4.6 [Academic Project Showcase Portal](#46-academic-project-showcase-portal)
5. [Database Architecture & Data Model Reference](#5-database-architecture--data-model-reference)
6. [API Specification Reference](#6-api-specification-reference)
7. [User Manuals: Students, Instructors & Super Administrators](#7-user-manuals-students-instructors--super-administrators)
   - 7.1 [Student User Guide](#71-student-user-guide)
   - 7.2 [Instructor Operation Manual](#72-instructor-operation-manual)
   - 7.3 [Super Administrator & DevOps Runbook](#73-super-administrator--devops-runbook)
8. [Deployment, DevOps & Environment Configuration](#8-deployment-devops--environment-configuration)
9. [Operational Troubleshooting & Failure Recovery](#9-operational-troubleshooting--failure-recovery)
10. [Future Roadmap & Research Directions](#10-future-roadmap--research-directions)
11. [Authors, Governance & Institutional Accreditation](#11-authors-governance--institutional-accreditation)

---

## 1. Executive Summary & Institutional Context

### 1.1 The Challenge in Modern Software Engineering Pedagogy
Undergraduate software engineering education at scale faces significant systemic bottlenecks:
- **Dispersed Code Repositories:** Students frequently host coursework on disparate personal accounts, making code audit, plagiarism detection, and institutional archival difficult.
- **Asymmetric Team Contributions:** Assessing individual participation within collaborative student teams is often opaque without commit telemetry and automated tracking.
- **Delayed Feedback Loops:** Manual evaluation of projects by instructors causes evaluation lag, denying students rapid formative feedback during sprint cycles.
- **Friction in Cloud Deployment:** Moving student code from local development environments to live web deployments involves non-trivial configuration hurdles.

### 1.2 The SWE-MLAB Solution
**SWE-MLAB (Software Engineering Machine Laboratory)** is an integrated pedagogical cloud platform engineered specifically for the Department of Software Engineering at FUTMinna. 

SWE-MLAB bridges the gap between academic theory and industry practices by providing:
1. **Automated GitHub Organization Management:** All student coursework, labs, and group projects are automatically created, permissioned, or transferred into the institutional GitHub organization (`software-engineering-futminna`).
2. **Institutional Verification:** Restricts enrollment to authorized institutional email domains (`@st.futminna.edu.ng`).
3. **Continuous Activity Tracking:** Ingests Git commits, pull requests, and co-author tags to provide real-time contribution analytics.
4. **Instantaneous Repository Migration:** Allows instructors to transfer student repositories into the central organization in milliseconds while preserving full Git history.
5. **Interactive Academic Showcase:** Automatically indexes student deliverables, public live URLs, and team dossiers into a responsive, accessible public showcase.

```mermaid
flowchart TD
    subgraph Students ["Student Tier (@st.futminna.edu.ng)"]
        S1[Student Login via GitHub] --> S2[Institutional Verification]
        S2 --> S3[Join / Create Project Teams]
        S3 --> S4[Push Code to Workspace]
    end

    subgraph SWEMLAB ["SWE-MLAB Core Platform"]
        S4 --> B1[Golang API Server]
        B1 --> B2[Team Formation & Roster Sync]
        B1 --> B3[Automated Migration Engine]
        B1 --> B4[Telemetry & Grading Engine]
        B1 --> DB[(Neon PostgreSQL Cloud)]
    end

    subgraph InstitutionalOrg ["GitHub Organization: software-engineering-futminna"]
        B3 --> G1[Automated Repo Creation]
        B3 --> G2[Instant Transfer API]
        B3 --> G3[Collaborator Permissions]
    end

    subgraph PublicPortal ["Public Academic Showcase"]
        B1 --> P1[showcase.html Standalone Portal]
        P1 --> P2[Live Web Demos & Team Dossiers]
    end
```

---

## 2. Pedagogical Framework & Academic Curriculum

### 2.1 Course Alignment: FUTM-SWE-221
SWE-MLAB was piloted during the 2025/2026 academic session for **FUTM-SWE-221: Individual Software Development Project I** (Second Semester).

The course is structured around real-world software engineering learning milestones:
- **Sprint 1 — Proposal & Architectural Wireframing:** System design, REST API selection, and component breakdown.
- **Sprint 2 — Asynchronous Data Integration:** Asynchronous JavaScript (`fetch`, `async/await`), schema validation, and handling rate limits / network dropouts.
- **Sprint 3 — Client-Side Resilience & Offline-First UX:** Progressive Web App (PWA) asset caching, `localStorage` / `IndexedDB` persistence, and Swiss design principles.
- **Sprint 4 — Production Cloud Deployment & Oral Defense:** Automated staging on Netlify/Vercel/Railway, continuous integration, and technical defense.

### 2.2 Academic Scope & Student Developmental Expectations
To maintain fair evaluation, SWE-MLAB explicitly contextualizes student work:
- **Focus Areas:** Real-time data consumption, responsive UI composition, client-side caching, and team Git branching hygiene.
- **Developmental Context:** Projects represent foundational milestones for 200-level undergraduates; they are academic demonstrations rather than hardened enterprise platforms.

---

## 3. System Architecture & Technology Stack

SWE-MLAB is built on a high-performance, containerized micro-monolith architecture designed for high availability, low latency, and zero build friction.

```mermaid
graph LR
    subgraph Client ["Client Presentation Layer"]
        A[React 18 SPA / Vite]
        B[Swiss Design Tokens / CSS Variables]
        C[Lucide Icons / Canvas Visualizers]
    end

    subgraph Gateway ["Reverse Proxy & Ingress"]
        D[Nginx / Cloudflare Edge]
    end

    subgraph AppServer ["Application Layer (Golang)"]
        E[Gin Web Framework]
        F[GORM ORM Engine]
        G[GitHub REST/GraphQL Client]
        H[OAuth2 Token Exchange]
    end

    subgraph Persistence ["Persistence Layer"]
        I[(Neon Cloud PostgreSQL)]
        J[Sentry Telemetry]
    end

    Client -->|HTTPS / REST API| Gateway
    Gateway --> AppServer
    AppServer --> Persistence
```

### 3.1 Backend Specifications
- **Language:** Go (Golang) 1.22+
- **HTTP Routing:** Gin Framework (`github.com/gin-gonic/gin`)
- **ORM & Database Client:** GORM (`gorm.io/gorm`) with `postgres` driver (`gorm.io/driver/postgres`)
- **Third-Party API Client:** `go-github` (`github.com/google/go-github/v60/github`)
- **Authentication:** OAuth2 (`golang.org/x/oauth2`) with GitHub Provider
- **Monitoring & Crash Reporting:** Sentry Go SDK (`github.com/getsentry/sentry-go`)

### 3.2 Frontend Specifications
- **Core Library:** React 18
- **Bundler & Tooling:** Vite 5
- **Routing:** React Router v6
- **UI Architecture:** Custom Swiss International Typographic Design System (pure Vanilla CSS with CSS custom properties, zero Tailwind dependency to ensure maximum performance and predictability)
- **Typography:** Inter (General Text), JetBrains Mono (Code & Telemetry)

### 3.3 Infrastructure & Cloud Persistence
- **Production Database:** Neon Serverless PostgreSQL with connection pooling (`ep-falling-darkness-al1utzuu-pooler.c-3.eu-central-1.aws.neon.tech`)
- **Containerization:** Multi-stage Docker containers with Alpine Linux base
- **Reverse Proxy:** Nginx with gzip compression, security headers, and Single Page Application fallback routing

---

## 4. Core Subsystems & Technical Deep Dives

### 4.1 Institutional Authentication & RBAC
SWE-MLAB implements an OAuth2 flow integrated with student email verification:

1. **GitHub OAuth Handshake:** The student initiates login via GitHub OAuth.
2. **Institutional Domain Enforcement:** The backend inspects the student's verified GitHub email addresses. If the email matches the configured `INSTITUTIONAL_DOMAIN` (`@st.futminna.edu.ng`), the account is flagged as `institutional_verified: true`.
3. **Role-Based Access Control (RBAC):**
   - `student`: Can view enrolled courses, join/create teams, push submissions, and view grades.
   - `instructor`: Can create courses, define activities/projects, trigger repository migrations, configure grading rubrics, and publish showcases.
   - `super_admin`: Full system configuration, organization API credential management, and audit logs.

### 4.2 Course & Activity Management
Instructors create structured learning modules with fine-grained controls:
- **Course Enrollment Management:** Students enroll in courses via course codes or institutional invitation links.
- **Activity Specification Engine:** Define deliverables, wireframe requirements, expected API endpoints, and markdown guidelines.
- **Submission Windows & Grace Periods:** Automated deadline enforcement with timestamped Git commit verification.

### 4.3 Team Formation & Collaboration Engine
To support collaborative engineering, SWE-MLAB features a team management subsystem:
- **Team Creation & Cap Enforcement:** Students create teams with a strictly enforced `max_team_size` parameter.
- **Invitation Protocol:** Team leads issue invitations via student matric numbers or GitHub usernames; invitees can accept or decline.
- **Team Roster Synchronization:** Team member profiles, matriculation numbers, and roles (Lead / Member) are tracked in the database and synchronized with GitHub repository collaborator permissions.

### 4.4 GitHub Organization Provisioning & Migration Pipeline
One of SWE-MLAB's most critical architectural innovations is the automated **Repository Migration Pipeline**.

#### The Challenge:
Students often start projects in their personal GitHub repositories (`Taiwrash/project-name` or `MoTaC1/LIVE-WEATHER-APP`). Manually copying code via file uploads loses commit history and author attribution.

#### The Solution (`TransferToOrg`):
SWE-MLAB leverages GitHub's Repository Transfer API:
1. **Single API Transfer:** Executes `client.Repositories.Transfer(ctx, owner, repo, TransferRequest{NewOwner: org})`.
2. **Asynchronous 202 Handling:** GitHub returns HTTP `202 Accepted` when scheduling large repository transfers. The SWE-MLAB backend captures `*github.AcceptedError` and resolves the target URL immediately without failing.
3. **Stale Repository Cleanup:** If an empty placeholder repo exists in the org from a previous attempt, SWE-MLAB detects and deletes it prior to initiating the transfer.
4. **Visibility & Permissions Enforcement:** Automatically updates repository visibility to `Public` and re-invites all registered team members as repository collaborators.

```mermaid
sequenceDiagram
    autonumber
    actor Instructor
    participant Frontend as Instructor Dashboard
    participant Backend as Golang API (/api/v1/teams/:id/migrate-repo)
    participant DB as PostgreSQL Database
    participant GitHub as GitHub API (api.github.com)
    participant Org as GitHub Org (software-engineering-futminna)

    Instructor->>Frontend: Click "Migrate to Org"
    Frontend->>Backend: POST /api/v1/teams/:id/migrate-repo
    Backend->>DB: Load Team & Member Usernames
    Backend->>GitHub: Check if Org Repo Exists
    alt Stale / Empty Repo Exists in Org
        Backend->>GitHub: DELETE /repos/software-engineering-futminna/{repo}
    end
    Backend->>GitHub: POST /repos/{source_owner}/{source_repo}/transfer (NewOwner: org)
    GitHub-->>Backend: HTTP 202 Accepted (Transfer Scheduled)
    Backend->>GitHub: PATCH /repos/software-engineering-futminna/{repo} (Private: false)
    loop For Each Team Member
        Backend->>GitHub: PUT /repos/software-engineering-futminna/{repo}/collaborators/{username}
    end
    Backend->>DB: Update team.repo_url & team.repo_full_name
    Backend-->>Frontend: HTTP 200 OK (Migration Completed)
    Frontend-->>Instructor: Show Success Toast & Updated Org Link
```

### 4.5 Evaluation, Submission & Automated Feedback Engine
- **Submission Webhooks:** SWE-MLAB listens to GitHub webhook push events on student repositories.
- **Commit Telemetry:** Parses commit authors, timestamps, and commit messages to verify continuous integration rather than single large dump commits.
- **Instructor Grading Console:** Instructors grade submissions across standardized rubric criteria (Functionality, UI/UX, API Integration, Code Quality) with direct markdown feedback delivery to students.

### 4.6 Academic Project Showcase Portal
SWE-MLAB includes a standalone, production-ready showcase portal ([`showcase.html`](https://software-engineering-futminna.github.io/showcase/)):
- **Zero-Dependency Architecture:** Single self-contained HTML/CSS/JS file deployable directly to GitHub Pages, Netlify, or Vercel.
- **Swiss Design System:** Clean typographic hierarchy, strict mathematical layout grids, and the official institutional palette (FUTMinna Navy `#0B132B`, SWE-MLAB Blue `#3B7DD8`, and Emerald `#10B981`).
- **Interactive Search & Filtering:** Real-time multi-dimensional search across project titles, team names, student names, matric numbers, APIs, and tech stacks.
- **Dual View Modes:** Toggle between modular card grid view and academic tabular data list view.
- **Academic Dossier Modals:** Deep-dive modal inspection showing problem statements, system architectures, live deployment links, and complete student engineering rosters.

---

## 5. Database Architecture & Data Model Reference

The SWE-MLAB database schema is fully normalized and managed via GORM migrations.

```mermaid
erDiagram
    USERS ||--o{ ENROLLMENTS : has
    COURSES ||--o{ ENROLLMENTS : contains
    COURSES ||--o{ ACTIVITIES : defines
    COURSES ||--o{ PROJECTS : includes
    PROJECTS ||--o{ TEAMS : forms
    TEAMS ||--o{ TEAM_MEMBERS : contains
    USERS ||--o{ TEAM_MEMBERS : belongs_to
    TEAMS ||--o{ TEAM_INVITATIONS : issues
    USERS ||--o{ TEAM_INVITATIONS : receives
    ACTIVITIES ||--o{ SUBMISSIONS : receives
    TEAMS ||--o{ SUBMISSIONS : submits
    SUBMISSIONS ||--o{ FEEDBACKS : receives

    USERS {
        uint id PK
        int64 github_id UK
        string username
        string email UK
        string full_name
        string matric_number
        string role
        boolean institutional_verified
        string session_token
        timestamp created_at
    }

    COURSES {
        uint id PK
        string title
        string code UK
        string description
        uint instructor_id FK
        string semester
        boolean hide_results
        timestamp created_at
    }

    PROJECTS {
        uint id PK
        uint course_id FK
        string title
        string description
        string api_endpoint
        int max_team_size
        string repo_url
        string repo_full_name
        timestamp created_at
    }

    TEAMS {
        uint id PK
        uint project_id FK
        string name
        string status
        string repo_url
        string repo_full_name
        timestamp created_at
    }

    TEAM_MEMBERS {
        uint id PK
        uint team_id FK
        uint user_id FK
        string role
        timestamp joined_at
    }

    SUBMISSIONS {
        uint id PK
        uint activity_id FK
        uint team_id FK
        string repo_url
        string live_url
        string status
        float score
        timestamp submitted_at
    }
```

---

## 6. API Specification Reference

### 6.1 Authentication & Profile
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| `GET` | `/auth/github` | Redirects to GitHub OAuth consent screen | No |
| `GET` | `/auth/github/callback` | Exchanges code for session token and verifies student profile | No |
| `GET` | `/api/v1/user/profile` | Retrieves authenticated user record with course enrollments | Yes |
| `POST` | `/auth/logout` | Invalidates active user session token | Yes |

### 6.2 Courses & Activities
| Method | Endpoint | Description | Role Required |
|---|---|---|---|
| `GET` | `/api/v1/courses` | Lists all active courses | Student / Instructor |
| `POST` | `/api/v1/courses` | Creates a new academic course | Instructor / Admin |
| `GET` | `/api/v1/courses/:id` | Retrieves course syllabus, activities, and enrolled student roster | Enrolled User |
| `POST` | `/api/v1/courses/:id/enroll` | Enrolls student into course | Student |
| `GET` | `/api/v1/courses/:id/activities` | Lists all activity deliverables for a course | Enrolled User |

### 6.3 Teams & Repository Migration
| Method | Endpoint | Description | Role Required |
|---|---|---|---|
| `POST` | `/api/v1/projects/:id/teams` | Creates a new student project team | Student |
| `POST` | `/api/v1/teams/:id/invitations` | Invites a student into a team | Team Lead |
| `POST` | `/api/v1/invitations/:id/accept` | Accepts pending team invitation | Invitee Student |
| `POST` | `/api/v1/teams/:id/migrate-repo` | Migrates team repo into the institutional GitHub organization | Instructor / Admin |

### 6.4 Submissions & Webhooks
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| `POST` | `/api/v1/activities/:id/submit` | Records project repository & live URL submission | Student |
| `POST` | `/api/v1/webhooks/github` | Ingests GitHub push/PR events for commit tracking | HMAC Signature |
| `POST` | `/api/v1/submissions/:id/grade` | Submits score and rubric feedback for a submission | Instructor |

---

## 7. User Manuals: Students, Instructors & Super Administrators

### 7.1 Student User Guide

```
+-------------------------------------------------------------------------+
|                         STUDENT WORKFLOW OVERVIEW                       |
|                                                                         |
|  [GitHub Login] ➔ [Institutional Verify] ➔ [Enroll in Course]           |
|         ➔ [Create / Join Team] ➔ [Push Code & Deploy] ➔ [Submit URLs]   |
+-------------------------------------------------------------------------+
```

#### Step 1: Authentication & Institutional Verification
1. Navigate to the SWE-MLAB portal in your browser.
2. Click **"Sign in with GitHub"** and authorize the application.
3. Ensure that your official university email (`<username>@st.futminna.edu.ng`) is added and marked as **verified** in your GitHub Email Settings (`github.com/settings/emails`).
4. SWE-MLAB will automatically detect your domain and assign your verified student profile.

#### Step 2: Course Enrollment
1. On your **Student Dashboard**, locate your enrolled course (e.g. `FUTM-SWE-221: Individual Software Development Project I`).
2. If enrolling via an invitation link or course code, click **"Join Course"** and enter the code provided by your instructor.
3. Access the course dashboard to review the syllabus, activity milestones, and rubric weights.

#### Step 3: Team Formation & Role Assignment
1. Switch to the **Projects** tab within the course.
2. Select a project domain from the approved catalog (e.g., *NaijaWeather*, *NutriFlow*, *Cinemaflix*, *DevJokes*).
3. **If you are the Team Lead:**
   - Click **"Create Team"** and specify a unique team name.
   - Enter the FUTMinna Matriculation Numbers or GitHub handles of your prospective teammates to send invitations.
   - Note: Team capacity is restricted by the course's `max_team_size` limit.
4. **If you are a Team Member:**
   - Open your dashboard **Invitations** widget.
   - Review pending invitations from team leads and click **"Accept"** to join the team workspace.

#### Step 4: Development, Collaboration & Deployment
1. Clone the team's designated repository or initialize development in your team workspace.
2. Ensure every team member commits and pushes using their verified GitHub account so commit telemetry correctly tracks individual contributions.
3. Adhere to the sprint guidelines:
   - Connect third-party RESTful APIs with asynchronous error handling.
   - Implement responsive Swiss UI layouts with high accessibility contrast.
   - Implement client-side persistence (`localStorage` / PWA caching).
4. Deploy the production application to a recognized cloud host (e.g. Netlify, Vercel, Railway, GitHub Pages).

#### Step 5: Deliverable Submission & Grade Review
1. When your project meets the sprint requirements, navigate to the **Submissions** tab.
2. Provide your **Live Application URL** and confirm your **Source Code Repository**.
3. Click **"Submit Deliverable"** before the deadline cutoff.
4. Once evaluated, check the **Feedback Console** to view automated metrics, rubric breakdowns, and instructor commentary.

---

### 7.2 Instructor Operation Manual

```
+-------------------------------------------------------------------------+
|                       INSTRUCTOR WORKFLOW OVERVIEW                      |
|                                                                         |
|  [Create Course] ➔ [Define Activities & Rubrics] ➔ [Monitor Teams]      |
|         ➔ [1-Click Org Migration] ➔ [Grade & Publish Feedback]         |
+-------------------------------------------------------------------------+
```

#### Step 1: Accessing the Instructor Console
1. Log in with your authorized GitHub instructor account (`role: instructor` or `role: super_admin`).
2. Switch to the **Instructor Dashboard** from the navigation bar.

#### Step 2: Course & Activity Provisioning
1. Click **"Create Course"**:
   - Specify Course Title (e.g., `Individual Software Development Project I`), Course Code (`FUTM-SWE-221`), and Semester (`Second Semester`).
2. Click **"Add Activity / Project"**:
   - Provide the project title, abstract guidelines, expected REST API specification, maximum team size, and deadline timestamps.
   - Configure rubric criteria (e.g. Functionality: 40%, UI/UX: 20%, API Integration: 20%, Code Quality: 20%).

#### Step 3: Real-Time Team & Telemetry Monitoring
1. Open the **Teams Management Matrix**.
2. Inspect team rosters: View student full names, matriculation numbers, team lead designations, and invitation statuses.
3. Monitor real-time commit activity to ensure balanced participation among teammates.

#### Step 4: One-Click Organization Migration (`Migrate to Org`)
When student teams develop repositories under personal GitHub accounts, migrate them into the central department organization:
1. Locate the team entry in the Instructor Dashboard.
2. Click the **"⇄ Migrate to Org"** button.
3. The automated migration pipeline will:
   - Transfer the personal repository directly to `software-engineering-futminna`.
   - Preserve all original commit histories and authorship tags.
   - Ensure the repository visibility is set to `Public`.
   - Automatically invite all registered student team members as collaborators.

#### Step 5: Evaluating Submissions & Publishing Feedback
1. Navigate to the **Submissions Review Console**.
2. Click on a submission to test the live application URL and inspect the source code repository.
3. Enter numeric scores across rubric dimensions and write constructive markdown feedback.
4. Toggle **"Publish Grades"** when evaluation is complete to release feedback to students.

#### Step 6: Updating the Academic Showcase Portal
1. Export the validated project rosters directly from the SWE-MLAB database.
2. Update the standalone [`showcase.html`](https://software-engineering-futminna.github.io/showcase/) file.
3. Push changes to the public showcase repository ([`software-engineering-futminna/showcase`](https://github.com/software-engineering-futminna/showcase)) to refresh the public exhibition.

---

### 7.3 Super Administrator & DevOps Runbook

```
+-------------------------------------------------------------------------+
|                     SUPER ADMIN & DEVOPS LIFECYCLE                      |
|                                                                         |
|  [OAuth & Org Config] ➔ [Cloud DB Provisioning] ➔ [Telemetry Monitoring]|
|         ➔ [Disaster Recovery & Sync Scripts] ➔ [Audit & Governance]     |
+-------------------------------------------------------------------------+
```

#### Step 1: Initial System Provisioning & Organization Setup
1. **Create GitHub Organization:** Create or access `software-engineering-futminna` on GitHub.
2. **Register OAuth Application:**
   - In GitHub Settings > Developer Settings > OAuth Apps > New OAuth App.
   - Application Name: `SWE-MLAB FUTMinna`.
   - Homepage URL: `https://your-production-domain.com`.
   - Authorization Callback URL: `https://your-production-domain.com/auth/github/callback`.
   - Generate and securely store `GITHUB_CLIENT_ID` and `GITHUB_CLIENT_SECRET`.
3. **Grant Organization Access:** Ensure the OAuth app has third-party access approved under the `software-engineering-futminna` organization.

#### Step 2: Database Lifecycle & Configuration Management
1. Provision a Serverless PostgreSQL instance on **Neon Cloud**.
2. Configure `.env` with SSL connection pooling parameters (`sslmode=require&channel_binding=require`).
3. GORM automatically executes database auto-migrations on application boot, generating and updating tables (`users`, `courses`, `projects`, `teams`, `submissions`).

#### Step 3: Error Telemetry & Health Monitoring
1. Monitor application crashes and slow queries via **Sentry**:
   - Backend traces and panic recoveries are piped directly to the Sentry project (`SENTRY_DSN`).
2. Check database connection pool health: Ensure active connections do not exceed Neon pool limits.
3. Monitor GitHub API rate limit consumption (guaranteed 5,000 req/hr per authenticated instructor token).

#### Step 4: Maintenance & Operational Disaster Recovery
- **Resetting a Stale Migration:**
  If an instructor experiences a conflict when migrating a repository whose URL points to an org repo that was manually deleted:
  ```sql
  UPDATE teams 
  SET repo_url = 'https://github.com/OriginalOwner/repo-name',
      repo_full_name = 'OriginalOwner/repo-name'
  WHERE id = <team_id>;
  ```
- **Force-Syncing Student Codebases:**
  If a student repository was created empty in the org while code exists in a student's personal account:
  ```bash
  git clone https://github.com/StudentLead/PERSONAL-REPO.git /tmp/sync_repo
  cd /tmp/sync_repo
  git remote set-url origin https://github.com/software-engineering-futminna/ORG-REPO.git
  git push origin main --force
  rm -rf /tmp/sync_repo
  ```

---

## 8. Deployment, DevOps & Environment Configuration

### 8.1 Environment Variables Reference (`.env`)
```bash
# Database — Neon Serverless PostgreSQL (Cloud)
DATABASE_URL=postgresql://user:password@ep-falling-darkness-al1utzuu-pooler.c-3.eu-central-1.aws.neon.tech/neondb?sslmode=require&channel_binding=require

# GitHub OAuth Credentials
GITHUB_CLIENT_ID=Ov23lifo4cN06RMi11w8
GITHUB_CLIENT_SECRET=022447ab1d08e72b516c9e2314a7efea3f284bee
GITHUB_REDIRECT_URL=http://localhost:82/auth/github/callback

# Institutional Organization Governance
GITHUB_ORG=software-engineering-futminna
INSTITUTIONAL_DOMAIN=@st.futminna.edu.ng

# Webhook Security
GITHUB_WEBHOOK_SECRET=your_production_webhook_secret_here

# Sentry Telemetry & Crash Reporting
SENTRY_DSN=https://1b233cac06fc0ba6233c347ca5d11933@o4510844875440128.ingest.de.sentry.io/4510844896477264
```

### 8.2 Containerized Deployment with Docker Compose
```yaml
version: '3.8'

services:
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "8080:8080"
    environment:
      - DATABASE_URL=${DATABASE_URL}
      - GITHUB_CLIENT_ID=${GITHUB_CLIENT_ID}
      - GITHUB_CLIENT_SECRET=${GITHUB_CLIENT_SECRET}
      - GITHUB_ORG=${GITHUB_ORG}
      - SENTRY_DSN=${SENTRY_DSN}
    restart: unless-stopped

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "80:80"
    depends_on:
      - backend
    restart: unless-stopped
```

---

## 9. Operational Troubleshooting & Failure Recovery

### 9.1 Handling GitHub API Rate Limits (403 Forbidden)
- **Symptom:** Anonymous or unauthenticated GitHub API calls return `HTTP 403 rate limit exceeded (60/hr)`.
- **Resolution:** All GitHub API service methods in SWE-MLAB (`github_service.go`) must pass the instructor's or user's active `SessionToken` via `oauth2.StaticTokenSource`, which expands the rate limit to **5,000 requests/hour**.

### 9.2 Resolving Stale Migration Records
- **Symptom:** Instructor clicks *Migrate to Org* and receives: `"The recorded repo URL points to the org but the repo no longer exists."`
- **Root Cause:** A previous migration recorded an org URL in the database, but the repository was later deleted or reset on GitHub.
- **Resolution:** Update the team's `repo_url` in the database to point to the actual student source repository (e.g. `Taiwrash/project-name` or `StudentUser/project-name`), then click *Migrate to Org* again.

### 9.3 Syncing Unmigrated Student Codebases
- **Symptom:** A team repository exists in the organization but has 0 commits because the student team lead pushed code to their personal profile.
- **Resolution:** Clone the student's personal repository and push it directly to the organization repository:
```bash
git clone https://github.com/StudentUsername/STUDENT-REPO.git temp_sync
cd temp_sync
git remote set-url origin https://github.com/software-engineering-futminna/ORG-REPO-NAME.git
git push origin main --force
cd .. && rm -rf temp_sync
```

---

## 10. Future Roadmap & Research Directions

1. **Automated CI/CD Test Harness Integration:**
   - Incorporate GitHub Actions workflows that automatically execute Jest, PyTest, and Cypress test suites on student pull requests and report grading metrics back to SWE-MLAB.
2. **AI-Assisted Code Quality & Refactoring Advisor:**
   - Integrate an LLM-assisted code review assistant to provide students with pedagogical feedback on cyclomatic complexity, code smells, and security anti-patterns.
3. **Plagiarism & Code Authorship Verification:**
   - Implement AST-based (Abstract Syntax Tree) code similarity analysis across student cohorts to identify duplicate logic while respecting open-source library usage.
4. **Peer Review & Continuous Assessment Module:**
   - Enable structured peer review rounds where student teams evaluate peer UI/UX prototypes against course rubrics.

---

## 11. Authors, Governance & Institutional Accreditation

**SWE-MLAB** is maintained by the Department of Software Engineering, School of Information and Communication Technology (SICT), Federal University of Technology Minna.

- **Institution:** Federal University of Technology Minna (FUTMinna), Niger State, Nigeria
- **Department:** Department of Software Engineering
- **Course:** FUTM-SWE-221 (Individual Software Development Project I)
- **GitHub Organization:** [`software-engineering-futminna`](https://github.com/software-engineering-futminna)
- **Academic Showcase:** [`https://software-engineering-futminna.github.io/showcase/`](https://software-engineering-futminna.github.io/showcase/)

---
*© 2026 Department of Software Engineering, Federal University of Technology Minna. All rights reserved.*

