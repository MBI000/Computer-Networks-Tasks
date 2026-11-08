# Command reference

Back to the [README](../README.md).

## Router and switch (IOS CLI)

| Command | Purpose |
|---|---|
| `enable` | Enter privileged mode (the prompt changes from `>` to `#`) |
| `configure terminal` | Enter global configuration mode |
| `hostname <name>` | Rename the prompt; changes nothing else |
| `interface gigabitEthernet 0/0/x` | Select a router port (prompt becomes `(config-if)`) |
| `description <text>` | Optional label for a port |
| `ip address <ip> <mask>` | Assign the port address; on a router LAN port this is the gateway |
| `no shutdown` | Turn the port on; two messages confirm link and line protocol up |
| `exit` | Leave the current sub-mode |
| `end` | Leave configuration mode (the `%SYS-5-CONFIG_I` message is normal) |
| `show running-config` | Print the active configuration held in RAM |
| `copy running-config startup-config` | Save to NVRAM; press Enter to accept the filename, `[OK]` confirms |

`running-config` lives in RAM and is lost on reload. `startup-config` lives in NVRAM and survives a reload.

## Verification on the router

| Command | What to look for |
|---|---|
| `show ip interface brief` | Both configured ports read `up / up`. `administratively down` needs `no shutdown`. `up / down` means the cable or the switch side is not ready |
| `show ip route` | `C` entries for `192.168.10.0/24` and `192.168.20.0/24`, `L` entries for the router's own addresses |

## Verification on a switch

| Command | What to look for |
|---|---|
| `show interfaces status` | `connected` means the cable is in and the other end is up; `notconnect` means nothing is plugged in. Three ports are connected on Switch0: PC0, PC1 and Router2 |
| `show vlan brief` | VLAN 1 holds every port by default; 1002 to 1005 are legacy default VLANs |
| `show mac address-table` | Run it after the pings. The switch learns each source MAC and the port it arrived on |

## On a PC (Desktop → Command Prompt)

| Command | Purpose |
|---|---|
| `ipconfig` | Show what the PC is really using; IPv4, mask and gateway must match the addressing plan |
| `ping <ip>` | Test reachability (the first ping can time out while ARP resolves) |
| `tracert <ip>` | List every router on the path; `tracert 192.168.20.10` from PC0 shows `192.168.10.1` then the target |
| `arp -a` | Show IP to MAC mappings learned from pings |
