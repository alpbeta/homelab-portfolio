# Projekt C: Security Hardening – SSH Absicherung mit Fail2ban

## 1. Projektziel
Ziel dieses Projekts war die Härtung (Hardening) eines Ubuntu Servers gegen unbefugte Zugriffsversuche, insbesondere SSH-Brute-Force-Angriffe. Dazu wurde Fail2ban eingesetzt, um Angriffe anhand von Logdateien automatisch zu erkennen und abzuwehren.

## 2. Begriffserklärung: Hardening
Hardening bezeichnet Maßnahmen zur Erhöhung der Sicherheit eines Systems, ohne dessen Funktionalität einzuschränken. Dabei werden unnötige Angriffsflächen reduziert und automatisierte Schutzmechanismen eingesetzt.

## 3. Ausgangssituation
- Ubuntu Server läuft als VM auf Proxmox VE
- SSH-Zugriff ist erforderlich für Administration
- SSH ist bereits gehärtet (Key-only Login, Root-Login deaktiviert)
- Firewall (UFW) ist aktiv
- Restrisiko: Öffentlich erreichbarer SSH kann Ziel automatisierter Brute-Force-Angriffe werden

## 4. Projektumgebung
- Hypervisor: Proxmox VE
- VM: Ubuntu Server (VM-ID 100)
- Zugriff: SSH
- Firewall: UFW
- Sicherheitsdienst: Fail2ban

## 5. Durchführung
### 5.1 Installation von Fail2ban
Fail2ban wurde über das Paketmanagement installiert und als Systemdienst gestartet.

### 5.2 Konfiguration eines SSH-Jails
Wichtige Parameter:
- maxretry = 3
- findtime = 10m
- bantime = 1h

### 5.3 Log-Analyse
Analyse über /var/log/auth.log (Invalid user / preauth etc.)

### 5.4 Test der Schutzfunktion
Mehrere falsche SSH-Logins → Fail2ban sperrt IP automatisch. Nachweis über fail2ban-client status sshd.

## 6. Ergebnis
- SSH wird kontinuierlich überwacht
- Brute-Force wird erkannt
- IPs werden automatisch gesperrt
- Schutz läuft automatisiert

## 7. Fazit
Fail2ban erhöht die Sicherheit signifikant und zeigt die Praxisrelevanz von Log-Analyse und Automatisierung im Hardening.
