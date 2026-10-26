# Skills Matrix

| Skill | Where demonstrated | Evidence in this repository |
|-------|--------------------|-----------------------------|
| Network segmentation and DMZ design | Projects 1, 3 | Architecture diagrams, policy table |
| Firewall policy (stateful, default deny) | Project 1 | [VyOS reference config](../01-firewall-configuration/configs/vyos-site-b-reference.cfg) |
| NAT (source, destination, 1:1) | Projects 1, 3 | Same config, and the 1:1 NAT note in Project 3 |
| Packet-flow reasoning | Project 1 | Root-cause analysis in the troubleshooting notes |
| Site-to-site VPN (IPsec, WireGuard, OpenVPN) | Project 2 | [VPN configs](../02-vpn/configs/) |
| Remote-access VPN (IKEv2, L2TP/IPsec, OpenVPN, WireGuard) | Project 2 | Inventory table and protocol comparison |
| RADIUS and AD integration (NPS) | Projects 2, 4 | [NPS notes](../02-vpn/configs/nps-radius.md) |
| Sensor deployment (Suricata, Security Onion) and PCAP workflow | Project 3 | Attack-to-signature table and workflow |
| Attack simulation in an isolated lab | Project 3 | Scenario table (CVE, module, detection signature) |
| 802.1X, MAB, CoA, VLAN-based NAC | Project 4 | [AOS-CX reference](../04-network-access-control/configs/aos-cx-port-access-reference.cfg) |
| Virtualization and network lab building | All | Nutanix AHV and EVE-NG |
| Troubleshooting and reporting | All | `troubleshooting.md` in each project |
