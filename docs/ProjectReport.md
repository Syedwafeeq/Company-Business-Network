# Project Technical Report

## Enterprise Company Network Design & Implementation

---

## 1. Introduction

This project involves the design and implementation of a fully functional enterprise network for a mid-sized company with multiple departments. The goal was to create a network that is **secure**, **scalable**, **manageable**, and **cost-effective**, using industry-standard Cisco technologies and best practices.

The simulation was built entirely in **Cisco Packet Tracer**, replicating what a real-world deployment would look like in a corporate environment.

---

## 2. Requirements Analysis

### Business Requirements
- Separate network segments per department (Finance, HR, IT, Management)
- Internet access for all departments
- Secure remote management of all network devices
- Wireless access for employees in designated areas
- Server infrastructure accessible across departments (with controlled access)

### Technical Requirements
- Layer 2 segmentation using VLANs
- Layer 3 routing between segments
- Dynamic IP allocation for end devices
- Outbound internet via NAT
- Traffic filtering via ACLs
- Secure CLI access via SSH

---

## 3. Network Design

### 3.1 Topology Choice: Three-Tier Hierarchy

The **Cisco Three-Tier Hierarchical Model** was selected because it:
- Separates functions clearly (Core, Distribution, Access)
- Scales easily as the organization grows
- Reduces fault domains
- Simplifies troubleshooting

### 3.2 Layer 2 Design

- **VLANs** are used to logically separate department traffic
- **802.1Q Trunking** carries multiple VLANs across uplinks
- **Native VLAN** set to VLAN 99 (not default VLAN 1) for security
- All unused ports disabled and placed in an unused VLAN

### 3.3 Layer 3 Design

- **Inter-VLAN Routing via Switch Virtual Interfaces (SVIs)** on the Core Switch
- Each VLAN SVI acts as the default gateway for that segment
- **Static routing** used between the router and core switch
- **Default route** on HQ Router pointing to ISP

### 3.4 IP Addressing

- Private address space (RFC 1918): `192.168.0.0/16`
- /24 subnet per VLAN — sufficient for department sizes and easy to manage
- Servers assigned static IPs for reliability
- End-user devices receive IPs via DHCP

---

## 4. Technology Configuration Details

### 4.1 DHCP

DHCP pools are configured on the **HQ Router**, one pool per VLAN:

```
ip dhcp pool VLAN20-FINANCE
  network 192.168.20.0 255.255.255.0
  default-router 192.168.20.1
  dns-server 192.168.50.10
```

DHCP exclusions are set for the first 9 addresses in each subnet (reserved for infrastructure).

### 4.2 Inter-VLAN Routing (SVI)

Switch Virtual Interfaces configured on the Core Switch:

```
interface vlan 20
  ip address 192.168.20.1 255.255.255.0
  no shutdown
```

IP routing enabled on the multilayer switch:
```
ip routing
```

### 4.3 NAT Overload (PAT)

Allows all internal hosts to share the router's WAN IP for internet access:

```
ip nat inside source list NAT-ACL interface Serial0/0/0 overload
access-list 10 permit 192.168.0.0 0.0.255.255
```

Inside/outside interfaces designated on the router.

### 4.4 ACLs

Example: Block Finance VLAN from accessing HR VLAN directly:

```
ip access-list extended BLOCK-FIN-TO-HR
  deny ip 192.168.20.0 0.0.0.255 192.168.30.0 0.0.0.255
  permit ip any any
```

Applied inbound on the Finance SVI interface.

### 4.5 SSH Configuration

Configured on all routers and switches for secure management:

```
hostname HQ-Router
ip domain-name company.local
crypto key generate rsa modulus 1024
username admin secret Cisco@1234
line vty 0 4
  transport input ssh
  login local
ip ssh version 2
```

### 4.6 Port Security

Configured on all access-layer switch ports facing end devices:

```
interface FastEthernet0/1
  switchport mode access
  switchport access vlan 20
  switchport port-security
  switchport port-security maximum 1
  switchport port-security violation shutdown
  switchport port-security mac-address sticky
```

### 4.7 WLAN Configuration

- Cisco wireless access points connected to Access Switch (VLAN 60 uplink)
- SSID: `CompanyWiFi`
- Authentication: WPA2-Personal
- DHCP served from Core Switch SVI for VLAN 60

---

## 5. Testing & Verification

| Test | Command Used | Result |
|---|---|---|
| End-to-end ping (same VLAN) | `ping 192.168.20.x` |  Success |
| Inter-VLAN ping (Finance → IT) | `ping 192.168.40.x` |  Success |
| Finance → HR (ACL block) | `ping 192.168.30.x` |  Blocked |
| Internet access from PC | `ping 8.8.8.8` |  Success (via NAT) |
| SSH login to router | `ssh -l admin 192.168.10.x` |  Success |
| DHCP assignment | `ipconfig` on PC |  IP assigned |
| Port security trigger | Connect 2nd device |  Port shutdown |

---

## 6. Challenges & Solutions

| Challenge | Solution |
|---|---|
| VLANs not passing across trunk | Set native VLAN consistently on both ends; verified with `show interfaces trunk` |
| DHCP not assigning IPs | Added `ip helper-address` on SVI pointing to DHCP server |
| Inter-VLAN routing not working | Enabled `ip routing` on multilayer switch |
| ACL blocking unintended traffic | Used `permit ip any any` at end of ACL; verified with `show access-lists` |

---

## 7. Conclusion

This project successfully demonstrates the design and implementation of a secure, segmented enterprise network. All key networking technologies were configured, tested, and verified. The experience mirrors real-world enterprise deployments and reflects skills directly applicable to network engineering roles.
