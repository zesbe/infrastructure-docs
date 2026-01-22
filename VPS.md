# VPS Inventory

> Detail lengkap semua VPS

---

## VPS 227 - zesbe

### Connection Info
```
Hostname    : zesbe
Zerotier IP : 10.180.160.227
Public IP   : 168.110.204.71
SSH Port    : 6746
User        : zesbe
```

### Quick Connect
```bash
ssh zesbe@10.180.160.227 -p 6746
```

### System Info
- OS: Ubuntu
- Node: v20.19.5
- NPM: 10.8.2

### Running Services

| Service | Port | Status |
|---------|------|--------|
| PostgreSQL | 5432, 5433 | :green_circle: Running |
| Redis | 6379 | :green_circle: Running |
| VPN (WireGuard) | 992, 1194, 5555 | :green_circle: Running |
| Zerotier | 9993 | :green_circle: Running |
| NocoDB | 8080 | :green_circle: Running |

### Docker Containers
```
demogolang-postgres  - PostgreSQL untuk demo
nocodb               - NocoDB instance
vpn-dashboard        - VPN management dashboard
```

### Databases
| Database | Description |
|----------|-------------|
| droiddb | DroidVPN database |
| nocodb | NocoDB data |
| vpndashboard | VPN Dashboard |

### Domains Pointing Here
- None currently (direct IP access only)

---

## VPS 60 - hallowa-server1

### Connection Info
```
Hostname    : hallowa-server1
Zerotier IP : 10.180.160.60
Public IP   : 168.110.212.151
SSH Port    : 6746
User        : zesbe
```

### Quick Connect
```bash
ssh zesbe@10.180.160.60 -p 6746
```

### System Info
- OS: Ubuntu
- Docker: Active

### Running Services

| Service | Port | Status |
|---------|------|--------|
| Nginx | 80, 443 | :green_circle: Running |
| Lumina Backend | 8082 | :green_circle: Running |
| Lumina Frontend | 3004 | :green_circle: Running |
| PostgreSQL (Docker) | 5432 | :green_circle: Running |
| Redis (Docker) | 6379 | :green_circle: Running |
| VPN | 992, 1194, 5555 | :green_circle: Running |
| Zerotier | 9993 | :green_circle: Running |
| Mikrotik Bot | 8080 | :green_circle: Running |

### Docker Containers
```
lumina-backend    - Go API untuk musik generation
lumina-frontend   - SvelteKit frontend
lumina-postgres   - PostgreSQL 16 database
lumina-redis      - Redis cache
mikrotik-bot      - Mikrotik automation bot
```

### Storage Volumes
```
lumina-ai-v2_lumina_uploads
├── audio/     # 51 MP3 + 5 MP4 (328 MB)
└── images/    # 34 album art images (20 MB)
```

### Databases
| Database | Description |
|----------|-------------|
| lumina_ai | Musik/Lumina AI data (56 songs) |

### Domains Pointing Here
| Domain | Service |
|--------|---------|
| luminaai.zesbe.my.id | Lumina Frontend (Docker) |

---

## VPS 137 - vmid3993

### Connection Info
```
Hostname    : vmid3993
Zerotier IP : 10.180.160.137
Public IP   : 206.183.130.150
SSH Port    : 6746
User        : zesbe
Status      : DOWN (as of 22 Jan 2026)
```

### Quick Connect
```bash
ssh zesbe@10.180.160.137 -p 6746
```

### System Info
- OS: Ubuntu
- Node: v20.19.6
- NPM: 10.8.2

### Cloudflare Tunnel
```
Tunnel Name : wacoex-tunnel
Tunnel ID   : 8a31ce0d-f0c0-48a3-8a0d-d93d87e6617c
Status      : Inactive (VPS Down)
```

### Planned Services (Wacoex)

| Service | Port | Status |
|---------|------|--------|
| Wacoex API | 5000 | :yellow_circle: Planned |
| Wacoex Landing | 3000 | :yellow_circle: Planned |
| Wacoex Dashboard | 3001 | :yellow_circle: Planned |

### Directory Structure
```
/opt/apps/
├── wacoex/          # Main project
├── kotlin-gemini/   # Side project
├── monitoring/      # System monitoring
├── infrastructure/  # Infra configs
└── backups/         # Backup files
```

### Domains Pointing Here (via CF Tunnel)
| Domain | Service |
|--------|---------|
| yudhalabs.dev | Landing (port 3000) |
| www.yudhalabs.dev | Landing (port 3000) |
| app.yudhalabs.dev | Dashboard (port 3001) |
| api.yudhalabs.dev | API (port 5000) |

---

## Security Configuration (All VPS)

### SSH Security
```
Port                    : 6746
ListenAddress           : Zerotier IP only
PasswordAuthentication  : yes
PubkeyAuthentication    : no (disabled by design)
```

### Firewall (UFW)
```
ALLOW: VPN (992, 1194, 5555) - Public
ALLOW: Zerotier (9993) - Public
ALLOW: SSH (6746) - Zerotier only
DENY:  All other ports from public
```

### Sudoers
```
File: /etc/sudoers.d/zesbe
Content: zesbe ALL=(ALL) NOPASSWD: ALL
```

---

## Maintenance Commands

### Check Docker Status
```bash
docker ps
docker stats
```

### Check Disk Usage
```bash
df -h
docker system df
```

### View Logs
```bash
# Docker logs
docker logs -f <container_name>

# System logs
journalctl -f
```

### Cleanup Docker
```bash
# Remove unused images
docker image prune -a

# Remove unused volumes
docker volume prune

# Full cleanup
docker system prune -a
```
