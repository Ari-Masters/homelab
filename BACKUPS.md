# DGS Homelab - Disaster Recovery Plan

**Frequency:** Weekly (Manual Execution) + Major Changes  
**Media:** Offline NVMe SSD via USB-C  
**Air-Gap Policy:** The drive must be physically disconnected after backup and stored separately.

---

## 1. Vaultwarden (Password Manager)

### Backup Procedure (Offline Drive)
1. Physically connect the NVMe backup drive to the DGS server.
2. Ensure the drive is mounted (update `/path/to/backup_drive/` as needed).
3. Execute the snapshot and sync:

```bash
# Create a safe SQLite snapshot before rsync to prevent corruption
docker exec vaultwarden sqlite3 /data/db.sqlite3 ".backup '/data/db_manual_snap.sqlite3'"

# Sync to offline media
rsync -avz --delete /srv/storage/vaultwarden/ /path/to/backup_drive/vaultwarden/
```

### Interim Sync (LGS - No Air Gap)
> [!WARNING]
> **LACK OF AIR GAP:** This procedure utilizes an always-online device (LGS). It is vulnerable to network-based threats (ransomware). Protection is for hardware failure ONLY.

```bash
rsync -avz --delete -e ssh /srv/storage/vaultwarden/ joe@100.118.178.62:C:/Users/Joe/backups/vaultwarden/
```

---

## 2. WeylandTavern (Blake)

### Backup Procedure (Offline Drive)
1. Physically connect the NVMe backup drive.
2. Execute the sync command:

```bash
rsync -avz --delete /srv/storage/weylandtavern/ /path/to/backup_drive/weylandtavern/
```

### Interim Sync (LGS)
```bash
rsync -avz --delete -e ssh /srv/storage/weylandtavern/ joe@100.118.178.62:C:/Users/Joe/backups/weylandtavern/
```

---

## 3. Environment & Configs
*Note: Do not commit raw .env files containing passwords to GitHub.*

- **Docker Configs:** Stored in `/srv/storage/docker-configs/`
- **Secrets:** Keep a physical "Break Glass" copy of your Master Password and Encryption Keys.

---

## 🚨 Recovery Procedure (Total Loss Scenario)

1. **Hardware:** Provision a new Linux host with Docker & Docker-Compose installed.
2. **Mount:** Connect the NVMe Backup Drive.
3. **Restore Vaultwarden:**
   ```bash
   rsync -avz /path/to/backup_drive/vaultwarden/ /srv/storage/vaultwarden/
   ```
4. **Restore WeylandTavern:**
   ```bash
   rsync -avz /path/to/backup_drive/weylandtavern/ /srv/storage/weylandtavern/
   ```
5. **Permissions:** Ensure proper ownership (`chown -R $USER:$USER /srv/storage/`).
6. **Launch:** Run `docker-compose up -d` for each service.
