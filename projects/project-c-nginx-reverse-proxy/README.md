# Projekt C – NGINX Reverse Proxy mit HTTPS

## Ziel
Aufbau eines sicheren Reverse-Proxys zur Weiterleitung mehrerer Dienste
über eine zentrale HTTPS-Adresse.

## Architektur
- Ubuntu Server VM (Proxmox)
- Docker & Docker Compose
- NGINX als Reverse Proxy
- Let’s Encrypt Zertifikate (DNS-01 über Cloudflare)
- Firewall (UFW)

## Umsetzung
- NGINX läuft als Docker-Container
- TLS-Zertifikate werden per Certbot und Cloudflare DNS-01 erstellt
- HTTP-Anfragen werden automatisch auf HTTPS umgeleitet
- Dienste werden per Pfad weitergeleitet:
  - `/echo`
  - `/whoami`
- Root (`/`) dient als Health-Endpoint des Reverse Proxys

## Sicherheit
- HTTPS mit gültigem Let’s-Encrypt-Zertifikat
- Firewall-Regeln über UFW
- Trennung von Reverse Proxy und Backend-Containern

## Ergebnis
- Zentrale HTTPS-Adresse für mehrere Services
- Saubere Trennung von Infrastruktur und Anwendungen
- Grundlage für weitere Services (Monitoring, Portfolio, CI/CD)

## Learnings
- Reverse-Proxys sind zentrale Bausteine moderner Infrastrukturen
- DNS-01 ist ideal für private IPs und Homelabs
- Pfad-basiertes Routing vereinfacht Service-Erweiterungen
