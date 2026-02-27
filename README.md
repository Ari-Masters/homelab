# Stratus-Hermes Homelab Documentation

# ☁️ Project Stratus-Hermes: The Personal Cloud

**Status:** Initial Deployment Phase  
**Flagship Node:** Stratus-Hermes  
**Objective:** Complete digital sovereignty through the phased decommissioning of third-party cloud services in favor of a private, self-hosted ecosystem.

---

## 🌩 Homelab Atmospheric Taxonomy (HAT)

The **HAT** (or **ACT**) system classifies hardware based on its "altitude" (compute capability) and its "mythological identity" (functional role).

### 1. Cloud Classes (Capability Tier)
*   **Fog:** Ultra-local edge compute; minimal resources, single-purpose or disposable services.
*   **Stratus:** Persistent baseline infrastructure; multi-service foundational platform capabilities.
*   **Cumulus:** Burst or task-focused compute; nodes for specialized or intermittent workloads.
*   **Nimbus:** Heavy sustained compute; high-load processing and large datasets.
*   **Nebula:** Dense multi-system environments; clusters supporting complex operations.
*   **Molecular Cloud:** Foundational compute mass; massive capacity for core data gravity.

### 2. Mythological Identities (Functional Role)
*   **Hermes:** Coordination, routing, authentication, and inter-system communication.
*   **Aeolus:** Flow regulation; traffic shaping and network management.
*   **Atlas:** Load-bearing infrastructure; critical backbone services.
*   **Nyx:** Hidden persistence; storage, archival, and secrets management.
*   **Thor:** Defensive force; security monitoring and intrusion prevention.

---

## 🖥️ Node Registry: Stratus-Hermes

As a **Stratus-Hermes** class system, this node serves as the baseline infrastructure coordinator for the entire homelab. It handles service orchestration and foundational exchange.

### Active Services

| Service | Access Link | Host Port | Status |
| :--- | :--- | :--- | :--- |
| ![Foundry](https://img.shields.io) | `https://stratus-hermes.ts.net:30000` | `30000` | Game Server |
| ![Vaultwarden](https://img.shields.io) | `https://stratus-hermes.ts.net:8081` | `8081` | Passwords |
| ![Portainer](https://img.shields.io) | `https://stratus-hermes.ts.net:9443` | `9443` | Management |
| ![UniFi](https://img.shields.io) | `https://stratus-hermes.ts.net:8443` | `8443` | Networking |
| ![Minecraft](https://img.shields.io) | `https://stratus-hermes.ts.net:25565` | `25565` | PaperMC (Java) |
| ![Geyser](https://img.shields.io) | `https://stratus-hermes.ts.net:19132` | `19132` | Bedrock Bridge |


---

## 💾 Storage & Data Continuity

Data is partitioned by "Altitude" to ensure that critical baseline services (Stratus) remain isolated from experimental edge services (Fog).

*   **Primary Data Path:** ``/srv/storage/``
*   **Backup Strategy:** Weekly ``rsync`` to external NVMe media.
*   **Secrets:** Managed via ``Nyx``-class protocols (Vaultwarden) and local environment secrets.

---

## 🚨 Disaster Recovery

1. **Host Failure:** Re-provision hardware and map volumes from the most recent NVMe rsync.
2. **Permission Reset:** Use the following to align with the ``Stratus-Hermes`` user profile:
   ``bash
   chown -R 1000:1000 /srv/storage/
   ``
3. **Redeploy:** 
   ``bash
   docker-compose up -d
   ``
