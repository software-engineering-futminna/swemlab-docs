# 03. GitHub Organization Migration Pipeline

---

## 1. The Migration Problem
Students often begin team projects in personal GitHub repositories (`Taiwrash/project-name` or `Student/repo`). Manually copying code via file uploads destroys commit histories, pull request discussions, and author attribution.

---

## 2. The SWE-MLAB Automated Transfer Pipeline
SWE-MLAB implements an automated 1-click migration pipeline leveraging GitHub's Repository Transfer API:

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

## 3. Asynchronous 202 Handling
When GitHub schedules repository transfers, it returns `HTTP 202 Accepted`. SWE-MLAB intercepts `*github.AcceptedError`, extracts the destination URL, and updates the database without treating the asynchronous scheduling as a failure.
