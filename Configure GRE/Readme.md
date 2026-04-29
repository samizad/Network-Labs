# Cisco Packet Tracer: Configuring GRE Tunnels

This repository contains a step-by-step guide and solution for configuring a Generic Routing Encapsulation (GRE) tunnel between two remote offices using Cisco IOS.

## Project Overview
The goal is to establish connectivity between two private networks (LANs) over an untrusted public network (Internet) using a GRE tunnel and static routing.

### Topology
- **RA (Local Office):** LAN 192.168.1.0/24
- **RB (Remote Office):** LAN 192.168.2.0/24
- **Tunnel Network:** 10.10.10.0/32

## Objectives
1. Verify initial connectivity (and explain why end-to-end ping fails initially).
2. Configure GRE Tunnel interfaces on both routers.
3. Configure static routes to redirect private traffic through the tunnel.
4. Verify final connectivity and analyze path trace.
