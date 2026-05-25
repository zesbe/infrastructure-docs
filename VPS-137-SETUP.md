# VPS 137 - peramix137 Setup Documentation

> Dokumentasi lengkap setup VPS 137 (peramix137) - Updated: 26 Mei 2026

---

## Server Info

```
Hostname    : peramix137
ZeroTier IP : 10.180.160.137
Public IP   : 206.183.130.150
SSH Port    : 6746
User        : zesbe
OS          : Ubuntu 24.04.4 LTS (Noble Numbat)
Kernel      : 6.8.0-117-generic
CPU         : 10 vCPU
RAM         : 32 GB
Disk        : 242 GB
```

### Quick Connect
```bash
ssh zesbe@10.180.160.137 -p 6746
# atau via ZeroTier
ssh -p 6746 zesbe@10.180.160.137
```

---

## Cloudflare Tunnel

```
Tunnel Name : appwrite
Tunnel ID   : c3f9fe91-ea64-4d2d-9b27-964840c1b055
Config      : /etc/cloudflared/config.yml
```

### Ingress Rules

| Hostname | Service | CF Access |
|----------|---------|-----------|
| appwrite.zesbe.my.id | http://localhost:80 | ❌ |
| browser.zesbe.my.id | http://localhost:8091 | ❌ |
| chrome.zesbe.my.id | http://10.180.160.60:8090 | ❌ |
| ppob.zesbe.my.id | http://localhost:8081 | ❌ |
| ttyd.zesbe.my.id | http://localhost:7681 | ✅ Email OTP |
| remote.zesbe.my.id | http://localhost:8093 | ✅ Email OTP |

---

## Cloudflare Access (Zero Trust)

Dua subdomain dilindungi CF Access — harus verifikasi email OTP sebelum bisa akses.

| Domain | App ID | Policy |
|--------|--------|--------|
| ttyd.zesbe.my.id | 185ed0b4-5f9d-4feb-914e-6b44b0668439 | yudiharyanto41@gmail.com only |
| remote.zesbe.my.id | 2898a65e-d6b9-42c4-8fa6-b15b18ce0ca6 | yudiharyanto41@gmail.com only |

**Flow akses:**
```
Browser → CF Access (kirim OTP ke email) → Input OTP → Akses service
```

Session duration: 24 jam (tidak perlu OTP ulang selama 24 jam)

---

## Services

### 1. ttyd - Web Terminal
```
URL      : https://ttyd.zesbe.my.id
User     : zesbe
Password : mifL7cTavw8pndk
Shell    : bash (as user zesbe)
Config   : /etc/default/ttyd
```

**Config `/etc/default/ttyd`:**
```bash
TTYD_OPTIONS=-i lo -p 7681 -W -c zesbe:mifL7cTavw8pndk -P 30 -t titleFixed=ttyd-peramix137 su - zesbe
```

- `-W` = writable (bisa input command)
- `-c` = HTTP basic auth
- `-P 30` = ping setiap 30 detik (cegah disconnect)
- `su - zesbe` = login sebagai user zesbe (bukan root)

```bash
# Restart ttyd
sudo systemctl restart ttyd
sudo systemctl status ttyd
```

---

### 2. Apache Guacamole - Remote Desktop
```
URL      : https://remote.zesbe.my.id/guacamole/
User     : guacadmin
Password : guacadmin  ← GANTI INI!
Location : /home/zesbe/guacamole/
```

**Koneksi yang sudah dikonfigurasi:**

| Nama | Protocol | Host | Port | User |
|------|----------|------|------|------|
| Laptop Zesbe (Fedora) | RDP | 10.180.160.93 | 3389 | zesbe |

**Catatan laptop Fedora:**
- OS: Fedora 44, GNOME + Wayland
- RDP via `gnome-remote-desktop`
- Audio disabled (incompatible dengan guacd 1.6.0)
- Password RDP: `090994`

```bash
# Restart Guacamole
cd /home/zesbe/guacamole && docker compose restart

# Logs
docker logs guacamole --tail 50
docker logs guacd --tail 50
```

---

### 3. n.eko - Browser di VPS
```
URL (ZeroTier) : http://10.180.160.137:8090
URL (Tunnel)   : https://browser.zesbe.my.id
Password       : 28l2glMOYsbzpUf5rPCx
Location       : /home/zesbe/neko/
Browser        : Google Chrome
```

**Catatan penting:**
- ⚠️ JANGAN buka file manager (Nautilus/Files) di dalam neko → bikin Chrome hang, CPU spike 200%+
- Kalau hang: `docker restart neko-neko-1`
- WebRTC ports: 52000-52100/UDP (ZeroTier only)

```bash
# Restart neko
docker restart neko-neko-1

# Status
docker stats neko-neko-1 --no-stream
```

---

### 4. SFTP / SSH Access
```
Host     : 10.180.160.137
Port     : 6746
User     : zesbe
Password : [password sistem]
Protocol : SFTP
```

