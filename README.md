# Homelab Portfolio – Systemintegration & Infrastruktur

Dieses Repository dokumentiert meine praxisorientierten Homelab-Projekte im Bereich **Systemintegration**, **Virtualisierung**, **Containerisierung**, **Netzwerk** und **IT-Security**.  
Fokus: realitätsnahe Szenarien, klare Struktur, nachvollziehbare Dokumentation – als Portfolio für Bewerbungen.

## Projekte (Dokumentation)

### Projekt A – Proxmox VE: Installation, Netzwerkstabilität, Ubuntu-VM & Hardening
**Ziel:** Aufbau einer stabilen Virtualisierungsplattform als Basis für weitere Projekte.  
➡️ Doku: `docs/PROJECT_A_PROXMOX.md`

### Projekt B – Container & Reverse Proxy (Docker/Compose + NGINX)
**Ziel:** Reproduzierbare Service-Bereitstellung auf Ubuntu (VM) sowie Veröffentlichung interner Services über einen zentralen Reverse Proxy.  
**Teildokus:**
- Docker & Docker Compose auf Ubuntu (inkl. Rechte-Fix, UFW, Snapshot): `docs/PROJECT_B_DOCKER_COMPOSE_UBUNTU.md`
- NGINX Reverse Proxy mit Docker Compose (path-based Routing): `docs/PROJECT_B_NGINX_REVERSE_PROXY.md`

### Projekt C – Security Hardening: SSH Absicherung mit Fail2ban
**Ziel:** Schutz gegen Brute-Force-Angriffe durch Log-Analyse und automatische IP-Bans.  
➡️ Doku: `docs/PROJECT_C_SSH_FAIL2BAN.md`

### Projekt D – Cloudflare Tunnel: Publishing von mesutslab.dev
**Ziel:** Interne Services sicher veröffentlichen **ohne Port-Forwarding** (Cloudflare Tunnel → NGINX).  
➡️ Doku: `docs/PROJECT_D_CLOUDFLARE_TUNNEL.md`

### <a id="projekt-e-monitoring"></a>Projekt E – Monitoring (Prometheus & Grafana)
**Ziel:** Überwachung von Host- und Container-Ressourcen mit Prometheus, Grafana, node-exporter und cAdvisor.  
➡️ Doku: `docs/PROJECT_E_MONITORING.md`

## Technologien (Auszug)
- Proxmox VE, Ubuntu Server
- Docker & Docker Compose, NGINX
- Cloudflare Tunnel, DNS
- UFW, SSH Hardening, Fail2ban
- Prometheus & Grafana (Monitoring-Stack im Portfolio-Site dokumentiert)

## Live Portfolio
- Website: https://mesutslab.dev
