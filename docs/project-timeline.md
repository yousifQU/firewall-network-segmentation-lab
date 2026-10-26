# Project Timeline and Dependencies

The four projects were completed in sequence over roughly one academic term, at about one project per month. Exact dates are omitted. Each builds on infrastructure or concepts from the one before it.

```mermaid
flowchart TB
  subgraph S1["Stage 1: Firewalls"]
    A1["Virtual networks and VMs"] --> A2["pfSense and VyOS firewalls"]
    A2 --> A3["Two Active Directory domain controllers"]
    A3 --> A4["NAT, DMZ publishing, firewall policy"]
  end
  subgraph S2["Stage 2: VPNs"]
    B1["Add remote sites and public clients"] --> B2["Site-to-site VPNs"]
    B2 --> B3["RADIUS (NPS) on the DCs"]
    B3 --> B4["Client-access VPNs"]
  end
  subgraph S3["Stage 3: IDS/IPS"]
    C1["Attack baseline"] --> C2["Passive sensor"]
    C2 --> C3["Inline sensor"]
  end
  subgraph S4["Stage 4: NAC"]
    D1["Captive portal with AD/RADIUS"] --> D2["PacketFence with 802.1X and MAB"]
  end
  S1 --> S2 --> S3 --> S4
```

| Stage | Focus | Builds on | Key carry-over |
|-------|-------|-----------|----------------|
| 1 | Perimeter firewalls | New environment | Two firewall platforms, two DMZs, two AD domains |
| 2 | VPNs | Stage 1 sites | Firewalls become VPN gateways; DCs become RADIUS servers |
| 3 | Sensors | Stage 1 firewall and DMZ pattern | Same segmentation pattern used to place and test sensors |
| 4 | NAC | Stage 2 RADIUS/AD experience | NPS/RADIUS reused for captive-portal auth; RADIUS reused for 802.1X |

## Ordering within projects

Steps are presented in the order they depend on each other:

- **Project 2:** RADIUS (NPS) is configured *before* the client-access VPNs that authenticate against it. Site-to-site tunnels come first because one remote-access VPN reaches its RADIUS server across a site-to-site tunnel.
- **Project 3:** the attack baseline targets a host that already exists at that stage. The `vsftpd` scenario targets the DMZ host and therefore appears in Phase 2.
- **Project 4:** NPS/RADIUS setup precedes the captive portal that depends on it.
