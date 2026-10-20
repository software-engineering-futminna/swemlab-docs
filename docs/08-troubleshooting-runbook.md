# 08. Operational Troubleshooting & Runbook

> **[← Back to Interactive Landing & Handover Portal (`index.html`)](./index.html)**

---

## 1. Rate Limit Handling (403 Forbidden)
- **Problem:** GitHub API calls fail with `rate limit exceeded (60/hr)`.
- **Solution:** Pass the user's active `SessionToken` via `oauth2.StaticTokenSource`, which expands rate limits to **5,000 req/hr**.

---

## 2. Stale Migration Recovery
- **Problem:** `"The recorded repo URL points to the org but the repo no longer exists."`
- **Solution:** Reset the team's `repo_url` in the database to point to the actual source repo (`Taiwrash/repo` or `Student/repo`), then re-run migration.

---

## 3. Syncing Empty Org Repositories
- **Problem:** An organization repository was initialized empty while student code exists in a personal repository.
- **Solution:**
```bash
git clone https://github.com/StudentLead/PERSONAL-REPO.git /tmp/sync_repo
cd /tmp/sync_repo
git remote set-url origin https://github.com/software-engineering-futminna/ORG-REPO.git
git push origin main --force
rm -rf /tmp/sync_repo
```
