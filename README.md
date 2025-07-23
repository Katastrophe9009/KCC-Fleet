# KCC-Fleet
Personal home network setup.

# The KCC Fleet: A Home Lab Infrastructure Portfolio

This repository documents the architecture, services, and ongoing evolution of the Katastrophe Command Center (KCC) home lab. This project serves as a practical, hands-on learning environment for modern system administration, network engineering, and DevOps principles.

## I. Core Philosophy

The KCC Fleet is built on a foundation of three core principles:

    **Ownership & Control:** Prioritizing self-hosted, open-source solutions to maintain full control over personal data and digital infrastructure.

    **Security & Stability:** Architecting a resilient and secure network that protects internal services from external threats and isolates workloads for maximum stability.

    **Continuous Learning:** Using this lab as a sandbox to master real-world technologies, from bare-metal virtualization to containerization and automation.

---

## II. The Fleet Roster

My home network is architected as a fleet of specialized machines, each with a distinct role.

### 1. Flagship/Carrier: **The KCC (Katastrophe Command Center)**

    **Class:** Proxmox VE Bare-Metal Hypervisor

    **Hardware:** Custom-built PC in a Cooler Master HAF chassis with an AMD Ryzen 5 2600 CPU and 16GB RAM.

    **Role:** The primary workhorse of the fleet. The KCC runs all 24/7 background services and infrastructure in isolated LXC containers and VMs. Its duties include application hosting, media serving, storage management, and future AI/compute workloads.

#### Key Services Hosted on the KCC:

| Service               | Type     | ID  | IP Address      | Purpose                                     | Status      |
| :-------------------- | :------- | :-- | :-------------- | :------------------------------------------ | :---------- |
| **Jellyfin**            | `LXC`    | 100 | `10.0.0.243`    | Media Streaming & Hardware Transcoding    | `Active`    |
| **Pterodactyl Panel**   | `LXC`    | 101 | `10.0.0.33`     | Web UI for Game Server Management         | `Active`    |
| **Pterodactyl Wings**   | `LXC`    | 102 | `10.0.0.50`     | Game Server Hosting Daemon                | `Active`    |
| **MeTube**              | `LXC`    | 103 | `10.0.0.12`     | Video & Content Downloader              | `Active`    |
| **Prowlarr**            | `LXC`    | 104 | `10.0.0.248`    | Indexer Manager for the *Arr Stack       | `Active`    |
| **VSCode Server**       | `LXC`    | 105 | `10.0.0.67`     | Remote Development Environment            | `Active`    |
| **Windows 11**          | `VM`     | 106 | `10.0.0.xx`     | General Purpose & Windows Dev Environment | `As Needed` |
| **Radarr**              | `LXC`    | 107 | `10.0.0.11`     | Automated Movie Library Management        | `Active`    |
| **WireGuard**           | `LXC`    | 108 | `10.0.0.46`     | Secure Remote Access VPN                  | `Active`    |
| **Home Assistant**      | `VM`     | 109 | `10.0.0.95`     | Core Smart Home Automation Hub            | `Active`    |
| **qBittorrent**         | `LXC`    | 110 | `10.0.0.167`    | Primary Download Client                   | `Active`    |
| **Vaultwarden**         | `LXC`    | 111 | `10.0.0.181`    | Self-Hosted Password Management           | `Active`    |
| **Nextcloud**           | `LXC`    | 112 | `10.0.0.246`    | Private Cloud for File Sync & Backup      | `WIP`       |
| **Prometheus**          | `LXC`    | 113 | `10.0.0.209`    | System & Service Metrics Collection       | `Active`    |
| **Grafana**             | `LXC`    | 114 | `10.0.0.36`     | Infrastructure Monitoring Dashboards      | `Active`    |
| **Heimdall**            | `LXC`    | 115 | `10.0.0.40`     | Application Launcher Dashboard            | `Active`    |
| **Uptime Kuma**         | `LXC`    | 116 | `10.0.0.13`     | Service Uptime & Health Monitoring      | `Active`    |
| **Runtipi**             | `LXC`    | 117 | `10.0.0.184`    | App Browser / One-Click Installer         | `Active`    |
| **AdGuard Home**        | `LXC`    | 118 | `10.0.0.137`    | Network-Wide DNS & Ad-Blocking          | `Active`    |
| **Streamlink**          | `LXC`    | 119 | `10.0.0.233`    | Web Stream Recording (DVR)                | `Active`    |
| **Ombi**                | `LXC`    | 120 | `10.0.0.98`     | User-Facing Media Request Portal          | `Active`    |
| **MariaDB**             | `LXC`    | 121 | `10.0.0.136`    | Centralized SQL Database Backend          | `Active`    |
| **Ollama**              | `LXC`    | 122 | `10.0.0.170`    | Local AI/LLM Inference Server             | `Active`    |
| **Nginx Proxy Manager** | `LXC`    | 124 | `10.0.0.232`    | Central Reverse Proxy & SSL Termination   | `Active`    |
| **FlareSolverr**        | `LXC`    | 125 | `10.0.0.48`     | Cloudflare Reverse Proxy Workaround       | `Disabled`  |
| **OpenMediaVault**      | `VM`     | 130 | `10.0.0.5`      | Primary NFS / SMB Network File Host       | `Active`    |
				

