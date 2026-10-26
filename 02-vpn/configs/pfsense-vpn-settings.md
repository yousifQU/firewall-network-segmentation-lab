# pfSense VPN settings (reference)

GUI paths and values only; no keys. Verify against the Netgate documentation for your version.

## IPsec site-to-site (pfSense-1 ↔ pfSense-2)

**VPN → IPsec → Add P1**

| Setting | Value |
|---------|-------|
| Key Exchange | IKEv2 |
| Interface / Remote Gateway | WAN / peer WAN address |
| Authentication | Mutual PSK (`<IPSEC_PSK>`) |
| Encryption / Hash / DH | AES 256 / SHA256 / group 14 (2048-bit) |
| Child SA close action | Restart/Reconnect |

**Add P2**

| Setting | Value |
|---------|-------|
| Mode | Tunnel IPv4 |
| Local network | pfSense-1: `192.168.1.0/24` · pfSense-2: `192.168.4.0/24` |
| Remote network | The other side's subnet (mirror image) |
| Protocol / Encryption | ESP / AES256-GCM |
| PFS group / Lifetime | 14 / 3600 s |

Firewall: allow UDP 500 and 4500 on WAN from the peer, and add pass rules on the *IPsec* tab for the tunnel traffic.

## OpenVPN site-to-site, pfSense-1 side (DMZ-A ↔ DMZ-B)

**VPN → OpenVPN → Servers → Add**

| Setting | Value |
|---------|-------|
| Server mode | Peer to Peer (Shared Key) |
| Protocol / Port | UDP / 1194 |
| Shared key | Auto-generate, copy to the VyOS side, and do not publish |
| Encryption / Auth digest | AES-256-CBC / SHA512 (must match VyOS) |
| IPv4 tunnel network | `10.200.1.0/30` |
| IPv4 remote network | `172.16.10.0/24` |

Firewall: WAN pass UDP/1194 from VyOS-1, plus pass rules on the OpenVPN tab.
Shared-key mode is deprecated in current OpenVPN. Prefer SSL/TLS (PKI) mode for anything beyond a lab.

## OpenVPN remote access (pfSense-1)

1. **Install** the OpenVPN client-export package (System → Package Manager).
2. **System → Certificates:** create a CA and a server certificate.
3. **VPN → OpenVPN → Servers → Add:** mode *Remote Access (User Auth)*, backend for authentication = the RADIUS server defined under System → User Manager → Authentication Servers (pointing at DC-A), IPv4 tunnel network `192.168.250.0/26`, local network `192.168.1.0/24`.
4. **VPN → OpenVPN → Client Export:** download the Windows installer for the user.
5. A local user can be created to test the tunnel before RADIUS is wired in.

## WireGuard remote access (pfSense-2)

1. **System → Package Manager:** install *WireGuard*.
2. **VPN → WireGuard → Tunnels → Add:** enable, listen port 51820, generate keys, address `10.6.210.1/24`.
3. **Peers → Add:** one per client, with the client's public key and allowed IPs `10.6.210.2/32`.
4. **Firewall:** WAN pass UDP/51820; pass rules on the WireGuard tab for traffic to `192.168.4.0/24`.
5. **Client** (Windows WireGuard app), split tunnel:

```ini
[Interface]
PrivateKey = <CLIENT_PRIVATE_KEY>
Address = 10.6.210.2/24

[Peer]
PublicKey = <PFSENSE_PUBLIC_KEY>
AllowedIPs = 10.6.210.0/24, 192.168.4.0/24
Endpoint = 203.0.113.8:51820
PersistentKeepalive = 25
```

A full tunnel (`AllowedIPs = 0.0.0.0/0`) additionally needs outbound NAT for the tunnel network and a DNS server. If you use the client's block-untunneled-traffic option, confirm the tunnel actually works first.
