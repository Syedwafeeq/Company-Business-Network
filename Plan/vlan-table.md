# VLAN Assignment Table

## VLAN Overview

| VLAN ID | Name | Department | Network | Ports (Access Layer) |
|---|---|---|---|---|
| 1 | Default | (Unused / Disabled) | — | None |
| 10 | MGMT | Management | 192.168.10.0/24 | SW-Mgmt Fa0/1–10 |
| 20 | FINANCE | Finance Department | 192.168.20.0/24 | SW-Finance Fa0/1–15 |
| 30 | HR | Human Resources | 192.168.30.0/24 | SW-HR Fa0/1–15 |
| 40 | IT | IT / Support | 192.168.40.0/24 | SW-IT Fa0/1–20 |
| 50 | SERVERS | Server Farm | 192.168.50.0/24 | SW-IT Fa0/21–24 |
| 60 | WLAN | Wireless / Guest | 192.168.60.0/24 | AP uplinks |
| 99 | NATIVE | Native VLAN (Trunks) | — | Trunk ports |

## Trunk Links

| From | To | Trunk VLANs Allowed |
|---|---|---|
| HQ Router | Core Switch | All (1–1005) |
| Core Switch | Distribution SW1 | 10, 20, 30, 40, 50, 60, 99 |
| Core Switch | Distribution SW2 | 10, 20, 30, 40, 50, 60, 99 |
| Distribution SW1 | Access SW-Finance | 20, 99 |
| Distribution SW1 | Access SW-HR | 30, 99 |
| Distribution SW2 | Access SW-IT | 40, 50, 99 |
| Distribution SW2 | Access SW-Mgmt | 10, 60, 99 |
