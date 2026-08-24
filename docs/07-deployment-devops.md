# 07. Deployment, DevOps & Environment Configuration

---

## 1. Environment Configuration (`.env`)
```bash
# Database — Neon Serverless PostgreSQL (Cloud)
DATABASE_URL=postgresql://user:password@ep-falling-darkness-al1utzuu-pooler.c-3.eu-central-1.aws.neon.tech/neondb?sslmode=require&channel_binding=require

# GitHub OAuth Credentials
GITHUB_CLIENT_ID=your_github_client_id
GITHUB_CLIENT_SECRET=your_github_client_secret
GITHUB_REDIRECT_URL=http://localhost:82/auth/github/callback

# Institutional Governance
GITHUB_ORG=software-engineering-futminna
INSTITUTIONAL_DOMAIN=@st.futminna.edu.ng

# Telemetry
SENTRY_DSN=https://your_sentry_dsn_here
```

---

## 2. Docker Compose Specification
```yaml
version: '3.8'

services:
  backend:
    build: ./backend
    ports:
      - "8080:8080"
    environment:
      - DATABASE_URL=${DATABASE_URL}
      - GITHUB_CLIENT_ID=${GITHUB_CLIENT_ID}
      - GITHUB_CLIENT_SECRET=${GITHUB_CLIENT_SECRET}
      - GITHUB_ORG=${GITHUB_ORG}
    restart: unless-stopped

  frontend:
    build: ./frontend
    ports:
      - "80:80"
    depends_on:
      - backend
    restart: unless-stopped
```
