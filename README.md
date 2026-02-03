Turning an Outdated MacBook Pro into an Ubuntu Server
__________________________
Project Overview:
This project documents how I converted an outdated MacBook Pro (13-inch, 2015 era) into a headless Ubuntu Server capable of functioning as a NAS (Network Attached Storage) and personal media server (similar to Netflix using Plex/Jellyfin).
_________________
The goal was to:

Extend the life of older hardware

Gain hands-on Linux server experience

Practice real-world troubleshooting (boot issues, networking, package management)

Build a portfolio-ready homelab project
_________________

This README is written as a reproducible, step-by-step guide.
________________

Hardware Used:

MacBook Pro 13” (Intel-based, unsupported by modern macOS)

USB flash drive (8GB+)

External internet source (Ethernet or phone USB tethering)
________________

Software Stack:

Ubuntu Server 24.04 LTS

systemd

netplan

NetworkManager

(Planned) Plex / Jellyfin

(Planned) Samba / NFS
_________________

Step 1: Create Ubuntu Server Bootable USB
On a working computer:

Download Ubuntu Server 24.04 LTS ISO from ubuntu.com

Flash the ISO to a USB drive using:

Balena Etcher (cross-platform)

or dd (Linux/macOS)


Example (Linux/macOS):
sudo dd if=ubuntu-24.04-live-server-amd64.iso of=/dev/diskX bs=4M status=progress
_________________

Step 2: Boot MacBook from USB

Insert USB into MacBook

Power on while holding Option (⌥)

Select EFI Boot

Choose Install Ubuntu Server
__________________

Step 3: Install Ubuntu Server

During installation:
Language: English

Keyboard: Default

Network: Skipped initially (Wi-Fi drivers not present yet)

Storage: Use entire disk

Profile:

Username: serveruser

Hostname: trackplix

Enable SSH: ✅ Yes

Installation completes and system reboots into terminal-only Ubuntu Server.
__________________

Step 4: First Boot Verification

Log in locally and verify system state:

lsb_release -a

uname -a

Confirmed:

Ubuntu Server 24.04 LTS

system boots successfully on legacy Apple hardware
_________________

Step 5: Troubleshooting Network Connectivity

[Problem Encountered]
Running:
sudo apt update

Resulted in:
Temporary failure resolving 'archive.ubuntu.com'

Root cause:
No active network connection

network-manager not installed

Wi-Fi unavailable by default on minimal server install
________________

Step 6: Establish Temporary Internet Connection

Solution A: USB Tethering (Used in This Project)

Connect smartphone to MacBook via USB

Enable USB Tethering / Personal Hotspot on phone

Ubuntu automatically detects new interface

Verify connection:
ping -c 4 google.com
___________

Step 7: Install Network Manager

Once internet access is available:

sudo apt update

sudo apt install network-manager -y

Enable and start service:

sudo systemctl enable NetworkManager

sudo systemctl start NetworkManager
__________
Step 8: Connect to Wi-Fi via CLI

List available networks

nmcli device wifi list

Connect to Wi-Fi:

sudo nmcli device wifi connect "SSID_NAME" password "PASSWORD"

Confirm:

ip a

ping -c 4 google.com
_____________
Step 9: System Update & Hardening

sudo apt update && sudo apt upgrade -y

sudo timedatectl set-timezone America/Phoenix

Optional:
Configure UFW firewall

Set static IP via netplan

___________
Step 10: Media Server (Planned)

Next steps (documented separately):
Install Plex or Jellyfin

Mount storage directories

Enable remote access

Set up Samba shares
_______________

Skills Demonstrated: 


Linux server installation on unsupported hardware

Bootloader & EFI troubleshooting

Network debugging (DNS, packages, services)

Headless server management

CLI-first administration

Homelab & self-hosting concepts
________________

Why This Matters?

This project demonstrates:

Resourcefulness with legacy systems

Real-world Linux problem solving

Comfort working without GUIs

Ability to document technical processes clearly
____________________



Author: 

Lawrence Becton Jr -
Aspiring Systems / Cloud / Infrastructure Engineer


_______________
License:

MIT (or Personal Educational Project)
