# Inter-VLAN Routing Project (Router-on-a-Stick)

## Project Overview
This project demonstrates a scalable network design using **Cisco Packet Tracer**. It implements a "Router-on-a-Stick" topology to enable communication between different VLANs using a single physical interface on a Cisco 4321 Router.

## Features
- **Subnetting:** Efficient IPv4 addressing scheme using `/25` masks.
- **VLAN Segmentation:** Isolated traffic for "Sales" (VLAN 10) and "HR" (VLAN 20).
- **Inter-VLAN Routing:** Using sub-interfaces on the router.
- **Trunking:** IEEE 802.1Q trunking protocol between switches and the router.
- **Security:** - Encrypted enable passwords.
  - Console and VTY line security.
  - SSH access configured for remote management.

## Topology
- **1x Cisco 4321 Router**
- **2x Cisco 2960 Switches**
- **2x End Devices (PCs)**



## IP Addressing Table
| Device | Interface | IP Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **R1** | G0/0/0.10 | 192.168.1.1 | 255.255.255.128 | N/A |
| **R1** | G0/0/0.20 | 192.168.1.129 | 255.255.255.128 | N/A |
| **PC1** | NIC | 192.168.1.10 | 255.255.255.128 | 192.168.1.1 |
| **PC2** | NIC | 192.168.1.140 | 255.255.255.128 | 192.168.1.129 |

## Verification Commands
The following commands were used to verify the network stability:
1. `show ip interface brief` - Check interface status and IP assignment.
2. `show ip route` - Verify routing table and connected subnets.
3. `show vlan brief` - Confirm VLAN assignments on switch ports.
4. `show interfaces trunk` - Ensure trunk links are active.
5. `show running-config` - Verify security and SSH configurations.

## How to Run
1. Download the `.pkt` file from this repository.
2. Open it using **Cisco Packet Tracer**.
3. Use the `ping` command from PC1 to PC2 to verify connectivity.
