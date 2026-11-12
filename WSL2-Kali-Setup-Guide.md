<div align="center">

<a id="-the-complete-guide-to-wsl2-kali-linux--win-kex-gui"></a><a id="the-complete-guide-to-wsl2-kali-linux--win-kex-gui"></a>
# 🐉 The Complete Guide to WSL2, Kali Linux & Win-KeX (GUI)

### *Production Security Lab Architecture: Bare-Metal Performance, GPU Acceleration, Sound Configuration, and Troubleshooting on Windows 11/10*

<br>

[![Guide License: CC BY-SA 4.0](https://img.shields.io/badge/Guide%20License-CC%20BY--SA%204.0-blue.svg)](./LICENSE-DOCS)
[![Host: Windows 11 / 10](https://img.shields.io/badge/Host-Windows%2011%20%7C%2010-0078D6?logo=windows&logoColor=white)](#0-prerequisites-hardware--host-setup)
[![Distro: Kali Linux Rolling](https://img.shields.io/badge/Distro-Kali%20Linux%20Rolling-557C99?logo=kalilinux&logoColor=white)](#2-step-2-install-kali-linux)
[![GUI: Win-KeX 3.0](https://img.shields.io/badge/GUI-Win--KeX%203.0-critical)](#5-step-5-install-win-kex-desktop-gui)
[![Hardware: vGPU & CUDA](https://img.shields.io/badge/Hardware-vGPU%20%26%20CUDA-76B900?logo=nvidia&logoColor=white)](#02-gpu-acceleration--graphics-drivers)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./.github/CONTRIBUTING.md)

<br>

*Written in plain English so beginners, cybersecurity researchers, and penetration testers can deploy a high-performance Kali Linux environment on Windows with zero VM overhead, direct GPU acceleration, interactive GUI desktop modes, and proven fixes for common errors.*

[Prerequisites](#0-prerequisites-hardware--host-setup) •
[WSL2 Setup](#1-step-1-update-and-configure-wsl2) •
[Kali Install](#2-step-2-install-kali-linux) •
[Win-KeX GUI](#5-step-5-install-win-kex-desktop-gui) •
[Troubleshooting 10053](#71-error-case-1-tigervnc-connection-abort-error-10053) •
[Toolkits & Prompts](#8-step-6-kali-security-toolkits--installer-prompts) •
[Performance Tuning](#9-wsl2-performance-tuning--system-management)

</div>

---

## 📖 Table of Contents

1. [Prerequisites: Hardware & Host Setup](#0-prerequisites-hardware--host-setup)
   * [0.1 CPU Virtualization Check (BIOS/UEFI)](#01-cpu-virtualization-check-biosuefi)
   * [0.2 GPU Acceleration & Graphics Drivers](#02-gpu-acceleration--graphics-drivers)
   * [0.3 Enable Required Windows Features](#03-enable-required-windows-features)
2. [Step 1: Update and Configure WSL2](#1-step-1-update-and-configure-wsl2)
3. [Step 2: Install Kali Linux](#2-step-2-install-kali-linux)
4. [Step 3: Set Kali as Default and Launch](#3-step-3-set-kali-as-default-and-launch)
5. [Step 4: Update Packages & Fix Mirror Routing](#4-step-4-update-packages--fix-mirror-routing)
   * [4.1 Fallback High-Speed Mirror Configuration](#41-fallback-high-speed-mirror-configuration)
6. [Step 5: Install Win-KeX (Desktop GUI)](#5-step-5-install-win-kex-desktop-gui)
   * [Win-KeX Operational Modes: Window, Seamless & ESM](#win-kex-operational-modes-window-seamless--esm)
   * [The KeX "Viewonly password (y/n)?" Prompt](#the-kex-viewonly-password-yn-prompt)
   * [TigerVNC Client Setup & Fallback](#tigervnc-client-setup--fallback)
7. [Troubleshooting & Error Recovery Protocols](#7-troubleshooting--error-recovery-protocols)
   * [7.1 Error Case 1: TigerVNC Connection Abort (Error 10053)](#71-error-case-1-tigervnc-connection-abort-error-10053)
   * [7.2 Error Case 2: Xfce Notification Daemon Wayland Warning](#72-error-case-2-xfce-notification-daemon-wayland-warning)
   * [7.3 Error Case 3: Command 'kex' Not Found & Interrupted dpkg](#73-error-case-3-command-kex-not-found--interrupted-dpkg)
8. [Step 6: Kali Security Toolkits & Installer Prompts](#8-step-6-kali-security-toolkits--installer-prompts)
   * [Prompt 1: Install Kismet "setuid root"?](#prompt-1-install-kismet-setuid-root)
   * [Prompt 2: Non-Superusers Packet Capture (Wireshark)](#prompt-2-non-superusers-packet-capture-wireshark)
   * [Prompt 3: Run sslh: inetd or standalone?](#prompt-3-run-sslh-inetd-or-standalone)
   * [Prompt 4: MacChanger Automatic MAC Randomization](#prompt-4-macchanger-automatic-mac-randomization)
9. [WSL2 Performance Tuning & System Management](#9-wsl2-performance-tuning--system-management)
   * [9.1 Resource Management via .wslconfig](#91-resource-management-via-wslconfig)
   * [9.2 Process Management: Stopping KeX vs Stopping WSL](#92-process-management-stopping-kex-vs-stopping-wsl)
   * [9.3 GPU Acceleration Verification](#93-gpu-acceleration-verification)
10. [Frequently Asked Questions (FAQ)](#10-frequently-asked-questions-faq)
11. [Contributing & Sharing](#-contributing--sharing)

---

<a id="-0-prerequisites-hardware--host-setup"></a><a id="0-prerequisites-hardware--host-setup"></a>
## ⚙️ 0. Prerequisites: Hardware & Host Setup

Before installing Kali Linux, you need to verify that your computer hardware and Windows operating system are configured to run virtualized containers at full speed.

<a id="-01-cpu-virtualization-check-biosuefi"></a><a id="01-cpu-virtualization-check-biosuefi"></a>
### 0.1 CPU Virtualization Check (BIOS/UEFI)

WSL2 is a lightweight utility virtual machine managed directly by Windows Hyper-V. It requires CPU hardware virtualization extensions to be active.

**How to verify:**

1. Open **Task Manager** (press `Ctrl + Shift + Esc`).
2. Click on the **Performance** tab and select **CPU**.
3. Look at the lower right details area for: **Virtualization: Enabled**.

<div align="center">

![](https://imgur.com/Z5pW4W4.png)

</div>

> [!NOTE]
> If Virtualization shows **Disabled**, reboot your computer into **BIOS/UEFI** (press `F2`, `F10`, `F12`, or `Del` during boot) and enable the virtualization feature:
> - **Intel CPUs:** Look for **Intel Virtualization Technology (VT-x)** or **Intel VMX**.
> - **AMD CPUs:** Look for **SVM Mode (Secure Virtual Machine)** or **AMD-V**.

---

<a id="-02-gpu-acceleration--graphics-drivers"></a><a id="02-gpu-acceleration--graphics-drivers"></a>
### 0.2 GPU Acceleration & Graphics Drivers

WSL2 supports native **vGPU (virtual GPU) acceleration**. This means Kali tools that rely on the graphics card (such as Hashcat for password recovery, PyTorch for machine learning, or desktop rendering inside Win-KeX) can access your host graphics card directly without extra Linux driver setups.

**NVIDIA GPU Users:**
- The recommended option for security labs and AI engineering is the **NVIDIA Studio Driver**.
- Studio Drivers prioritize long-term compute stability, CUDA compatibility, and reliable desktop compositing, while still running games smoothly.
- Alternatively, the standard Game Ready Driver also includes WSL2 vGPU pass-through support.

**Check current graphics driver:**

1. Open **Task Manager → Performance → GPU**.
2. Check the **Driver date** at the bottom right.

<div align="center">

![](https://imgur.com/UXHWyuo.png)

</div>

If your driver is older than 6 months, update it through the official NVIDIA, AMD, or Intel website before proceeding.

---

<a id="-03-enable-required-windows-features"></a><a id="03-enable-required-windows-features"></a>
### 0.3 Enable Required Windows Features

WSL2 relies on two underlying Windows virtualization components.

**Method 1: Graphical Setup**

1. Press the **Windows Key**, type **"Turn Windows features on or off"**, and press Enter.
2. Scroll through the list and check both boxes:
   - **Virtual Machine Platform**
   - **Windows Subsystem for Linux**
3. Click **OK**, let Windows apply the changes, and restart your computer when prompted.

<div align="center">

![](https://imgur.com/PDDBRJG.png)

</div>

**Method 2: Fast PowerShell (Admin) One-Liner**

If you prefer the command line, open **PowerShell as Administrator** and run:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

Restart your computer after running these commands to finalize installation.

---

<a id="-1-step-1-update-and-configure-wsl2"></a><a id="1-step-1-update-and-configure-wsl2"></a>
## 🚀 1. Step 1: Update and Configure WSL2

Once your system restarts, open **PowerShell as Administrator** to update the WSL kernel to the latest version.

```powershell
wsl --update
wsl --version
```

Verify that WSL is configured to use version 2 by default:

```powershell
wsl --set-default-version 2
```

Inspect your subsystem status:

```powershell
wsl --status
```

This command confirms your default distribution version, your active kernel build, and whether the underlying Hyper-V utility architecture is operational.

---

<a id="-2-step-2-install-kali-linux"></a><a id="2-step-2-install-kali-linux"></a>
## 📦 2. Step 2: Install Kali Linux

Check the list of officially available Linux distributions from Microsoft:

```powershell
wsl.exe --list --online
```

Install the official Kali Linux distribution:

```powershell
wsl --install -d kali-linux
```

**What happens during this step:**

1. Windows downloads the official Kali Linux root filesystem image from the canonical store feed.
2. The image is extracted and initialized as a lightweight WSL2 container.
3. A terminal window opens automatically and prompts you to create a default UNIX user account:
   - **Enter new UNIX username:** Choose a username (lowercase letters, numbers, no spaces).
   - **New password:** Enter a secure password (keystrokes will remain invisible on screen for security).
   - **Retype new password:** Confirm your password.

---

<a id="-3-step-3-set-kali-as-default-and-launch"></a><a id="3-step-3-set-kali-as-default-and-launch"></a>
## 🎯 3. Step 3: Set Kali as Default and Launch

If you work primarily with Kali Linux, make it your default WSL distribution so running `wsl` opens Kali immediately:

```powershell
wsl --set-default kali-linux
```

You can now start your Kali shell anytime from PowerShell, Command Prompt, or Windows Terminal using any of these commands:

```powershell
wsl
```

Or target Kali specifically:

```powershell
wsl -d kali-linux
```

---

<a id="-4-step-4-update-packages--fix-mirror-routing"></a><a id="4-step-4-update-packages--fix-mirror-routing"></a>
## 🔄 4. Step 4: Update Packages & Fix Mirror Routing

Once you are inside your Kali shell, your first task is to update the package repositories.

```bash
sudo apt update && sudo apt upgrade -y
```

<a id="-41-fallback-high-speed-mirror-configuration"></a><a id="41-fallback-high-speed-mirror-configuration"></a>
### 4.1 Fallback High-Speed Mirror Configuration

If `sudo apt update` hangs, fails with network timeouts, or throws mirror synchronization errors, Kali's default geo-DNS redirector might be experiencing downtime or throttling in your region.

**How to switch to a reliable high-speed mirror:**

1. Edit your APT sources list:

```bash
sudo nano /etc/apt/sources.list
```

2. Replace the contents of the file with the official RWTH Aachen high-speed mirror:

```text
deb https://ftp.halifax.rwth-aachen.de/kali kali-rolling main contrib non-free non-free-firmware
```

3. Save and close the editor (in nano: press `Ctrl + O`, then `Enter`, then `Ctrl + X`).

4. If your system throws HTTPS transport or certificate warnings, reinstall the keyring and update:

```bash
sudo apt update
```

---

<a id="-5-step-5-install-win-kex-desktop-gui"></a><a id="5-step-5-install-win-kex-desktop-gui"></a>
## 🖥️ 5. Step 5: Install Win-KeX (Desktop GUI)

While the terminal is great for scripts and network scanners, many advanced penetration testing workflows (such as Burp Suite web proxies, Wireshark packet analysis, and Ghidra reverse engineering) benefit from a full graphical desktop.

**Win-KeX (Windows Kali Experience)** provides a dedicated Xfce desktop environment tailored specifically for WSL2.

Inside your Kali terminal, install the Win-KeX package:

```bash
sudo apt update && sudo apt install -y kali-win-kex
```

<a id="-win-kex-operational-modes-window-seamless--esm"></a><a id="win-kex-operational-modes-window-seamless--esm"></a>
### Win-KeX Operational Modes: Window, Seamless & ESM

Win-KeX offers three flexible display modes depending on your workflow:

| Mode | Command | Display Behavior | Audio Support | Best Used For |
| :--- | :--- | :--- | :---: | :--- |
| **Window Mode** | `kex --win -s` | Dedicated window running full Kali Xfce desktop | Yes (`-s`) | Full lab immersion, multi-workspace isolation |
| **Seamless Mode** | `kex --sl -s` | Kali panel sits at top of Windows; apps float naturally | Yes (`-s`) | Daily multitasking alongside Windows software |
| **Enhanced Session Mode** | `kex --esm -s` | RDP client connection with native client clipboard | Yes (`-s`) | High DPI scaling and multi-monitor setups |

To launch standard Window Mode with sound enabled, run:

```bash
kex --win -s
```

---

<a id="-the-kex-viewonly-password-prompt"></a><a id="the-kex-viewonly-password-prompt"></a>
### The KeX "Viewonly password (y/n)?" Prompt

On your first launch, the setup wizard asks you to set a VNC desktop password.

```text
Password: [Enter your password]
Verify:   [Confirm your password]
Would you like to enter a view-only password (y/n)?
```

> [!IMPORTANT]
> **Answer: `n`**
> - If you select **`y` (yes)**, the session locks into read-only viewing. You will be unable to click buttons, type commands, or interact with any windows.
> - If you select **`n` (no)**, you are granted full interactive keyboard and mouse control over the entire desktop.
> - **Password Length Rule:** VNC authentication truncates passwords to a maximum of 8 characters. Choose a password between 6 and 8 characters long.

Once confirmed:
1. The KeX VNC server starts inside your Kali instance.
2. The TigerVNC Viewer window launches automatically on your Windows desktop.
3. The complete Kali Linux Xfce graphical environment appears.

---

<a id="-tigervnc-client-setup--fallback"></a><a id="tigervnc-client-setup--fallback"></a>
### TigerVNC Client Setup & Fallback

Win-KeX uses TigerVNC under the hood to bridge graphical frames between WSL2 and Windows. In standard installations, the `kali-win-kex` package automatically ships the Windows executable.

However, if your Windows security software blocks the spawned viewer, or if the client window fails to open automatically, you can download the standalone Windows client:
- Download the official standalone viewer from the [TigerVNC Project](https://sourceforge.net/projects/tigervnc/).
- Run `tigervnc64.exe` on Windows and connect manually to `localhost:5901` using the VNC password you created earlier.

To start KeX in lighter seamless mode during daily work:

```bash
kex --sl -s
```

---

<a id="-7-troubleshooting--error-recovery-protocols"></a><a id="7-troubleshooting--error-recovery-protocols"></a>
## 🛠️ 7. Troubleshooting & Error Recovery Protocols

When working with virtualization and graphical display servers, socket locks or interrupted shutdowns can occasionally cause connection warnings. Use these verified troubleshooting protocols to resolve them instantly.

<a id="-71-error-case-1-tigervnc-connection-abort-error-10053"></a><a id="71-error-case-1-tigervnc-connection-abort-error-10053"></a>
### 7.1 Error Case 1: TigerVNC Connection Abort (Error 10053)

During your usage of Win-KeX, TigerVNC Viewer may display this message:

<div align="center">

![](https://imgur.com/whmNxXu.png)

</div>

```text
VNC Viewer: Connection closed
An established connection was aborted by the software in your host machine.
(10053)
Do you wish to attempt to reconnect to localhost:5901?
```

This dialog can be either normal expected behavior or an actual crash. How you respond depends entirely on **when** the message appears.

#### Scenario A: The dialog appears after you stopped KeX or shut down WSL (Normal)

If you ran `kex --stop`, `sudo poweroff`, or `wsl --shutdown` while the TigerVNC window was open:
- **What is happening:** The background VNC server terminated as instructed. Windows notices that the listening port closed and TigerVNC reports the disconnect with code 10053.
- **Correct Action:** Click **No** and close the viewer. Everything is functioning properly. Do not click Reconnect and do not run recovery scripts.

#### Scenario B: The dialog appears while Kali is running or during startup (Real Problem)

If the window disconnects unexpectedly during active work, or loops endlessly on startup:
- **What is happening:** A stale X11 lock file (`/tmp/.X11-unix/`), an orphaned `Xtigervnc` process, or a corrupted user profile is blocking display socket `:1`.

**The 6-Step Recovery Protocol (Run inside Kali shell):**

1. Stop any remaining KeX services:

```bash
kex --stop
```

2. Terminate all orphaned VNC and KeX processes:

```bash
pkill Xtigervnc
pkill kex
```

3. Remove broken X11 display socket files:

```bash
rm -rf ~/.vnc /tmp/.X11-unix/X1* /tmp/.X1-lock
```

4. Clear user session configurations:

```bash
rm -rf ~/.config/kex
```

5. Reset your KeX password cleanly:

```bash
kex --passwd
```
*(When prompted for view-only password, remember to answer `n`)*

6. Relaunch KeX in force mode:

```bash
kex --win --force
```

#### Quick Decision Matrix

| Situation | Recommended Action |
| :--- | :--- |
| Error 10053 appears after `kex --stop` or WSL shutdown | Click **No**, close TigerVNC (normal behavior) |
| Error 10053 appears immediately upon starting KeX | Click **No**, close viewer, run the 6-step recovery protocol above |
| Error 10061 (Connection refused) | The VNC server is not running yet. Run `kex --win` or restart WSL |
| GUI opens smoothly | Everything is healthy; disregard past logs |

---

<a id="-72-error-case-2-xfce-notification-daemon-wayland-warning"></a><a id="72-error-case-2-xfce-notification-daemon-wayland-warning"></a>
### 7.2 Error Case 2: Xfce Notification Daemon Wayland Warning

On some installations, launching the desktop shows a popup warning:

<div align="center">

![](https://imgur.com/DWDVshC.png)

</div>

```text
Wayland compositor does not support required protocol wlr-layer-shell
```

**Why this happens:**
The Xfce notification daemon (`xfce4-notifyd`) probes whether a modern Wayland compositor is present before falling back to X11. Because Win-KeX runs as a dedicated X11 display server, this notice is purely informational and does not affect the stability of your desktop tools.

**How to permanently silence this notice:**

1. Create a local autostart override directory:

```bash
mkdir -p ~/.config/autostart
```

2. Copy the default notification desktop definition:

```bash
cp /etc/xdg/autostart/xfce4-notifyd.desktop ~/.config/autostart/
```

3. Open the file in nano:

```bash
nano ~/.config/autostart/xfce4-notifyd.desktop
```

4. Locate the line `Hidden=false` (or add it if missing) and set it to:

```ini
Hidden=true
```

5. Save the file (`Ctrl + O`, `Enter`, `Ctrl + X`). Restart KeX with `kex --stop` and `kex --win`. The warning will never appear again.

---

<a id="-73-error-case-3-command-kex-not-found--interrupted-dpkg"></a><a id="73-error-case-3-command-kex-not-found--interrupted-dpkg"></a>
### 7.3 Error Case 3: Command 'kex' Not Found & Interrupted dpkg

If your initial package installation was interrupted by a network drop or system reboot, running `kex --win` may return:

```text
Command 'kex' not found, but can be installed with: sudo apt install kali-win-kex
```

Attempting to reinstall may trigger:

```text
E: dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem.
```

**How to restore the package manager:**

1. Complete interrupted package configurations:

```bash
sudo dpkg --configure -a
```

2. Repair broken dependencies:

```bash
sudo apt --fix-broken install
```

3. Update repository catalogs:

```bash
sudo apt update
```

4. Cleanly reinstall Win-KeX:

```bash
sudo apt install --reinstall kali-win-kex
```

5. Launch the desktop environment:

```bash
kex --win
```

---

<a id="-8-step-6-kali-security-toolkits--installer-prompts"></a><a id="8-step-6-kali-security-toolkits--installer-prompts"></a>
## 🧰 8. Step 6: Kali Security Toolkits & Installer Prompts

A fresh Kali WSL installation contains only minimal base utilities. To turn it into a comprehensive offensive security workstation, you can install one of Kali's curated metapackages:

```bash
# Recommended: Comprehensive toolkit for web, network, and reverse engineering (~10 GB)
sudo apt install -y kali-linux-large

# Lightweight alternative: Core top 10 pentesting utilities
sudo apt install -y kali-tools-top10
```

During the installation of `kali-linux-large`, the Debian package manager (`debconf`) presents several interactive configuration prompts. Here is the exact breakdown of how to answer each prompt and why.

---

<a id="-prompt-1-install-kismet-setuid-root"></a><a id="prompt-1-install-kismet-setuid-root"></a>
### Prompt 1: Install Kismet "setuid root"?

<div align="center">

![](https://imgur.com/8MwsvoU.png)

</div>

```text
Install Kismet with "setuid root"?
[ Yes ]       [ No ]
```

**Recommended Choice: Yes**

**Technical Explanation:**
Kismet is an industry-standard wireless network detector and packet sniffer. Capturing raw packets and toggling interface modes requires root privileges. Selecting **Yes** grants the capture executable elevated socket capabilities while restricting general usage to members of the `kismet` user group. This allows you to launch Kismet without running the entire graphical program as root.

---

<a id="-prompt-2-non-superusers-packet-capture-wireshark"></a><a id="prompt-2-non-superusers-packet-capture-wireshark"></a>
### Prompt 2: Non-Superusers Packet Capture (Wireshark)

<div align="center">

![](https://imgur.com/htxIEyF.png)

</div>

```text
Should non-superusers be able to capture packets?
[ Yes ]       [ No ]
```

**Recommended Choice: Yes**

**Technical Explanation:**
Running complex packet analyzers like Wireshark directly as `root` is a security risk because any memory vulnerability in a protocol dissector could compromise the entire operating system. Selecting **Yes** installs the `dumpcap` binary with elevated packet capture capabilities restricted to the `wireshark` group.

**Post-Installation Group Setup (Mandatory):**

Ensure your current user belongs to the `wireshark` group:

```bash
groups
```

If `wireshark` is missing from the list, add your user:

```bash
sudo usermod -aG wireshark $USER
```

Restart your WSL instance to apply the new group permissions:

```powershell
# Run in Windows PowerShell
wsl --shutdown
```

Reopen Kali and verify that `wireshark` now appears in your `groups` output.

---

<a id="-prompt-3-run-sslh-inetd-or-standalone"></a><a id="prompt-3-run-sslh-inetd-or-standalone"></a>
### Prompt 3: Run sslh: inetd or standalone?

<div align="center">

![](https://imgur.com/NkwsQPk.png)

</div>

```text
Run sslh:
> from inetd
  standalone
```

**Recommended Choice: standalone**

**Technical Explanation:**
`sslh` is an applicative protocol demultiplexer that lets you share port 443 between an HTTPS web server and an SSH or OpenVPN listener.
- **standalone:** Runs as a persistent background daemon, handling incoming connection multiplexing with minimal latency and high throughput. This is the recommended choice for labs and security testing.
- **from inetd:** Spawns a brand-new process for each incoming connection, creating noticeable overhead and potential instability during high-frequency scans.

---

<a id="-prompt-4-macchanger-automatic-mac-randomization"></a><a id="prompt-4-macchanger-automatic-mac-randomization"></a>
### Prompt 4: MacChanger Automatic MAC Randomization

```text
Change MAC automatically on network interface state change?
[ Yes ]       [ No ]
```

**Recommended Choice: No**

**Technical Explanation:**
Inside WSL2, networking is handled through a virtual Hyper-V network adapter (`eth0`) bridged to your Windows host machine. Enabling automatic MAC address randomization on virtual vNIC state changes can break the virtual switch link, causing your Kali instance to lose internet connectivity until WSL is completely restarted. Keep this disabled in virtualized environments.

---

<a id="-9-wsl2-performance-tuning--system-management"></a><a id="9-wsl2-performance-tuning--system-management"></a>
## ⚡ 9. WSL2 Performance Tuning & System Management

To prevent WSL2 from consuming excessive host RAM during heavy tasks (like dictionary attacks or compiling large toolsets), you can fine-tune resource limits using a global configuration file.

<a id="-91-resource-management-via-wslconfig"></a><a id="91-resource-management-via-wslconfig"></a>
### 9.1 Resource Management via .wslconfig

Create or edit the `.wslconfig` file in your Windows user profile folder (`C:\Users\<YourUsername>\.wslconfig`):

```ini
[wsl2]
# Limit memory allocated to the WSL2 virtual machine
memory=8GB

# Limit virtual processors
processors=4

# Allocate swap memory
swap=4GB

# Enable gradual memory reclaim when Linux processes idle
autoMemoryReclaim=gradual

# Enable localhost networking forwarding between Windows and Kali
localhostForwarding=true
```

Save the file and apply the limits by running in PowerShell:

```powershell
wsl --shutdown
```

When you relaunch Kali, it will operate strictly within your specified hardware envelope.

---

<a id="-92-process-management-stopping-kex-vs-stopping-wsl"></a><a id="92-process-management-stopping-kex-vs-stopping-wsl"></a>
### 9.2 Process Management: Stopping KeX vs Stopping WSL

Understanding how to stop services cleanly prevents file corruption and lingering memory usage.

```text
+------------------------------------------------------------------------------------------------+
|                             WSL2 PROCESS LIFECYCLE MANAGEMENT                                  |
+------------------------------------------------------------------------------------------------+
|                                                                                                |
|   1. Stop GUI Only:          kex --stop                  (Leaves Kali terminal active)        |
|   2. Exit Active Shell:      exit                        (Closes current session)             |
|   3. Terminate Distro:       wsl --terminate kali-linux  (Immediately stops only Kali)        |
|   4. Complete Hyper-V Stop:  wsl --shutdown              (Shuts down all WSL virtual machines)|
|                                                                                                |
+------------------------------------------------------------------------------------------------+
```

1. **Stop Only the Desktop GUI:**
   ```bash
   kex --stop
   ```
   Stops the VNC server and closes display windows while keeping your active terminal sessions and background scripts running.

2. **Terminate Only Kali Linux from Windows:**
   ```powershell
   wsl --terminate kali-linux
   ```
   Immediately halts all processes inside Kali without affecting other WSL distributions (such as Ubuntu or Debian).

3. **Complete Subsystem Shutdown:**
   ```powershell
   wsl --shutdown
   ```
   Shuts down the entire Hyper-V utility VM, releasing all cached RAM and swap storage back to Windows.

---

<a id="-93-gpu-acceleration-verification"></a><a id="93-gpu-acceleration-verification"></a>
### 9.3 GPU Acceleration Verification

To verify that your graphics card is properly passed through to Kali:

1. Inside your Kali terminal, check your NVIDIA GPU status:

```bash
nvidia-smi
```

You should see your host GPU model, active driver version, and CUDA version reported accurately inside Linux.

2. Verify OpenCL and compute platform readiness for Hashcat:

```bash
hashcat -I
```

The output will list your hardware compute devices, confirming that high-speed hardware acceleration is fully operational.

---

<a id="-10-frequently-asked-questions-faq"></a><a id="10-frequently-asked-questions-faq"></a>
## ❓ 10. Frequently Asked Questions (FAQ)

### Q1: Can I put internal Wi-Fi adapters into Monitor Mode inside WSL2?
No. WSL2 connects to the internet through a virtualized Hyper-V Ethernet adapter (`eth0`). Because the physical Wi-Fi card is managed by Windows drivers, Linux cannot access raw 802.11 radio frames. To perform wireless penetration testing (such as `airmon-ng` or Aircrack-ng), connect a compatible external USB Wi-Fi adapter (like Alfa or Panda) and pass it directly into WSL2 using `usbipd-win`.

### Q2: How do I share files between Windows and Kali Linux?
- **Accessing Windows files from Kali:** Windows drives are automatically mounted under `/mnt/` (e.g., `C:\Users\` is accessible at `/mnt/c/Users/`).
- **Accessing Kali files from Windows:** Open Windows File Explorer and navigate to `\\wsl$\kali-linux\home\<username>\`. Always store your projects and Git repositories inside the Linux virtual filesystem (`/home/<username>/`) rather than `/mnt/c/` for up to 5x faster disk I/O performance.

### Q3: Does Win-KeX support copy and paste between Windows and Kali?
Yes. Both Window Mode (`kex --win`) and Enhanced Session Mode (`kex --esm`) support bidirectional clipboard synchronization. You can copy text, terminal commands, or code snippets in Windows and paste them directly into Kali applications with `Ctrl + V` or right-click paste.

### Q4: Can I run Burp Suite or Metasploit proxies through host browsers?
Yes. Because WSL2 enables `localhostForwarding=true` by default, any port opened inside Kali (such as Burp Suite's HTTP proxy on `127.0.0.1:8080` or Metasploit listeners) is accessible directly from your Windows host browser at `http://localhost:8080`.

---

<div align="center">

<a id="-contributing--sharing"></a><a id="contributing--sharing"></a>
## 🤝 Contributing & Sharing

Did this guide help you build a high-performance Kali Linux security lab on Windows?

⭐ **Star this repository** to help more security researchers, developers, and students discover it!  
🔁 **Share this guide** with a teammate or cybersecurity study group.

<br>

**Authored by [Sagar Biswas (@SagarBiswas-MultiHAT)](https://github.com/SagarBiswas-MultiHAT)**  
*Part of the [Developer Survival Kit](./README.md)*  
🌐 [multihat.dev](https://multihat.dev)

*Break Boundaries. Secure Systems. Practice Responsibly.*

[Back to Top](#-the-complete-guide-to-wsl2-kali-linux--win-kex-gui) • [Survival Kit Catalog (README)](./README.md) • [Contributing](./.github/CONTRIBUTING.md)

</div>

---
