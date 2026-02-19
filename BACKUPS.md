# DGS Homelab - Disaster Recovery Plan

**Frequency:** Weekly (Manual Execution)
**Media:** Offline NVMe SSD via USB-C
**Air-Gap Policy:** The drive must be physically disconnected after backup and stored separately.

---

*## 1. Vaultwarden (Password Manager)*

*### Backup Procedure*

1.  Physically connect the NVMe backup drive to the DGS server.
2.  Execute the following `rsync` *command, updating* `/path/to/backup_drive/` *to the correct mount point.*


rsync -avz --delete /srv/storage/vaultwarden/ /path/to/backup_drive/vaultwarden/


---

*## 2. WeylandTavern (Blake)*

*### Backup Procedure*

*1.  Physically connect the NVMe backup drive.
2.  Execute the following command:*



rsync -avz --delete /srv/storage/weylandtavern/ /path/to/backup_drive/weylandtavern/



# DGS Homelab - Disaster Recovery Plan (Interim)

**Frequency:** Weekly (Manual Execution)

**WARNING: LACK OF AIR GAP**
*This procedure utilizes an always-online device (LGS) as the backup target. This backup is NOT air-gapped and is vulnerable to network-based threats (e.g., ransomware) that could compromise both the live server and the backup simultaneously. This plan provides protection against hardware failure ONLY.*

---

*## 1. Vaultwarden (Password Manager)*

*### Backup Procedure*

1.  Ensure the LGS is online and accessible via its Tailscale IP address.
2.  Execute the following `rsync` *command, replacing* `<LGS_TAILSCALE_IP>` *with the correct address.*


rsync -avz --delete -e ssh /srv/storage/vaultwarden/ joe@<LGS_TAILSCALE_IP>:/path/on/lgs/backups/vaultwarden/


---

*## 2. WeylandTavern (Blake)*

*### Backup Procedure*

1.  Ensure the LGS is online and accessible.
2.  Execute the following `rsync` *command.*



rsync -avz --delete -e ssh /srv/storage/weylandtavern/ joe@<LGS_TAILSCALE_IP>:/path/on/lgs/backups/weylandtavern/



