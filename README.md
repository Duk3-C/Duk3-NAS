# Duk3-NAS

Home Cloud server created using my **Raspberry Pi 5** with **Open Media Vault (OMV)** and **Tilescale**.

![NAS Banner](https://raw.githubusercontent.com/Duk3-C/Duk3-NAS/main/images/OMV_Dashboard.png)

## Contents
- [Overview](#overview)
- [Features](#features)
- [Hardware](#hardware)
- [Software Stack](#software-stack)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Configuration](#configuration)
- [Usage](#usage)
- [Remote Access](#remote-access)
- [Monitoring & Maintenance](#monitoring--maintenance)
- [Security Considerations](#security-considerations)
- [Future Improvements](#future-improvements)

## Overview

This project transforms a regular **Raspberry Pi 5** into an energy-efficient home cloud server. It provides a network-attached storage (NAS) service, file synchronization, and secure remote access; all while having the data under your control.

**Open Media Vault** handles the NAS functionality with a clean web UI, while **Tailscale** creates a secure WireGuard-based mesh VPN for accessing the server from anywhere without exposing ports to the internet.

## Features

- **NAS & File Sharing** - SMB, NFS, FTP
- **Secure Remote Access** - Tailscale VPN (no port forwarding)
- **Low Power COnsumption** - ~8-15W under load
- **Web-based Management** - OMV admin panel

## Hardware

|      Component         |             Specification                 |                         Notes                          |
|----------------------- |-------------------------------------------|--------------------------------------------------------|
| **Single Board Computer** | Raspberry Pi 5 (8GB recommended)       | 4GB might work but 8GB is better for multiple services |
| **Storage**            | External USB 3.0 Seagate SSD storage      | Boot from the MicroSD card                             |
| **Power Supply**       | Official 27W USB-C PD                     | Critical for stability                                 |
| **Networking**         | Gigabit Ethernet + Wi-Fi 6 (optional)     | Ethernet preferred in all cases                        |

## Software Stack

- **OS**: Raspberry Pi OS Lite (64-bit)
- **NAS Platform**: OpenMediaVault 7
- **VPN**: Tailscale
- **Other**: Samba

## Prerequisites

- Raspberry Pi 5 with at least 4GB RAM
- High-Quality MicroSD card (for initial boot) or NVMe SSD (more expensive)
- Stable internet connection (that's why Ethernet connection is a critical part of the server)
- Tailscale account

## Installation & Setup

### 1. Prepare the Raspberry Pi

```bash
# Flash Raspberry Pi OS Lite (64-bit) using Raspberry Pi Imager
# Enable SSH and set username/password
```

### 2. Install OpenMediaVault

```bash
# After first boot, update your system with...
sudo apt update && sudo apt upgrade

# After updating, install OMV with this command...
sudo wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash
```
Installation normally takes about 25-30 mins

### 3. Install Tailscale

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up --advertise-routes=192.168.1.0/24  # Adjust to your subnet or just leave the first three arguments
```

### 4. Post-Installation Configuration

- Access OMV web UI: http://<your-pi-ip>
- Change Admin password (default username is 'admin' and default password is 'openmediavault')
- Set up storage (drivers, filesystems, shared folders)
- Enable services (SMB, NFS, etc.)

## Configuration

Key configuration files and settings are documented in the config/ folder.
    - tailscale/ - Exit node and subnet router setup 
    - omv/ - Recommended plugins and settings

## Usage

- **Local Access**: ssh to your Raspberry Pi using the following command:
```bash
ssh pi-username@pi-ip
# Will ask for Raspberry Pi password to let you in
```
or you can also use smb by typing:
    *smb://your-pi-ip*

- **Remote Access**: Connect via Tailscale -> use the same SMB path
- **Web UI**: OMV at *http://<tailscale-ip>:80*

## Remote Access

Tailscale provides:
    - **Zero-config** secure VPN
    - **MagicDNS** (e.g., mypiserver.tailnet.ts.net)
    - **ACLs** for fine-grained access control
    - **Exit node** capability (route all traffic through home)

## Monitoring & Maintenance

- **OMV Dashboard**
- *htop* and *iotop*
- **Optional**: Netdata or Prometheus stack (I don't really use them, but saw they are pretty good)

## Security COnsiderations

- **Never** expose OMV posts directly to the internet
- Use **strong passwords** + 2FA where possible 
- Keep **system and containers** updated
- **Regular Backups** (3-2-1 rule)
- Tailscale **ACLs and SSH** key-only authentication

## Future Improvements

- Implement automated backups to *Backblaze B2*
- Set up **WireGuard** fallback
- **Home Assistant** integration
- **PiKVM** for *remote hardware management*

---

Thanks for reading this project's documentation. I intend to make everything as secure as possible whether it is through coding or the implementation of already created applications. Any feedback on the project is welcome!