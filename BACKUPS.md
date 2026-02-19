# DGS Homelab - Disaster Recovery Plan

**Frequency:** Weekly (Manual) + Major Changes  
**Media:** Offline NVMe SSD (USB-C)  
**Air-Gap Policy:** Disconnected & stored in backpack after sync.

## 🛠️ Preparation
```bash
export BKP="/path/to/backup_drive"
```

---

## 1. Vaultwarden (Password Manager)

### Backup Procedure (Offline Drive)
1. **Snap:** Create a consistent database snapshot.
   ```bash
   docker exec vaultwarden sqlite3 /data/db.sqlite3 ".backup '/data/db_manual_snap.sqlite3'"
   ```
2. **Sync:** 
   ```bash
   rsync -avz --delete /srv/storage/vaultwarden/ $BKP/vaultwarden/
   ```
3. **Verify:** `du -sh /srv/storage/vaultwarden/ $BKP/vaultwarden/`

### Interim Sync (LGS - Handheld)
```bash
rsync -avz --delete -e ssh /srv/storage/vaultwarden/ joe@100.118.178.62:C:/Users/Joe/backups/vaultwarden/
```

---

## 2. WeylandTavern

### Backup Procedure (Offline Drive)
```bash
rsync -avz --delete /srv/storage/weylandtavern/ $BKP/weylandtavern/
```

### Interim Sync (LGS - Handheld)
```bash
rsync -avz --delete -e ssh /srv/storage/weylandtavern/ joe@100.118.178.62:C:/Users/Joe/backups/weylandtavern/
```

---

## 3. Environment & Configs
*Note: Do not commit raw .env files to GitHub.*

- **Docker Configs:** Backup `/srv/storage/docker-configs/` to `$BKP/configs/`
- **Secrets:** Physical "Break Glass" copy of Master Password and 2FA Recovery Codes.

---

## 🚨 Recovery Procedure (Total Loss)

1. **Hardware:** Provision Linux host + Docker + Compose.
2. **Restore Data:**
   ```bash
   rsync -avz $BKP/vaultwarden/ /srv/storage/vaultwarden/
   rsync -avz $BKP/weylandtavern/ /srv/storage/weylandtavern/
   ```
3. **Permissions:** `sudo chown -R $USER:$USER /srv/storage/`
4. **Launch:** `docker-compose up -d`
