# Projekt B – Docker & Docker Compose: Service-Bereitstellung auf Ubuntu Server (Proxmox VM)

**Projektart:** Systemintegration / Containerisierung / Serverdienste  
**Durchgeführt von:** Mesut Sirtikara  
**Zeitraum:** 03.01.2026 – 05.01.2026  
**Ausbildungsberuf:** Fachinformatiker für Systemintegration (Umschulung)  
**Projektumfeld:** Virtualisierte Serverumgebung (Proxmox VE, Lab / Heimnetz)

---

## 1. Ziel

Ziel des Projekts war die Bereitstellung einer Container-Laufzeitumgebung auf einem Ubuntu Server (VM unter Proxmox VE), um Dienste reproduzierbar per Docker/Compose ausrollen zu können. Zusätzlich wurden Netzwerkzugriffe über eine Firewall kontrolliert und ein Snapshot als stabiler Wiederherstellungspunkt erstellt.

**Soll-Zustand**
- Docker Engine installiert und funktionsfähig
- Docker Compose verfügbar
- Benutzer kann Docker ohne Root-Rechte nutzen
- Dienst (Nginx) läuft containerisiert und ist aus dem LAN erreichbar
- UFW aktiv mit minimal nötigen Freigaben (SSH + Web-Port)
- Proxmox Snapshot „docker-ready“ vorhanden

---

## 2. Ausgangssituation
- Proxmox VE ist bereits eingerichtet und stabil erreichbar.
- Ubuntu Server läuft als VM (ID 100) und wird per SSH administriert.
- Grundabsicherung (SSH/Netzwerk) war vorhanden, nun sollte die Umgebung für containerisierte Workloads erweitert werden.

---

## 3. Projektumgebung
- **Hypervisor:** Proxmox VE
- **VM:** Ubuntu Server, VM-ID 100
- **Storage:** local-lvm (LVM-thin)
- **Clients (Admin):** MacBook 2017 (Intel) & Mac mini M4
- **Docker Engine:** 29.1.3
- **Docker Compose:** v5.0.0
- **Firewall:** UFW aktiv, Ports 22/tcp und 8080/tcp freigegeben (IPv4/IPv6)

---

## 4. Durchführung

### 4.1 Installation von Docker Engine & Docker Compose
Docker wurde über das offizielle Repository installiert (GPG-Key, apt-Repo, Installation der Engine und Plugins). Nach der Installation wurden Versions-Checks durchgeführt.

**Nachweis (Ist-Stand)**
- Docker version 29.1.3
- Docker Compose version v5.0.0

### 4.2 Fehleranalyse: „permission denied … docker.sock“
**Problem:** Beim ersten Test mit `docker run --rm hello-world` trat ein Berechtigungsfehler auf (Zugriff auf `/var/run/docker.sock` nicht erlaubt).

**Ursache:** Der Benutzer `mesut` war nicht Mitglied der Gruppe `docker` (fehlende Socket-Berechtigung).

**Lösung**
- Benutzer zur Gruppe docker hinzugefügt (`usermod -aG docker …`)
- Neue SSH-Session gestartet, damit Gruppenmitgliedschaft aktiv wird

**Ergebnis:** Docker-Kommandos funktionieren ohne `sudo`.

### 4.3 Service-Bereitstellung mit Docker Compose (Nginx)
Als Proof-of-Concept wurde ein Webdienst (Nginx) via Docker Compose bereitgestellt:
- Projektordner erstellt (z. B. `~/compose-nginx`)
- `compose.yml` angelegt
- Dienst mit `docker compose up -d` gestartet

**Ziel:** Wiederholbares Deployment über Compose statt Einzelkommandos.

### 4.4 Firewall-Konfiguration (UFW)
Zur Minimierung der Angriffsfläche wurde UFW so konfiguriert, dass nur notwendige Ports offen sind.

**Freigegeben**
- SSH: 22/tcp
- Webdienst: 8080/tcp  
  (jeweils für IPv4 und IPv6)

**Nachweis (Auszug)**
- Status: active
- 22/tcp ALLOW Anywhere
- 8080/tcp ALLOW Anywhere
- 22/tcp (v6) ALLOW Anywhere (v6)
- 8080/tcp (v6) ALLOW Anywhere (v6)

### 4.5 Snapshot „docker-ready“ (Wiederherstellungspunkt)
Nach erfolgreicher Installation, Tests und Firewall-Konfiguration wurde ein Snapshot auf Proxmox erstellt, um einen stabilen Zustand jederzeit wiederherstellen zu können.

- Snapshot-Name: `docker-ready`

**Technischer Nachweis (Proxmox-Task)**
- `snap_vm-100-disk-1_docker-ready` erstellt
- `snap_vm-100-disk-0_docker-ready` erstellt (EFI-Disk)
- Ergebnis: TASK OK

---

## 5. Ergebnisse / Verifikation
- Docker und Compose installiert (29.1.3 / v5.0.0)
- Docker ohne Root nutzbar (Gruppe docker)
- Nginx per Compose bereitgestellt, LAN-Zugriff über Port 8080 möglich
- UFW aktiv, minimal notwendige Regeln gesetzt (22/8080, IPv4+IPv6)
- Snapshot docker-ready erfolgreich erstellt (TASK OK)

---

## 6. Fazit
Das Projekt erweitert die Ubuntu-VM um eine moderne Container-Plattform. Die Kombination aus Compose-Deployment, restriktiver Firewall und Snapshot-Strategie ermöglicht einen stabilen und reproduzierbaren Betrieb sowie schnelle Wiederherstellung bei Änderungen oder Fehlern.
