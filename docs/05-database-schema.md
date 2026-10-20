# 05. Database Architecture & Data Model Reference

> **[← Back to Interactive Landing & Handover Portal (`index.html`)](./index.html)**

---

## 1. Entity-Relationship Overview
The SWE-MLAB database schema is fully normalized and managed via GORM migrations on Neon Serverless PostgreSQL.

```
  +-------------+       +---------------+       +------------------+
  |    USERS    | <---> |  ENROLLMENTS  | <---> |     COURSES      |
  +-------------+       +---------------+       +------------------+
         │                                               │
         │ (1:N)                                         │ (1:N)
         ▼                                               ▼
  +-------------+       +---------------+       +------------------+
  |TEAM_MEMBERS | <---> |     TEAMS     | <---> |     PROJECTS     |
  +-------------+       +---------------+       +------------------+
                                │
                                ▼
                        +---------------+       +------------------+
                        |  SUBMISSIONS  | <---> |    FEEDBACKS     |
                        +---------------+       +------------------+
```

---

## 2. Core Tables & Schemas

### `users`
- `id` (PK, Serial)
- `github_id` (BigInt, Unique)
- `username` (VarChar, Unique)
- `email` (VarChar, Unique)
- `full_name` (VarChar)
- `matric_number` (VarChar)
- `role` (VarChar: `student`, `instructor`, `super_admin`)
- `institutional_verified` (Boolean)
- `session_token` (VarChar)

### `teams`
- `id` (PK, Serial)
- `project_id` (FK -> `projects.id`)
- `name` (VarChar)
- `status` (VarChar)
- `repo_url` (VarChar)
- `repo_full_name` (VarChar)

### `team_members`
- `id` (PK, Serial)
- `team_id` (FK -> `teams.id`)
- `user_id` (FK -> `users.id`)
- `role` (VarChar: `lead`, `member`)

### `submissions`
- `id` (PK, Serial)
- `activity_id` (FK -> `activities.id`)
- `team_id` (FK -> `teams.id`)
- `repo_url` (VarChar)
- `live_url` (VarChar)
- `score` (Float)
- `status` (VarChar)
