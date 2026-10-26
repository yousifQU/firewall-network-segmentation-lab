# Project 1 Troubleshooting

## 1. pfSense VM would not boot

- **Symptom:** the pfSense installer would not boot after the VM was created following the Windows VM template.
- **Cause:** Secure Boot was enabled. pfSense is FreeBSD-based and failed to boot with it enabled.
- **Fix:** create the VM with Secure Boot **off**. Keep Secure Boot and the vTPM for Windows 11.
- **Lesson:** check platform requirements per guest OS instead of reusing one template.

## 2. Hosts behind VyOS could not reach external addresses (e.g. 8.8.8.8)

- **Symptom:** a DC could ping VyOS's internal address but nothing beyond it.
- **Checks:** firewall and ICMP rules, then IP configuration on the router.
- **Cause:** the default gateway was set to a host in the public subnet (`.33.1`) instead of the real upstream gateway. Packets had no valid next hop.
- **Fix:** correct the static default route to the real upstream gateway.
- **Lesson:** when the first hop works and the second does not, check the routing table before the firewall rules.

## 3. Hosts behind VyOS reached the router but not the internet

- **Symptom:** after fixing the route, internal hosts still got no replies from external hosts.
- **Diagnosis:** packet inspection showed traffic leaving the WAN with **private source addresses**, so return traffic could not come back.
- **Cause:** no source NAT rule for the private network.
- **Fix:** add source NAT (masquerade) scoped to the WAN interface.
- **Lesson:** compare captures on the inside and outside interfaces. Unchanged source addresses on the outside point to NAT.
