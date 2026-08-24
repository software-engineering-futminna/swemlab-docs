# 02. System Architecture & Technology Stack

---

## 1. Architecture Overview
SWE-MLAB uses a containerized micro-monolith architecture designed for high availability, low latency, and zero build friction.

```
[CLIENT PRESENTATION LAYER]
React 18 SPA + Vite 5 + Swiss Design Tokens + Vanilla CSS Custom Properties
          │
          │  HTTPS / JSON REST API
          ▼
[INGRESS & REVERSE PROXY]
Nginx Alpine (Gzip Compression, Security Headers, SPA Fallback Routing)
          │
          ▼
[APPLICATION SERVER (Golang 1.22+)]
├── Gin HTTP Router & Middleware (RBAC, Rate Limiting, Auth Context)
├── GORM ORM Layer (Transactions, Auto-Migrations, Model Associations)
├── GitHub API Client (OAuth2 Token Source, Org Repo Transfers, Webhooks)
└── Sentry Go SDK (Telemetry, Panic Recovery, Error Tracking)
          │
          ├──▶ [PERSISTENCE TIER] Neon Serverless PostgreSQL Cloud (SSL Pooled)
          └──▶ [ORGANIZATION TIER] GitHub Org: software-engineering-futminna
```

---

## 2. Technology Stack Breakdown

### Backend (Golang)
- **Language:** Go (Golang) 1.22+
- **HTTP Routing:** Gin Framework (`github.com/gin-gonic/gin`)
- **ORM & Database Client:** GORM (`gorm.io/gorm`) with `postgres` driver
- **GitHub Client:** `go-github` (`github.com/google/go-github/v60/github`)
- **OAuth2:** `golang.org/x/oauth2`
- **Error Tracking:** Sentry Go SDK (`github.com/getsentry/sentry-go`)

### Frontend (React 18 & Swiss Design)
- **Framework:** React 18 + Vite 5
- **Routing:** React Router v6
- **Design Tokens:** Pure Vanilla CSS custom properties based on Swiss International Typographic Style (zero Tailwind dependency).
- **Icons:** Lucide Icons

### Cloud & Database
- **Database:** Neon Serverless PostgreSQL with SSL connection pooling
- **Containers:** Multi-stage Alpine Docker containers
- **Proxy:** Nginx with SPA fallback rules
