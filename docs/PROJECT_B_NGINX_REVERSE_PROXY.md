# Projekt B: NGINX Reverse Proxy mit Docker Compose

## Ziel
Aufbau einer produktionsnahen Container-Architektur, bei der mehrere interne Services über einen zentralen Reverse Proxy erreichbar sind. Dadurch werden Ports konsolidiert und Services bleiben intern isoliert.

## Umgebung
- Proxmox VE → Ubuntu Server VM (ubuntu-server-01)
- Docker Engine + Docker Compose
- UFW aktiv
- Docker Network: web (external)
- Reverse Proxy: nginx:alpine
- Backend Services: traefik/whoami, ealen/echo-server

## Umsetzung
- Projektverzeichnis: `~/projects/nginx-reverse-proxy`
- Docker Netzwerk `web` (external), damit Proxy und Services im selben privaten Netz laufen.
- `docker-compose.yml` definiert drei Services:
  - `reverse-proxy` publiziert HTTP nach außen (Port 80)
  - `whoami` intern
  - `echo` intern
- NGINX Routing (path-based):
  - `/whoami/` → whoami
  - `/echo/` → echo-server

## Firewall
UFW erlaubt mindestens:
- `22/tcp` (SSH)
- `80/tcp` (HTTP)

## Verifikation
- Lokal: `curl http://127.0.0.1/echo/`
- LAN (Mac): `curl http://192.168.178.40/echo/`

## Ergebnis
- Zentraler Reverse Proxy stellt mehrere Services über eine einzige öffentliche Schnittstelle bereit.
- Services sind intern über Docker Network isoliert.
- Zugriff von Client im LAN erfolgreich bestätigt.
