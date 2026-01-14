# Projektname: Proxmox VE – Installation, Netzwerk-Fehleranalyse, Ubuntu-Server-VM & Hardening

**Projektart:** Systemintegration / Server- & Netzwerkadministration  
**Durchgeführt von:** Mesut Sirtikara  
**Datum:** 30.12.2025 – 02.01.2026  
**Ausbildungsberuf:** Fachinformatiker für Systemintegration (Umschulung)  
**Projektumfeld:** Virtualisierungs- und Serverumgebung (Lab / Heimnetz)

---

## 1. Projektziel
Ziel des Projekts war der Aufbau einer stabilen Virtualisierungsumgebung mit Proxmox VE sowie die Bereitstellung eines Ubuntu Server als virtuelle Maschine inklusive sicherer Administration und Wiederherstellungsstrategie.
Der Ubuntu-Server sollte:
* stabil im LAN erreichbar sein (Web/SSH),
* per SSH administrierbar sein,
* sicher gehärtet werden (Key-only SSH, Root-Login deaktiviert, Firewall),
* durch einen Snapshot als „sauberer Referenzstand“ abgesichert werden.

## 2. Ist- / Soll-Zustand
### 2.1 Ist-Zustand
* Keine vorhandene Virtualisierungsplattform
* Kein zentraler Server im Netzwerk
* Keine sichere Remote-Administration
* Netzwerkumgebung mit instabiler Hardware (Hub)
* Keine Wiederherstellungsstrategie

### 2.2 Soll-Zustand
* Proxmox VE als stabile Virtualisierungsplattform
* Ubuntu Server als virtuelle Maschine
* Sichere Administration per SSH (Key-only)
* Firewall zur Minimierung der Angriffsfläche
* Snapshot als Wiederherstellungspunkt
* Stabile Netzwerkkommunikation im LAN

## 3. Projektumgebung
### 3.1 Server / Plattform
* **Hypervisor:** Proxmox VE (Webverwaltung über Port 8006)
* **Storage:** local (ISO / Backups), local-lvm (VM-Disks, LVM-thin)
* **Netzwerk:** Bridge vmbr0 (VM im gleichen LAN wie Clients)

### 3.2 Client-Hardware (Administration & Tests)
* **MacBook (Intel, 2017):** Hauptgerät für Administration (WebGUI, Konsole, SSH)
* **Mac mini (Apple Silicon M4):** Zweites Testgerät zur Validierung der geräteunabhängigen Stabilität.

## 4. Projektdurchführung
### 4.2 Netzwerk-Fehleranalyse (zentrales Projektproblem)
* **Symptome:** Proxmox und VM zeitweise nicht erreichbar.
* **Analyse:** Ping-Tests und Porttests (nc -zv <IP> 8006).
* **Ursache:** Inkompatibler Hub.
* **Lösung:** Austausch des Netzwerkgeräts.
* **Ergebnis:** Netzwerkverbindung stabil.

### 4.6 SSH-Hardening
* **Ziel:** Nur SSH-Zugriff per Public Key.
* **Lösung:** Hardening über `/etc/ssh/sshd_config.d/`. Key-only Login erzwungen, Root-Login deaktiviert.
* **Ergebnis:** SSH nur noch per Key, Passwortlogin deaktiviert.

### 4.7 Firewall (UFW)
* UFW aktiv, SSH (Port 22) erlaubt, Rest Deny.

### 4.8 Snapshot
* Name: `post-install-secured` erfolgreich erstellt.

## 5. Probleme & Lösungen (Übersicht)
* **Netzwerk instabil:** Inkompatibler Hub -> Austausch.
* **Copy/Paste Problem:** noVNC Limitierung -> Umstieg auf SSH.
* **SSH Passwort-Abfrage:** Override Defaults -> sshd -T Hardening.

## 6. Testplan
* **Ping:** ping <IP> -> Antwort [OK]
* **Porttest:** nc -zv <IP> 8006 -> succeeded [OK]
* **Firewall:** ufw status -> aktiv [OK]

## 7. Fazit
Das Projekt zeigte, dass reale Systemintegrationsprojekte häufig an Netzwerk- und Hardware-Kompatibilität scheitern. Durch strukturierte Fehleranalyse konnte eine stabile, sichere Serverumgebung aufgebaut werden.
