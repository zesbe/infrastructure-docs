# Domain & DNS Mapping

> Semua domain dan kemana mereka mengarah

---

## Active Domains

### musik.zesbe.my.id
```
Type     : CNAME
Target   : CF Tunnel (laptopdell-zesbe)
Proxied  : Yes
Device   : Local Laptop
Port     : 5173
App      : Musik App Frontend (SvelteKit)
Status   : ✅ Active
```

### musik-api.zesbe.my.id
```
Type     : CNAME
Target   : CF Tunnel (laptopdell-zesbe)
Proxied  : Yes
Device   : Local Laptop
Port     : 5170
App      : Musik App Backend (Go)
Status   : ✅ Active
```

### musik-admin.zesbe.my.id
```
Type     : CNAME
Target   : CF Tunnel (laptopdell-zesbe)
Proxied  : Yes
Device   : Local Laptop
Port     : 5174
App      : Musik App Admin Dashboard
Status   : ✅ Active
```

### luminaai.zesbe.my.id
```
Type     : A
Target   : 168.110.212.151 (VPS 60)
Proxied  : Yes
Device   : VPS 60
Port     : 3004 (Docker)
App      : Lumina AI Frontend (Legacy)
Status   : ✅ Active
```

### kas-ibnu-sina.pages.dev
```
Type     : Cloudflare Pages
Target   : Auto-deployed
Device   : Cloudflare Edge
App      : KAS Kelas (Vue 3)
Repo     : KAS-KELAS-TERAKHIR-HARUS-BERES
Status   : ✅ Active
```

### watermark-tool-90f.pages.dev
```
Type     : Cloudflare Pages
Target   : Auto-deployed
Device   : Cloudflare Edge
App      : Watermark Killer (Gemini AI)
Repo     : gemini-watermark-remover
Status   : ✅ Active
```

---

## Development Domains (VPS 137 Down)

### yudhalabs.dev
```
Type     : CNAME
Target   : CF Tunnel (wacoex-tunnel)
Proxied  : Yes
Device   : VPS 137
Port     : 3000
App      : Wacoex Landing Page (SSG)
Status   : ⚠️ VPS Down
```

### www.yudhalabs.dev
```
Type     : CNAME
Target   : CF Tunnel (wacoex-tunnel)
Proxied  : Yes
Device   : VPS 137
Port     : 3000
App      : Wacoex Landing Page (SSG)
Status   : ⚠️ VPS Down
```

### app.yudhalabs.dev
```
Type     : CNAME
Target   : CF Tunnel (wacoex-tunnel)
Proxied  : Yes
Device   : VPS 137
Port     : 3001
App      : Wacoex Dashboard (SSR)
Status   : ⚠️ VPS Down
```

### api.yudhalabs.dev
```
Type     : CNAME
Target   : CF Tunnel (wacoex-tunnel)
Proxied  : Yes
Device   : VPS 137
Port     : 5000
App      : Wacoex Backend API (Go)
Status   : ⚠️ VPS Down
```

---

## Deleted Domains

| Domain | Deleted Date | Reason |
|--------|--------------|--------|
| minimax.hallowa.id | 22 Jan 2026 | Replaced by luminaai.zesbe.my.id |
| vscodeperamix.hallowa.id | 22 Jan 2026 | VPS 137 cleanup |
| terminalpremix.hallowa.id | 22 Jan 2026 | VPS 137 cleanup |

---

## Cloudflare Zones

### zesbe.my.id
```
Zone ID  : 234d934ac56ca1134cb33d5d4167be01
Status   : Active
Records  : musik, musik-api, musik-admin, luminaai
```

### yudhalabs.dev
```
Zone ID  : 640d54694506dad8a328023a141b4dec
Status   : Active
Records  : @, www, app, api, send
```

### hallowa.id
```
Zone ID  : (check CF dashboard)
Status   : Active
Records  : Various legacy records
```

---

## Cloudflare Tunnels

### laptopdell-zesbe
```
Tunnel ID    : 546eeece-1cba-43f4-b8c4-c1c42ab3a8df
Status       : ✅ Active
Device       : Local Laptop
Config File  : /etc/cloudflared/config.yml
Credentials  : /home/zesbe/.cloudflared/546eeece-*.json

Routes:
  musik.zesbe.my.id      → localhost:5173
  musik-api.zesbe.my.id  → localhost:5170
  musik-admin.zesbe.my.id → localhost:5174
```

### wacoex-tunnel
```
Tunnel ID    : 8a31ce0d-f0c0-48a3-8a0d-d93d87e6617c
Status       : ⚠️ Inactive (VPS Down)
Device       : VPS 137
Cloudflared  : v2025.11.1

Routes:
  yudhalabs.dev     → localhost:3000
  www.yudhalabs.dev → localhost:3000
  app.yudhalabs.dev → localhost:3001
  api.yudhalabs.dev → localhost:5000
```

---

## Quick DNS Lookup

```bash
# Check DNS resolution
dig +short musik.zesbe.my.id
dig +short luminaai.zesbe.my.id

# Check if domain is proxied through CF
curl -sI https://musik.zesbe.my.id | grep -i "cf-ray"

# Check SSL certificate
echo | openssl s_client -servername musik.zesbe.my.id -connect musik.zesbe.my.id:443 2>/dev/null | openssl x509 -noout -dates
```

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLOUDFLARE                               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐           │
│  │ zesbe.my.id  │  │yudhalabs.dev │  │  hallowa.id  │           │
│  └──────┬───────┘  └──────┬───────┘  └──────────────┘           │
│         │                 │                                      │
│  ┌──────┴───────┐  ┌──────┴───────┐                             │
│  │ CF Tunnel    │  │ CF Tunnel    │                             │
│  │laptopdell    │  │wacoex-tunnel │                             │
│  └──────┬───────┘  └──────┬───────┘                             │
└─────────┼─────────────────┼─────────────────────────────────────┘
          │                 │
          ▼                 ▼
   ┌──────────────┐  ┌──────────────┐
   │ LOCAL LAPTOP │  │   VPS 137    │
   │              │  │   (DOWN)     │
   │ :5173 FE     │  │              │
   │ :5170 BE     │  │ :3000 Landing│
   │ :5174 Admin  │  │ :3001 App    │
   └──────────────┘  │ :5000 API    │
          │          └──────────────┘
          │
          ▼ (Database & Storage)
   ┌──────────────┐
   │   VPS 60     │
   │              │
   │ PostgreSQL   │
   │ Redis        │
   │ Audio Files  │
   │ Images       │
   └──────────────┘
```
