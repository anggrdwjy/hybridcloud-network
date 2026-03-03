## Overview Network Technology

This network technology running 24Hour/7days in my labs and production system with harderning best practice model.

<p align="center">
<img src="img/banner.png">
</p>

### Information

Virtual Cloud Computing :
- VPS (AWS/GCP/DigitalOcean/Hostinger/Local Provider VPS) 1 CPU, 1 RAM, 30GB Storage
- Install MikroTik CHR x86 on VPS (Include license P1)
- 1 Public IP

Local Hardware (Consumer Grade) :
- Router MikroTik RB750/RB951/RB2011/CHR x86 (CHR x86 include license P1)
- PC Intel i5 Gen 10, RAM 32GB, HDD 1TB and SSD 500GB (For Production Case)
- LAN

System Open-Source Software :
- Proxmox Virtualization Environment (For Virtualizaition Host)
- MikroTik RouterOS 6.49.18 or Newer

Etc :
- Electircal system support 24/7
- Internet Broadband FTTH (Minimal Bandwidth 10MB or Higher)
- Backup GSM Internet (Optional Case)

## Configuration Scope

### Router Scope

MikroTik Configuration :
- Loopback IP (Support Routing OSPF, Routing BGP)
- Static IP (IP Virtualization Host, Tunnel L2TP, SSTP)
- DHCP Server (IP Virtualization Host, LAN Management)
- DHCP Client (Access Internet For Router Local)
- Tunnel SSTP (P2P OSPF Network)
- Tunnel L2TP (VPN Access)
- VLAN (Management, Production)
- Static Routing (Routing to Public IP VPS)
- Routing OSPF (Routing for Hope Router)
- Routing BGP (Routing for Private IP and VPN Access)
- Routing Filter OSPF and BGP (Accept, Discard Rules)
- Firewall NAT (Src-NAT, Dst-NAT, Access List NAT, Masquerade)
- User Management (Username, Password, Group)
- Harderning IP Services Router (Disable Port, Custom Port Services)
- DNS Over HTTPs (Cloudflare Case)

### Virtualization Scope

Proxmox Configuration :
- Disable Proxmox-Subscription
- VLAN Management
- VLAN Host Virtualization
- Harderning (Fail2ban, SSH)

## Build Router on VPS

### Setup VPS

- Order VPS (AWS/GCP/DigitalOcean/Hostinger/Local Provider VPS)
- Change type OS MikroTik CHRx86 or Ubuntu Newer (Special Case)
- Access VPS via SSH from Public IP or Console from platform

### MikroTik Running on Ubuntu VPS (Special Case)

- Access VPS via SSH from Public IP
- Update Ubuntu `apt update && apt upgrade -y`
- Install Git `apt install git -y`
- Clone Script Install MikroTik on Ubuntu VPS, Access Detail Documentation : "https://github.com/anggrdwjy/mikrotik-ubuntukvm.git"
- First Step, Access MikroTik via Winbox from Public IP
- Step two, Change New Password (Please Harderning Username Password First)
- Install License P1 or Upgrade License
- Ping 1.1.1.1 or 8.8.8.8 from MikroTik Router
- Request Time Out (RTO) Ping, Check your DNS from MikroTIk Router until Replay Response

## Configuration Router

## Baremetal Server

## Virtualization Host on Baremetal

## Testing

## Support
