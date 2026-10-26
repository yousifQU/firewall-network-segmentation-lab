# Project 3 Troubleshooting

## 1. Kali Linux install kept failing

- **Symptom:** the installer failed every time after the configuration menus.
- **Tried:** toggling Secure Boot, the vTPM, memory and disk sizes, and memory overcommit, in several combinations.
- **Resolution:** the install succeeded once Secure Boot was disabled. Kali does not support UEFI Secure Boot.
- **Lesson:** change one VM setting at a time and record the result.

## 2. Could not SCP the PCAP to the sensor

- **Symptom:** SCP from the Windows host to Security Onion failed to connect every time.
- **Tried:** disabling the sensor's firewall, restarting `sshd`, and checking NIC settings.
- **Cause:** one interface on the sensor had a **/32** prefix instead of /24, so it had no route to the subnet.
- **Fix:** correct the prefix length, after which SCP worked.
- **Lesson:** when a service is up and the firewall is open, check addressing and prefix lengths.
- **Command note:** the import tool is `so-import-pcap`, run with the full path of the copied file.
