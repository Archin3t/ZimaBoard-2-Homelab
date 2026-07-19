# ZimaBoard 2 Homelab Blueprint

Public architecture documentation for a **single-node Proxmox Homelab** built around the **IceWhale ZimaBoard 2 (1664)** — and reusable on similar (or totally different) hardware.

This repo answers: what the box runs, why it’s split that way, how to recreate the **platform**, and where app install guides live.

## Companion repos (two — not three)

| Repository | Covers |
|------------|--------|
| [ZimaBoard-Media-Stack](https://github.com/Archin3t/ZimaBoard-Media-Stack) | **Movies + TV + books** — Jellyfin, Jellyseerr, Sonarr, Radarr, Prowlarr, qBittorrent, Kavita, Calibre-Web Automated, LazyLibrarian |
| [ZimaBoard-NAS (and everything else)](https://github.com/Archin3t/ZimaBoard-NAS) | **NAS + storage + everything else** — OpenMediaVault patterns, disk passthrough, shares, backups, extras |

**HTML manual:** [archin3t.github.io/ZimaBoard-2-Homelab](https://archin3t.github.io/ZimaBoard-2-Homelab/)

## ZimaBoard 2 1664 (reference hardware)

| Spec | Value |
|------|--------|
| CPU | Intel N150 (quad-core, up to ~3.6 GHz) |
| RAM | 16 GB LPDDR5 |
| Boot | 64 GB eMMC (hypervisor OS only) |
| NIC | 2× 2.5GbE |
| I/O | 2× SATA · 2× USB 3.0 · Mini DisplayPort |
| GPU | Intel iGPU (VAAPI / Quick Sync when passed to Jellyfin) |

Same **concepts** apply on other N-series boxes, NUCs, SFF PCs, or larger nodes — scale RAM/disk; keep role separation.

## What it does

```text
Request show/movie → *arr + torrent client → media disk → Jellyfin streams
Books ingest/fetch  → books guest disk      → Kavita / Calibre-Web
File shares/backups → NAS guest (optional)
