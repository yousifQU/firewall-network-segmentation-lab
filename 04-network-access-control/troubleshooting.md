# Project 4 Troubleshooting

## 1. Captive-portal login failed or hung

- **Symptom:** the login page loaded, but valid domain credentials were rejected or the page hung.
- **Checked and corrected:** DNS settings on pfSense and the client; pfSense reachability to the DC; NPS registration in AD; the shared secret re-entered on both sides; firewall rules and the Windows firewall for RADIUS (UDP 1812/1813).
- **Cause:** mismatched RADIUS settings (notably the shared secret) plus prerequisite DNS errors.
- **Fix:** aligned the secret and corrected DNS.
- **Lesson:** debug in order: network path, RADIUS client entry and secret, NPS policy match, then the authentication method.

## 2. Client on the captive network was not redirected

- **Symptom:** the Windows client either had no access or browsed freely without a redirect.
- **Checked:** DHCP lease and gateway, firewall rules on the captive interface, and the interface the zone was bound to.
- **Cause:** the captive-portal zone was bound to the wrong interface, and the DHCP options were wrong.
- **Fix:** enable the portal on the captive interface, fix DHCP options, and renew the lease.
- **Lesson:** confirm which interface the zone is bound to first.

## 3. PacketFence enforcement not working

- **Symptom:** endpoints behind the switch stayed in a denied state.
- **Checked:** cloud mapping and routing in EVE-NG, PacketFence enforcement mode, switch RADIUS configuration and secret, how the switch was added in PacketFence, and the VLAN IDs on both sides.
- **Cause:** a mismatch between VLAN IDs, and a RADIUS shared secret that differed between switch and PacketFence.
- **Fix:** aligned the VLAN IDs and RADIUS settings, after which clients were placed in the registration, production, or deny states according to policy.
- **Lesson:** verify the switch ↔ PacketFence RADIUS exchange end to end before testing endpoint behavior.
