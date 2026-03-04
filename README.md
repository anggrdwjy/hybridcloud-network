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
- DHCP Server, DHCP Client
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

### Step 1. Basic Configuration

#### A. MikroTik CHR on VPS

#### Username and Password

* Add New User
```
/user add name=user.mikrotik password=changemenow group=full
```

* Check List User
```
/user print
```

#### System Identity and System Clock

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

#### Static IP
```
/ip address
add address=103.150.191.25/23 interface=ether1 network=103.150.190.0
```

#### Routing Static
```
/ip route
add distance=1 gateway=103.150.191.25
```

#### DNS Static
```
/ip dns
set allow-remote-requests=yes servers=1.1.1.1,1.0.0.1,8.8.8.8,8.8.4.4
```

#### B. MikroTik RB2011 (Router Local)

#### Username and Password

* Add New User
```
/user add name=user.mikrotikrb password=changemenow group=full
```

* Check List User
```
/user print
```

#### System Identity and System Clock

* System Indentity
```
/system identity
set name=ROUTECORE-RB2011-CPE
```

* System Clock
```
/system clock
set time-zone-name=Asia/Jakarta
```

#### DHCP Client
```
/ip dhcp-client
add default-route-distance=2 disabled=no interface=ether1
```

#### VLAN Management
```
/interface vlan
add interface=ether2 name=vlan12 vlan-id=12
add interface=ether2 name=vlan13 vlan-id=13
add interface=ether2 name=vlan2374 vlan-id=2374
```

#### Static IP
```
/ip address
add address=172.23.74.65/26 interface=vlan2374 network=172.23.74.64
add address=192.168.1.2/24 interface=ether1 network=192.168.1.0
add address=10.13.3.49/28 interface=vlan13 network=10.13.3.48
add address=10.12.2.49/29 interface=vlan12 network=10.12.2.48
```

#### DNS Static
```
/ip dns
set allow-remote-requests=yes servers=1.1.1.1,1.0.0.1,8.8.8.8,8.8.4.4
```

#### DHCP Server
* Set IP Pool
```
/ip pool
add name=dhcp_pool1 ranges=172.23.74.66-172.23.74.126
```

* Set DHCP Server
```
/ip dhcp-server
add address-pool=dhcp_pool1 disabled=no interface=vlan2374 name=dhcp1
```

* Set DHCP Server Network
```
/ip dhcp-server network
add address=172.23.74.64/26 dns-server=1.1.1.1,1.0.0.1 gateway=172.23.74.65
```

### Step 2. Firewall NAT

#### A. MikroTik CHR on VPS

#### Firewall NAT
```
/ip firewall nat
add action=src-nat chain=srcnat out-interface=ether1 src-address-list=access-list to-addresses=103.xx.yy.zz
```

#### Firewall Address-list
```
/ip firewall address-list
add address=10.13.3.0/31 list=access-list
add address=10.13.3.2/31 list=access-list
add address=10.13.3.48/28 list=access-list
add address=10.12.2.48/29 list=access-list
add address=172.23.74.64/26 list=access-list
```

#### B. MikroTik RB2011 (Router Local)

#### Firewall NAT
```
/ip firewall nat
add action=masquerade chain=srcnat out-interface=ether1 src-address-list=access-list
```

#### Firewall Address-list
```
/ip firewall address-list
add address=10.13.3.48/28 list=access-list
add address=10.12.2.48/29 list=access-list
add address=172.23.74.64/26 list=access-list
add address=10.15.0.0/24 list=access-list
```

### Step 3. VPN Tunnel SSTP and L2TP

#### A. MikroTik CHR on VPS

#### VPN Server SSTP
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

#### VPN Server L2TP
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

#### B. MikroTik RB2011 (Router Local)

#### VPN Client SSTP
* Setup Connection SSTP
```
/interface sstp-client
add add-default-route=yes connect-to=103.xx.yy.zz:49341 disabled=no http-proxy=0.0.0.0:49341 name=sstp-out1 password=changemenow profile=default-encryption user=sstp.proxmox
```

### Step 4. Routing OSPF

#### A. MikroTik CHR on VPS

* Set Loopback
```
/interface bridge
add name=Lo0
```

* Set IP Loopback
```
/ip address
add address=192.168.150.1 interface=Lo0 network=192.168.150.1
```

* Set OSPF Instance
```
/routing ospf instance
set [ find default=yes ] router-id=192.168.150.1
```

* Set OSPF Network Advertise
```
/routing ospf network
add area=backbone network=10.13.3.4/31
add area=backbone network=192.168.150.1/32
```

* Set OSPF Routing Filter
```
/routing filter
add action=accept chain=ospf-in prefix-length=31-32
add action=discard chain=ospf-in

```

#### B. MikroTik RB2011 (Router Local)

* Set Loopback
```
/interface bridge
add name=Lo0
```

* Set IP Loopback
```
/ip address
add address=192.168.150.2 interface=Lo0 network=192.168.150.2
```

* Set OSPF Instance
```
/routing ospf instance
set [ find default=yes ] router-id=192.168.150.2
```

* Set OSPF Network Advertise
```
/routing ospf network
add area=backbone network=10.13.3.4/31
add area=backbone network=192.168.150.2/32
```

* Set OSPF Routing Filter
```
/routing filter
add action=accept chain=ospf-in prefix-length=31-32
add action=discard chain=ospf-in
```

### Step 5. Routing BGP

#### A. MikroTik CHR on VPS

* Set BGP Routing Filter
```
/routing filter
add action=accept chain=bgp-out prefix=10.13.3.0/31 prefix-length=31
add action=accept chain=bgp-out prefix=10.13.3.2/31 prefix-length=31
add action=discard chain=bgp-out
```

* Set BGP Instance
```
/routing bgp instance
set default as=65000 client-to-client-reflection=no router-id=192.168.150.1
```

* Set BGP Peer
```
/routing bgp peer
add name=peer-ROUTECORE out-filter=bgp-out remote-address=192.168.150.2 remote-as=65000 tcp-md5-key=changemenow update-source=Lo0
```

* Set BGP Network Advertise
```
/routing bgp network
add network=10.13.3.0/31 synchronize=no
add network=10.13.3.2/31 synchronize=no
```

#### B. MikroTik RB2011 (Router Local)

* Set BGP Routing Filter
```
/routing filter
add action=accept chain=bgp-out prefix=10.13.3.0/31 prefix-length=31
add action=accept chain=bgp-out prefix=10.13.3.2/31 prefix-length=31
add action=discard chain=bgp-out
```

* Set BGP Instance
```
/routing bgp instance
set default as=65000 client-to-client-reflection=no router-id=192.168.150.
```

* Set BGP Peer
```
/routing bgp peer
add name=peer-INETGW out-filter=bgp-out remote-address=192.168.150.1 remote-as=65000 tcp-md5-key=changemenow update-source=Lo0
```

* Set BGP Network Advertise
```
/routing bgp network
add network=10.13.3.48/28 synchronize=no
add network=10.12.2.48/29 synchronize=no
add network=172.23.74.64/26 synchronize=no
add network=10.15.0.0/24 synchronize=no
```

## Baremetal Server

## Virtualization Host on Baremetal

## Testing

## Support
