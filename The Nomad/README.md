# Scout Shuttle Analysis: The Nomad

This document provides a detailed overview of the fleet's mobile support and remote operations laptop, "The Nomad." In contrast to the specialized power of the KCC and Nexus Terminal, The Nomad's mission is defined by flexibility, reliability, and the ability to operate "in the field" where other systems cannot.

`## I. Mission & Role`

The Nomad's primary mission is to serve as a portable, self-sufficient, and powerful systems administration and diagnostics terminal. It was revived from hardware previously considered e-waste, embodying the fleet's philosophy of maximizing the utility and lifespan of technology through open-source software. It acts as a "field command post," a secondary workstation, and a crucial recovery tool.

`#### Key Roles:`

*   **Remote Hands:** Provides a physical interface for managing headless systems, as demonstrated during the initial setup and troubleshooting of the KCC server.
*   **Mobile Workstation:** Acts as a lightweight secondary computer for tasks when away from the main Nexus Terminal.
*   **Network Diagnostics:** Serves as a reliable, known-good client for testing network services, VPNs, and DNS from a different machine.
*   **System Rescue:** The base Arch install and its suite of tools make it an effective ad-hoc system recovery environment.

`---`

`## II. Hardware Manifest`

The Nomad is an HP Notebook circa ~2010's that has been given a new life with a lightweight Arch Linux installation and planned component upgrades. The hardware is a testament to the efficiency of a minimal Linux environment.

| Component | Make & Model | Specifications | Notes |
| :--- | :--- | :--- | :--- |
| **CPU** | `[CPU Model]` | `[Cores/Threads/Speed]` | `Low-power mobile CPU, sufficient for its administrative and light-workstation role.` |
| **RAM** | `[RAM Amount/Type]` | `[RAM Speed]` | ` ` |
| **GPU** | `[Integrated GPU Model]` | ` ` | `Relies on integrated graphics for maximum power efficiency and battery life.` |
| **Boot Drive**| `[SSD/HDD Model]` | `[Size/Type]` | `Hosts the lightweight Arch Linux OS.` |
| **Battery** | `OEM Original` | `[Capacity]` | **`STATUS: Degraded.`** Slated for replacement to restore full "in-the-field" operational range. |
| **Thermal System** | `OEM Original` | ` ` | `Requires servicing and new thermal paste application to ensure optimal performance and longevity.` |

`---`

`## III. Operating System & Configuration**

`The Nomad runs a minimal Arch Linux installation to maximize performance and battery life on its modest hardware.`

*   **Operating System:** `Arch Linux` (Rolling Release)
*   **Desktop Environment:** A lightweight DE such as `XFCE` or a window manager like `i3` is used to keep resource usage to an absolute minimum.
*   **Philosophy:** The software selection is intentionally lean. It contains only the essential tools required for its mission: a modern web browser, a powerful terminal, a text editor, and a full suite of networking and system diagnostic tools (`nfs-utils`, `android-tools`, etc.).
*   **Power Management:** Services and kernel parameters are tuned for maximum battery life when not plugged in.

`---`

`## IV. System Integration & Role Within the Fleet`

`The Nomad, while capable of independent operation, is a fully integrated member of the KCC fleet.`

*   **Storage Access:** Like the Nexus Terminal, The Nomad has permanent access to the KCC's 4TB storage pool via a system-level **NFS mount** at `/mnt/KCC-Storage`, allowing for seamless file access and transfer from the field.
*   **DNS & Ad-Blocking:** The network configuration is set to use the KCC's **AdGuard Home** container (`10.0.0.106`) as its DNS server, providing it with full access to internal `.kcc` hostnames and network-wide ad blocking.
*   **VPN Client:** The Nomad is configured as a **WireGuard client**. This allows it to form a secure, encrypted tunnel back to the KCC's VPN server, giving it full, secure access to the entire home lab from any external network in the world. It is the primary "away team" vessel.
*   **Remote Management:** Serves as the secondary management console for Proxmox and other KCC services, providing a vital fallback in case the Nexus Terminal is unavailable.

`---`

`## V. Planned Upgrades`

*   **Objective:** To restore the machine to its full potential as a mobile platform.
*   **Immediate Plans:**
    1.  **Replace the degraded battery** to restore its long-duration operational capability.
    2.  **Re-apply thermal paste** to the CPU to improve thermal performance, reduce fan noise, and extend the lifespan of the hardware.
