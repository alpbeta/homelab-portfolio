# Projekt C – HTTPS / TLS mit Let’s Encrypt (DNS-01) + NGINX

## Ziel
Ziel dieses Projekts war es, den bestehenden NGINX Reverse Proxy produktionsnah abzusichern:
- TLS/HTTPS aktivieren (Port 443)
- HTTP → HTTPS Weiterleitung
- Zertifikatsverwaltung automatisieren (Renewal)

## Ausgangslage
Die Services liefen containerisiert über Docker Compose hinter einem NGINX Reverse Proxy.
Da die Umgebung im lokalen Netzwerk (private IP 192.168.x.x) betrieben wird, ist eine klassische HTTP-01 Validierung über das Internet nicht zuverlässig möglich.

## Lösungsansatz
Um TLS auch in einer privaten Netzwerkumgebung sauber umzusetzen, wurde **Let’s Encrypt via DNS-01 Challenge** verwendet.
Als DNS-Provider dient Cloudflare.

## Umsetzung (Kurzüberblick)
1. Domain registriert und DNS-Zone bei Cloudflare verwaltet
2. A-Record auf die Server-IP gesetzt: `mesutslab.dev → 192.168.178.40`
3. Certbot + Cloudflare DNS Plugin installiert
4. Cloudflare API Token mit minimalen Rechten erstellt (Zone DNS Edit, nur für die Zone)
5. Zertifikat per DNS-01 Challenge bezogen:
   - `mesutslab.dev`
   - `www.mesutslab.dev`
6. NGINX so konfiguriert, dass:
   - Port 80 auf 443 weiterleitet
   - TLS-Zertifikate aus `/etc/letsencrypt/live/…` verwendet werden
7. Firewall (UFW) auf minimale Ports:
   - 22/tcp (SSH)
   - 80/tcp (HTTP)
   - 443/tcp (HTTPS)

## Tests
- DNS-Auflösung geprüft (Resolver-Test):
  - `dig @1.1.1.1 mesutslab.dev`
- HTTP Redirect geprüft:
  - `curl -I http://mesutslab.dev` → `Location: https://mesutslab.dev/`
- HTTPS Reverse Proxy geprüft:
  - `curl -k https://mesutslab.dev/echo/`

## Ergebnis
- TLS aktiv (Port 443)
- HTTP → HTTPS Redirect aktiv
- Zertifikat erfolgreich ausgestellt und automatisches Renewal geplant
- Lösung ist auch in privaten Netzwerken produktionsnah umsetzbar (DNS-01)

## Learnings
- DNS-01 Challenge ist eine robuste Methode für TLS in internen/privaten Umgebungen
- Strikte Rechtevergabe (API Token minimal) erhöht die Sicherheit
- Saubere Trennung zwischen Infrastruktur (NGINX) und Services (Docker) erleichtert Wartung und Erweiterung
