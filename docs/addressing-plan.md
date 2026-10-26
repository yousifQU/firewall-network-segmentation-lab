# Addressing Plan

Each project built a fresh environment, so addresses are scoped per project and are not meant to be globally unique. Public-side addresses use the documentation range `203.0.113.0/24` (RFC 5737). Private ranges use RFC 1918.

## Project 1: Firewalls

| Segment | Subnet | Gateway | Hosts |
|---------|--------|---------|-------|
| Public | 203.0.113.0/24 | 203.0.113.1 | Workstation .10, pfSense WAN .6, VyOS WAN .4 |
| HQ-A | 192.168.1.0/24 | 192.168.1.1 (pfSense) | DC-A .10 |
| DMZ-A | 192.168.3.0/24 | 192.168.3.1 (pfSense) | Web/FTP host .10 |
| HQ-B | 192.168.2.0/24 | 192.168.2.1 (VyOS) | DC-B .10 |
| DMZ-B | 172.16.10.0/24 | 172.16.10.1 (VyOS) | Web/FTP/TFTP host .10 |

## Project 2: VPNs (adds to Project 1)

| Segment / item | Value |
|----------------|-------|
| pfSense-2 WAN / Remote-A | 203.0.113.8 / 192.168.4.0/24 (gateway .1, host .10) |
| VyOS-2 WAN / Remote-B | 203.0.113.9 / 192.168.6.0/24 (gateway .1, host .10) |
| Public clients | 203.0.113.15 – .18 |
| WireGuard S2S tunnel | 10.10.10.0/24 (VyOS-1 .1, VyOS-2 .2) |
| OpenVPN S2S tunnel | 10.200.1.0/30 (pfSense-1 .1, VyOS-1 .2) |
| WireGuard remote-access tunnel | 10.6.210.0/24 |
| OpenVPN remote-access pool | 192.168.250.0/26 |
| IKEv2 remote-access pool | 192.168.200.0/24 |
| L2TP pool | 192.168.255.2 – .200 |

## Project 3: IDS/IPS

| Segment | Subnet | Hosts |
|---------|--------|-------|
| Public | 203.0.113.0/24 | Kali .10, pfSense-A WAN .5, pfSense-B WAN .4, Metasploitable 3 published at .6 |
| DMZ-A | 192.168.1.0/24 | Metasploitable 3 .10 |
| Private-A | 192.168.3.0/24 | Sensor .6, admin host .10 |
| Private-B | 192.168.2.0/24 | Inline sensor management .6, admin host .10 |
| DMZ-B | 172.16.10.0/24 | Metasploitable 2 .7 |

## Project 4: NAC

| Segment | Subnet | Hosts |
|---------|--------|-------|
| Public | 203.0.113.0/24 | pfSense WAN .24 |
| Private | 192.168.1.0/24 | DC/NPS .10 |
| Captive portal | 192.168.2.0/24 | Windows 11 client (DHCP) |
| NAC | 192.168.3.0/24 | PacketFence .20, EVE-NG .50, AOS-CX management .60 |
