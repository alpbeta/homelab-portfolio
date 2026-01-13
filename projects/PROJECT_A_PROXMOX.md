# 🚀 Projekt: Proxmox VE Integration & Ubuntu Server Hardening

**Durchgeführt von:** Mesut Sirtikara
**Datum:** 30.12.2025 – 02.01.2026

---

## 1. Projektziel
Das Ziel dieses Projekts war der Aufbau einer stabilen Virtualisierungsumgebung. Dabei wurde ein MacBook Pro als Proxmox-Node konfiguriert, um darauf eine gehärtete Ubuntu Server VM zu betreiben.

## 2. Infrastruktur & Hardware
Die Umgebung nutzt eine direkte Netzwerkverbindung zwischen Host und Admin-Client, um Latenzen zu minimieren:

* **Server (Host):** MacBook Pro 2017 – Bare-Metal Proxmox VE 8.x.
* **Management-Konsole:** Mac Mini M4 – Administration via Web-GUI & SSH.
* **Netzwerk:** Aktuell über direkte Bridge-Konfiguration (Vermeidung von fehlerhaften Hubs).

<div align="center">
  <img src="../screenshots/macbook_server.jpg" width="80%" alt="MacBook Pro Proxmox Host">
  <p><i>Abbildung 1: Das MacBook Pro 2017 im aktiven Server-Betrieb.</i></p>
</div>

---

## 3. Netzwerk-Optimierung (Troubleshooting)
Ein kritischer Punkt war die Stabilität der Netzwerkbrücke (`vmbr0`).

* **Problem:** Instabile Verbindung bei Nutzung alter Netzwerk-Hardware (Hubs).
* **Lösung:** Umgehung der fehlerhaften Hardware durch direkte Port-Zuweisung und Optimierung der Proxmox-Netzwerkkonfiguration. 
* **Zukunft:** Einbau eines dedizierten Managed Gigabit-Switches zur besseren VLAN-Trennung geplant.

<div align="center">
  <img src="../screenshots/network_config.jpg" width="80%" alt="Proxmox Network Config">
</div>

---

## 4. SSH-Hardening & Sicherheit
Die Sicherheit steht im Fokus der Administration:

* **Key-only Login:** Deaktivierung der Passwort-Authentifizierung für den SSH-Zugriff.
* **Root-Security:** Sperrung des Root-Logins zur Reduzierung der Angriffsfläche.
* **Ressourcen-Check:** Kontinuierliches Monitoring der CPU-Last zur Erkennung von Anomalien.

<div align="center">
  <img src="../screenshots/cpu_monitoring.jpg" width="80%" alt="CPU Monitoring Dashboard">
</div>

---

## 5. Fazit
Trotz hardwareseitiger Einschränkungen (kein Switch) wurde eine performante Umgebung geschaffen. Die VM ist nun durch SSH-Keys abgesichert ve bereit für den produktiven Testeinsatz.
