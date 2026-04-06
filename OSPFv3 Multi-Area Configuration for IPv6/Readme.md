# OSPFv3 Multi-Area Configuration for IPv6

## Project Overview
This lab demonstrates the implementation of **OSPFv3** in a multi-router topology using IPv6. The goal was to establish full connectivity across different OSPF areas (Area 0, Area 1, and Area 2) and handle IPv6-specific routing challenges.

## Topology Details
- **Area 0 (Backbone):** Connects RA, RB, and RC via Serial links.
- **Area 1:** Contains RA's local GigabitEthernet LANs.
- **Area 2:** Contains RC's local GigabitEthernet LANs.

## Addressing Table Reference
| Device | Interface | IPv6 Address | OSPF Area |
| :--- | :--- | :--- | :--- |
| RA | G0/0 | 2001:db8:1:a1::1/64 | Area 1 |
| RA | S0/0/0 | 2001:db8:1:ab::2/64 | Area 0 |
| RB | S0/0/0 | 2001:db8:1:ab::1/64 | Area 0 |
| RC | G0/0 | 2001:db8:1:c1::1/64 | Area 2 |

## Key Implementation Steps
1. **Enable IPv6 Routing:** Executed `ipv6 unicast-routing` globally.
2. **OSPFv3 Process:** Initialized OSPFv3 with Process ID 1.
3. **Router ID:** Manually assigned 32-bit IDs (1.1.1.1, 2.2.2.2, 3.3.3.3) as OSPFv3 requires them in IPv6-only environments.
4. **Interface Assignment:** Enabled OSPF directly on interfaces using `ipv6 ospf 1 area [X]`.

## Troubleshooting Log (Lessons Learned)
### 1. Area Mismatch Error
- **Issue:** Received `%OSPFv3-4-AREA_MISMATCH` logs.
- **Cause:** Discovered a mismatch where one side of a link was in Area 0 and the other in Area 1.
- **Fix:** Aligned both ends of the link to the same Area ID based on the addressing table.

### 2. IPv6 Routing Not Enabled
- **Issue:** Router rejected OSPF configuration commands.
- **Fix:** Enabled `ipv6 unicast-routing` to allow the router to forward IPv6 packets.

## Verification
show ipv6 ospf neighbor: Confirmed FULL adjacency with all neighbors, ensuring the OSPFv3 neighbor state machine is complete.

show ipv6 route ospf: Confirmed that Inter-Area routes are present in the routing table (marked with OI), proving successful routing updates between Area 0, 1, and 2.

show ipv6 route: Verified the entire IPv6 routing table, confirming the coexistence of Connected (C), Local (L), and OSPF (O) routes.

show ipv6 ospf database: Inspected the Link State Advertisement (LSA) types, confirming the router has a consistent map of the entire multi-area topology.

show ipv6 ospf interface: Verified that OSPFv3 is active on specific interfaces, ensuring correct Area ID assignment and matching Hello/Dead timers.


