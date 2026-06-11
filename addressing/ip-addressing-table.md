# IP Addressing Table

## WAN / Internet Segment

| Device | Interface | IP Address | Subnet Mask | Notes |
|---|---|---|---|---|
| ISP Router | Se0/0/0 | 10.10.10.1 | /30 | ISP Side |
| HQ Router | Se0/0/0 | 10.10.10.2 | /30 | WAN Interface |

## LAN Segments (per VLAN)

| Device | Interface | IP Address | Subnet Mask | Gateway |
|---|---|---|---|---|
| Core Switch | VLAN 10 SVI | 192.168.10.1 | /24 | — |
| Core Switch | VLAN 20 SVI | 192.168.20.1 | /24 | — |
| Core Switch | VLAN 30 SVI | 192.168.30.1 | /24 | — |
| Core Switch | VLAN 40 SVI | 192.168.40.1 | /24 | — |
| Core Switch | VLAN 50 SVI | 192.168.50.1 | /24 | — |
| Core Switch | VLAN 60 SVI | 192.168.60.1 | /24 | — |

## Static Assignments (Servers & Infrastructure)

| Device | IP Address | VLAN | Purpose |
|---|---|---|---|
| DNS Server | 192.168.50.10 | 50 | Domain resolution |
| HTTP/Web Server | 192.168.50.11 | 50 | Internal web |
| DHCP Server (Router) | 192.168.50.1 | 50 | Dynamic IP assignment |
| HQ Router LAN | 192.168.1.1 | 1 | Uplink to core |

## DHCP Pool Summary

| Pool Name | Network | Range | Gateway | DNS |
|---|---|---|---|---|
| VLAN10-POOL | 192.168.10.0/24 | .10 – .254 | 192.168.10.1 | 192.168.50.10 |
| VLAN20-POOL | 192.168.20.0/24 | .10 – .254 | 192.168.20.1 | 192.168.50.10 |
| VLAN30-POOL | 192.168.30.0/24 | .10 – .254 | 192.168.30.1 | 192.168.50.10 |
| VLAN40-POOL | 192.168.40.0/24 | .10 – .254 | 192.168.40.1 | 192.168.50.10 |
| VLAN60-POOL | 192.168.60.0/24 | .10 – .254 | 192.168.60.1 | 192.168.50.10 |

## NAT Translation

| Internal (Private) | External (Public) | Type |
|---|---|---|
| 192.168.0.0/16 | 10.10.10.2 | PAT (Overload) |

> **Note:** All internal addresses are translated to the router's WAN IP using Port Address Translation (PAT), allowing multiple internal hosts to share a single public IP.
