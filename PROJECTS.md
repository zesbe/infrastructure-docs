# Projects Detail

> Detail lengkap semua project beserta repository dan tech stack

---

## 1. Musik App

### Overview
AI-powered music generator menggunakan MiniMax API. User bisa generate musik dengan berbagai genre dan style.

### URLs
| Type | URL |
|------|-----|
| Frontend | https://musik.zesbe.my.id |
| Backend | https://musik-api.zesbe.my.id |
| Admin | https://musik-admin.zesbe.my.id |

### Repository
```
GitHub: https://github.com/zesbe/musik-app
Local:  ~/projects/musik-app
```

### Tech Stack
| Component | Technology |
|-----------|------------|
| Frontend | SvelteKit 5, TailwindCSS, TypeScript |
| Backend | Go 1.24, Fiber v2.52, GORM |
| Database | PostgreSQL 16 |
| Cache | Redis |
| AI | MiniMax API |
| Storage | Local + Cloudflare R2 |
| Auth | JWT |

### Directory Structure
```
musik-app/
├── frontend/      # SvelteKit frontend
├── backend/       # Go API
├── admin/         # Admin dashboard
└── mobile/        # Flutter app (planned)
```

### Local Development
```bash
cd ~/projects/musik-app

# Start frontend (port 5173)
cd frontend && npm run dev

# Start backend (port 5170)
cd backend && go run main.go

# Start admin (port 5174)
cd admin && npm run dev
```

### Environment Variables
```
# Backend (.env)
DATABASE_URL=postgres://...
MINIMAX_API_KEY=sk-...
JWT_SECRET=...
CLOUDFLARE_R2_*=...
```

### Features
- [x] Generate music dari text prompt
- [x] Multiple genres (24 genres)
- [x] Voice clone
- [x] Music history
- [x] Admin dashboard
- [x] Album art generation
- [ ] Mobile app (Flutter)

---

## 2. Lumina AI (Legacy)

### Overview
Versi lama dari Musik App. Masih running di VPS 60 sebagai backup/legacy.

### URLs
| Type | URL |
|------|-----|
| Frontend | https://luminaai.zesbe.my.id |

### Repository
```
GitHub: https://github.com/zesbe/Lumina-by-sveltekit
        https://github.com/zesbe/Lumina-backend
        https://github.com/zesbe/Lumina-AI
```

### Tech Stack
| Component | Technology |
|-----------|------------|
| Frontend | SvelteKit |
| Backend | Go, Fiber |
| Database | PostgreSQL 16 |
| AI | MiniMax API |

### Docker Deployment
```bash
# On VPS 60
docker compose up -d

# Containers:
# - lumina-backend (port 8082)
# - lumina-frontend (port 3004)
# - lumina-postgres
# - lumina-redis
```

### Database Info
```
Database : lumina_ai
Host     : VPS 60 Docker
Records  : 56 songs (51 MP3, 5 MP4)
Storage  : 328 MB audio, 20 MB images
```

---

## 3. Wacoex (In Development)

### Overview
Multi-tenant SaaS untuk WhatsApp Business API COEX dengan Meta Embedded Signup QR.

### URLs
| Type | URL | Status |
|------|-----|--------|
| Landing | https://yudhalabs.dev | ⚠️ VPS Down |
| Dashboard | https://app.yudhalabs.dev | ⚠️ VPS Down |
| API | https://api.yudhalabs.dev | ⚠️ VPS Down |

### Repository
```
GitHub: https://github.com/zesbe/waba-coex (PRIVATE)
Local:  ~/projects/waba-coex
```

### Tech Stack
| Component | Technology |
|-----------|------------|
| Landing | SvelteKit 2.50 (SSG) |
| Dashboard | SvelteKit 2.50 (SSR) |
| Mobile | Flutter 3.38.7 (PRIMARY) |
| Backend | Go 1.24, Fiber v2.52 |
| Database | PostgreSQL 16 (RLS) |
| Cache | Redis |
| Auth | Firebase Google OAuth |
| AI | DeepSeek ($0.28/1M tokens) |
| UI | shadcn-svelte, TailwindCSS |
| i18n | Paraglide v2 |

