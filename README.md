# ZimaBoard 2 Portable Homelab — Archinet Labs

Proxmox homelab on ZimaBoard 2: Jellyfin media, Zimanas NAS, Security Onion, optimized for the N150 / 16 GB platform.

---

# Quick Access

| Service | URL |
|---------|-----|
| Proxmox VE | https://10.10.10.104:8006 |
| Jellyfin | http://10.10.10.10:8096 |
| OpenMediaVault (Zimanas) | https://10.10.10.11 |
| Netdata | http://10.10.10.104:19999 |

## Pinned network drives (Windows)

| Drive | Path | User / Password |
|-------|------|-----------------|
| **P:** | `\\10.10.10.104\jellyfin-media` | `archinet` / `Content_Bridge$@` |
| **Z:** | `\\10.10.10.11\media` | `archinet` / `Content_Bridge$@` |

NAS appears as **Zimanas**. iPhone: Files → Connect to Server → `smb://10.10.10.11`.

---

# Architecture

```
ZimaBoard 2 (Proxmox @ 10.10.10.104)
├── CT 110  Jellyfin          10.10.10.10   USB 4TB media (bind)
├── VM 111  OpenMediaVault    10.10.10.11   SATA 1TB (Zimanas)
└── VM 120  Security Onion    (install)     200GB + dual NIC
```

## Storage

| Disk | Role |
|------|------|
| eMMC | Proxmox OS (ZFS) |
| sda 1TB | `VM_Storage` — guest disks + local compressed backups |
| sdb 1TB | OMV data passthrough (Zimanas) |
| sdc 4TB USB | Jellyfin media (`/mnt/media-disk`) |

## Network

- `vmbr0` / `nic1` — LAN (management, Jellyfin, NAS)
- `vmbr1` / `nic0` — Security Onion monitor (plug second Ethernet when ready)

---

# Docs

| Doc | Topic |
|-----|-------|
| `docs/jellyfin.md` | Media server |
| `docs/nas.md` | Zimanas NAS |
| `docs/security-onion.md` | SO install guide |
| `docs/operations.md` | Backups & maintenance |
| `docs/security.md` | Firewalls & access model |
| `PRIVATE-CREDENTIALS.txt` | Passwords (gitignored) |

---

# Security model

- LAN-only management (Proxmox, OMV, SMB)
- Guest firewalls: DROP inbound except required LAN ports
- OMV `archinet`: SMB only (no SSH, no web UI)
- OMV `root`: SSH with password
- Jellyfin: LAN (+ Tailscale range if used)
- Security Onion: start only when needed (RAM)

---

Maintained by Archinet Labs
