Scenario 1: The "Silent" Gateway (IPv4)The Problem: PC-A cannot reach the Internet.
You can ping the local switch, but pings to 8.8.8.8 time out.
The Troubleshooting: * Perform a tracert 8.8.8.8.
The Catch: The trace stops at the default gateway (192.168.1.1).
The ICMP Lesson: Explain the difference between a Request Timeout (no response) and Destination Unreachable (a router told you it doesn't have a route).
The Fix: Check the Gateway's routing table (show ip route). In this case, the gateway is missing a "Default Route" ($0.0.0.0/0$).
Scenario 2: The Giant Packet (IPv6 Path MTU)The 
Problem: A user can ping a remote IPv6 server, but they cannot download large files or load web pages.
The Troubleshooting: * Run a ping with a large payload and the "do not fragment" bit (or just a large size in IPv6).
ping -6 -s 1500 <address>The ICMP Lesson: In IPv6, routers do not fragment. 
If a packet is too big for a link, the router drops it and sends back an ICMPv6 Type 2 (Packet Too Big) message. 
If a firewall blocks this ICMP message, the "Path MTU Discovery" fails, and the connection hangs.
The Fix: Adjust the MTU on the interface or ensure ICMPv6 Type 2 is allowed through the firewall.
Scenario 3: The "U" in the Trace (IPv4 ACLs)
The Problem: Connectivity is lost between two branch offices. When you ping, you see U.U.U instead of !!!!!.
The Troubleshooting: * Run ping 10.1.1.1.The Catch: The router responds with U (ICMP Unreachable).
The ICMP Lesson: This indicates the router knows where the network is, but it is explicitly being told it cannot send the packet there—usually due to an Access Control List (ACL).
The Fix: Review the ip access-group commands on the outbound interface of the reporting router.
Scenario 4: Neighbor Discovery Failure (IPv6)
The Problem: Two devices are on the same VLAN, but they cannot ping each other's Link-Local addresses.
The Troubleshooting: * Check the neighbor table: show ipv6 neighbors.
The Catch: The state shows INCOMPLETE.
The ICMP Lesson: IPv6 uses ICMPv6 Neighbor Solicitation (NS) and Neighbor Advertisement (NA) instead of ARP. If these multicast messages are filtered by "Private VLANs" or "Port Security," the devices can't resolve MAC addresses.
The Fix: Verify that ICMPv6 multicast traffic is not being suppressed by the switch.
Scenario 5: The "Incomplete" Neighbor (IPv6 NDP)
The Problem: A host can ping its own IPv6 address but cannot ping the default gateway on the same subnet.

The Troubleshooting: Run show ipv6 neighbors on the router.

The Evidence: The neighbor entry for the host shows a state of INCOMPLETE.

The ICMP Lesson: This points to a failure in ICMPv6 Neighbor Discovery (NDP). The Router Solicitation (RS) or Neighbor Solicitation (NS) messages are being blocked or dropped.

The Fix: Check for Layer 2 security features like "IPv6 ND Inspection" or "RA Guard" that might be misconfigured.

Scenario 6: The Routing Loop (TTL Exceeded)
The Problem: A ping to a remote network returns very high latency or fails, and a traceroute shows the packet bouncing back and forth between two routers.

The Troubleshooting: Run traceroute 10.10.10.1.

The Evidence: The trace shows: 10.1.1.1 -> 10.1.1.2 -> 10.1.1.1 -> 10.1.1.2... until it hits the max hops.

The ICMP Lesson: This is a Routing Loop. Eventually, the packet’s TTL (IPv4) or Hop Limit (IPv6) hits 0, and the router sends back an ICMP Type 11 (Time Exceeded).

The Fix: Investigate the routing table (show ip route) on both routers to find the conflicting static or dynamic routes.

Scenario 7: Administratively Prohibited (IPv6 ACL)
The Problem: You are trying to SSH into a remote IPv6 server. The connection is instantly refused.

The Troubleshooting: Run a ping to the server.

The Evidence: You receive a specific error: Reply from <address>: Destination unreachable: Administratively prohibited.

The ICMP Lesson: Unlike a simple timeout, ICMPv6 Type 1, Code 1 (Administratively Prohibited) is a "polite" way for a firewall or ACL to tell you, "I am intentionally blocking this."

The Fix: Update the IPv6 Access Control List (ACL) to permit traffic on TCP Port 22.

Scenario 8: Asymmetric Routing (One-Way Ping)
The Problem: Router A can ping Router B, but Router B cannot ping Router A.

The Troubleshooting: Use debug ip icmp on both routers.

The Evidence: Router A sends an Echo Request and receives an Echo Reply. Router B sends an Echo Request, but the reply never arrives.

The ICMP Lesson: The path a packet takes to a destination isn't always the path it takes back. The ICMP Echo Reply is likely being dropped or routed to a "null" interface on the return trip.

The Fix: Check the routing table of the destination device to ensure it has a valid path back to the source network.

Scenario 9: Duplicate IP Address Conflict (IPv4)
The Problem: Connectivity to a server is intermittent. Some pings work; some time out.

The Troubleshooting: Ping the server and then check the ARP table (show ip arp).

The Evidence: The MAC address associated with the IP address keeps changing.

The ICMP Lesson: When two devices share an IP, they fight for the ICMP Echo Reply. The one that responds faster gets the "Success" in the ping tool.

The Fix: Use show mac address-table to trace the flapping MAC address to the offending port and re-address the duplicate device.

Scenario 10: MTU Mismatch in OSPF
The Problem: OSPF neighbors are stuck in the EXSTART or EXCHANGE state.

The Troubleshooting: Ping across the link with the "Do Not Fragment" bit set and the maximum MTU.

The Evidence: Small pings pass; large pings (1500 bytes) fail with ICMP Type 3, Code 4 (Fragmentation Needed).

The ICMP Lesson: OSPF exchanges Large Database Description (DBD) packets. If the MTU doesn't match on both sides, the ICMP feedback loop prevents the adjacency from forming.

The Fix: Ensure ip mtu 1500 matches on both sides or use the ip ospf mtu-ignore command.

Scenario 11: The Default Gateway ICMP Redirect
The Problem: A host is sending traffic to Router A, but Router A keeps sending ICMP messages back to the host before forwarding the data.

The Troubleshooting: Capture traffic using Wireshark on the host.

The Evidence: You see ICMP Type 5 (Redirect) messages.

The ICMP Lesson: Router A is telling the host, "There is a better first-hop router (Router B) on this same segment for this destination."

The Fix: This is an efficiency issue. Update the host's Default Gateway or static routes to point directly to Router B.
