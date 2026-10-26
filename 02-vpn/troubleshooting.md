# Project 2 Troubleshooting

## 1. WireGuard remote-access: no handshake

- **Symptom:** the pfSense status page showed no handshake received. The client could ping the server's public address, but the tunnel address was unreachable.
- **Checks:** WAN rule allowing UDP/51820; matching public keys on both sides; the listen port; allowed IPs covering the client's tunnel address and the remote LAN; whether the tunnel traffic was being NATed.
- **Resolution recorded in the lab:** after excluding the tunnel from automatic outbound NAT and correcting allowed IPs on both peers, the handshake completed.
- **Note:** a handshake fails at the key-exchange stage. The usual causes are wrong keys, a blocked UDP port, or a wrong endpoint. Outbound NAT on the tunnel interface mainly breaks *traffic* after the handshake. If you hit this symptom, verify keys and the WAN rule first.

## 2. OpenVPN remote-access: connected but no traffic

- **Symptom:** authentication succeeded and a tunnel address was assigned, but internal hosts and the internet were unreachable.
- **Checks:** firewall rules on the OpenVPN interface, routes to the internal network, tunnel-network consistency between server and client.
- **Cause:** the server was not telling clients which networks to route through the tunnel.
- **Fix:** set the correct *Local Network* for the internal subnet (pfSense generates the pushed route), or add an explicit push route in the custom options, then re-export the client configuration. Add a pass rule on the OpenVPN interface.
- **Lesson:** a successful login proves authentication, not routing. Check the client's routing table after connecting.

## 3. IPsec site-to-site: tunnel not establishing

- **Symptom:** Phase 1 stuck at "connecting" with log messages such as "no proposal chosen". Pings between the public addresses worked, but no encrypted traffic was seen.
- **Checks:** identical IKE and ESP proposals, pre-shared key, NAT traversal, and firewall rules allowing UDP 500 and 4500.
- **Resolution recorded in the lab:** proposals were aligned (AES-256, SHA-256, DH 14), the key was re-entered, UDP 500/4500 was allowed, and Phase 2 local/remote networks were corrected so each side's *local* subnet matched the other side's *remote* subnet.
- **Diagnostic mapping:**
  - Phase 1 not completing with "no proposal chosen" → encryption, hash, or DH mismatch, or an authentication problem.
  - Phase 1 up but Phase 2 down → traffic selectors (local/remote subnets) or ESP proposal mismatch.
  - Fix both ends in one session, then reconnect from the status page.
