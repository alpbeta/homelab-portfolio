# Projekt B – Docker & NGINX Reverse Proxy

## Ziel des Projekts
Ziel dieses Projekts war der Aufbau einer produktionsnahen Serverumgebung,
in der mehrere interne Services über einen zentralen Zugriffspunkt
bereitgestellt werden.

Dabei lag der Fokus auf:
- sauberer Service-Trennung
- minimaler Portfreigabe
- sicherer und übersichtlicher Architektur

---

## Architekturübersicht
Die Umgebung besteht aus einer Ubuntu Server VM, die auf Proxmox VE betrieben wird.
Innerhalb der VM werden alle Services containerisiert mit Docker betrieben.

Ein zentraler NGINX Reverse Proxy übernimmt die Weiterleitung der eingehenden
HTTP-Anfragen an die internen Services.

**Architektur (vereinfacht):**
- Client (LAN)
- NGINX Reverse Proxy (Port 80)
- Interne Docker-Services (isoliert im Docker-Netzwerk)

---

## Umsetzung

### 1. Virtuelle Maschine
- Proxmox VE als Hypervisor
- Ubuntu Server ohne grafische Oberfläche
- Verwaltung ausschließlich per SSH

### 2. Docker & Docker Compose
- Installation von Docker Engine
- Nutzung von Docker Compose zur Definition mehrerer Services
- Externes Docker-Netzwerk zur Service-Kommunikation

### 3. Reverse Proxy
- Einsatz von NGINX als Reverse Proxy
- Veröffentlichung aller Services über **einen einzigen Port (80)**
- Path-based Routing:
  - `/whoami` → whoami-Service
  - `/echo` → echo-server

### 4. Firewall
- Aktivierung von UFW
- Freigabe nur notwendiger Ports:
  - 22/tcp (SSH)
  - 80/tcp (HTTP)
- Entfernen von temporären Test-Ports (z. B. 8080)

---

## Sicherheit
- Kein direkter Zugriff auf interne Container
- Alle Services ausschließlich über den Reverse Proxy erreichbar
- Minimierte Firewall-Regeln
- Trennung von Test- und Produktionskonfiguration

---

## Tests
Die Funktionalität wurde auf mehreren Ebenen überprüft:

- Lokale Tests auf der VM:
```bash
curl http://localhost/echo
