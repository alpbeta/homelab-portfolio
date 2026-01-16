# scripts — Operational Safety (Ops)

Dieser Ordner enthält kleine Hilfs-Skripte für den **sicheren Betrieb (Operational Safety)** meines Homelab-Setups.

## Ops-Prinzipien
- **Erst prüfen, dann ändern:** z. B. `nginx -t` vor einem Reload, optional Dry-Run, wo möglich.
- **Rollback/Backup:** vor Änderungen werden Konfigurationen gesichert.
- **Nachvollziehbarkeit:** kurze, klare Skripte mit sinnvollen Ausgaben/Logs.

## Vorhandene Skripte (Beispiele)
- `apply-nginx.sh` — NGINX-Config testen + reload (minimiert Ausfallzeiten)
- `backup-*.sh` — Snapshot-/rsync-basierte Backups (folgt im Backup-Projekt)
- `healthcheck-*.sh` — Checks für HTTP-Endpoints / Container-Health

> Hinweis: Skript-Namen und Umfang werden mit den nächsten Projekten erweitert und angepasst.
