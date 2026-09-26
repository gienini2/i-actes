# i-ACTES · Police Records Management System

A desktop application for digitizing paper-based police record books at the Policia Local de l'Arboç (Tarragona, Spain). Built with Electron — runs fully offline, no server, no cloud.

Developed by Juan Pablo Gienini Donato.

---

## Features

- Single login with username and password (local SQLite, no internet required)
- Shared session across all record books — log in once, access everything
- Automatic annual numbering per record type (AA-2026-0001)
- Immutable records — entries can only be voided, never deleted, with mandatory justification
- Full audit trail on every action
- Role-based access control: agent / supervisor / admin
- Direct print from the app
- 100% offline — no server, no cloud, no dependencies

## Record Books

| Code | Name | Status |
|------|------|--------|
| AA | Administrative Proceedings | ✅ Active |
| EV | Vehicle Intake & Release | ✅ Active |
| SC | Public Safety | ✅ Active |
| NI | Internal Notes | ✅ Active |
| VA | Abandoned Vehicles | ✅ Active |
| IV | Vehicle Immobilizations | ✅ Active |
| SS | Social Services | ✅ Active |
| OB | Lost & Found Objects | 🔄 Pending |
| DP | Criminal Proceedings | 🔄 Pending |
| TI | Legal Aid Phone Log (ICAT) | 🔄 Pending |
| LC | Cash Register | 🔄 Pending |

## Tech Stack

- **Electron** — cross-platform desktop app (deployed on Windows)
- **sql.js** — SQLite compiled to WebAssembly; zero native dependencies, no Python required
- **Vanilla HTML/CSS/JS** — no frameworks
- Architecture: `main.js` → IPC → `preload.js` → HTML renderers

## Project Structure
i-actes/
├── main.js              # Electron main process
├── preload.js           # Secure IPC bridge (contextBridge)
├── package.json
├── services/
│   ├── auth.js          # PBKDF2 authentication via SQLite
│   └── db.js            # Database layer (sql.js / WebAssembly)
├── db/                  # Auto-created at runtime (git-ignored)
│   └── database.db      # SQLite database with users and records
└── renderer/
├── selector.html    # Login screen + book selector
├── shared.css       # Shared styles across all record books
├── shared.js        # Shared logic (session, DB helpers, agents...)
├── AA.html          # Administrative Proceedings
├── EV.html          # Vehicle Intake & Release
└── ...

## Getting Started

Requirements: **Node.js** (any recent LTS version)

```bash
git clone https://github.com/YOUR_USERNAME/i-actes
cd i-actes
npm install
npm start
```

No Python, no Visual Studio, no native compilation.  
`sql.js` is pure WebAssembly — it just works.

## Default Credentials

| Username | Password | Role |
|----------|----------|------|
| admin | 1234 | Administrator |
| inspector | 001 | Supervisor |
| cabo1 | 002 | Supervisor |
| agent010 | 010 | Agent |

**Change all passwords before deploying to production.**

## Why This Project

The Policia Local de l'Arboç was managing all police records in physical paper books — slow, unsearchable, and impossible to audit. This app replaces that workflow with a structured digital system while keeping the same familiar record structure officers already know.

Key technical decisions:
- **sql.js over better-sqlite3** — avoids native ABI compilation issues on Windows; Node.js is the only requirement
- **No framework** — keeps the bundle small and the codebase readable by non-specialists
- **Shared CSS/JS architecture** — reduced individual record book files from ~900 KB to ~19 KB (97% reduction) by extracting common logic into `shared.css` and `shared.js`

## License

Free for internal use by public administrations.  
Commercial use requires explicit authorization from the author.
