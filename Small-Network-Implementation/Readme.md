🌐 Small Network Implementation (Cisco Packet Tracer)📝 Project OverviewThis project demonstrates the manual construction and configuration of a small-scale enterprise network. It focuses on implementing a router as the gateway between two distinct LAN segments, applying security hardening on infrastructure devices, and establishing a management plane for switches.🚀 Key Learning ObjectivesPhysical Topology: Correct selection and deployment of Copper Straight-Through cabling between end-devices, switches, and routers.Initial Configuration: Establishing device identity (Hostnames), legal notices (Banners), and operational efficiency (Disabling DNS lookups).Security Hardening: Implementing encrypted passwords for the Console and Privileged EXEC modes to prevent unauthorized access.Network Management: Configuration of Switch Virtual Interfaces (SVI) and Default Gateways to allow out-of-band management.Connectivity: Validating the end-to-end data path via ICMP (Ping) across different subnets.🗺️ Network TopologyThe network consists of one ISR 4321 Router (RTA), two 2960 Switches (SW1, SW2), and two end-user workstations (PC-1, PC-2).📊 Addressing TableDeviceInterfaceIP AddressSubnet MaskDefault GatewayRTAG0/010.10.10.1255.255.255.0N/ARTAG0/110.10.20.1255.255.255.0N/ASW1VLAN 110.10.10.2255.255.255.010.10.10.1SW2VLAN 110.10.20.2255.255.255.010.10.20.1PC-1NIC10.10.10.10255.255.255.010.10.10.1PC-2NIC10.10.20.10255.255.255.010.10.20.1⚙️ Configuration SnippetsRouter (RTA) ConfigurationFocuses on gateway assignment and basic hardening.BashRouter(config)# hostname RTA
RTA(config)# enable secret class
RTA(config)# line con 0
RTA(config-line)# password cisco
RTA(config-line)# login
RTA(config)# interface g0/0
RTA(config-if)# ip address 10.10.10.1 255.255.255.0
RTA(config-if)# no shutdown
Switch Management (SW1 Example)Enabling remote management capabilities via SVI.BashSW1(config)# interface vlan 1
SW1(config-if)# ip address 10.10.10.2 255.255.255.0
SW1(config-if)# no shutdown
SW1(config)# ip default-gateway 10.10.10.1
🔍 Verification Checklist[x] ICMP Reachability: Ping PC-1 to PC-2 is successful (Verifies Inter-VLAN routing at RTA).[x] Management Reachability: Ping PC-1 to SW1 is successful (Verifies SVI and Gateway config).[x] Interface Status: show ip interface brief confirms all required links are up/up.[x] Security Audit: show running-config confirms password encryption and MOTD banners are active.
