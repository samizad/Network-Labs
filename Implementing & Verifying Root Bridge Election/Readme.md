STP Root Bridge Election & Path Optimization Lab

## 📌 Overview
This lab demonstrates the implementation and verification of **Spanning Tree Protocol (STP)** in a hierarchical network topology. The primary objective is to manually influence the Root Bridge election to ensure a deterministic traffic flow and prevent Layer 2 loops.

## 🛠️ Topology Description
- **Access Layer:** Switch A1
- **Distribution Layer:** Switches D1 and D2
- **Interconnections:** Redundant GigabitEthernet links between Access and Distribution layers.

## 🚀 Implementation Steps

### 1. Enabling Rapid-PVST
To achieve faster convergence compared to legacy STP (802.1D), we enabled **Rapid Per-VLAN Spanning Tree (PVRST+)** on all switches:
```bash
Switch(config)# spanning-tree mode rapid-pvst
2. Manual Root Bridge Selection
By default, the switch with the lowest MAC address becomes the Root. To ensure D1 (our core switch) takes the Root role, we decreased its priority:

Bash
D1(config)# spanning-tree vlan 1 priority 4096
Note: The actual Priority becomes 4097 (4096 + VLAN ID 1).

3. Trunk Configuration
All inter-switch links were configured as 802.1Q trunks to allow VLAN propagation:

Bash
A1(config-if-range)# switchport trunk encapsulation dot1q
A1(config-if-range)# switchport mode trunk
✅ Verification & Results
Root Bridge Identification
On A1, we verified that D1 was successfully elected as the Root Bridge.

Bash
A1# show spanning-tree vlan 1
Key Observations:

Root ID Priority: 4097 (Matches D1)

Root ID Address: (Matches D1's MAC)

Cost: 4 (Standard for GigabitEthernet)

Port Roles & States
The Spanning Tree algorithm calculated the best paths:

Root Port (Root/FWD): The interface on A1 leading directly to D1.

Alternate Port (Altn/BLK): The redundant link to D2 was placed in Blocking state to prevent a loop.

💡 Troubleshooting & Lessons Learned
Priority vs. MAC: While the MAC address is the tie-breaker, the Bridge Priority is the primary administrative tool for network design.

Convergence Speed: Transitioning from PVST to Rapid-PVST significantly reduced the "Listening/Learning" delay.

BPDU Verification: Used show spanning-tree detail to confirm the successful exchange of Bridge Protocol Data Units (BPDUs) between peers.

Author: Samira Bashman

Environment: PNETLab / Cisco IOL
