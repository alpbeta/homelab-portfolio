# Proxmox VE – Installation, Netzwerk-Fehleranalyse, Ubuntu-Server-VM & Hardening

**Projektart:** Systemintegration / Server- & Netzwerkadministration
**Durchgeführt von:** Mesut Sirtikara
**Datum:** 30.12.2025 – 02.01.2026
**Ausbildungsberuf:** Fachinformatiker für Systemintegration (Umschulung)
**Projektumfeld:** Virtualisierungs- und Serverumgebung (Lab / Heimnetz)

---

## 1. Projektziel
Ziel des Projekts war der Aufbau einer stabilen Virtualisierungsumgebung mit Proxmox VE sowie die Bereitstellung eines Ubuntu Server als virtuelle Maschine inklusive sicherer Administration und Wiederherstellungsstrategie.

**Der Ubuntu-Server sollte:**
- stabil im LAN erreichbar sein (Web/SSH),
- per SSH administrierbar sein,
- sicher gehärtet werden (Key-only SSH, Root-Login deaktiviert, Firewall),
- durch einen Snapshot als „sauberer Referenzstand“ abgesichert werden.

## 2. Ist- / Soll-Zustand
### 2.1 Ist-Zustand
- Keine vorhandene Virtualisierungsplattform
- Kein zentraler Server im Netzwerk
- Netzwerkumgebung mit instabiler Hardware (Hub)
- Keine Wiederherstellungsstrategie

### 2.2 Soll-Zustand
- Proxmox VE als stabile Virtualisierungsplattform
- Ubuntu Server als virtuelle Maschine
- Sichere Administration per SSH (Key-only)
- Snapshot als Wiederherstellungspunkt
- Stabile Netzwerkkommunikation im LAN

## 3. Projektumgebung
### 3.1 Server / Plattform
- **Hypervisor:** Proxmox VE (Webverwaltung über Port 8006)
- **Storage:** local (ISO / Backups), local-lvm (VM-Disks)
- **Netzwerk:** Bridge vmbr0

### 3.2 Client-Hardware
- **MacBook (Intel, 2017):** Hauptgerät für Administration.
- **Mac mini (Apple Silicon M4):** Test- und Administrationsgerät.
> Durch zwei unterschiedliche Clients konnte sichergestellt werden, dass die Konfiguration geräteunabhängig und stabil funktioniert.

## 4. Projektdurchführung

### 4.2 Netzwerk-Fehleranalyse (Zentrales Problem)
- **Symptome:** Proxmox und VM zeitweise erreichbar, zeitweise değil.
- **Ursache:** Ein eingesetztes Netzwerkgerät (Hub) erwies sich als inkompatibel bzw. instabil im Zusammenspiel mit Linux und Bridged Networking.
- **Lösung:** Austausch des Netzwerkgeräts.
- **Ergebnis:** Netzwerkverbindung stabil und reproduzierbar.

### 4.6 SSH-Hardening
Hardening über `/etc/ssh/sshd_config.d/`:
- Key-only Login erzwungen.
- Root-Login deaktiviert.

```bash
# Diagnose mit:
sshd -T
