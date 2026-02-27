# 🍺 Weyland Tavern: Service Node Config

Configuration for the `Weyland Tavern` (SillyTavern fork) application layer. This setup uses a Docker Wrapper to manage a bare-metal repository for persistence and automatic updates.

---

## 🚀 Key Features

- **Auto-Update on Boot:** Every container restart pulls the latest code from the `release` branch and patches dependencies.
- **Permission Sync:** Runs as `1000:1000` to prevent file ownership drift.

---

## 🛠 Preparation

1. **Create the storage path:**
   ```bash
   mkdir -p /Your/Path/Here/weylandtavern
   ```
2. **Clone the official release branch:**
   ```bash
   git clone https://github.com/Shirubaurufu/WeylandTavern -b release
   ```

---

## 📂 Docker Workspace Setup

Place the following files in `/Your/Path/Here/weylandtavern/`:

### 1. `docker-compose.yml`

```yaml
services:
  weylandtavern:
    build: .
    container_name: weylandtavern
    user: "1000:1000"
    restart: unless-stopped
    environment:
      - DOMAIN=stratus-hermes.your-tailnet.ts.net
      - NODE_ENV=production
    ports:
      - "8005:8000" # Host 8005 -> Container 8000
    volumes:
      - /Your/Path/Here/weylandtavern:/app
```

### 2. `Dockerfile`

```dockerfile
FROM node:22-bookworm-slim
RUN apt-get update && apt-get install -y git bash curl && rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
RUN chmod +x /usr/local/bin/docker-entrypoint.sh
USER 1000:1000
EXPOSE 8000
ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
```

---

## ⚡ Deployment & Maintenance

1. **Build and start:**

   ```bash
   cd Your/Path/Here/weylandtavern
   docker compose up -d --build
   ```

2. **Security Whitelist:**
   Ensure `Your/Path/Here/weylandtavern/SillyTavern/config.yaml` has `whitelistMode: false` (whitelist is broken at time of build) or includes `172.16.0.0/12` to allow the Docker bridge connection.

3. **Update Tavern:**
   Run `docker compose restart weylandtavern`.
