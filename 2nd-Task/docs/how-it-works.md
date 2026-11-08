# How it works

Back to the [README](../README.md).

## Two subnets, one router

| LAN | Switch | Hosts | Network | Gateway on Router2 |
|---|---|---|---|---|
| Left | Switch0 | PC0, PC1 | 192.168.10.0/24 | 192.168.10.1 (`Gi0/0/0`) |
| Right | Switch1 | PC2, PC3 | 192.168.20.0/24 | 192.168.20.1 (`Gi0/0/1`) |

When a router port gets an `ip address` and `no shutdown`, the router automatically adds a **connected** route for that network. With both ports configured, Router2 already knows both LANs, so **no static route and no routing protocol is needed**.

## Why a switch cannot join the LANs

- Every port on a 2960 sits in **VLAN 1** by default (`show vlan brief`).
- **One VLAN is one broadcast domain**, so one subnet per switch.
- Moving traffic between two subnets needs a router, which is why Router2 sits between the switches.

## What a PC does with a ping

1. **Same LAN** (PC0 to PC1): the destination is in the same subnet, so PC0 ARPs for PC1's MAC and sends the frame directly. The reply TTL is **128**, the Windows default.
2. **Other LAN** (PC0 to PC2): the destination is outside the subnet, so PC0 sends the frame to its **default gateway** (`192.168.10.1`). Router2 forwards it out `Gi0/0/1`. The reply TTL is **127** because the router is one hop.

If the gateway is empty, the second case fails while the first still works.

## What the tables show

- **ARP table on PC0** (`arp -a`): contains `192.168.10.1` (Router2) and `192.168.10.11` (PC1). There is no `192.168.20.x` entry because PC0 never ARPs for hosts on another LAN.
- **MAC table on Switch0** (`show mac address-table`): contains PC0, PC1 and Router2. PC2 and PC3 never appear because the router rewrites the frame, so Router2's MAC stands in for everything on the other LAN.
- **Traceroute from PC0** (`tracert 192.168.20.10`): hop 1 is `192.168.10.1`, hop 2 is the target. One router, so TTL drops by 1.

## Reverse direction

From PC2, `ping 192.168.20.11` (PC3) replies with TTL 128 and `ping 192.168.10.10` (PC0) replies with TTL 127. The reverse path works with no extra router setup.
