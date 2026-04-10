# Implement Single-Area OSPFv2 Lab

This repository contains the OSPFv2 configuration for a three-router topology (R1, R2, and R3) designed to establish full connectivity using Area 0.

## Topology Overview
- **Routing Protocol:** OSPFv2
- **Area:** 0 (Backbone)
- **Process ID:** 10
- **Addressing:** 172.16.x.x and 192.168.x.x subnets.

## Key Requirements
- Static Router IDs (1.1.1.1, 2.2.2.2, 3.3.3.3)
- Passive Interfaces on all LAN-facing ports.
- Explicit network statements using quad-zero wildcard masks.
