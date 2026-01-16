# Projekt E: Infrastruktur Monitoring – Prometheus & Grafana

## Ziel
Aufbau eines Monitoring-Stacks zur transparenten Überwachung von Host- und Container-Ressourcen (CPU/RAM/Disk/Netzwerk).
Nachweis über Prometheus Targets (UP), Grafana Dashboards und cAdvisor-Übersicht.

## Umgebung
- Plattform: Proxmox VE → Ubuntu Server VM (ubuntu-server-01 / 192.168.178.40)
- Container: Docker + Docker Compose
- Services/Ports:
  - Grafana: 3000
  - Prometheus: 9090
  - cAdvisor: 8081 → 8080
  - node-exporter: 9100 (intern)

## Architektur (Kurz)
- Prometheus sammelt Metriken von Exportern (Scrape Targets)
- Grafana visualisiert die Metriken über Dashboards
- cAdvisor liefert Container-/cgroup-Metriken
- node-exporter liefert Host-Metriken

## Verifikation (Nachweise)
- `docker compose ps` → Services laufen (grafana, prometheus, cadvisor, node-exporter)
- Prometheus Targets: `/targets` → relevante Targets sind **UP**
- Grafana Dashboard zeigt Host-/Systemmetriken
- cAdvisor Übersicht unter `/containers/` erreichbar

## Screenshots (Live-Portfolio)
Die Nachweis-Screenshots sind im Live-Portfolio dokumentiert:
- https://mesutslab.dev/docs/monitoring/
