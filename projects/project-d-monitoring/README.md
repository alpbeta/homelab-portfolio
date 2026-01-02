# Projekt D – Monitoring mit Prometheus & Grafana

## Ziel
Ziel dieses Projekts ist die transparente Überwachung der Infrastruktur
(Host-System und Docker-Container) in einer Homelab-Umgebung.

## Architektur
- Ubuntu Server VM (Proxmox)
- Docker Compose als separater Monitoring-Stack
- Prometheus als Metrics-Collector
- Grafana zur Visualisierung
- node-exporter für Host-Metriken
- cAdvisor für Container-Metriken

## Umsetzung
- Prometheus sammelt Metriken im 15-Sekunden-Intervall
- node-exporter liefert CPU-, RAM-, Disk- und Netzwerkmetriken des Hosts
- cAdvisor liefert Container-Status und Ressourcennutzung
- Grafana visualisiert die Daten über Dashboards

## Dashboards
- **Node Exporter Full** (Grafana Dashboard ID: 1860)
- **Docker / cAdvisor** (Grafana Dashboard ID: 14282)

## Ergebnis
- Echtzeit-Übersicht über Systemzustand
- Transparenz über Docker-Container und Ressourcennutzung
- Grundlage für Alerting und Kapazitätsplanung

## Learnings
- Trennung von Applikations- und Monitoring-Stack erhöht Wartbarkeit
- Prometheus + Grafana sind de-facto-Standard im Monitoring-Umfeld
- Metrik-basierte Überwachung ermöglicht proaktives Handeln
