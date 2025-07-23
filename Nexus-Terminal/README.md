# Command Post Analysis: The Nexus Terminal

`This document provides a detailed overview of my primary workstation, the "Nexus Terminal." While the KCC server handles the fleet's backend infrastructure, the Nexus Terminal serves as the high-performance "cockpit" for all user-facing tasks, including gaming, development, creative work, and fleet(netowork) management.`

`## I. Mission & Role`

`The mission of the Nexus Terminal is to provide a powerful, customized, and responsive user experience. It is optimized for single-user, low-latency workloads and acts as the primary interface for managing and utilizing the services hosted on the KCC server. It is both a powerful asset in its own right and a seamless client for the broader home lab ecosystem.`

`---`

`## II. Hardware Manifest`

`The Nexus Terminal is a custom-built Arch Linux PC, restored into an HP Victus chassis. The hardware was selected for a balance of modern single-core CPU performance and reliable graphics capabilities on a Linux platform.`

| Component | Make & Model | Specifications | Notes |
| :--- | :--- | :--- | :--- |
| **CPU** | `Intel Core i3-12100F` | `4 Cores / 8 Threads @ 4.3 GHz Turbo` | `Chosen for its excellent single-thread performance, ideal for gaming and responsive desktop usage.` |
| **Motherboard**| `HP OEM Victus Motherboard` | ` ` | `Standard OEM board for the HP Victus chassis.` |
| **RAM** | `HP` | `8GB DDR4 @ 3200MHz` | `XMP profile is enabled for full performance.` |
| **GPU** | `NVIDIA GeForce GTX 1650` | `4GB GDDR5` | `A reliable and power-efficient GPU with solid proprietary driver support on Linux.` |
| **Boot Drive**| `[NVMe SSD Manufacturer]`| `500GB NVMe SSD` | `Hosts the Arch Linux OS, home directory, and locally installed applications.` |
| **Secondary Storage**| `Samsung` | `250GB SATA SSD`| `Primarily used as a library for locally installed Steam games.` |
| **Chassis** | `HP Victus 15L` | ` ` | `A compact and efficient pre-built chassis.` |
| **PSU** | `HP` | `[Wattage]` | ` ` |

`---`

`## III. Operating System & Customization`

`The Nexus Terminal runs a highly customized installation of Arch Linux, leveraging a rolling-release model to maintain access to the latest software while being tailored for a specific, high-performance workflow.`

*   **Operating System:** `Arch Linux` (Rolling Release)
*   **Kernel:** `Linux Zen Kernel` (e.g., `6.15.x-zen`) - Chosen for its optimizations in desktop responsiveness and gaming performance.
*   **Bootloader:** The bootloader was manually recovered and reinstalled using `GRUB` after a full system migration and `timeshift` restore. The system now utilizes a stable Legacy BIOS/CSM boot on a GPT-partitioned drive via a dedicated `bios_grub` partition.
*   **Desktop Environment:** `KDE Plasma 6.x.x` running on the **Wayland** display server.
*   **Graphics Drivers:** `NVIDIA Proprietary Driver` (`nvidia-dkms`), configured with Early KMS for a smooth, flicker-free boot experience.
*   **Terminal & Shell:** The command-line interface is customized ("riced") for efficiency and aesthetics:
    *   **Emulator:** `Ghostty`, Modern terminal emulator with hardware acceleration support.
    *   **Shell:** `Zsh` (Z Shell)
    *   **Prompt:** `Powerlevel10k`, providing a dynamic and informative Git-aware prompt.
    *   **Fonts:** `Nerd Fonts` are used system-wide to enable glyphs and icons in the terminal.
    *   **Plugins:** `zsh-autosuggestions` and `zsh-syntax-highlighting` for an enhanced interactive shell experience.

`---`

`## IV. System Integration & Role Within the Fleet`

`The Nexus Terminal is not a standalone machine; it is deeply integrated with the KCC server infrastructure.`

*   **Storage Access:** The terminal has permanent, high-performance access to the KCC's 4TB storage pool via a system-level **NFS mount**. The remote `/mnt/storage_4tb` directory on the KCC is mounted locally at `/mnt/KCC-Storage`, making it seamlessly available to all applications (e.g., RetroArch, Dolphin file manager).
*   **Fleet Management:** It serves as the primary management console for all KCC services, accessing the Proxmox, OpenMediaVault, and Pterodactyl web UIs.
*   **Service Client:** It is the primary client for most user-facing KCC services:
    *   **Jellyfin:** Acts as a playback client.
    *   **DNS:** The network configuration is manually set to use the KCC's **AdGuard Home** container as its sole DNS resolver, providing network-wide ad blocking.
    *   **Reverse Proxy:** The local `/etc/hosts` file (soon to be replaced by central DNS Rewrites in AdGuard) is used to resolve custom `.kcc` domain names to the **Nginx Proxy Manager**, providing clean and secure access to services like `http://jellyfin.kcc`.

`---`

`## V. Key Software & Workflow`

*   **Primary Game Client:** `Steam`, utilizing `Proton-GE` for enhanced compatibility with Windows games. `GameMode` is configured for automatic performance optimization.
*   **Development:** `VSCode` connecting to the `code-server` instance on the KCC. Learning `Golang` and `Python`.
*   **System Backup:** The core OS is backed up via `timeshift` to the OMV network share. Personal data in the `/home` directory is stored directly on the NFS file share drive.