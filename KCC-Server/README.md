# Flagship Analysis: The KCC (Katastrophe Command Center)

This document provides a detailed overview of the primary Proxmox VE hypervisor node, "The KCC." This server forms the core of the fleet's infrastructure, providing the compute, storage, and virtualization resources for all self-hosted services.

`## I. Mission & Role`

The KCC's primary mission is to serve as a stable, reliable, and power-efficient 24/7 platform for a diverse range of containerized (LXC) and fully virtualized (VM) workloads. It is architected for flexibility, scalability, and ease of management, acting as the central hub for the entire home lab.

`---`

`## II. Hardware Manifest`

The KCC is a custom-built server, assembled from a combination of new and repurposed hardware ("Franken-PC"), optimized for a balance of performance and power efficiency.

| Component | Make & Model | Specifications | Notes |
| :--- | :--- | :--- | :--- |
| **CPU** | `AMD Ryzen 5 2600` | `6 Cores / 12 Threads @ 3.4 GHz` | `Provides a strong multi-core baseline for concurrent services.` |
| **Motherboard**| `ASUS ROG Strix B-450` | `AM4 Socket, 6x SATA Ports` | `A robust consumer-grade board with sufficient PCIe and SATA expansion for future growth.` |
| **RAM** | `Corsair Vengeance LPX` | `16GB DDR4 @ 3200MHz` | `Currently sufficient; primary candidate for future capacity upgrade.` |
| **GPU** | `AMD Radeon R9 390` | `8GB GDDR5` | `Currently acting as a basic display adapter for the host console. Known issues with advanced driver features prevent reliable use for hardware acceleration within Proxmox at this time.` |
| **Boot Drive** | `Samsung` | `500GB SATA SSD` | `Hosts the Proxmox VE OS and all LXC/VM root disks for fast performance.` |
| **Mass Storage**| `Western Digital`| `4TB NTFS HDD` | `Primary data storage. Passed through to a dedicated OMV VM.` |
| **Chassis** | `Cooler Master HAF` | `5x HDD Bays` | `High-Airflow case, providing excellent thermal performance and physical expansion capacity.` |
| **PSU** | `[PSU Manufacturer]` | `[Wattage]` | ` ` |

`---`

`## III. Core Operating System & Virtualization Strategy`

`The KCC runs Proxmox Virtual Environment (VE) as its bare-metal, Type-1 hypervisor. This provides a robust and flexible foundation for all hosted services.`

*   **Hypervisor:** `Proxmox VE 8.x` on a Debian 12 (Bookworm) base.
*   **Kernel:** `Linux 6.x.x` (pve)
*   **Boot Environment:** The Proxmox bootloader was manually repaired using `proxmox-boot-tool` to fix a broken EFI installation, resulting in a stable and reliable boot process.

*   **Primary Virtualization Method: Linux Containers (LXC)**
    *   **Strategy:** The "service-per-container" model is the default architecture for the KCC. The majority of applications (Jellyfin, Pterodactyl, the Arr stack, network services, etc.) are deployed in their own isolated, unprivileged (where possible) Debian or Ubuntu LXC containers.
    *   **Benefits:** This approach maximizes resource efficiency, allowing a large number of services to run concurrently with minimal CPU and RAM overhead.

*   **Secondary Virtualization Method: Full Virtual Machines (QEMU/KVM)**
    *   **Strategy:** VMs are used exclusively for workloads that cannot be containerized or require deep, exclusive hardware access.
    *   **Current VMs:** `OpenMediaVault` (requires low-level disk control) and `Windows 11` (non-Linux OS). Home Assistant is also deployed as a VM due to its need for broad hardware access.

`---`

`## IV. Storage Architecture & Management`

`The KCC employs a hybrid storage strategy, leveraging both fast and high-capacity media.`

*   **Boot & Application Storage:** The primary boot SSD is formatted with LVM-thin, managed by Proxmox. All VM and LXC root disks reside here to ensure fast application performance.
*   **Mass Data Storage:** The 4TB HDD is managed by a dedicated **OpenMediaVault VM**. The physical disk is passed through to the VM, which then serves the data to the rest of the network via **SMB** (for universal access) and **NFS** (for high-performance Linux client access). This decouples the primary storage from the hypervisor, allowing for more flexible management and greater data security.

`---`

`## V. Planned Upgrades & Future Roadmap`

`The KCC is an evolutionary platform with a clear upgrade path designed to enhance its capabilities.`

1.  **Hardware Acceleration (GPU):**
    *   **Objective:** To enable reliable, hardware-accelerated video transcoding and AI inference.
    *   **Plan:** Replace the current, problematic R9 390 GPU with a modern, well-supported card. The primary candidate is an **Intel Arc A380** due to its exceptional transcoding performance and low power draw. This would allow the host to share the GPU's capabilities with multiple LXC containers simultaneously (Jellyfin, Ollama). A secondary, more powerful **AMD Radeon GPU** is being considered for exclusive passthrough to a dedicated gaming VM.

2.  **Storage Redundancy & Expansion (ZFS):**
    *   **Objective:** To migrate from a single-drive storage solution to a redundant, enterprise-grade storage pool to protect against data loss.
    *   **Plan:** Introduce 2-3 new, high-capacity HDDs and build a **ZFS Mirror or RAID-Z1 pool** directly within Proxmox. The OMV VM would then be deprecated, with the file-sharing services (Samba/NFS) moving to a simple LXC container managing the ZFS pool. This would provide superior data integrity, snapshot capabilities, and performance.

3.  **Increased Memory:**
    *   **Objective:** To provide more resources for VMs and for ZFS's ARC cache.
    *   **Plan:** Upgrade the server's RAM from 16GB to **32GB** or **64GB**.