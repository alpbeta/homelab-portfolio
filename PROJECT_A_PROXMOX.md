<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Proxmox Dokumentation - Mesut Sirtikara</title>
    <link rel="stylesheet" href="../assets/css/style.css">
    <style>
        .doc-body { background: #161b22; padding: 40px; border-radius: 12px; border: 1px solid #30363d; color: #c9d1d9; line-height: 1.7; }
        .section-header { color: #58a6ff; border-bottom: 1px solid #30363d; padding-bottom: 10px; margin-top: 30px; }
        .sub-header { color: #79c0ff; margin-top: 20px; font-weight: bold; }
        pre { background: #0d1117; padding: 15px; border-radius: 8px; border: 1px solid #30363d; overflow-x: auto; color: #e6edf3; }
        .project-meta { background: #0d1117; padding: 20px; border-radius: 8px; margin-bottom: 30px; border: 1px solid #30363d; }
        .highlight-red { color: #ff7b72; font-weight: bold; }
        .highlight-green { color: #3fb950; font-weight: bold; }
    </style>
</head>
<body>
    <div class="container">
        <a href="../index.html" class="backlink">← Zurück zur Übersicht</a>
        
        <div class="doc-body">
            <div class="project-meta">
                <p><strong>Projektname:</strong> Proxmox VE – Installation, Netzwerk-Fehleranalyse, Ubuntu-Server-VM & Hardening</p>
                <p><strong>Projektart:</strong> Systemintegration / Server- & Netzwerkadministration</p>
                <p><strong>Durchgeführt von:</strong> Mesut Sirtikara</p>
                <p><strong>Datum:</strong> 30.12.2025 – 02.01.2026</p>
                <p><strong>Ausbildungsberuf:</strong> Fachinformatiker für Systemintegration (Umschulung)</p>
                <p><strong>Projektumfeld:</strong> Virtualisierungs- und Serverumgebung (Lab / Heimnetz)</p>
            </div>

            <h2 class="section-header">1. Projektziel</h2>
            <p>Ziel des Projekts war der Aufbau einer stabilen Virtualisierungsumgebung mit Proxmox VE sowie die Bereitstellung eines Ubuntu Server als virtuelle Maschine inklusive sicherer Administration und Wiederherstellungsstrategie.</p>
            <p>Der Ubuntu-Server sollte:</p>
            <ul>
                <li>stabil im LAN erreichbar sein (Web/SSH),</li>
                <li>per SSH administrierbar sein,</li>
                <li>sicher gehärtet werden (Key-only SSH, Root-Login deaktiviert, Firewall),</li>
                <li>durch einen Snapshot als „sauberer Referenzstand“ abgesichert werden.</li>
            </ul>

            <h2 class="section-header">2. Ist- / Soll-Zustand</h2>
            <h3 class="sub-header">2.1 Ist-Zustand</h3>
            <ul>
                <li>Keine vorhandene Virtualisierungsplattform</li>
                <li>Kein zentraler Server im Netzwerk</li>
                <li>Keine sichere Remote-Administration</li>
                <li>Netzwerkumgebung mit instabiler Hardware (Hub)</li>
                <li>Keine Wiederherstellungsstrategie</li>
            </ul>
            <h3 class="sub-header">2.2 Soll-Zustand</h3>
            <ul>
                <li>Proxmox VE als stabile Virtualisierungsplattform</li>
                <li>Ubuntu Server als virtuelle Maschine</li>
                <li>Sichere Administration per SSH (Key-only)</li>
                <li>Firewall zur Minimierung der Angriffsfläche</li>
                <li>Snapshot als Wiederherstellungspunkt</li>
                <li>Stabile Netzwerkkommunikation im LAN</li>
            </ul>

            <h2 class="section-header">3. Projektumgebung</h2>
            <h3 class="sub-header">3.1 Server / Plattform</h3>
            <ul>
                <li><strong>Hypervisor:</strong> Proxmox VE (Webverwaltung über Port 8006)</li>
                <li><strong>Storage:</strong> local (ISO / Backups), local-lvm (VM-Disks, LVM-thin)</li>
                <li><strong>Netzwerk:</strong> Bridge vmbr0 (VM im gleichen LAN wie Clients)</li>
            </ul>
            <h3 class="sub-header">3.2 Client-Hardware (Administration & Tests)</h3>
            <p><strong>MacBook (Intel, 2017):</strong> Hauptgerät für Proxmox WebGUI, SSH-Zugriff ve Konfiguration.</p>
            <p><strong>Mac mini (Apple Silicon M4):</strong> Zweites Testgerät für Erreichbarkeits-Tests (Ping / Port) und Validierung.</p>
            <p><em>➡️ Durch zwei unterschiedliche Clients konnte sichergestellt werden, dass die Konfiguration geräteunabhängig und stabil funktioniert.</em></p>

            <h2 class="section-header">4. Projektdurchführung</h2>
            <h3 class="sub-header">4.1 Zugriff auf Proxmox Webinterface</h3>
            <p>Zugriff über: <code>https://&lt;Proxmox-IP&gt;:8006</code>. Browserwarnung („nicht privat“) aufgrund self-signed Zertifikat akzeptiert. WebGUI erreichbar.</p>

            <h3 class="sub-header">4.2 Netzwerk-Fehleranalyse (zentrales Projektproblem)</h3>
            <p><span class="highlight-red">Symptome:</span> Proxmox und VM zeitweise erreichbar, zeitweise nicht. Inkonsistenter Zugriff via WebGUI und SSH.</p>
            <p><strong>Analyse:</strong> Ping-Tests zur Erreichbarkeit, Porttests (nc -zv), Vergleichstests mit MacBook und Mac mini.</p>
            <p><strong>Ursache:</strong> Ein eingesetztes Netzwerkgerät (Hub) erwies sich als inkompatibel bzw. instabil im Zusammenspiel mit Linux und Bridged Networking.</p>
            <p><strong>Lösung:</strong> Austausch des Netzwerkgeräts. <span class="highlight-green">Ergebnis:</span> Netzwerkverbindung stabil ve reproduzierbar.</p>

            <h3 class="sub-header">4.3 ISO-Upload & VM-Erstellung</h3>
            <p>Ubuntu Server LTS hochgeladen. VM-Konfiguration: BIOS: OVMF (UEFI), Machine: q35, Disk: SCSI auf local-lvm (≈ 32 GB), Netzwerk: VirtIO, Bridge vmbr0.</p>

            <h3 class="sub-header">4.6 SSH-Hardening</h3>
            <p>Hardening über <code>/etc/ssh/sshd_config.d/</code>. Key-only Login erzwungen, Root-Login deaktiviert.</p>
            <pre># Diagnose mit:
sshd -T</pre>

            <h3 class="sub-header">4.8 Snapshot (Wiederherstellungspunkt)</h3>
            <p>Snapshot erstellt: <code>post-install-secured</code> (inkl. RAM / State). <span class="highlight-green">TASK OK.</span></p>

            <h2 class="section-header">5. Probleme & Lösungen (Übersicht)</h2>
            <ul>
                <li><strong>Netzwerk instabil:</strong> Inkompatibler Hub -> Austausch des Geräts</li>
                <li><strong>Copy/Paste nicht möglich:</strong> noVNC Limitierung -> SSH Terminal</li>
                <li><strong>SSH fragt Passwort:</strong> Override Defaults -> Hardening via sshd_config.d</li>
            </ul>

            <h2 class="section-header">6. Testplan</h2>
            <pre>
Ping: ping <IP> -> Antwort
Porttest: nc -zv <IP> 8006 -> succeeded
SSH: ssh user@IP -> Login erfolgreich
Firewall: ufw status -> aktiv</pre>

            <h2 class="section-header">7. Fazit</h2>
            <p>Das Projekt zeigte, dass reale Systemintegrationsprojekte häufig an Hardware-Kompatibilität scheitern. Durch strukturierte Fehleranalyse konnte eine stabile, sichere und wartbare Serverumgebung aufgebaut werden.</p>
        </div>
    </div>
</body>
</html>
