# 06. Operational User Manuals

---

## 1. Student User Guide
1. **Sign In & Verify:** Sign in with GitHub. Add and verify your `@st.futminna.edu.ng` email in GitHub settings.
2. **Enroll in Course:** Select `FUTM-SWE-221` from the dashboard.
3. **Form a Team:** Create a team or accept an invitation from your team lead.
4. **Develop & Deploy:** Push commits using your verified GitHub account and deploy live to Netlify/Vercel/Railway.
5. **Submit & Review:** Submit repository and live URLs. Review instructor rubric feedback in the Feedback Console.

---

## 2. Instructor Operation Manual
1. **Create Courses & Projects:** Configure syllabus, project specifications, expected REST APIs, and rubric weights.
2. **Monitor Telemetry:** View formed teams, member rosters, and live commit analytics.
3. **1-Click Org Migration:** Click **"⇄ Migrate to Org"** to transfer student repositories into `software-engineering-futminna`.
4. **Grade Submissions:** Test live URLs, inspect source code, enter rubric scores, and publish feedback.
5. **Update Showcase:** Export validated dossiers to `showcase.html` and publish to the public repository.

---

## 3. Super Administrator Runbook
1. **OAuth Setup:** Maintain GitHub OAuth App client credentials and organization permissions.
2. **Database Management:** Monitor Neon Serverless PostgreSQL connection pools and SSL certificates.
3. **Observability:** Inspect error rates and panic recoveries via Sentry (`SENTRY_DSN`).
4. **Disaster Recovery:** Execute stale migration resets and force-sync scripts when needed.
