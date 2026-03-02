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
- Tunnel SSTP (P2P OSPF Network)
- Tunnel L2TP (VPN Access)
- VLAN (Management, Production)
- Static Routing (Routing to Public IP VPS)
- Routing OSPF (Routing for Hope Network)
- Routing BGP (Routing for Private IP and VPN Access)
- Routing Filter OSPF and BGP (Accept, Discard Rules)
- Firewall NAT (Src-NAT, Dst-NAT, Access List NAT, Masquerade)
- User Management (Username, Password, Group)
- Harderning IP Services Router (Custom Port services)
- DNS Over HTTPs (Cloudflare Case)

### Virtualization Scope

Proxmox Configuration :
- Disable Proxmox-Subscription
- VLAN Management
- VLAN Host Virtualization
- Harderning (Fail2ban)

## First Build Router on Cloud

## Router For Baremetal Server

## Virtualization Host on Baremetal

## Testing

## Support
