# Multi-Protocol Minecraft Infrastructure (Java & Bedrock)

A containerized, microservice-based Minecraft deployment demonstrating cross-protocol bridging, isolated container networking, and custom environment builds.

## 🏗️ Architecture
- **Backend (PaperMC):** A high-performance Java Minecraft server running on `itzg/minecraft-server`.
- **Edge Proxy (GeyserMC):** A custom-built **Standalone Proxy** that translates Bedrock (UDP) traffic to Java (TCP).
- **Identity Bridge (Floodgate):** Utilizes RSA public-key cryptography to allow Bedrock clients to authenticate with the Java backend without requiring a separate Java account.

## 🌐 Network Configuration
The deployment utilizes a custom Docker bridge network (`mc-net`) to ensure service discovery via container hostname and to isolate backend traffic.


| Service | Protocol | External Port | Internal Port | Role |
| :--- | :--- | :--- | :--- | :--- |
| **PaperMC** | TCP | 25565 | 25565 | Core Game Service |
| **Geyser** | UDP | 19132 | 19132 | Protocol Translation |

## 🛠️ Implementation Details (Portfolio Highlights)

- **Custom Docker Build:** I developed a specialized `Dockerfile` for the Geyser Proxy using **Eclipse Temurin Java 21 (JRE)** to ensure a lightweight, secure, and current runtime environment.
- **Microservice Decoupling:** By separating Geyser into its own container, the edge proxy remains available even during backend maintenance or PaperMC version updates.
- **Volume Persistence & Security:** 
  - Implemented host-path bind mounts for stateful data.
  - Managed UID/GID mismatches between host and container to ensure crash-consistency for the PaperMC remapping engine.
- **Encrypted Handshake:** Orchestrated the secure sharing of `key.pem` between the PaperMC plugin and the Standalone Proxy to enable seamless cross-platform play.

## 🚀 Deployment
1. Ensure `/srv/storage` and `/srv/docker` directories are owned by `UID:1000`.
2. Run `docker-compose up -d --build`.
3. Sync `key.pem` from the PaperMC volume to the Geyser config volume.
4. Update `config.yml` to point to the `papermc` container and restart.
