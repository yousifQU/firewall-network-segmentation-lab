# RADIUS with Windows NPS (reference)

Used by the OpenVPN, IKEv2, and L2TP/IPsec remote-access VPNs (and reused in Project 4).

1. **Install the role:** Server Manager → Add Roles and Features → *Network Policy and Access Services* → Network Policy Server.
2. **Register NPS in Active Directory:** NPS console → right-click *NPS (Local)* → *Register server in Active Directory*.
3. **Add each VPN gateway as a RADIUS client:** RADIUS Clients → New. Enter a friendly name and the gateway's address, then generate a shared secret.
   - The address must match the **source address NPS actually sees**. For a gateway that reaches NPS through a tunnel (IKEv2 on Remote-B via the WireGuard tunnel), that is the tunnel address (`10.10.10.2`), not the gateway's LAN address.
4. **Create a Network Policy:** condition on a Windows group (for example, a "VPN Users" group); grant access; select only the authentication methods the gateway actually uses (MS-CHAPv2 for L2TP/PPP and EAP-MSCHAPv2, PAP if a gateway can only send PAP).
5. **Windows Firewall:** NPS normally opens UDP 1812/1813 automatically. Confirm.
6. **Use long, unique secrets per client.** PAP carries the password inside the RADIUS packet, protected only by the shared secret, so keep the RADIUS path on a trusted network.

Common failure causes, in rough order: shared-secret mismatch, wrong client IP in NPS, policy conditions that do not match the user, unsupported auth method, firewall.
