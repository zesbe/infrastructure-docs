# Infrastructure Documentation

> Dokumentasi lengkap semua aplikasi, website, dan infrastruktur milik **zesbe**

**Last Updated:** 22 January 2026

---

## Quick Overview

| Status | Aplikasi | URL | Device | Repo |
|--------|----------|-----|--------|------|
| :green_circle: | Musik App (FE) | musik.zesbe.my.id | Laptop | [musik-app](https://github.com/zesbe/musik-app) |
| :green_circle: | Musik App (BE) | musik-api.zesbe.my.id | Laptop | [musik-app](https://github.com/zesbe/musik-app) |
| :green_circle: | Musik Admin | musik-admin.zesbe.my.id | Laptop | [musik-app](https://github.com/zesbe/musik-app) |
| :green_circle: | Rima (AI Music) | rima.zesbe.my.id | VPS 60 | [Rima](https://github.com/zesbe/Rima) |
| :yellow_circle: | Wacoex | yudhalabs.dev | VPS 137 | [waba-coex](https://github.com/zesbe/waba-coex) |
| :green_circle: | KAS Kelas | kas-ibnu-sina.pages.dev | Cloudflare Pages | [KAS-KELAS-TERAKHIR-HARUS-BERES](https://github.com/zesbe/KAS-KELAS-TERAKHIR-HARUS-BERES) |
| :green_circle: | Watermark Killer | watermark-tool-90f.pages.dev | Cloudflare Pages | [gemini-watermark-remover](https://github.com/zesbe/gemini-watermark-remover) |
| :green_circle: | ClaudeAll | NPM Package | - | [ClaudeAll](https://github.com/zesbe/ClaudeAll) |

**Legend:** :green_circle: Active | :yellow_circle: In Development / Down | :red_circle: Deprecated

---

## VPS Infrastructure

### VPS 227 (zesbe)
| Property | Value |
|----------|-------|
| **Zerotier IP** | 10.180.160.227 |
| **Public IP** | 168.110.204.71 |
| **Hostname** | zesbe |
| **SSH** | `ssh zesbe@10.180.160.227 -p 6746` |
| **OS** | Ubuntu |

**Running Services:**
- PostgreSQL (port 5432, 5433)
- Redis (port 6379)
- VPN Dashboard
- NocoDB

**Docker Containers:**
- `demogolang-postgres`
- `nocodb`
- `vpn-dashboard`

---

### VPS 60 (hallowa-server1)
| Property | Value |
|----------|-------|
| **Zerotier IP** | 10.180.160.60 |
| **Public IP** | 168.110.212.151 |
| **Hostname** | hallowa-server1 |
| **SSH** | `ssh zesbe@10.180.160.60 -p 6746` |
| **OS** | Ubuntu |

**Running Services:**
- Rima Backend (localhost:8082 via CF Tunnel)
- Rima Frontend (localhost:3004 via CF Tunnel)
- PostgreSQL - lumina_ai database
- Redis

**Docker Containers:**
- `rima-backend` - Go API (127.0.0.1:8082)
- `rima-frontend` - SvelteKit (127.0.0.1:3004)
- `lumina-postgres` - PostgreSQL 16
- `lumina-redis` - Redis cache

**Cloudflare Tunnel:** `rima-tunnel` (e31e58bc-b9f3-4827-be6d-b72d87686bcc)

**Storage:**
- Audio files: `/var/lib/docker/volumes/lumina-ai-v2_lumina_uploads/_data/audio/`
- Images: `/var/lib/docker/volumes/lumina-ai-v2_lumina_uploads/_data/images/`
- 56 songs (51 MP3 + 5 MP4), 328 MB total

---

### VPS 137 (vmid3993)
| Property | Value |
|----------|-------|
| **Zerotier IP** | 10.180.160.137 |
| **Public IP** | 206.183.130.150 |
| **Hostname** | vmid3993 |
| **SSH** | `ssh zesbe@10.180.160.137 -p 6746` |
| **Status** | :yellow_circle: Currently DOWN |

**Planned Services:**
- Wacoex (WhatsApp Business API COEX)
- Cloudflare Tunnel: `wacoex-tunnel`

**Docker (Planned):**
- wacoex-api (port 5000)
- wacoex-landing (port 3000)
- wacoex-dashboard (port 3001)

---

## Applications Detail

### 1. Musik App (AI Music Generator)

> Generate musik menggunakan AI MiniMax

| Property | Value |
|----------|-------|
| **Status** | :green_circle: Production |
| **Frontend URL** | https://musik.zesbe.my.id |
| **Backend URL** | https://musik-api.zesbe.my.id |
| **Admin URL** | https://musik-admin.zesbe.my.id |
| **Device** | Local Laptop (via CF Tunnel) |
| **Database** | VPS 60 - lumina_ai |
| **Storage** | VPS 60 - Docker volume |

**Tech Stack:**
- Frontend: SvelteKit 5, TailwindCSS
- Backend: Go + Fiber
- Database: PostgreSQL 16
- AI: MiniMax API
- Storage: Local + Cloudflare R2

**Local Ports:**
- Frontend: `localhost:5173`
- Backend: `localhost:5170`
- Admin: `localhost:5174`

**Cloudflare Tunnel:** `laptopdell-zesbe` (546eeece-1cba-43f4-b8c4-c1c42ab3a8df)

**GitHub:** [zesbe/musik-app](https://github.com/zesbe/musik-app)

**Admin Login:**
- Email: yudiharyanto41@gmail.com

---

### 2. Rima (AI Music Generator - Mobile)

> AI Music Generator untuk Flutter mobile app

| Property | Value |
|----------|-------|
| **Status** | :green_circle: Production |
| **Frontend URL** | https://rima.zesbe.my.id |
| **API URL** | https://rima-api.zesbe.my.id |
| **Device** | VPS 60 (via CF Tunnel) |
| **Database** | lumina_ai |

**Tech Stack:**
- Frontend: SvelteKit (Web)
- Mobile: Flutter (PRIMARY)
- Backend: Go + Fiber
- Database: PostgreSQL 16
- AI: MiniMax API

**Docker Containers:**
```bash
rima-backend      -> 127.0.0.1:8082 (localhost only)
rima-frontend     -> 127.0.0.1:3004 (localhost only)
lumina-postgres   -> 5432
lumina-redis      -> 6379
```

**Cloudflare Tunnel:** `rima-tunnel` (e31e58bc-b9f3-4827-be6d-b72d87686bcc)

**GitHub:**
- Flutter: [zesbe/Rima](https://github.com/zesbe/Rima)
- Backend: [zesbe/Lumina-backend](https://github.com/zesbe/Lumina-backend)
- Web: [zesbe/Lumina-by-sveltekit](https://github.com/zesbe/Lumina-by-sveltekit)

---

### 3. Wacoex (WhatsApp Business API COEX)

> Multi-tenant SaaS untuk WhatsApp Business API dengan QR scanning

| Property | Value |
|----------|-------|
| **Status** | :yellow_circle: In Development |
| **Landing URL** | https://yudhalabs.dev |
| **Dashboard URL** | https://app.yudhalabs.dev |
| **API URL** | https://api.yudhalabs.dev |
| **Device** | VPS 137 (via CF Tunnel) |

**Tech Stack:**
- Frontend: SvelteKit 2.50 + Svelte 5.47 (SSG/SSR)
- Mobile: Flutter 3.38.7 (PRIMARY platform)
- Backend: Go 1.24 + Fiber v2.52
- Database: PostgreSQL 16 + Redis
- Auth: Firebase Google OAuth

**Features:**
- Inbox, Conversations, Messages, Templates
- Automation: Scheduled messages, Auto-reply (DeepSeek AI)
- COEX: Meta Embedded Signup QR
- Analytics & Reporting

**Cloudflare Tunnel:** `wacoex-tunnel` (8a31ce0d-f0c0-48a3-8a0d-d93d87e6617c)

**GitHub:** [zesbe/waba-coex](https://github.com/zesbe/waba-coex) (PRIVATE)

---

### 4. KAS Kelas (Class Fund Management)

> Sistem pengelolaan kas kelas untuk SD Islam Al Husna - Kelas 1B

| Property | Value |
|----------|-------|
| **Status** | :green_circle: Production |
| **URL** | https://kas-ibnu-sina.pages.dev |
| **Device** | Cloudflare Pages (Static) |

**Tech Stack:**
- Frontend: Vue 3 + Vite
- Styling: TailwindCSS
- Export: ExcelJS (migrated from xlsx)
- Hosting: Cloudflare Pages

**GitHub:** [zesbe/KAS-KELAS-TERAKHIR-HARUS-BERES](https://github.com/zesbe/KAS-KELAS-TERAKHIR-HARUS-BERES)

---

### 5. Watermark Killer

> Tool untuk menghapus watermark dari gambar menggunakan Gemini AI

| Property | Value |
|----------|-------|
| **Status** | :green_circle: Production |
| **URL** | https://watermark-tool-90f.pages.dev |
| **Device** | Cloudflare Pages (Static) |

**Tech Stack:**
- Frontend: HTML/CSS/JS
- AI: Google Gemini API
- Hosting: Cloudflare Pages

**GitHub:** [zesbe/gemini-watermark-remover](https://github.com/zesbe/gemini-watermark-remover)

---

### 6. ClaudeAll (AI Launcher)

> Advanced AI launcher system dengan skill-based architecture

| Property | Value |
|----------|-------|
| **Status** | :green_circle: Published |
| **NPM** | claude-all-ai-launcher |
| **Version** | 8.4.10 |

**Features:**
- 34 Skills
- 14 Agents
- 3 Commands
- MCP Integration

**GitHub:** [zesbe/ClaudeAll](https://github.com/zesbe/ClaudeAll)

---

## Cloudflare Configuration

### Zones

| Zone | Zone ID |
|------|---------|
| hallowa.id | `(check CF dashboard)` |
| zesbe.my.id | `234d934ac56ca1134cb33d5d4167be01` |
| yudhalabs.dev | `640d54694506dad8a328023a141b4dec` |

### DNS Records

#### zesbe.my.id
| Record | Type | Target | Proxied |
|--------|------|--------|---------|
| musik | CNAME | CF Tunnel laptopdell-zesbe | Yes |
| musik-api | CNAME | CF Tunnel laptopdell-zesbe | Yes |
| musik-admin | CNAME | CF Tunnel laptopdell-zesbe | Yes |
| rima | CNAME | CF Tunnel rima-tunnel | Yes |
| rima-api | CNAME | CF Tunnel rima-tunnel | Yes |
| luminaai | CNAME | CF Tunnel rima-tunnel | Yes |
| api-luminaai | CNAME | CF Tunnel rima-tunnel | Yes |

> **Note:** `luminaai` dan `api-luminaai` adalah backward compatibility untuk frontend yang masih hardcoded.

#### yudhalabs.dev
| Record | Type | Target | Proxied |
|--------|------|--------|---------|
| @ | CNAME | CF Tunnel wacoex-tunnel | Yes |
| www | CNAME | CF Tunnel wacoex-tunnel | Yes |
| app | CNAME | CF Tunnel wacoex-tunnel | Yes |
| api | CNAME | CF Tunnel wacoex-tunnel | Yes |

### Cloudflare Tunnels

| Tunnel Name | Tunnel ID | Status | Device |
|-------------|-----------|--------|--------|
| laptopdell-zesbe | 546eeece-1cba-43f4-b8c4-c1c42ab3a8df | :green_circle: Active | Laptop |
| rima-tunnel | e31e58bc-b9f3-4827-be6d-b72d87686bcc | :green_circle: Active | VPS 60 |
| wacoex-tunnel | 8a31ce0d-f0c0-48a3-8a0d-d93d87e6617c | :yellow_circle: VPS Down | VPS 137 |

---

## Local Development (Laptop)

**Device:** Dell Inspiron N5010
**Local IP:** 192.168.3.2
**Zerotier IP:** 10.180.160.122

### Running Services

| Service | Port | Exposed Via |
|---------|------|-------------|
| Musik Frontend | 5173 | CF Tunnel → musik.zesbe.my.id |
| Musik Backend | 5170 | CF Tunnel → musik-api.zesbe.my.id |
| Musik Admin | 5174 | CF Tunnel → musik-admin.zesbe.my.id |

### Cloudflared Config

Location: `/etc/cloudflared/config.yml`

```yaml
tunnel: 546eeece-1cba-43f4-b8c4-c1c42ab3a8df
credentials-file: /home/zesbe/.cloudflared/546eeece-...json

ingress:
  - hostname: musik.zesbe.my.id
    service: http://localhost:5173
  - hostname: musik-api.zesbe.my.id
    service: http://localhost:5170
  - hostname: musik-admin.zesbe.my.id
    service: http://localhost:5174
  - service: http_status:404
```

---

## Deleted/Deprecated Projects

| Project | Reason | Deleted Date |
|---------|--------|--------------|
| Replybox | Abandoned, incomplete source code | 22 Jan 2026 |
| minimax.hallowa.id | Replaced by luminaai.zesbe.my.id | 22 Jan 2026 |
| vscodeperamix.hallowa.id | VPS 137 cleanup | 22 Jan 2026 |
| terminalpremix.hallowa.id | VPS 137 cleanup | 22 Jan 2026 |

> **Note:** `luminaai.zesbe.my.id` dan `api-luminaai.zesbe.my.id` tetap aktif (via CF Tunnel) untuk backward compatibility dengan frontend yang masih hardcoded.

---

## Security Notes

> :warning: **IMPORTANT: Never trust the client, all processes must be in backend**

### VPS Access
- SSH via Zerotier only (port 6746)
- Password authentication enabled
- Public IP SSH disabled

### API Keys Location
- All API keys stored in environment variables
- Never commit secrets to git
- Use `.env` files (gitignored)

---

## Quick Commands

### SSH to VPS
```bash
# VPS 227
ssh zesbe@10.180.160.227 -p 6746

# VPS 60
ssh zesbe@10.180.160.60 -p 6746

# VPS 137
ssh zesbe@10.180.160.137 -p 6746
```

### Start Musik App (Local)
```bash
cd ~/projects/musik-app

# Frontend
cd frontend && npm run dev

# Backend
cd backend && go run main.go

# Admin
cd admin && npm run dev
```

### Check Docker (VPS 60)
```bash
docker ps
docker logs rima-backend
docker logs rima-frontend
```

### Cloudflare Tunnel Status
```bash
sudo systemctl status cloudflared
cloudflared tunnel list
```

---

## Contact

- **GitHub:** [zesbe](https://github.com/zesbe)
- **Email:** yudiharyanto41@gmail.com

---

*Generated by Claude Code - 22 January 2026*
