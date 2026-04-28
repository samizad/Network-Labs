# Packet Tracer - Configure NAT for IPv4

## Overview
This repository contains the configuration and lab details for implementing Network Address Translation (NAT) on a Cisco Router using Packet Tracer. The lab covers Dynamic NAT with Port Address Translation (PAT) and Static NAT.

## Objectives
* Configure **Dynamic NAT with PAT** to allow multiple internal LANs to access the internet using a single public IP.
* Configure **Static NAT** to map an internal server (local.pka) to a specific public IP address.
* Define NAT inside and outside interfaces.

## Topology Summary
The network consists of three internal LANs connected via Router R2 to an outside network.
- **Inside Networks:** 192.168.10.0/24, 192.168.20.0/24, 192.168.30.0/24
- **Public Pool:** 209.165.202.128/30
- **Inside Server:** 192.168.20.254

## Key Concepts Applied
1. **Access Control Lists (ACLs):** Used to identify "interesting traffic" from the LANs.
2. **NAT Pool:** Defined a range of public IP addresses for translation.
3. **PAT (Overload):** Enabled multiple devices to share one public IP by tracking port numbers.
4. **Static Mapping:** Created a permanent 1-to-1 mapping for the internal server.

## Verification
To verify the configuration, use the following Cisco IOS commands:
- `show ip nat translations`
- `show ip nat statistics`