**Dari terminal:**
```bash
sftp -P 6746 zesbe@10.180.160.137
```

**Dari FileZilla/WinSCP:**
```
Protocol : SFTP
Host     : 10.180.160.137
Port     : 6746
User     : zesbe
```

**Syarat:** Device harus join ZeroTier network `08752e18b1872bfc`

---

### 5. Appwrite
```
URL      : https://appwrite.zesbe.my.id
Location : /home/zesbe/appwrite/ (atau default appwrite dir)
```

---

### 6. PPOB
```
URL      : https://ppob.zesbe.my.id
Frontend : http://localhost:8081 (Docker)
Backend  : port 3001 (Docker)
Database : PostgreSQL (Docker)
```

---

## Security Configuration

### SSH
```
Config files:
- /etc/ssh/sshd_config
- /etc/ssh/sshd_config.d/zerotier-only.conf
- /etc/ssh/sshd_config.d/60-cloudimg-settings.conf

Key settings:
- Port: 6746
- ListenAddress: 10.180.160.137 (ZeroTier only)
- PasswordAuthentication: yes
- PubkeyAuthentication: no (zerotier-only.conf)
- PermitRootLogin: no (zerotier-only.conf)
- MaxAuthTries: 3
- Subsystem sftp: /usr/lib/openssh/sftp-server
- AllowUsers: zesbe
```

### CrowdSec
```bash
# Status
sudo systemctl status crowdsec

# Lihat decisions (IP yang diblok)
sudo cscli decisions list

# Lihat alerts
sudo cscli alerts list
```

### ZeroTier
```
Network ID : 08752e18b1872bfc
ZT IP      : 10.180.160.137/24
Interface  : ztcdcksm2n
```

### Security Layers
```
Internet → Cloudflare Tunnel (zero exposed ports)
        → CF Access (email OTP untuk ttyd & remote)
        → Service auth (basic auth / guacadmin)

SSH     → ZeroTier only (private mesh)
        → Port 6746 (non-standard)
        → No root login
        → CrowdSec (brute force protection)
```

---

## Docker Containers

```bash
# Lihat semua container
docker ps

# Resource usage
docker stats --no-stream
```

| Container | Service | Status |
|-----------|---------|--------|
| neko-neko-1 | Browser (Chrome) | Running |
| guacamole | Remote Desktop | Running |
| guacd | Guacamole daemon | Running |
| guac_db | MySQL (Guacamole) | Running |
| ppob-frontend | PPOB Frontend | Running |
| ppob-backend | PPOB Backend | Running |
| ppob-postgres | PPOB Database | Running |
| appwrite-* | Appwrite stack | Running |

---

## Lynis Security Audit

**Score: 60/100** (dijalankan 26 Mei 2026)

**Pending fixes (low priority untuk dev VPS):**
- Vulnerable packages → `sudo apt upgrade`
- Redis tanpa password → `/etc/redis/redis.conf`
- Old kernels (10 kernels) → `sudo apt autoremove`
- SSH: AllowTcpForwarding, MaxSessions, LogLevel

---

## Maintenance Commands

```bash
# Health check semua container
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"

# Disk usage
df -h
docker system df

# Memory
free -h

# Restart cloudflared tunnel
sudo systemctl restart cloudflared

# Backup cloudflared config
sudo cp /etc/cloudflared/config.yml /etc/cloudflared/config.yml.bak.$(date +%Y%m%d)

# Docker cleanup (hati-hati)
docker system prune -f
```

---

## File Locations

```
/etc/cloudflared/config.yml     → Cloudflare Tunnel config
/etc/default/ttyd               → ttyd config
/etc/ssh/sshd_config            → SSH main config
/etc/ssh/sshd_config.d/         → SSH drop-in configs
/home/zesbe/neko/               → n.eko docker-compose
/home/zesbe/guacamole/          → Guacamole docker-compose
/home/zesbe/neko/docker-compose.yml
/home/zesbe/guacamole/docker-compose.yml
```

---

## Troubleshooting

### ttyd tidak bisa diakses
```bash
sudo systemctl status ttyd
sudo systemctl restart ttyd
sudo ss -tlnp | grep 7681
```

### Guacamole "waiting for response"
```bash
# Restart guacd dulu
docker restart guacd
sleep 3
docker restart guacamole
```

### neko Chrome hang (CPU tinggi)
```bash
docker restart neko-neko-1
# Tunggu 15 detik sampai healthy
docker stats neko-neko-1 --no-stream
```

### Cloudflare Tunnel down
```bash
sudo systemctl status cloudflared
sudo journalctl -u cloudflared --since "10 minutes ago"
sudo systemctl restart cloudflared
```

### SFTP "subsystem not found"
```bash
# Pastikan ada di sshd_config
grep -r 'Subsystem' /etc/ssh/
# Kalau tidak ada:
echo "Subsystem sftp /usr/lib/openssh/sftp-server" | sudo tee -a /etc/ssh/sshd_config
sudo systemctl restart ssh
```
