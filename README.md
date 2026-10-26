# Secure Office Network Using VLANs and Inter-VLAN Routing

A Cisco Packet Tracer project that segments a small office network into three departments using VLANs, 802.1Q trunking and Router-on-a-Stick inter-VLAN routing.

| | |
|---|---|
| **Student** | Mark Gichuhi Gichohi |
| **Institution** | Multimedia University |
| **Program** | Computer Science |
| **Subject** | Computer Networking |
| **Software** | Cisco Packet Tracer |
| **Submission date** | 20 October 2026 |

## Repository contents

| File | Description |
|---|---|
| `Mark_Gichuhi_Gichohi_VLAN_Routing_Project.pkt` | The completed Packet Tracer network. Open it with Cisco Packet Tracer. |
| `Mark_Gichuhi_Gichohi_VLAN_Routing_Report.docx` | The full project report with configuration, verification screenshots, test results, Simulation Mode analysis, troubleshooting and conclusion. |

## Project overview

A company with three departments (HR, IT and Finance) needs its staff separated into different logical networks. This reduces unnecessary broadcast traffic, makes the network easier to manage and isolates the departments from each other. Traffic between departments is allowed only through the router.

The network uses one Cisco 2911 router, two Cisco 2960-24TT switches and six PCs. Each switch has one PC from every department, so each VLAN spans both switches over a trunk link.

```
                      Router1 (2911)
                            | Gi0/0  (802.1Q trunk, sub interfaces .10 .20 .30)
                            | Fa0/1
 HR-PC1  --Fa0/2--  [ Switch1 ]  --Fa0/24 (802.1Q trunk)-- Fa0/24--  [ Switch2 ]  --Fa0/3-- HR-PC2
 Fin-PC1 --Fa0/3--     |                                                |        --Fa0/4-- Fin-PC2
 IT-PC1  --Fa0/4--                                                      +--------Fa0/2-- IT-PC2
```

## Addressing plan

| VLAN | Name | Network | Gateway (router sub interface) |
|---|---|---|---|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 (Gi0/0.10) |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 (Gi0/0.20) |
| 30 | FINANCE | 192.168.30.0/24 | 192.168.30.1 (Gi0/0.30) |

| Device | Switch and port | VLAN | IP address | Subnet mask |
|---|---|---|---|---|
| HR-PC1 | Switch1 Fa0/2 | 10 | 192.168.10.11 | 255.255.255.0 |
| HR-PC2 | Switch2 Fa0/3 | 10 | 192.168.10.12 | 255.255.255.0 |
| IT-PC1 | Switch1 Fa0/4 | 20 | 192.168.20.11 | 255.255.255.0 |
| IT-PC2 | Switch2 Fa0/2 | 20 | 192.168.20.12 | 255.255.255.0 |
| FIN-PC1 | Switch1 Fa0/3 | 30 | 192.168.30.11 | 255.255.255.0 |
| FIN-PC2 | Switch2 Fa0/4 | 30 | 192.168.30.12 | 255.255.255.0 |

## Key configuration

**Switches** (both): VLANs 10, 20 and 30 created, PC ports set as access ports in their department VLAN.

| Trunk link | Ports |
|---|---|
| Router to Switch1 | Router1 Gi0/0 and Switch1 Fa0/1 |
| Switch1 to Switch2 | Switch1 Fa0/24 and Switch2 Fa0/24 |

**Router** (Router-on-a-Stick):

```
interface GigabitEthernet0/0
 no shutdown
!
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
!
interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
```

## Verification summary

| Test | Result |
|---|---|
| **A. Same-VLAN pings** (HR, IT and Finance pairs) | Successful, 0% loss, TTL=128 (switched at Layer 2, not routed) |
| **B. Inter-VLAN pings** (HR to IT, HR to Finance, Finance to HR) | Successful, 0% loss, TTL=127 (one router hop) |
| **C. VLAN isolation** (router link shut down) | Inter-VLAN pings failed with 100% loss; connectivity recovered once the link was restored |
| **D. Simulation Mode** (HR-PC1 to FIN-PC1) | Frame is tagged to the router, MAC addresses are rewritten at the router, IP addresses stay unchanged |

Commands used for verification:

- **Switches:** `show vlan brief`, `show interfaces trunk`, `show interfaces status`, `show mac address-table`, `show running-config`
- **Router:** `show ip interface brief`, `show ip route`, `show running-config`, `show interfaces`

## How to open the project

1. Install Cisco Packet Tracer.
2. Download `Mark_Gichuhi_Gichohi_VLAN_Routing_Project.pkt` from this repository.
3. Open it in Packet Tracer (**File > Open**).
4. Open the command prompt on any PC (**Desktop > Command Prompt**) and run `ping` between the addresses listed above to repeat the tests.

## Report

The full write-up is in `Mark_Gichuhi_Gichohi_VLAN_Routing_Report.docx`. It covers:

1. Executive summary
2. Network topology and architecture
3. IP addressing plan
4. VLAN and switch configuration
5. Router-on-a-Stick configuration
6. Testing and verification (Tests A to D)
7. Command reference
8. Troubleshooting report
9. Conclusion

## Author

Mark Gichuhi Gichohi, Computer Science, Multimedia University
