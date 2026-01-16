# Projekt D: Cloudflare Tunnel – Publishing von mesutslab.dev

## Ziel
Sichere Veröffentlichung interner Services ohne Port-Forwarding durch Cloudflare Tunnel (ausgehende Verbindung).

## Umgebung
- Domain: mesutslab.dev + www
- Origin: Ubuntu VM (192.168.178.40) → NGINX auf http://127.0.0.1:80
- cloudflared als systemd Service
- Proxmox 192.168.178.39:8006 bleibt intern (kein Publishing-Ziel)

## Ingress (config.yml)
tunnel: 03e1b69e-06f3-48d8-acb9-fac1a2e39d46
credentials-file: /root/.cloudflared/03e1b69e-06f3-48d8-acb9-fac1a2e39d46.json

ingress:
  - hostname: mesutslab.dev
    service: http://127.0.0.1:80
  - hostname: www.mesutslab.dev
    service: http://127.0.0.1:80
  - service: http_status:404

## Problem & Fix (502)
Root Cause: Tunnel zeigte fälschlich auf 127.0.0.1:8006 (Proxmox).
Fix: Origin korrekt auf http://127.0.0.1:80 gesetzt.

## Verifikation
- Lokal: curl -I http://127.0.0.1:80
- Extern: curl -I https://mesutslab.dev
