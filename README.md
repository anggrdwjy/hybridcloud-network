## Overview Network Technology

<p align="center">
<img src="img/banner.png">
</p>

### Information

- Virtual Private Server (AWS/GCP/DigitalOcean/Hostinger/Local Provider VPS) 1 CPU, 1 RAM, 30GB Storage Include Public IP
- OS Virtual Private Server, MikroTik RoS 6.49.xx or Newer
- RouterBoard MikroTik RB750/RB951/RB2011 or MikroTik CHR x86 (CHR x86 include license P1) RoS 6.49.xx or Newer
- Build PC Intel i5 Gen 10, RAM 32GB, HDD 1TB and SSD 500GB
- Proxmox Virtualization Environment (For Virtualizaition Host)
- Electircal system support 24/7
- Internet Broadband FTTH (Minimal Bandwidth 10MB or Higher)

## Network Configuration Scope

### MikroTik Configuration 
- Static IP 
- DHCP Server and DHCP Client
- Tunneling VPN SSTP, L2TP
- VLAN Management
- Routing Static, OSPF, BGP, Route Filter
- Firewall NAT, Port Forwarding
- User Management
- Harderning IP Services
- DNS Over HTTPs (Cloudflare Case)

### Proxmox Configuration
- Disable Proxmox-Subscription
- VLAN Management
- VLAN Host Virtualization
- Harderning

## Build Router on Virtual Private Server

### Setup Virtual Private Server
- Order VPS (AWS/GCP/DigitalOcean/Hostinger/Local Provider VPS)
- Change type OS MikroTik CHRx86 or Ubuntu Newer (Special Case VPS)
- Access VPS via SSH from Public IP or Console from platform

### Install MikroTik on Ubuntu VPS (Special Case VPS)
- Access VPS via SSH from Public IP
- Update Ubuntu `apt update && apt upgrade -y`
- Install Git `apt install git -y`
- Clone Script Install MikroTik on Ubuntu VPS, Access Detail Documentation : "https://github.com/anggrdwjy/mikrotik-ubuntukvm.git"
- First Step, Access MikroTik via Winbox from Public IP
- Step two, Change New Password (Please Harderning Username Password First)
- Check and Validation License, Install License P1 or Upgrade License
- Ping 1.1.1.1 or 8.8.8.8 from MikroTik Router
- Request Time Out (RTO) Ping, Check your DNS from MikroTIk Router until Replay Response

## Configuration Router

### Step 1. MikroTik CHR on VPS

#### Harderning Username and Password (Add New Username, Group Full Admin, and Delete Default Username Admin)

<p align="left">
<img src="img/user-list.png">
</p>

#### Add New User
```
/user add name=user.mikrotik password=changemenow group=full
```

#### Remove User (Admin Default)
```
/user remove admin
```

#### Check List User
```
/user print
```

#### Custom Port IP Services (SSH, Winbox and Disable Port API, API-SSL, FTP, Telnet, WWW, WWW-SSL)
```
/ip service
set telnet disabled=yes
set ftp disabled=yes
set www disabled=yes
set ssh port=23452  \\ Custom Port SSH
set api disabled=yes
set winbox port=58291  \\ Custom Port Winbox
set api-ssl disabled=yes
```

#### Setup Static Routing (Set Public IP and Routing to 0.0.0.0/0)
```
/ip route
add distance=1 gateway=103.xx.yy.zzz
```

#### Setup DNS (Set 1.1.1.1, 1.0.0.1 or 8.8.8.8, 8.8.4.4)
```
/ip dns
set allow-remote-requests=yes servers=1.1.1.1,1.0.0.1,8.8.8.8,8.8.4.4
```

#### Setup VPN SSTP Tunnel (For Between Routers and Custom Port SSTP)

* Setup Profile SSTP
```
/ppp secret
add local-address=10.13.3.4 name=sstp.proxmox password=changeme profile=default-encryption remote-address=10.13.3.5 service=sstp
```

* Setup VPN SSTP
```
/interface sstp-server server
set default-profile=default-encryption enabled=yes port=49431
```

#### Setup VPN L2TP Tunnel (For VPN Access to Local Network)

* Setup Profile L2TP
```
/ppp secret
add local-address=10.13.3.0 name=vpn.l2tp1 password=changeme profile=default-encryption remote-address=10.13.3.1 service=l2tp
add local-address=10.13.3.2 name=vpn.l2tp2 password=changeme profile=default-encryption remote-address=10.13.3.3 service=l2tp
```

* Setup VPN L2TP
```
/interface l2tp-server server
set enabled=yes ipsec-secret=changemenow use-ipsec=yes
```

#### Setup Firewall NAT (Set Out Interface and SRC-NAT to Public IP)
```
/ip firewall nat
add action=src-nat chain=srcnat out-interface=ether1 to-addresses=103.xx.yy.zz
```

#### Disable Neighbor Discovery
```
/ip neighbor discovery-settings
set discover-interface-list=none protocol=""
```

#### Disable SMB Default MikroTik
```
/ip smb
set allow-guests=no
```

#### Disable Bandwidth-Server
```
/tool bandwidth-server
set authenticate=no enabled=no
```

#### Set System Identity and System Clock

* System Indentity
```
/system identity
set name=INETGW-CHRx86-VPS
```

* System Clock
```
/system clock
set time-zone-name=Asia/Jakarta
```

### Step 2. MikroTik RB2011 (Router Local)

### Step 3. Routing OSPF

### Step 4. Routing BGP

### Step 5. Routing Filter

## Baremetal Server

## Virtualization Host on Baremetal

## Testing

## Support
