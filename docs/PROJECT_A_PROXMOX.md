# Projekt A – Proxmox VE: Installation, Netzwerk-Fehleranalyse, Ubuntu-Server-VM & Hardening

**Projektart:** Systemintegration / Server- & Netzwerkadministration  
**Durchgeführt von:** Mesut Sirtikara  
**Zeitraum:** 30.12.2025 – 02.01.2026  
**Ausbildungsberuf:** Fachinformatiker für Systemintegration (Umschulung)  
**Projektumfeld:** Virtualisierungs- und Serverumgebung (Lab / Heimnetz)

---

## 1. Projektziel

Ziel des Projekts war der Aufbau einer stabilen Virtualisierungsumgebung mit **Proxmox VE** sowie die Bereitstellung eines **Ubuntu Server** als virtuelle Maschine inklusive sicherer Administration und Wiederherstellungsstrategie.

Der Ubuntu-Server sollte:

- stabil im LAN erreichbar sein (Web/SSH)
- per SSH administrierbar sein
- sicher gehärtet werden (Key-only SSH, Root-Login deaktiviert, Firewall)
- durch einen Snapshot als „sauberer Referenzstand“ abgesichert werden

---

## 2. Ist- / Soll-Zustand

### 2.1 Ist-Zustand
- Keine vorhandene Virtualisierungsplattform
- Kein zentraler Server im Netzwerk
- Keine sichere Remote-Administration
- Netzwerkumgebung mit instabiler Hardware (Hub)
- Keine Wiederherstellungsstrategie

### 2.2 Soll-Zustand
- Proxmox VE als stabile Virtualisierungsplattform
- Ubuntu Server als virtuelle Maschine
- Sichere Administration per SSH (Key-only)
- Firewall zur Minimierung der Angriffsfläche
- Snapshot als Wiederherstellungspunkt
- Stabile Netzwerkkommunikation im LAN

---

## 3. Projektumgebung

### 3.1 Server / Plattform
- **Hypervisor:** Proxmox VE (Webverwaltung über Port **8006**)
- **Storage:**
  - `local` (ISO / Backups)
  - `local-lvm` (VM-Disks, LVM-thin)
- **Netzwerk:** Bridge **vmbr0** (VM im gleichen LAN wie Clients)

### 3.2 Client-Hardware (Administration & Tests)
- **MacBook (Intel, 2017)**  
  Proxmox WebGUI, VM-Konsole, SSH-Zugriff, Konfiguration
- **Mac mini (Apple Silicon M4)**  
  Erreichbarkeits-Tests (Ping/Port), Zugriff auf Proxmox, Validierung über zweiten Client

➡️ Durch zwei unterschiedliche Clients konnte sichergestellt werden, dass die Konfiguration geräteunabhängig und stabil funktioniert.

---

## 4. Projektdurchführung

### 4.1 Zugriff auf Proxmox Webinterface
- Zugriff über: `https://<Proxmox-IP>:8006`
- Browserwarnung („nicht privat“) aufgrund self-signed Zertifikat (typisch in Test- und Lab-Umgebungen)

**Ergebnis:** WebGUI erreichbar, Administrationszugang funktioniert.

---

### 4.2 Netzwerk-Fehleranalyse (zentrales Projektproblem)

**Symptome**
- Proxmox und VM zeitweise erreichbar, zeitweise nicht
- Unterschiedliches Verhalten je nach Client
- Inkonsistenter Zugriff via WebGUI und SSH

**Analyse**
- Ping-Tests zur Erreichbarkeit
- Porttests (z. B. `nc -zv <IP> 8006`)
- Browser- und Zertifikatsthemen ausgeschlossen
- Vergleichstests mit MacBook und Mac mini

**Ursache**  
Ein eingesetztes Netzwerkgerät (Hub) erwies sich als inkompatibel bzw. instabil im Zusammenspiel mit Linux und Bridged Networking.

**Lösung**
- Austausch des Netzwerkgeräts
- Erneute Tests: Ping, Port 8006, WebGUI-Zugriff von beiden Clients

