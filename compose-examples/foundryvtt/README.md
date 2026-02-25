# Stratus-Hermes Homelab - Foundry VTT Server

**Status:** Operational  
**Container Image:** [felddy/foundryvtt](https://github.com) (Community maintained)  
**Port:** 30000

---

## 🚀 Deployment Configuration

The server is deployed via Docker Compose. Key features include signal propagation (`init: true`) and extended shutdown windows to prevent world-file corruption.

```yaml
services:
  foundry:
    image: felddy/foundryvtt:release
    container_name: foundry
    hostname: foundry-server
    restart: unless-stopped
    init: true                        # Ensures proper signal propagation to child processes
    stop_grace_period: 30s            # Gives Foundry time to clean up before SIGKILL
    volumes:
      - /srv/storage/foundry/data:/data                        # Persistent data: worlds, modules, Config/
      - /srv/storage/foundry/install:/home/node/foundryvtt     # Persistent install: skips re-extract (WIP)
    ports:
      - "30000:30000"
    environment:
      CONTAINER_UID: 1000             # Run as host user to avoid permission issues
      CONTAINER_GID: 1000
      FOUNDRY_USERNAME: "your_foundry_email"
      FOUNDRY_PASSWORD: "your_foundry_password"
      FOUNDRY_ADMIN_KEY: "your_admin_password"
      CONTAINER_PRESERVE_CONFIG: "true"           # CRITICAL: prevents overwriting options.json
```

---

## 💾 Backup & Persistence

### 1. Data Directory
All worlds, modules, and system configurations are stored in `/srv/storage/foundry/data`. This directory **must** be included in the weekly rsync backup to the NVMe drive.

### 2. Persistent Install (WIP)
The volume mapping for `/home/node/foundryvtt` is intended to skip the extraction process on container restart. 
> [!NOTE]
> This is currently in a **Work-In-Progress** state and may not function as expected until the entrypoint permissions are fully aligned.

---

## 🚨 Disaster Recovery

1. **Restore Mounts:** Ensure `/srv/storage/foundry/` is populated from the backup media.
2. **Permissions:** Fix ownership if UID/GID differs on the new host:
   ```bash
   chown -R 1000:1000 /srv/storage/foundry/
   ```
3. **Spin Up:**
   ```bash
   docker-compose up -d
   ```

---

## ✅ Post-Setup Checklist
- [x] Verify login to the `/setup` page using `FOUNDRY_ADMIN_KEY`.
- [x] Check `/data/Config/options.json` after first boot to ensure custom settings (like `proxyPort`) persisted.
- [x] Confirm `/data` volume is populating with `worlds` and `modules` folders.
