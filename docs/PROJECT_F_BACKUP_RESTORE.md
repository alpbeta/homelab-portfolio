# Projekt F: Backup & Restore (in Vorbereitung)

## Ziel
Ein belastbares Backup-/Restore-Konzept für die Homelab-Infrastruktur:
- Automatisierte Backups von Proxmox VMs/CTs
- Backup-Ziel: externe SSD (lokal)
- Optional später: Offsite-Kopie (3-2-1 Prinzip)
- Regelmäßiger Restore-Test als Nachweis der Wiederherstellbarkeit

## Geplante Umgebung
- Proxmox VE Host
- Externe SSD (Backup Storage)
- Optional: Offsite (später)

## Umsetzung (Plan)
1. SSD anschließen, Device prüfen: `lsblk`, `blkid`
2. Filesystem + Mount: z. B. `/mnt/backup-ssd` (fstab)
3. Proxmox Storage hinzufügen (Directory Storage)
4. Backup Job anlegen (Schedule + Retention)
5. Restore-Test durchführen (Recovery Drill)
6. Dokumentation mit Screenshots + Logs (TASK OK)

## Nachweise (werden ergänzt)
- Storage-Setup (Screenshot)
- Backup Job + Schedule (Screenshot)
- Backup/Restore Tasks (TASK OK)
