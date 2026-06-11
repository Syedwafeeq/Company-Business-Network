#  Testing & Verification Results

All configurations were systematically tested after implementation. Results documented below.

---

## 1. VLAN & Connectivity Tests

### Same-VLAN Communication
| Source | Destination | VLAN | Result |
|---|---|---|---|
| Finance-PC1 | Finance-PC2 | 20 |  Pass |
| HR-PC1 | HR-PC2 | 30 |  Pass |
| IT-PC1 | IT-PC2 | 40 |  Pass |

### Inter-VLAN Communication (Permitted)
| Source | Destination | Route Via | Result |
|---|---|---|---|
| Finance-PC1 (VLAN 20) | Web Server (VLAN 50) | Core SVI |  Pass |
| IT-PC1 (VLAN 40) | DNS Server (VLAN 50) | Core SVI |  Pass |
| Mgmt-PC (VLAN 10) | Any VLAN | Core SVI |  Pass |

---

## 2. ACL Tests (Security Enforcement)

| Source | Destination | Expected | Result |
|---|---|---|---|
| Finance-PC (192.168.20.x) | HR Server (192.168.30.x) |  Blocked |  Pass |
| HR-PC (192.168.30.x) | Finance PC (192.168.20.x) |  Blocked |  Pass |
| IT-PC (192.168.40.x) | Finance (192.168.20.x) |  Allowed |  Pass |

> ACL verification command: `show access-lists`

---

## 3. DHCP Tests

| Device | VLAN | IP Received | Gateway | DNS | Result |
|---|---|---|---|---|---|
| Finance-PC1 | 20 | 192.168.20.10 | 192.168.20.1 | 192.168.50.10 |  Pass |
| HR-PC1 | 30 | 192.168.30.10 | 192.168.30.1 | 192.168.50.10 |  Pass |
| IT-PC1 | 40 | 192.168.40.10 | 192.168.40.1 | 192.168.50.10 |  Pass |
| WiFi-Laptop | 60 | 192.168.60.10 | 192.168.60.1 | 192.168.50.10 |  Pass |

> DHCP verification: `show ip dhcp binding` on router

---

## 4. NAT / Internet Access Tests

| Device | Test | NAT Translation | Result |
|---|---|---|---|
| Finance-PC1 | ping 8.8.8.8 | 192.168.20.10 → 10.10.10.2 |  Pass |
| HR-PC1 | ping 8.8.8.8 | 192.168.30.10 → 10.10.10.2 |  Pass |
| WiFi-Laptop | ping 8.8.8.8 | 192.168.60.10 → 10.10.10.2 |  Pass |

> NAT verification: `show ip nat translations`

---

## 5. SSH Tests

| Device | SSH From | Username | Result |
|---|---|---|---|
| HQ-Router | Mgmt-PC | admin |  Pass |
| Core-Switch | Mgmt-PC | admin |  Pass |
| Access-SW-Finance | Mgmt-PC | admin |  Pass |

> Telnet attempt:  Refused (as expected — disabled on all devices)

---

## 6. Port Security Tests

| Switch | Port | Test | Result |
|---|---|---|---|
| Access-SW-Finance | Fa0/1 | Connect authorised PC |  Port Active |
| Access-SW-Finance | Fa0/1 | Connect second PC (diff MAC) |  Port Shutdown (violation) |
| Access-SW-HR | Fa0/3 | Connect authorised PC |  Port Active |

> Port security status: `show port-security interface Fa0/1`

---

## 7. Wireless Tests

| Client | SSID | IP via DHCP | Ping to Server | Result |
|---|---|---|---|---|
| Laptop-WiFi1 | CompanyWiFi | 192.168.60.11 | 192.168.50.11 |  Pass |
| Laptop-WiFi2 | CompanyWiFi | 192.168.60.12 | 8.8.8.8 |  Pass |

---

## Summary

| Category | Tests Run | Passed | Failed |
|---|---|---|---|
| VLAN Connectivity | 6 | 6 | 0 |
| ACL Enforcement | 3 | 3 | 0 |
| DHCP Assignment | 4 | 4 | 0 |
| NAT / Internet | 3 | 3 | 0 |
| SSH Access | 3 | 3 | 0 |
| Port Security | 3 | 3 | 0 |
| Wireless | 2 | 2 | 0 |
| **Total** | **24** | **24** | **0** |

 All 24 test cases passed successfully.