**Ergebnis:** Netzwerkverbindung stabil und reproduzierbar.

---

### 4.3 ISO-Upload & VM-Erstellung
- ISO: Ubuntu Server LTS in `local` hochgeladen
- VM-Konfiguration:
  - BIOS: OVMF (UEFI)
  - Machine: q35
  - Disk: SCSI auf `local-lvm` (≈ 32 GB)
  - Netzwerk: VirtIO, Bridge `vmbr0`
  - CPU/RAM: 2 Cores, 2–4 GB RAM

**Ergebnis:** VM erfolgreich erstellt, Installer startet.

---

### 4.4 Ubuntu-Installation
- Guided Storage / LVM
- Benutzer `mesut` angelegt
- OpenSSH Server installiert

**Problem:** „Please remove the installation medium …“  
**Ursache:** ISO noch im virtuellen Laufwerk eingebunden  
**Lösung:** ISO entfernt → Neustart  
**Ergebnis:** Ubuntu bootet korrekt von Disk.

---

### 4.5 Administration & Copy/Paste-Problem
**Beobachtung**
- Einschränkungen bei Copy/Paste in noVNC-Konsole
- Tastaturlayout-Probleme (z. B. `@`)

**Lösung:** Umstieg auf SSH über macOS Terminal  
**Ergebnis:** Zuverlässige und professionelle Serveradministration.

---

### 4.6 SSH-Hardening
**Ziel:** Nur SSH-Zugriff per Public Key, kein Passwortlogin.  
**Problem:** SSH-Einstellungen wurden teilweise überschrieben.

**Analyse:** Diagnose mit `sshd -T`

**Lösung:** Hardening über `/etc/ssh/sshd_config.d/`
- Key-only Login erzwungen
- Root-Login deaktiviert
- Tests mit paralleler SSH-Session

**Ergebnis:** SSH nur noch per Key, Passwortlogin deaktiviert.

---

### 4.7 Firewall (UFW)
- UFW installiert und aktiviert
- Default:
  - Incoming: deny
  - Outgoing: allow
- SSH (Port 22) explizit erlaubt

**Ergebnis:** Firewall aktiv, sichere Erreichbarkeit gewährleistet.

---

### 4.8 Snapshot (Wiederherstellungspunkt)
- Snapshot erstellt: `post-install-secured` (inkl. RAM/State)

**Ergebnis:** Snapshot erfolgreich (TASK OK).

---

## 5. Probleme & Lösungen (Übersicht)
- Instabile LAN-Erreichbarkeit → Hub inkompatibel/instabil → Hardwaretausch → stabil
- noVNC Copy/Paste & Keyboard → Administration via SSH
- SSH Settings überschrieben → `sshd -T` + `sshd_config.d/` → stabile Hardening-Konfiguration
- Installation Medium Meldung → ISO entfernen → Boot OK

---

## 6. Testplan

### 6.1 Funktionstests (Beispiele)
- WebGUI Zugriff: `https://<Proxmox-IP>:8006`
- Erreichbarkeit: `ping <IP>`
- Porttest: `nc -zv <IP> 8006`
- SSH Login (Key-only): `ssh <user>@<ip>`
- UFW: `ufw status`
- Snapshot vorhanden: Proxmox GUI / Task OK

### 6.2 Screenshot-Liste (für PDF)
- Proxmox Login-WebGUI
- Netzwerk-Bridge `vmbr0`
- Ubuntu VM Übersicht
- SSH Login Terminal
- `sshd -T` Ausgabe
- `ufw status`
- Snapshot „post-install-secured“

---

## 7. Fazit
Das Projekt zeigte, dass reale Systemintegrationsprojekte häufig an Netzwerk- und Hardware-Kompatibilität scheitern und nicht ausschließlich an Software. Durch strukturierte Fehleranalyse und konsequente Sicherheitsmaßnahmen konnte eine stabile, sichere und wartbare Serverumgebung aufgebaut werden.
MD
