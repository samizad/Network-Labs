```markdown
# Lab Analysis & Results

### 1. Why did the initial ping between PCA and PCB fail?
Before configuring the GRE tunnel, the ping failed because:
- The intermediate routers on the "Internet" do not have routes to private IP addresses (192.168.x.x).
- Private IP addresses are non-routable over the public internet.

### 2. Path Analysis (Traceroute)
After the tunnel is established, running a `traceroute` from PCA to PCB shows the following:
1. **First Hop:** 192.168.1.1 (Local Gateway)
2. **Second Hop:** 10.10.10.2 (Remote Tunnel Endpoint)
3. **Third Hop:** 192.168.2.2 (Destination PC)

**Observation:**
Notice that the public IP addresses of the ISP/Internet routers are **not visible**. 

**Explanation:** GRE encapsulates the original IP packet. To the routers in the public network, the packet looks like a simple point-to-point IP packet between RA and RB's public interfaces.
The internal TTL and headers are hidden from the underlay network, making the two remote routers appear as if they are directly connected.
