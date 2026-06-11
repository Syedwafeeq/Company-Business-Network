# Enterprise Company Network Design & Implementation
### Cisco Packet Tracer | Hierarchical Network Architecture

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
![VLANs](https://img.shields.io/badge/VLANs-Configured-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Ongoing-brightgreen?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Fresher%20%7C%20Entry--Level-orange?style=for-the-badge)

---

## Project Overview

This project demonstrates the **end-to-end design and implementation** of a secure, scalable enterprise network for a multi-department company using Cisco Packet Tracer. The network follows a **three-tier hierarchical model** (Core → Distribution → Access) and incorporates industry-standard technologies used in real-world enterprise environments.

> **Context:** Designed as a capstone networking project to demonstrate practical skills in enterprise network design, IP addressing, security, and routing — technologies central to Cisco's networking solutions.

---
---

## Technologies Implemented

| Technology | Purpose | Layer |
|---|---|---|
| **Hierarchical Network Design** | 3-tier architecture (Core/Dist/Access) | All |
| **VLANs** | Department segmentation | Layer 2 |
| **Inter-VLAN Routing (SVI)** | Communication between VLANs | Layer 3 |
| **DHCP** | Automatic IP assignment per VLAN | Layer 3 |
| **NAT Overload (PAT)** | Internet access for internal hosts | Layer 3 |
| **ACLs** | Traffic filtering & access control | Layer 3 |
| **SSH** | Secure remote device management | Management |
| **Port Security** | MAC-based switch port protection | Layer 2 |
| **WLAN** | Wireless access point configuration | Layer 2 |
| **Static IPv4 Addressing** | Servers & infrastructure devices | Layer 3 |

---

## Network Architecture

### Three-Tier Hierarchical Model

```
                    ┌─────────────┐
                    │   INTERNET  │
                    │    (ISP)    │
                    └──────┬──────┘
                           │ WAN Link
                    ┌──────┴──────┐
                    │  HQ ROUTER  │  ← NAT/PAT, ACL, SSH
                    │  (Gateway)  │
                    └──────┬──────┘
                           │
              ─────────────────────────
                      CORE LAYER
              ─────────────────────────
                    ┌──────┴──────┐
                    │ CORE SWITCH │  ← Inter-VLAN Routing (SVI)
                    └──┬──────┬───┘
                       │      │
          ─────────────────────────────────
                   DISTRIBUTION LAYER
          ─────────────────────────────────
               ┌───┴───┐    ┌───┴───┐
               │ DIST  │    │ DIST  │
               │  SW1  │    │  SW2  │
               └┬──┬───┘    └──┬──┬─┘
                │  │            │  │
          ───────────────────────────────
                     ACCESS LAYER
          ───────────────────────────────
         ┌──┴──┐ ┌──┴──┐ ┌──┴──┐ ┌──┴──┐
         │ SW  │ │ SW  │ │ SW  │ │ SW  │
         │Fin. │ │ HR  │ │ IT  │ │Mgmt │
         └──┬──┘ └──┬──┘ └──┬──┘ └──┬──┘
            │       │       │       │
         PCs/    PCs/    PCs/    PCs/
         Phones  Phones  Servers WiFi APs
```

---

## VLAN Design

| VLAN ID | Department | Network | Default GW |
|---|---|---|---|
| VLAN 10 | Management | 192.168.10.0/24 | 192.168.10.1 |
| VLAN 20 | Finance | 192.168.20.0/24 | 192.168.20.1 |
| VLAN 30 | HR | 192.168.30.0/24 | 192.168.30.1 |
| VLAN 40 | IT / Support | 192.168.40.0/24 | 192.168.40.1 |
| VLAN 50 | Server Farm | 192.168.50.0/24 | 192.168.50.1 |
| VLAN 60 | WLAN / Guest | 192.168.60.0/24 | 192.168.60.1 |

---

## Security Implementation

### 1. Port Security
- Configured on all access-layer switch ports
- Max 1 MAC address per port
- Violation mode: **shutdown**

### 2. SSH Remote Access
- SSH v2 enabled on all routers and switches
- Telnet disabled
- Local user authentication configured

### 3. ACLs (Access Control Lists)
- Restrict inter-department traffic (e.g., HR cannot reach Finance servers)
- Block unauthorized access to server VLAN
- Applied inbound on router interfaces

### 4. NAT Overload (PAT)
- Internal RFC 1918 addresses translated to single public IP
- Only return traffic allowed back in
- Prevents direct external access to internal hosts

---

## Wireless LAN (WLAN)

- Cisco Wireless Access Points configured per floor/department
- SSID mapped to VLAN 60 (WLAN segment)
- DHCP served from core switch SVI
- WPA2 security configured

---

## Testing & Verification

All configurations were verified using:

- `ping` — end-to-end connectivity across VLANs
- `show ip route` — routing table verification
- `show vlan brief` — VLAN membership
- `show ip nat translations` — NAT/PAT entries
- `show port-security` — port security status
- `show run` — full configuration review

See [Testing Results](docs/testing-results.md) for detailed output.

---
*Built using Cisco Packet Tracer*
