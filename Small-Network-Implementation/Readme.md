📝 Project Overview
This project involves building and configuring a small network topology from scratch. It demonstrates the ability to set up a router as a gateway for multiple LAN segments, secure network devices with basic hardening techniques, and ensure full end-to-end connectivity.
🚀 Key Learning 
ObjectivesPhysical Topology: Selecting and connecting the correct media (Copper Straight-Through).
Initial Configuration: Setting hostnames, banners, and disabling DNS lookups.
Security Hardening: Configuring encrypted passwords for the console and privileged EXEC mode.
Network Management: Configuring Switch Virtual Interfaces (SVI) for remote management.
Connectivity: Verifying communication via ICMP (Ping) across subnets.
🗺️ Network Topology
📊 Addressing Table
Device,Interface,IP Address,Subnet Mask,Default Gateway
RTA,G0/0,10.10.10.1,255.255.255.0,N/A
RTA,G0/1,10.10.20.1,255.255.255.0,N/A
SW1,VLAN 1,10.10.10.2,255.255.255.0,10.10.10.1
SW2,VLAN 1,10.10.20.2,255.255.255.0,10.10.20.1
PC-1,NIC,10.10.10.10,255.255.255.0,10.10.10.1
PC-2,NIC,10.10.20.10,255.255.255.0,10.10.20.1

⚙️ Configuration Snippets
Router (RTA) Configuration
Basic interface setup and security:
Bash
Router(config)# hostname RTA
RTA(config)# enable secret class
RTA(config)# line con 0
RTA(config-line)# password cisco
RTA(config-line)# login
RTA(config)# interface g0/0
RTA(config-if)# ip address 10.10.10.1 255.255.255.0
RTA(config-if)# no shutdown
Switch Management
(SW1)Allowing the switch to be managed from the RTA or PC-2:
Bash
SW1(config)# interface vlan 1
SW1(config-if)# ip address 10.10.10.2 255.255.255.0
SW1(config-if)# no shutdown
SW1(config)# ip default-gateway 10.10.10.1
🔍 Verification Checklist
[x] Ping PC-1 to PC-2: Successful (Verifies routing at RTA).
[x] Ping PC-1 to SW1: Successful (Verifies SVI configuration).
[x] Show IP Interface Brief: All interfaces are up/up.[x] Show Running-Config: All passwords are encrypted and banners are set.
📁 Repository 
Filesimage_09fc7c.png: Topology screenshot.
Small_Network_Implementation.pka: The completed Cisco Packet Tracer Activity file.
configs/: Folder containing the full running-configs for RTA, SW1, and SW2.
