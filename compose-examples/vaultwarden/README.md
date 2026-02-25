# DGS Homelab - Vaultwarden Server

**Status:** Operational  
**Container Image:** [vaultwarden/server](https://github.com) (Bitwarden compatible)  
**Port:** 8081

---

## 🚀 Deployment Configuration

The server is deployed via Docker Compose. It provides a lightweight implementation of the Bitwarden API, suitable for low-resource environments.

```yaml
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    volumes:
      - /srv/storage/vaultwarden:/data
    ports:
      - '8081:80'
    environment:
      - ADMIN_TOKEN=!!!_GENERATE_A_NEW_TOKEN_AND_REPLACE_THIS_!!!
      - SIGNUPS_ALLOWED=true            # Set to false after you sign up!
      - DOMAIN=http://localhost:8081     # Update this if using a reverse proxy
```

---

## 💾 Backup & Persistence

### 1. Data Directory
All vault data, attachments, and the SQLite database are stored in `/srv/storage/vaultwarden`. 
> [!CAUTION]
> **Encryption:** Data is encrypted at rest by Vaultwarden, but the Master Password is required to decrypt. Do not lose it.

### 2. Admin Interface
Access the administrative backend at `http://<host-ip>:8081/admin`. Use the `ADMIN_TOKEN` defined in the environment variables to log in.

---

## 🚨 Security Hardening

1. **Disable Signups:** Once your primary account is created, set `SIGNUPS_ALLOWED=false` in the environment variables to prevent unauthorized users from creating accounts on your server.
2. **HTTPS Required:** Bitwarden clients (mobile/browser) require HTTPS to function. Use a reverse proxy (Nginx/Caddy) or a Cloudflare Tunnel for production use.
3. **Token Generation:** Use a strong random string for the `ADMIN_TOKEN`.

---

## ✅ Post-Setup Checklist
- [x] Create initial user account.
- [x] Set `SIGNUPS_ALLOWED=false` in `docker-compose.yml` and re-deploy.
- [x] Verify access to the `/admin` panel.
- [x] Confirm `db.sqlite3` exists in `/srv/storage/vaultwarden`.