### 2. Command Post: **The Nexus Terminal**

    **Class:** Arch Linux Workstation

    **Hardware:** HP Victus with an Intel i3-12100F CPU, 8GB RAM, and an NVIDIA GTX 1650 GPU.

    **Role:** My primary "cockpit" for interacting with the fleet. It is a low-budget, high-performance, single-user machine optimized for gaming (including VR), software development, creative work, and managing the KCC via SSH and web UIs.

### 3. Scout Shuttle: **The Nomad**

    **Class:** Lightweight Arch Linux Laptop

    **Role:** A mobile, "in-the-field" utility machine. The Nomad is used for on-site diagnostics, remote administration, and as a secondary workstation when away from the Nexus Terminal. Its lightweight OS ensures maximum performance on limited hardware.

---

## III. Current Network Architecture

The fleet currently operates on a single, flat LAN provided by the ISP router (10.0.0.0/24 range). All core services on the KCC are assigned static IP addresses or static DHCP leases for reliable access.

    DNS: Centralized, network-wide DNS and ad-blocking is handled by the AdGuard Home container. Custom internal hostnames (e.g., jellyfin.kcc) are managed via its DNS Rewrites feature.

    Web Access: All HTTP/S traffic is routed through the Nginx Proxy Manager container, which provides a single, unified entry point for web-based services.

    Storage: Primary storage is a 4TB HDD physically located in the KCC and served to the network via a dedicated OpenMediaVault VM. Network access is provided via SMB (for universal compatibility) and NFS (for high-performance Linux client access).

---

## IV. Future Expansion & Roadmap

This home lab is a constantly evolving project. The following major upgrades are currently in the planning phase:

    Tier 1: Network Infrastructure Overhaul

        Objective: To replace the ISP router with a dedicated, professional-grade firewall to enable a fully segmented, zero-trust network architecture.

        Planned Hardware: A Mini PC running OPNsense and a VLAN-aware Wi-Fi Access Point (e.g., TP-Link Omada).

        Key Goals:

            Implement VLANs to isolate Trusted, Server, IoT, and Guest networks.

            Establish robust firewall rules to control inter-VLAN traffic.

            Deploy a self-hosted WireGuard VPN on the firewall for secure remote access to the entire home lab.

            Implement network-wide Quality of Service (QoS) to prioritize critical traffic.

    Tier 2: KCC Hardware Upgrades

        Objective: To enhance the compute and storage capabilities of the KCC flagship.

        Planned Hardware:

            A modern, well-supported GPU (e.g., Intel Arc A380) to provide reliable hardware acceleration for Jellyfin and Ollama.

            Additional RAM (upgrade to 32GB) to support more services and ZFS caching.

            Additional HDDs to create a redundant ZFS storage pool for enhanced data integrity and protection.

            A CPU upgrade (e.g., Ryzen 7 5700X) to increase available compute cores.

    Tier 3: Full "Private Cloud" Deployment

        Objective: To fully replace reliance on commercial cloud services.

        Planned Services: Deployment of a Nextcloud instance on the KCC to handle file sync, photo backups, calendars, and contacts for the household.`