### Features (Planned)
- [ ] Multi-tenant architecture
- [ ] Meta Embedded Signup QR (COEX)
- [ ] Inbox & Conversations
- [ ] Message templates
- [ ] Auto-reply with AI
- [ ] Scheduled messages
- [ ] Webhooks & API
- [ ] Analytics dashboard
- [ ] Mobile app (Flutter)

### Pricing Model
| Plan | Price | Contacts | Users |
|------|-------|----------|-------|
| Free | Rp 0 | 100 | 1 |
| Starter | Rp 199K | 1,000 | 3 |
| Pro | Rp 599K | 10,000 | 10 |
| Enterprise | Rp 1.5M+ | Custom | Unlimited |

---

## 4. KAS Kelas

### Overview
Sistem pengelolaan kas kelas untuk SD Islam Al Husna - Kelas 1B.

### URLs
| Type | URL |
|------|-----|
| Web | https://kas-ibnu-sina.pages.dev |

### Repository
```
GitHub: https://github.com/zesbe/KAS-KELAS-TERAKHIR-HARUS-BERES
Local:  ~/projects/KAS-KELAS-TERAKHIR-HARUS-BERES
```

### Tech Stack
| Component | Technology |
|-----------|------------|
| Frontend | Vue 3, Vite |
| Styling | TailwindCSS |
| Export | ExcelJS |
| Hosting | Cloudflare Pages |

### Deployment
```bash
# Auto-deploy on git push to main
git push origin main
# → Cloudflare Pages will build and deploy
```

### Features
- [x] Track student payments
- [x] Generate financial reports
- [x] Export to Excel
- [x] PDF reports

---

## 5. Watermark Killer

### Overview
Tool untuk menghapus watermark dari gambar menggunakan Google Gemini AI.

### URLs
| Type | URL |
|------|-----|
| Web | https://watermark-tool-90f.pages.dev |

### Repository
```
GitHub: https://github.com/zesbe/gemini-watermark-remover
```

### Tech Stack
| Component | Technology |
|-----------|------------|
| Frontend | HTML, CSS, JavaScript |
| AI | Google Gemini API |
| Hosting | Cloudflare Pages |

### Features
- [x] Upload image
- [x] AI watermark detection
- [x] Watermark removal
- [x] Download result

---

## 6. ClaudeAll

### Overview
Advanced AI launcher system dengan skill-based architecture dan superpowers framework.

### Installation
```bash
npm install -g claude-all-ai-launcher
```

### Repository
```
GitHub: https://github.com/zesbe/ClaudeAll
NPM:    claude-all-ai-launcher@8.4.10
Local:  ~/projects/ClaudeAll
```

### Features
- 34 Skills
- 14 Agents (proactive-mode, code-generator, code-reviewer, etc.)
- 3 Commands (brainstorm, execute-plan, write-plan)
- MCP Integration
- Hooks system

### Directory Structure
```
ClaudeAll/
├── claude-all           # Main executable
├── claude-suite/        # Suite tools
├── superpowers/
│   ├── agents/          # 14 agents
│   ├── skills/          # 35 skills
│   ├── commands/        # 3 commands
│   └── hooks/           # Auto hooks
├── model/               # AI models config
└── docs/                # Documentation
```

---

## Deleted Projects

### Replybox
- **Status:** Deleted (22 Jan 2026)
- **Reason:** Abandoned, incomplete source code, empty database
- **Cleaned:** Docker containers, volumes, database, SSL certs, GitHub repo

---

## Project Locations Summary

| Project | Local Path | GitHub |
|---------|------------|--------|
| Musik App | ~/projects/musik-app | zesbe/musik-app |
| Wacoex | ~/projects/waba-coex | zesbe/waba-coex |
| KAS Kelas | ~/projects/KAS-KELAS-TERAKHIR-HARUS-BERES | zesbe/KAS-KELAS-... |
| ClaudeAll | ~/projects/ClaudeAll | zesbe/ClaudeAll |
| Infrastructure Docs | ~/projects/infrastructure-docs | zesbe/infrastructure-docs |
