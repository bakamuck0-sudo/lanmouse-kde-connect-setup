# Seamless Linux Workstation Setup Guide: Arch (Server) & CachyOS (Client)

This repository documents the configuration of a unified, high-performance Linux workspace connecting a primary PC and a secondary laptop.

The goal is to achieve a seamless workflow where a single keyboard and mouse control both machines without physical KVM hardware, while keeping clipboards and notifications synchronized.

---

## 🎯 Project Overview

* **Primary Workstation (Server):** Arch Linux (Custom CLI-centric setup with Hyprland/Neovim).
* **Secondary Laptop (Client):** CachyOS (Optimized Arch-based distribution).
* **Tools Used:**
    * **Lan Mouse** (`lanmouse-git`) - For low-latency software KVM functionality.
    * **KDE Connect** - For clipboard sync and notification mirroring.
    * **UFW/Firewalld** - For network security configuration.

---

## ⚙️ Prerequisites

Before starting, ensure the following requirements are met:

1.  **Network:** Both machines must be connected to the same Local Area Network (LAN). Ethernet is recommended for minimal latency.
2.  **IP Addressing:** Static IP addresses (or DHCP reservations) are recommended to maintain stable connections.
3.  **Packages:** Install necessary packages on **both** machines:
    * `yay -S lanmouse-git` (AUR)
    * `sudo pacman -S kdeconnect ufw`

---

## 📚 Phase 1: Lan Mouse Configuration (Software KVM)

Lan Mouse facilitates cursor and keyboard sharing over the network. The default port used is **14197**.

### Step 1.1: Firewall Configuration (Critical)

Connectivity issues are almost always caused by firewalls blocking the UDP/TCP ports. You must open port **14197** on BOTH machines.

**Command for UFW (Uncomplicated Firewall):**
Run this on both Arch and CachyOS:

```bash
# Allow incoming/outgoing traffic on default Lan Mouse port
sudo ufw allow 14197/udp
# Reload to apply changes
sudo ufw reload
# Verify status
sudo ufw status verbose

Step 1.2: Server Setup (Primary Arch PC)

The machine with the physical keyboard and mouse acts as the Server.

    Find your Local IP:
    Bash

    ip addr show | grep "inet " | grep -v 127.0.0.1

    (Note down the IP, e.g., 192.168.1.50).

    Launch Lan Mouse:

        Set mode to Server.

        Configure the screen layout (e.g., add the Client screen to the "Right" or "Left" of the Server screen).

Step 1.3: Client Setup (Secondary CachyOS Laptop)

    Launch Lan Mouse:

        Set mode to Client.

    Connect:

        Enter the Server's IP address (from Step 1.2).

        Click Connect.

    Test: Move your cursor to the edge of the screen. It should transition to the laptop seamlessly.

📚 Phase 2: KDE Connect Integration

While Lan Mouse handles input, KDE Connect manages data synchronization.

Step 2.1: Pairing

    Ensure kdeconnectd is running on both systems.

    Open KDE Connect on the Arch PC. It should discover the CachyOS laptop via local network discovery.

    Click "Request Pair".

    Accept the pairing request on the laptop.

Step 2.2: Essential Modules for Admin Workflow

Enable these specific plugins to improve productivity:

    📋 Clipboard Sync: Allows copying commands/logs on the Arch PC and pasting them directly into the terminal on the CachyOS laptop.

    🔔 Notifications: Mirrors system alerts and messenger notifications from the laptop to the main desktop, ensuring you never miss an alert while focusing on the main screen.

🔧 Troubleshooting

Issue	Solution
Connection Refused	Check Firewall status on both machines. Ensure port 14197 is open.
Devices not visible	Verify both devices are on the same subnet (e.g., 192.168.1.x). Disable "AP Isolation" on router.
High Latency	Switch from Wi-Fi to Ethernet. Ensure lanmouse is using UDP, not TCP.
Protocol Mismatch	Update lanmouse-git on both machines to the latest version using yay -Syu.

🚀 Skills Demonstrated

    Linux System Administration: Management of Arch Linux and CachyOS environments.

    Network Security: Configuration of local firewalls (UFW) and understanding of TCP/UDP ports.

    Infrastructure Optimization: Implementation of open-source tools to create an efficient, unified workspace.
