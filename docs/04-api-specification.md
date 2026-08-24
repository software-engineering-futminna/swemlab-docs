# 04. REST API Specification

---

## 1. Authentication Endpoints
- `GET /auth/github`: Initiates GitHub OAuth authentication.
- `GET /auth/github/callback`: Exchanges OAuth authorization code for session token and verifies student profile.
- `GET /api/v1/user/profile`: Retrieves authenticated user data and enrolled courses.
- `POST /auth/logout`: Invalidates active session token.

---

## 2. Course & Activity Endpoints
- `GET /api/v1/courses`: Lists all available courses.
- `POST /api/v1/courses`: Creates a new course (Instructor/Admin only).
- `GET /api/v1/courses/:id`: Retrieves course details, syllabus, and enrolled students.
- `POST /api/v1/courses/:id/enroll`: Enrolls student into course.
- `GET /api/v1/courses/:id/activities`: Lists activities and milestones for a course.

---

## 3. Team & Repository Migration Endpoints
- `POST /api/v1/projects/:id/teams`: Creates a new project team.
- `POST /api/v1/teams/:id/invitations`: Invites a student into a team.
- `POST /api/v1/invitations/:id/accept`: Accepts pending team invitation.
- `POST /api/v1/teams/:id/migrate-repo`: Transfers team repo into the institutional organization (Instructor only).

---

## 4. Submissions & Grading
- `POST /api/v1/activities/:id/submit`: Submits repository and live deployment URLs.
- `POST /api/v1/webhooks/github`: Ingests GitHub push/PR events for telemetry.
- `POST /api/v1/submissions/:id/grade`: Submits rubric score and markdown feedback.
