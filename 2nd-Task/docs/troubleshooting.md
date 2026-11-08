# Troubleshooting

Back to the [README](../README.md).

| Symptom | Likely cause | Fix |
|---|---|---|
| Orange dots on the links | Ports are still converging | Wait for the dots to turn green |
| `show ip interface brief` shows `administratively down` | Port never enabled | Select the interface and run `no shutdown` |
| Router port shows `up / down` | Cable or switch side not ready | Check the cable type (Copper Straight-Through) and that the switch port is connected |
| First ping times out | ARP is still resolving | Run the ping again |
| Ping works inside the LAN but not across the router | Empty or wrong default gateway on the PC | Set the gateway to Router2's address on that LAN (`192.168.10.1` or `192.168.20.1`) |
| Nothing answers across the router, and the addresses look right | `Gi0/0/0` and `Gi0/0/1` are swapped relative to the cables | Swap the two interface configurations, or swap the cables |
| Router interface name rejected | Used the old two-part name (`G0/0`) | The PT8200 uses three-part names, such as `gigabitEthernet 0/0/0` |
| Configuration is gone after a reload | Only `running-config` was changed | Run `copy running-config startup-config` |
| PC shows the right address but the ping still fails | Settings entered on the wrong page | Run `ipconfig` and compare IPv4, mask and gateway with the addressing plan |

## Quick checklist

1. All links green?
2. `show ip interface brief` on Router2 shows both ports `up / up`?
3. `ipconfig` on the PC matches the [addressing plan](../README.md#addressing-plan)?
4. `ping` the gateway first, then the other LAN.
