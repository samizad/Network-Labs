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

## Verification
RA# show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
Tunnel0                10.10.10.1      YES manual up                    up

RA# show interface tunnel 0
Tunnel0 is up, line protocol is up
  Hardware is Tunnel
  Internet address is 10.10.10.1/30
  MTU 17916 bytes, BW 100 Kbit/sec, DLY 50000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive set (10 sec, 3 retries)
  Tunnel source 64.103.211.2 (Serial0/0/0), destination 209.165.122.2
  Tunnel protocol/transport GRE/IP

  RA# ping 10.10.10.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.10.10.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5)

RA# show ip route
...
S    192.168.2.0/24 [1/0] via 10.10.10.2, Tunnel0
