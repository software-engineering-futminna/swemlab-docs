# SWE-MLAB Staff Onboarding & System Handover Workshop Manual
## Comprehensive Technical & Pedagogical Curriculum for Departmental Staff

**Institution:** Federal University of Technology Minna (FUTMinna)  
**Faculty:** School of Information and Communication Technology (SICT)  
**Department:** Department of Software Engineering  
**Departmental Super Admin Account:** `softwareng@futminna.edu.ng` | GitHub Profile & Org: [`https://github.com/software-engineering-futminna/`](https://github.com/software-engineering-futminna/)  
**Live Academic Showcase:** [`https://software-engineering-futminna.github.io/showcase/`](https://software-engineering-futminna.github.io/showcase/)  
**Full Technical Documentation:** [`https://github.com/software-engineering-futminna/swemlab-docs`](https://github.com/software-engineering-futminna/swemlab-docs)  
**Interactive Landing & Handover Portal:** [`docs/index.html`](./docs/index.html)  

---

## 1. Executive Context & Domain-Based Onboarding Architecture

As part of the contract completion and institutional handover in the Department of Software Engineering, this comprehensive workshop manual documents the complete architecture, daily instructor operations, AI/RAG workflows, GitHub Organization pipelines, and administrative custody of **SWE-MLAB (Software Engineering Machine Laboratory)**.

### 1.1 Zero-Friction Institutional Role Assignment (`backend/pkg/api/auth.go`)
SWE-MLAB uses **automated email-domain role resolution** via GitHub OAuth2 (`Scopes: ["user:email", "repo"]`). During login (`GET /auth/github/callback`), the backend calls `client.Users.ListEmails()` to inspect **all verified email addresses** (primary or secondary) attached to the user's GitHub account:

| Role | Institutional Rule / Identity | Assigned Capabilities in SWE-MLAB |
| :--- | :--- | :--- |
| **Super Admin** (`super_admin`) | Email: **`softwareng@futminna.edu.ng`**<br>GitHub Account: **`software-engineering-futminna`** | • Full departmental oversight across **all courses** and **all leaderboards** (`canSeeAllLeaderboards()`)<br>• GitHub Organization ownership (`software-engineering-futminna`)<br>• Bypasses individual course ownership restrictions (`RequireRole` middleware) |
| **Instructor** (`instructor`) | Any verified staff email ending in **`@futminna.edu.ng`**<br>*(excluding `@st.futminna.edu.ng`)* | • **Automatic onboarding—no manual DB query required**<br>• Full access to `/instructor` (**SWEMLAB Instructor Panel**)<br>• Create courses, generate weekly activities & projects with Gemini AI, upload RAG lecture notes, launch/migrate team repos, grade submissions, and export CSV results |
| **Student** (`student`) | Any verified student email ending in **`@st.futminna.edu.ng`** | • Access to `/dashboard` (**Student Portal**)<br>• Browse & enroll in courses, launch GitHub Codespaces ("Open Forge"), chat with the AI Lecture Note Tutor, create/join project teams, and view published results |
| **Rejected** (`guest`) | No verified `@futminna.edu.ng` or `@st.futminna.edu.ng` email | • Strictly blocked by domain gatekeeper and redirected to `/?error=institutional_email_required` |

> [!IMPORTANT]
> **Key Takeaway for Staff Onboarding:** Lecturers and Lab Technologists do **not** need manual database edits to become instructors. They simply add their official **`<name>@futminna.edu.ng`** email as a primary or secondary email in **`github.com/settings/emails`**, click the verification link in their staff inbox, and sign in to SWE-MLAB with GitHub.

---

## 2. Intensive Full-Day Workshop Agenda (6 Modules — 5.5 Hours)

| Time | Duration | Module | Core Focus & Codebase Subsystem | Target Staff |
| :--- | :--- | :--- | :--- | :--- |
| **09:00 – 09:45** | 45 mins | **Module 01** | **Platform Architecture, Domain RBAC & Staff Account Onboarding** (`auth.go`, `middleware.go`) | HOD, All Lecturers, TAs |
| **09:45 – 10:45** | 60 mins | **Module 02** | **Courses, Weekly Activities, Gemini AI Generation & Codespaces Autograding** (`activity.go`, `ai.go`, `webhook.go`) | Course Instructors, TAs |
| **10:45 – 11:00** | 15 mins | *Break* | *Staff GitHub `@futminna.edu.ng` Verification Check* | All Participants |
| **11:00 – 12:00** | 60 mins | **Module 03** | **Lecture Notes RAG Pipeline & Student AI Tutor** (`chat.go`, `rag_service.go`, `AIChatWidget.jsx`) | Course Instructors, TAs |
| **12:00 – 13:00** | 60 mins | **Module 04** | **Teams, Projects, Repo Launch & 1-Click Org Migration Pipeline** (`team.go`, `github_service.go`) | Course Instructors, TAs |
| **13:00 – 13:45** | 45 mins | **Module 05** | **Grading Engine, Results Moderation, CSV Export & Academic Showcase** (`InstructorDashboard.jsx`, `showcase.html`) | HOD, Instructors, Exam Officers |
| **13:45 – 14:30** | 45 mins | **Module 06** | **Departmental Super Admin Handover (`softwareng@futminna.edu.ng`), DevOps & Sign-Off** | HOD, Incoming Custodian |

---

## 3. Comprehensive Module-by-Module Technical Guide

---

### Module 01: Platform Architecture, Domain RBAC & Staff Account Onboarding (45 Mins)

#### 3.1.1 Live Onboarding Walkthrough for Departmental Staff (15 Mins)
Every participating staff member performs these 4 steps at the start of the workshop:
1. Log into GitHub and open **[https://github.com/settings/emails](https://github.com/settings/emails)**.
2. Under **Add email address**, enter your official university staff email (`<username>@futminna.edu.ng`).
   - *Note:* Staff can keep their personal Gmail/email as their primary GitHub email if they wish—SWE-MLAB inspects **all verified emails** on the GitHub account (`client.Users.ListEmails`).
3. Open your FUTMinna staff webmail and click the GitHub verification link so the email displays a green **`Verified`** badge on GitHub.
4. Navigate to the SWE-MLAB portal, click **Sign in with GitHub**, and verify that:
   - Your profile loads at `/dashboard` with the **Instructor Panel** button enabled.
   - Clicking **Instructor Panel** (`/instructor`) displays `INSTRUCTOR` (or `SUPER ADMIN` for `softwareng@futminna.edu.ng`) in the top-right header alongside your `@futminna.edu.ng` email.

#### 3.1.2 Important Edge Case: Existing Account Role Preservation
- Look at `backend/pkg/api/auth.go` (lines 153–163): When a user logs in for the **first time**, SWE-MLAB creates their record in `users` with the detected `role`. On subsequent logins, it updates `github_id`, `username`, `email`, `avatar_url`, `institutional_verified`, and `session_token`, **preserving** their existing `role` column.
- Therefore, if a staff member previously logged in *before* verifying their `@futminna.edu.ng` email (or tested with a student email), update their role once in PostgreSQL:
  ```sql
  UPDATE users
  SET role = 'instructor', institutional_verified = true
  WHERE email LIKE '%@futminna.edu.ng' AND email NOT LIKE '%@st.futminna.edu.ng';
  ```

---

### Module 02: Courses, Weekly Activities, Gemini AI Generation & Codespaces Autograding (60 Mins)

This module covers **Tab 1 (`My Courses`)** and **Tab 2 (`Weekly Activities`)** in `frontend/src/pages/InstructorDashboard.jsx`.

#### 3.2.1 Managing Courses (`Tab 1: My Courses`)
- **Course Visibility Rule:**
  - Regular instructors (`role = 'instructor'`) see only the courses they created (`course.instructor_id === user.id`).
  - The Departmental Super Admin (`softwareng@futminna.edu.ng` / `software-engineering-futminna`) sees **all courses across the department**.
- **Creating a Course:**
  - Enter **Course Title** (e.g., `Individual Software Development Project I`), **Course Code** (e.g., `FUTM-SWE-221`), and **Semester** (e.g., `2025/2026 Second Semester`), then click **Create Course**.
- **Empty Course Safeguard:**
  - When you create a course, SWE-MLAB automatically switches to the **Weekly Activities** tab. If you navigate away from `Weekly Activities` while a course still has `0` activities, SWE-MLAB prompts: *`Course "<Title>" has no activities. Delete it?`* to prevent empty orphan courses in the student catalog. Always create at least one activity (or click **Plan Semester**) right after creating a course.

#### 3.2.2 Creating Weekly Lab Activities (`Tab 2: Weekly Activities`)
Instructors have **three ways** to populate weekly activities:

1. **Method A — ✨ AI Activity Generator (`POST /api/v1/courses/:id/activities/generate`):**
   - Click **"🪄 AI Generate"** in the Add Activity panel.
   - Enter:
     - **Topic*** (e.g., *REST API Design with Go and Gin*)
     - **Learning Objectives** (e.g., *Implement CRUD endpoints, JSON validation, and unit tests*)
     - **Language** (`go`, `python`, `javascript`, `java`, `rust`)
     - **Lecture Excerpt (Optional)** (paste raw notes to ground the activity)
   - Click **Generate Activity**. Gemini AI generates the title, description, Markdown instructions, structured step-by-step tasks (`title`, `description`, `hints`), `dependencies`, `test_command`, `max_score`, and `industry_skills`.
   - Review the preview modal and click **"✅ Apply to Form"**, then click **Publish Activity**.
2. **Method B — 📅 Plan Semester Bulk Provisioning (`POST /api/v1/courses/:id/activities/bulk`):**
   - Click **"📅 Plan Semester"** and enter the number of weeks (e.g., `12`).
   - SWE-MLAB generates 12 sequential weekly activity slots (`Week 1` to `Week 12`) that the instructor can edit week by week.
3. **Method C — Manual Step-by-Step Builder:**
   - Fill in Basic Info, Environment Configuration (`language`, `dependencies`, `template_repo`), dynamic **Step-by-Step Instructions** (`+ Add Step`), **Autograding** (`Enable Autograding` + `Test Command`, e.g., `go test ./...` or `npm test`), `Max Score`, `Due Date`, and `Industry Skills`.

#### 3.2.3 Draft vs. Live Publishing & Student "Open Forge" (Codespaces)
- **Draft vs. Live Toggle:** Newly created activities default to **`Draft`** (`is_published: false`). Students cannot see an activity until the instructor clicks the **`Draft` → `Live`** toggle button on the activity card.
- **What Happens When a Student Clicks "Start Activity / Open Forge" (`POST /api/v1/activities/:id/start`):**
  1. SWE-MLAB provisions a student repository (from `template_repo` or fresh) and injects `.devcontainer/devcontainer.json` (configured for the activity's language & dependencies) plus `.github/workflows/autograde.yml` (configured with the instructor's `test_command`).
  2. Launches the student's cloud development environment (**GitHub Codespaces**) and records a submission with `status = 'started'`.

---

### Module 03: Lecture Notes RAG Pipeline & Student AI Tutor (60 Mins)

This module covers **Tab 5 (`Lecture Notes`)** in `frontend/src/pages/InstructorDashboard.jsx` (lines 685–788) and `backend/pkg/api/chat.go`.

#### 3.3.1 How the Course-Grounded RAG Tutor Works
SWE-MLAB includes a Retrieval-Augmented Generation (RAG) study assistant so students can ask questions directly against official course lecture notes:
1. **Strict File Validation (`UploadLectureNote`):**
   - Instructors select a `.pdf` or `.docx` file (up to **20 MB**) and an optional title (e.g., *Week 3 — Asynchronous JavaScript & REST APIs*).
   - The backend verifies both the file extension and **binary magic bytes** (`%PDF` for PDFs; `PK\x03\x04` ZIP header for DOCX) to prevent malformed uploads.
2. **Asynchronous Background Indexing (`go h.rag.ProcessLectureNote`):**
   - Upon upload, the note status is set to `⏳ Processing`.
   - A background goroutine extracts the text, splits it into semantic `LectureChunk` records in PostgreSQL, and transitions the status to `✅ Ready`.
3. **Live RAG Telemetry Bar:**
   - Displays four real-time metrics at the top of the **Lecture Notes** tab:
     - **Notes Uploaded** (`total_notes`)
     - **Ready for AI** (`ready_notes`)
     - **Knowledge Chunks** (`total_chunks`)
     - **Student Chat** (`✅` enabled as soon as `ready_notes > 0`)
4. **Student AI Chat Experience (`AIChatWidget.jsx`):**
   - Enrolled students open the floating AI Chat widget on `/dashboard`, select their course, and ask questions (up to 2,000 characters). The RAG service retrieves the relevant `LectureChunk` passages and answers with grounded explanations.

---

### Module 04: Teams, Projects, Repo Launch & 1-Click Org Migration Pipeline (60 Mins)

This module covers **Tab 6 (`Teams & Projects`)** in `frontend/src/pages/InstructorDashboard.jsx` (lines 790–1274) and `backend/pkg/api/team.go`.

#### 3.4.1 Creating Course Projects (Manual & ✨ AI Project Generator)
1. Open the **Teams & Projects** tab and click **`+ New Project`**.
2. **Option A — 🪄 AI Generate (`POST /api/v1/courses/:id/projects/generate`):**
   - Click **`🪄 AI Generate`** in the New Project modal.
   - Enter a **Topic** (e.g., *Real-Time Nigerian Weather & Agro-Advisory Dashboard*) and **Additional Context** (e.g., *Must integrate Open-Meteo REST API and support offline caching*).
   - Gemini AI populates the `Project Title`, Markdown `Description`, `Details URL Link`, and `API Endpoint URL`.
3. **Option B — Manual Configuration:**
   - Specify `Project Title`, Markdown `Description`, `Details URL Link`, `API Endpoint URL`, and select **Max Team Size** (`2`, `3`, `4`, `5`, or `6` members).

#### 3.4.2 How Students Form Teams on `/dashboard`
1. Students open the **Teams & Projects** view on their dashboard and pick a project from the catalog.
2. The **Team Lead** clicks **Create Team**, enters a team name, and is assigned `role = 'lead'` (`👑`).
3. The Team Lead searches for classmates by **GitHub Username** or **Matriculation Number** and sends team invitations (`POST /api/v1/teams/:id/invite`).
4. Invited students see pending invites in their dashboard (auto-polled every 30 seconds) and click **Accept** (`POST /api/v1/invites/:id/respond`).

#### 3.4.3 Managing Team Repositories in `software-engineering-futminna`
On each project card in the Instructor Panel, instructors see every formed team (`Team Name`, member count `3/4`, inline **Rename Team** `✏️` button, and full member roster with Matric Numbers). Each team card provides two repository actions:

1. **🚀 Launch (`POST /api/v1/teams/:id/launch-repo`) — When a Team Has No Repo Yet:**
   - Clicking **🚀 Launch** automatically creates a brand-new repository for that team inside the **`software-engineering-futminna`** GitHub Organization and invites all team members as collaborators.
2. **⇄ Migrate to Org (`POST /api/v1/teams/:id/migrate-repo`) — When a Team Built in a Personal Repo:**
   - If students started coding in a personal GitHub repository, clicking **⇄ Migrate to Org** executes the automated migration pipeline:
     1. Checks if an empty placeholder repo with the same name exists in `software-engineering-futminna` and deletes it if empty.
     2. Calls GitHub's Repository Transfer API (`Repositories.Transfer`) to move the student's repository into `software-engineering-futminna` while preserving 100% of commit history, branches, and PRs.
     3. Handles GitHub's asynchronous `HTTP 202 Accepted` response cleanly.
     4. Enforces `Public` visibility on the organization repository.
     5. Loops through all team members and adds them as GitHub collaborators (`push` access).
     6. Updates `repo_url` and `repo_full_name` in PostgreSQL.

---

### Module 05: Grading Engine, Results Moderation, CSV Export & Academic Showcase (45 Mins)

This module covers **Tab 3 (`Grade Submissions`)**, **Tab 4 (`Leaderboard`)**, **Tab 7 (`Results`)**, and [`showcase.html`](https://software-engineering-futminna.github.io/showcase/).

#### 3.5.1 Three Ways Submissions Are Graded (`Tab 3: Grade Submissions`)
1. **Automated GitHub Actions Webhook (`POST /api/v1/webhooks/github`):**
   - When a student pushes code, GitHub Actions runs the activity's `test_command` and fires a `workflow_run` webhook signed with `GITHUB_WEBHOOK_SECRET`.
   - If `conclusion == "success"`, SWE-MLAB automatically marks the submission `graded` with `100%` of `max_score`. If `failure`, it awards `50%` partial credit and links the workflow logs.
2. **AI-Assisted Grading (`POST /api/v1/submissions/:id/ai-grade`):**
   - Evaluates the submission logs and activity steps via Gemini AI to append structured **Strengths**, **Areas for Improvement**, and a **Bonus Score**.
3. **Instructor Manual Grading & Override (`Tab 3` & `Tab 7`):**
   - In **Grade Submissions** (auto-refreshes every 5s), enter a numeric score and feedback and click **Submit Grade**.
   - Or in **Tab 7 (`Results`)**, click **`+ Add Result`** (or **`Edit`** on any student row), select the **Activity**, enter the student's **GitHub Username**, input the **Score** and **Feedback**, and click **Save Result**.

#### 3.5.2 Results Moderation & CSV Export for Departmental Processing
- **Hide Results from Students (`Tab 7: Results`):**
  - Check the **`Hide Results from Students`** box (`POST /api/v1/courses/:id/toggle-results`) while grading or moderating scores. Uncheck it when ready to release grades to students' dashboards.
- **Exporting Official CSV Sheets (`Tab 4: Leaderboard`):**
  - Click **`⬇ Export CSV`** to download `<COURSE_CODE>_leaderboard_<YYYY-MM-DD>.csv` containing `Rank, Full Name, Matric Number, Username, Email, Completed Activities, Total Score`.

#### 3.5.3 Updating the Public Academic Showcase ([`showcase.html`](https://software-engineering-futminna.github.io/showcase/))
- At the end of each semester, update the `PROJECTS` array in [`showcase.html`](https://software-engineering-futminna.github.io/showcase/) and push to [`https://github.com/software-engineering-futminna/showcase`](https://github.com/software-engineering-futminna/showcase) to publish the cohort's live web deployments and team dossiers.

---

### Module 06: Departmental Super Admin Handover (`softwareng@futminna.edu.ng`), DevOps & Sign-Off (45 Mins)

#### 3.6.1 Transitioning Super Admin Custody to the Department
The codebase (`backend/pkg/api/auth.go` and `backend/pkg/api/middleware.go`) has been updated so that the official departmental identity is automatically recognized as **`super_admin`**:
- **Departmental Email:** `softwareng@futminna.edu.ng`
- **Departmental GitHub Account / Organization:** `software-engineering-futminna`

When signed in with the GitHub account `software-engineering-futminna` (or any GitHub account with `softwareng@futminna.edu.ng` verified), SWE-MLAB grants **`super_admin`** status, enabling visibility and management of **all courses, instructors, teams, and leaderboards** across the department.

#### 3.6.2 Production Environment Variables Checklist (`.env`)
| Variable | Purpose | Handover Action |
| :--- | :--- | :--- |
| `DATABASE_URL` | Neon Serverless PostgreSQL connection string (`sslmode=require`) | Transfer Neon project ownership to `softwareng@futminna.edu.ng` |
| `GITHUB_CLIENT_ID` | GitHub OAuth App Client ID | Verify OAuth App is owned by `software-engineering-futminna` Org |
| `GITHUB_CLIENT_SECRET` | GitHub OAuth App Client Secret | Store in Render/Docker production environment |
| `GITHUB_REDIRECT_URL` | OAuth callback (`/auth/github/callback`) | Match production backend URL |
| `GITHUB_ORG` | `software-engineering-futminna` | Departmental GitHub Organization |
| `GITHUB_WEBHOOK_SECRET` | HMAC-SHA256 secret for GitHub Actions autograding webhooks | Keep synchronized with Org/Repo webhook settings |
| `GEMINI_API_KEY` | Google Gemini API key for AI Activity/Project generation & RAG Lecture Tutor | Provision under `softwareng@futminna.edu.ng` Google Cloud account |
| `JWT_SECRET` | Secret key for signing 24-hour user JWT tokens | Maintain in production `.env` |
| `SENTRY_DSN` | Sentry error & crash telemetry endpoint | Invite `softwareng@futminna.edu.ng` to Sentry project |

---

## 4. Operational Troubleshooting & SQL/Git Runbook

### 4.1 Staff Member Logged In Before Verifying `@futminna.edu.ng` Email
Because `auth.go` preserves a user's existing `role` after initial account creation, if a lecturer logged in before verifying their `@futminna.edu.ng` email, run:
```sql
UPDATE users
SET role = 'instructor',
    institutional_verified = true
WHERE email LIKE '%@futminna.edu.ng'
  AND email NOT LIKE '%@st.futminna.edu.ng';
```

### 4.2 Ensuring Departmental Account Has `super_admin` in Existing DB
```sql
UPDATE users
SET role = 'super_admin',
    institutional_verified = true
WHERE LOWER(email) = 'softwareng@futminna.edu.ng'
   OR LOWER(username) = 'software-engineering-futminna';
```

### 4.3 Resetting a Stale Migration Record
If an instructor clicks **⇄ Migrate to Org** and receives `"The recorded repo URL points to the org but the repo no longer exists"`:
```sql
UPDATE teams
SET repo_url = 'https://github.com/StudentOwner/repo-name',
    repo_full_name = 'StudentOwner/repo-name'
WHERE id = <team_id>;
```
Then click **⇄ Migrate to Org** again in the Instructor Panel.

### 4.4 Force-Syncing an Empty Organization Repo From a Student's Personal Repo
If a team repo was launched empty (`0 commits`) in `software-engineering-futminna` while students pushed to their personal repo:
```bash
git clone https://github.com/StudentUsername/PERSONAL-REPO.git temp_sync
cd temp_sync
git remote set-url origin https://github.com/software-engineering-futminna/ORG-REPO.git
git push origin main --force
cd .. && rm -rf temp_sync
```

---

## 5. Official Departmental Handover Sign-Off

| # | Asset / Credential | Target Custodian Identity | Verified |
| :-: | :--- | :--- | :-: |
| **1** | **Departmental GitHub Account & Org** | `software-engineering-futminna` (`softwareng@futminna.edu.ng`) | ☐ |
| **2** | **SWE-MLAB Super Admin Access** | Verified in `auth.go`, `middleware.go` & PostgreSQL `users` table | ☐ |
| **3** | **Staff Instructor Auto-Onboarding** | Verified `@futminna.edu.ng` OAuth domain rule tested with staff | ☐ |
| **4** | **Neon Serverless PostgreSQL Database** | Admin ownership transferred to `softwareng@futminna.edu.ng` | ☐ |
| **5** | **Gemini AI (`GEMINI_API_KEY`) & RAG Pipeline** | Tested AI Activity Generator, AI Project Generator & Lecture Note RAG | ☐ |
| **6** | **Cloud Hosting (Render / Vercel / Docker `.env`)** | Production environment variables & webhook secrets documented | ☐ |
| **7** | **Public Showcase (`software-engineering-futminna/showcase`)** | Live showcase & dossier update workflow demonstrated | ☐ |

- **Outgoing Platform Engineer:** ___________________________ **Date:** _______________
- **Incoming Departmental Custodian (`softwareng@futminna.edu.ng`):** ___________________________ **Date:** _______________
- **Head of Department (Software Engineering, FUTMinna):** ___________________________ **Date:** _______________
