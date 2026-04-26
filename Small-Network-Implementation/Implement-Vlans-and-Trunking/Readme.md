# Lab: Implement VLANs and Trunking

## Overview
This lab covers VLAN creation, port assignment, voice VLAN configuration, management SVIs, and both static and dynamic trunking on Cisco 2960 switches.

## Topology
Three switches (SWA, SWB, SWC) connected as follows:
- SWA G0/1 ↔ SWB G0/1 — Static trunk
- SWA G0/2 ↔ SWC G0/2 — Dynamic trunk
- SWB G0/2 ↔ SWC G0/2 — Dynamic trunk

## Addressing Table

| Device | Interface | IP Address      | Subnet Mask     | Switchport | VLAN              |
|--------|-----------|-----------------|-----------------|------------|-------------------|
| PC1    | NIC       | 192.168.10.10   | 255.255.255.0   | SWB F0/1   | VLAN 10           |
| PC2    | NIC       | 192.168.20.20   | 255.255.255.0   | SWB F0/2   | VLAN 20           |
| PC3    | NIC       | 192.168.30.30   | 255.255.255.0   | SWB F0/3   | VLAN 30           |
| PC4    | NIC       | 192.168.10.11   | 255.255.255.0   | SWC F0/1   | VLAN 10           |
| PC5    | NIC       | 192.168.20.21   | 255.255.255.0   | SWC F0/2   | VLAN 20           |
| PC6    | NIC       | 192.168.30.31   | 255.255.255.0   | SWC F0/3   | VLAN 30           |
| PC7    | NIC       | 192.168.10.12   | 255.255.255.0   | SWC F0/4   | VLAN 10 + VLAN 40 (Voice) |
| SWA    | SVI       | 192.168.99.252  | 255.255.255.0   | N/A        | VLAN 99           |
| SWB    | SVI       | 192.168.99.253  | 255.255.255.0   | N/A        | VLAN 99           |
| SWC    | SVI       | 192.168.99.254  | 255.255.255.0   | N/A        | VLAN 99           |

## VLAN Table

| VLAN Number | VLAN Name  |
|-------------|------------|
| 10          | Admin      |
| 20          | Accounts   |
| 30          | HR         |
| 40          | Voice      |
| 99          | Management |
| 100         | Native     |

## Lab Objectives

- **Part 1** — Configure VLANs on all three switches
- **Part 2** — Assign access ports, configure voice VLAN, configure management SVIs
- **Part 3** — Configure static trunking between SWA and SWB
- **Part 4** — Configure dynamic trunking between SWA and SWC

## Files

| File | Description |
|------|-------------|
| `configs/SWA.txt` | Full IOS configuration for SWA |
| `configs/SWB.txt` | Full IOS configuration for SWB |
| `configs/SWC.txt` | Full IOS configuration for SWC |
| `notes/concepts.md` | VLAN and trunking concepts explained |
| `notes/exam-tips.md` | ENCOR exam tips and common mistakes |

## Verification Commands
show vlan brief
show interface trunk
show interfaces vlan 99
show interfaces Fa0/4 switchport
show running-config
```
