# 📄 ResumeForge — Full-Stack ATS Resume Builder

> **Production-grade full-stack resume platform** built with **React 19**, **Express 5**, **PostgreSQL 18**, and **Prisma ORM**. Features live debounced auto-save, five ATS-friendly templates, dual PDF/DOCX exports, a 9-category ATS scoring engine, and JWT-authenticated security.

[![Tests](https://img.shields.io/badge/tests-53%20passing-brightgreen.svg)]()
[![Smoke](https://img.shields.io/badge/smoke-31%2F31%20passing-brightgreen.svg)]()
[![React](https://img.shields.io/badge/React-19.1-61dafb.svg)]()
[![Express](https://img.shields.io/badge/Express-5.0-black.svg)]()
[![Prisma](https://img.shields.io/badge/Prisma-6.5-2D3748.svg)]()
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-18.4-336791.svg)]()
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

---

## 📸 Overview

ResumeForge bridges the gap between modern design and applicant tracking systems (ATS). Candidates can compose resumes across **5 distinct templates** and **6 color accents**, monitor real-time ATS compliance across 9 scoring dimensions, and export pixel-perfect **A4 PDF** or native **Microsoft Word (.docx)** documents with a single click.

```
┌────────────────────────────────────────────────────────────────────────┐
│                               RESUMEFORGE                              │
│                                                                        │
│   ┌───────────────────────────┐         ┌──────────────────────────┐   │
│   │       React 19 SPA        │         │     Express 5 REST API   │   │
│   │   (Vite + React Router)   │ ──────> │  (JWT + Helmet + CORS)   │   │
│   │                           │ <────── │                          │   │
│   │  • Debounced Auto-Save    │  Axios  │  • Zod Validation        │   │
│   │  • 5 ATS Templates        │         │  • Rate Limiting         │   │
│   │  • HTML2PDF + DOCX Export │         │  • Bcrypt Password Hash  │   │
│   └───────────────────────────┘         └─────────────┬────────────┘   │
│                                                       │                │
│                                                       │ Prisma 6 ORM   │
│                                                       ▼                │
│                                         ┌──────────────────────────┐   │
│                                         │      PostgreSQL 18       │   │
│                                         │ (Embedded Dev / Neon Prd)│   │
│                                         └──────────────────────────┘   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## ✨ Features

| Category | Highlights |
|---|---|
| **Live Editor** | Two-pane workspace with synchronized preview, debounced auto-save (1 200 ms), draft-typing resilience, zoom controls (75 % – 125 %), and collapsible panels. |
| **5 ATS Templates** | **Professional** (clean header + border rules), **Modern** (accent banners + skill pills), **Minimal** (typography-focused serif), **ATS Clean** (monochrome zero-frills for strict parsers), and **Classic** (traditional Ivy-league centered serif). |
| **6 Theme Accents** | Blue `#2563eb`, Navy `#0ea5e9`, Green `#10b981`, Purple `#8b5cf6`, Black `#111827`, and Monochrome `#475569`. Styled via dynamic CSS variables (`--resume-accent`). |
| **Flexible Sections** | Personal info, Summary, Education, Experience, Projects, Skills (with categories), Certifications, Achievements, Languages (with proficiency), and Hobbies. Add, remove, and reorder on the fly. |
| **ATS Scoring Engine** | Heuristic 9-category weighted audit: Contact completeness (15 %), Summary strength (10 %), Work Experience (20 %), Action verbs (15 %), Measurable metrics (10 %), Skills balance (10 %), Education (10 %), Section structure (5 %), and Document length (5 %). Includes keyword recommendations and an explicit heuristic disclaimer. |
| **Multi-Format Export** | **PDF**: High-resolution client-side canvas render via `html2pdf.js` with auto page breaks and A4 sizing. **DOCX**: Standalone Word-compliant XML/HTML document generated directly from the normalized resume model. |
| **Dashboard** | Resume metrics, search filter, template / ATS-score filtering, sort by title/date/score, instant duplication, and accessible deletion modals. |
| **Security & Auth** | HTTP-only compatible JWT tokens, 12-round bcrypt password hashing, input sanitization, strict Zod payload validation, cross-user authorization barriers, and rate-limiting (`express-rate-limit`). |

---

## 🗂️ Architecture & Tech Stack

```
resume-builder-ATS-main/
├── client/                     # React 19 Single Page Application
│   ├── public/
│   │   ├── _redirects          # SPA rewrites for Netlify / Render
│   │   └── favicon.svg
│   ├── src/
│   │   ├── components/         # Navbar, ResumePreview, AtsPanel, Section editors
│   │   ├── hooks/              # useAutosave, useToast, useConfirm
│   │   ├── pages/              # Landing, Login, Signup, Dashboard, Builder, Preview, Profile
│   │   ├── styles/             # Modular CSS: app.css, builder.css, dashboard.css
│   │   └── utils/              # resume-model.js (view-model mapper), export.js (PDF/DOCX)
│   ├── vercel.json             # Vercel SPA routing configuration
│   └── vite.config.js
│
├── server/                     # Express 5 REST API
│   ├── prisma/
│   │   ├── schema.prisma       # User, Resume, and 8 nested section models
│   │   └── seed.js             # Demo account + sample developer resume
│   ├── scripts/
│   │   ├── pg.mjs              # Zero-install embedded PostgreSQL daemon
│   │   └── smoke.mjs           # 31-step end-to-end API integration test
│   ├── src/
│   │   ├── controllers/        # auth, resumes, sections, ats
│   │   ├── middleware/         # auth (JWT), errorHandler, validate
│   │   ├── routes/             # Versioned /api router mounts
│   │   ├── services/           # ats.service.js, resume.service.js
│   │   └── validators/         # schemas.js (Zod definitions)
│   └── tests/                  # Vitest suite (resumes, auth, ATS engine)
│
└── (legacy/ removed — the old static/Supabase prototype was retired; the
    React + Express app above is the only supported implementation)
```

---

## ⚡ Quick Start (Local Development)

### Prerequisites
- **Node.js**: v20 or v22 LTS
- **npm**: v10+

No external PostgreSQL installation is required for local development — ResumeForge bundles an **embedded PostgreSQL 18** binary for seamless setup.

### 1. Clone & Install
```bash
git clone https://github.com/your-username/resume-builder-ATS-main.git
cd resume-builder-ATS-main
npm install
```

### 2. Environment Configuration

Copy the example environment files:

```bash
# Server environment
cp server/.env.example server/.env

# Client environment
cp client/.env.example client/.env
```

Default `server/.env`:
```env
PORT=5000
NODE_ENV=development
DATABASE_URL="postgresql://postgres:postgres@127.0.0.1:5433/resumeforge"
JWT_SECRET="resumeforge-super-secret-dev-key-change-in-production-min-32-chars"
JWT_EXPIRES_IN="7d"
CLIENT_URL="http://localhost:5173"   # allowed CORS origin(s), comma-separated
RATE_LIMIT_MAX=100
```

Default `client/.env`:
```env
VITE_API_URL="http://localhost:5000/api"
```

### 3. Initialize the Database
```bash
# Start embedded PostgreSQL on port 5433
npm run db:up

# Run Prisma migrations to generate the schema
npm run db:migrate

# Seed demo account + comprehensive sample resume
npm run db:seed
```

### 4. Start the Application
```bash
# Start both client and server concurrently
npm run dev
```

- **Frontend**: [http://localhost:5173](http://localhost:5173)
- **API**: [http://localhost:5000/api](http://localhost:5000/api)
- **Health Check**: [http://localhost:5000/api/health](http://localhost:5000/api/health)

---

## 🔑 Demo Account

You can log in immediately after running `npm run db:seed`:

| Attribute | Value |
|---|---|
| **Email** | `demo@resumeforge.dev` |
| **Password** | `Demo12345` |
| **Preloaded Content** | Full Senior Full-Stack Engineer resume with 92/100 ATS score across all 9 sections |

---

## 🧪 Testing & Quality Assurance

ResumeForge runs automated unit, integration, and live end-to-end smoke tests across both workspaces.

```bash
# Run all 53 server & client unit/integration tests
npm test

# Run Vitest in client workspace only (23 model & payload tests)
npm test --workspace client

# Run Vitest in server workspace only (30 DB & controller tests)
npm test --workspace server

# Run the 31-assertion end-to-end API smoke test (requires running server)
npm run smoke
```

### Test Coverage Highlights
- **Auth**: Registration, bcrypt hashing, JWT issuance, duplicate checks, password change, invalid tokens.
- **Resumes**: CRUD lifecycle, cross-user ownership isolation, deep section persistence, duplication, cascade deletions.
- **ATS Engine**: Keyword scanning, metric heuristics, section weights, deterministic scoring.
- **Client Models**: Auto-save payload sanitization, Word-compatible HTML generation, XSS prevention.

---

## 📡 API Reference

Base URL: `/api`

### Authentication
| Method | Endpoint | Description | Auth Required |
|---|---|---|:---:|
| `POST` | `/auth/register` | Register new user account | No |
| `POST` | `/auth/login` | Authenticate and obtain JWT | No |
| `GET` | `/auth/me` | Fetch authenticated user profile | Yes |
| `PUT` | `/auth/profile` | Update profile (name, preferences) | Yes |
| `PUT` | `/auth/password` | Change account password | Yes |
| `POST` | `/auth/logout` | Invalidate client session | Yes |

### Resumes
| Method | Endpoint | Description | Auth Required |
|---|---|---|:---:|
| `GET` | `/resumes` | List user resumes with ATS scores | Yes |
| `POST` | `/resumes` | Create empty resume draft | Yes |
| `GET` | `/resumes/:id` | Fetch full resume with nested sections | Yes |
| `PUT` | `/resumes/:id` | Full upsert/save of resume & sections | Yes |
| `DELETE` | `/resumes/:id` | Delete resume and cascade sections | Yes |
| `POST` | `/resumes/:id/duplicate` | Clone resume with all sections | Yes |
| `GET` | `/resumes/:id/ats` | Run ATS audit on saved resume | Yes |

### ATS Engine & Sections
| Method | Endpoint | Description | Auth Required |
|---|---|---|:---:|
| `POST` | `/ats/score` | Score an arbitrary draft payload | Yes |
| `POST` | `/resumes/:id/:section` | Append entry to specific section | Yes |
| `PUT` | `/:section/:entryId` | Update individual section entry | Yes |
| `DELETE` | `/:section/:entryId` | Remove individual section entry | Yes |

---

## 🚀 Deployment Guide

### Database (Neon PostgreSQL)
1. Sign up at [neon.tech](https://neon.tech) and create a free PostgreSQL project.
2. Copy the pooled connection string:
   ```
   postgresql://user:password@ep-xyz-pooler.us-east-2.aws.neon.tech/neondb?sslmode=require
   ```

### Backend (Render / Railway)
1. Link your GitHub repository to [Render](https://render.com).
2. Create a new **Web Service**:
   - **Root Directory**: `server`
   - **Build Command**: `npm install && npx prisma generate && npx prisma migrate deploy`
   - **Start Command**: `node src/index.js`
3. Add Environment Variables:
   - `DATABASE_URL`: *(Your Neon connection string)*
   - `JWT_SECRET`: *(Generate a secure 64-character random string)*
   - `CLIENT_URL`: *(Your frontend URL — this is the CORS allowlist. Use `CLIENT_URL`, **not** `CORS_ORIGIN`; e.g. `https://resumeforge.vercel.app`. Multiple origins are comma-separated.)*
   - `NODE_ENV`: `production`

   > ⚠️ If `CLIENT_URL` is wrong or missing, the browser blocks every API call
   > and the UI shows **"Failed to fetch"** on login/sign-up. This is the single
   > most common cause of that error in production.

### Frontend (Vercel / Netlify)
1. Link your repository to [Vercel](https://vercel.com).
2. Configure project:
   - **Framework Preset**: `Vite`
   - **Root Directory**: `client`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
3. Add Environment Variables:
   - `VITE_API_URL`: *(Your Render backend URL + `/api`, e.g., `https://resumeforge-api.onrender.com/api`)*
4. SPA routing is automatically handled via `client/vercel.json` and `client/public/_redirects`.

---

## 💼 LinkedIn Showcase Post

Looking to showcase this build on LinkedIn? Here is a ready-to-share post:

```markdown
🚀 Excited to share my latest full-stack build: ResumeForge — a production-ready, ATS-optimized Resume Platform!

Job seekers often struggle with ATS (Applicant Tracking Systems) rejecting resumes due to non-standard layouts or missing keywords. I designed and built ResumeForge to bridge that gap with a live, two-pane editor and instant feedback.

💡 What was built:
• Frontend: React 19 + Vite SPA with debounced auto-save (1200ms), 5 battle-tested ATS templates, and 6 custom theme palettes.
• Backend: Express 5 REST API backed by PostgreSQL 18 & Prisma ORM with strict cross-user tenancy isolation.
• Heuristic ATS Audit: A 9-category weighted scoring engine that analyzes action verbs, metrics, contact points, and structure in real time.
• Dual Exports: Pixel-perfect paginated A4 PDF generation and native Microsoft Word (.docx) builder directly from normalized JSON.
• Robust Testing: 53 automated unit/integration tests with Vitest, plus a 31-assertion end-to-end smoke test suite.
• Zero-friction DX: Bundled embedded PostgreSQL for 1-command local setup — no external database installation needed!

Tech Stack: React 19 · Express 5 · PostgreSQL · Prisma · Zod · Vitest · JWT · CSS3

#FullStack #ReactJS #NodeJS #PostgreSQL #Prisma #WebDevelopment #SoftwareEngineering #OpenSource #ATS
```

---

## 🔒 Security & Disclaimers

- Passwords are salted and hashed using **bcrypt** (12 work factor rounds).
- All API endpoints strictly enforce **tenant boundaries** — foreign resume access returns a clean 404.
- The ATS score is a **heuristic estimate** based on common parsing patterns across modern applicant tracking systems.

---

Built by **Harshadvali Shaik** · MIT License

