# Homelab Portfolio – Systemintegration & Infrastruktur

Dieses Repository dokumentiert meine praxisorientierten Homelab-Projekte
im Bereich Systemintegration, Virtualisierung, Containerisierung und IT-Security.

Der Fokus liegt auf realitätsnahen Szenarien, klarer Struktur und nachvollziehbarer Dokumentation.

---

## Projekt A – Proxmox & Virtualisierung

**Ziel:** Aufbau einer stabilen Virtualisierungsplattform als Basis für alle weiteren Projekte.

### Inhalte
- Installation und Grundkonfiguration von **Proxmox VE**
- Verwaltung virtueller Maschinen (VM Lifecycle)
- Snapshot-Strategie und Wiederherstellung
- Netzwerkgrundlagen im virtualisierten Umfeld (Bridges, Interfaces)

---

## Projekt B – Docker & NGINX Reverse Proxy

**Ziel:** Bereitstellung mehrerer Dienste über eine zentrale Infrastruktur.

### Inhalte
- Aufbau einer **Ubuntu Server VM** auf Proxmox
- Einrichtung von **Docker** und **Docker Compose**
- Implementierung eines **NGINX Reverse Proxys**
- Veröffentlichung mehrerer interner Services über eine zentrale HTTP-Schnittstelle
- Firewall-Härtung mit **UFW**
- Tests aus dem lokalen Netzwerk (externer Client)

---

## Projekt C – HTTPS / TLS

**Ziel:** Absicherung der Infrastruktur durch moderne Transportverschlüsselung.

### Inhalte
- TLS-Verschlüsselung mit **Let’s Encrypt**
- HTTP → HTTPS Weiterleitung
- DNS-01 Validierung über Cloudflare
- Produktionsnahe Sicherheitskonfiguration

---

## Projekt D – Monitoring (Prometheus & Grafana)

**Ziel:** Transparente Überwachung von Host- und Container-Ressourcen.

### Inhalte
- Prometheus als Metrics-Collector
- Grafana zur Visualisierung
- node-exporter (Host-Metriken)
- cAdvisor (Container-Metriken)
- Dashboards:
  - Node Exporter Full (ID 1860)
  - Docker / cAdvisor (ID 14282)

---

## Ziel des Homelabs
- Vertiefung praktischer Kenntnisse in der Systemintegration
- Aufbau einer modularen, erweiterbaren Infrastruktur
- Vorbereitung auf produktionsnahe Szenarien
- Saubere Dokumentation als Grundlage für Wissenstransfer und Portfolio

---

## Technologien
- Proxmox VE
- Ubuntu Server
- Docker & Docker Compose
- NGINX
- Let’s Encrypt / TLS
- Prometheus & Grafana
- UFW / SSH / Fail2Ban
