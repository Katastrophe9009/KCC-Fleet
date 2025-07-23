# KCC Fleet Network Architecture

This document provides a detailed overview of the network infrastructure for the KCC fleet. It covers the current physical and logical layout, the services that support the network, and the comprehensive future-state architecture designed to enhance security, performance, and control.

`## I. Current State: "Flat LAN" Architecture (Phase 1)`

The KCC network currently operates in a "flat" LAN topology, which is a common starting point for a home lab. All devices, from servers to user workstations and IoT gadgets, reside on a single broadcast domain managed by the ISP-provided router.

*   **Primary Network Hardware:**
    *   **Modem/Router:** Xfinity Combination Modem/Router
    *   **DHCP Server:** Xfinity Router (Controls the `10.0.0.0/24` address space)
    *   **Switching:** An unmanaged switch is used for physical port expansion.

*   **Logical Layout (`10.0.0.0/24`):**
    *   All core infrastructure services on the KCC (Proxmox host, containers, VMs) are assigned **Static DHCP Leases** (Reservations) from the Xfinity router to ensure stable, predictable IP addresses.
    *   All other client devices (PCs, laptops, phones) receive dynamic IP addresses from the same pool.

*   **Core Network Services:**
    `Even in the current flat architecture, key network services have been "overlaid" onto the network and are centrally managed by dedicated LXC containers on the KCC server.`
    *   **DNS Resolution & Ad-Blocking:** A container running **AdGuard Home (`10.0.0.106`)** has been deployed. All Linux clients (`Nexus Terminal`, `The Nomad`) are manually configured to use this as their sole DNS server. This provides network-wide ad blocking for configured clients and allows for the resolution of custom internal hostnames (e.g., `jellyfin.kcc`).
    *   **Reverse Proxy:** A container running **Nginx Proxy Manager (`10.0.0.232`)** acts as the central ingress point for all internal web services. This simplifies access by mapping hostnames like `jellyfin.kcc` to their corresponding backend container IP and port.

*   **Current Limitations:**
    *   The flat topology provides **no network segmentation**, meaning a compromised device (e.g., an IoT camera) could have unrestricted access to critical servers.
    *   Limited configurability of the ISP router prevents the implementation of advanced features like a remote-access VPN, QoS, and true network-wide DNS enforcement.

`---`

`## II. Future State: Segmented Zero-Trust Architecture (Phase 2)`

The next major evolution of the KCC network is a full migration to a professional-grade, segmented network based on a dedicated firewall and managed hardware. This will transform the network from a simple LAN into a secure, multi-zone infrastructure.

*   **Planned Core Network Hardware:**
    *   **Modem:** The Xfinity router will be reconfigured into **"Bridge Mode,"** functioning only as a cable modem.
    *   **Firewall/Router:** A dedicated **Mini PC** will be deployed to run **OPNsense**. This will become the new "brain" of the network, managing all routing, firewalling, DHCP, and VPN services.
    *   **Switching:** A **VLAN-aware managed switch** will be added.
    *   **Wireless:** A dedicated **Wi-Fi Access Point** (e.g., TP-Link Omada or Ubiquiti UniFi) will be deployed to provide segmented wireless access.

*   **Planned Logical Layout (VLANs):**
    The flat network will be broken down into multiple, logically isolated Virtual LANs. All inter-VLAN traffic will be explicitly denied by default and only allowed via specific, written firewall rules.

| VLAN ID | Name | IP Range | Purpose & Security Policy |
| :--- | :--- | :--- | :--- |
| **VLAN 10** | **Kat's HQ** | `10.0.10.0/24` | For trusted personal devices (`Nexus Terminal`, `The Nomad`, personal phone). Highest QoS priority. Full access to the Servers VLAN. |
| **VLAN 15** | **Family Net** | `10.0.15.0/24` | For trusted family devices. Standard QoS. Access to the internet and specific services on the Servers VLAN (like Jellyfin), but not administrative interfaces. |
| **VLAN 20** | **Servers** | `10.0.20.0/24` | High-security zone for the KCC and all its VMs/containers. Limited outbound internet access. No unsolicited inbound traffic allowed from any other VLAN except for specific, open ports (e.g., Jellyfin from the Family/Kat nets). |
| **VLAN 30** | **IoT Devices** | `10.0.30.0/24` | **Zero-Trust Zone.** For untrusted "smart" devices (TVs, cameras, Alexas). Allowed to access the internet, but **completely blocked** from initiating any contact with any other internal VLAN. |
| **VLAN 40** | **Guest Net** | `10.0.40.0/24` | For visitors. Allowed to access the internet. **Completely blocked** from all internal VLANs. |

*   **Future Service Integration:**
    *   **DNS:** The OPNsense DHCP server will be configured to assign the **AdGuard Home** IP address as the default DNS server for **all VLANs**, making ad-blocking truly network-wide. Different blocking policies can be applied to different VLANs.
    *   **VPN:**
        *   **Inbound:** A **WireGuard** server will be configured on OPNsense, providing a single, secure entry point for remote access.
        *   **Outbound:** The firewall will be configured with a connection to a **commercial VPN provider**. Policy-based routing rules will be created to force traffic from specific containers (like qBittorrent) or specific VLANs out through the commercial VPN tunnel for privacy.
