Unit COIT12206 TCP/IP Principles and Protocols
Student Name Bhavesh Chaudhari
Student ID 12314739
Overview
This week covered two important networking concepts: ARP (Address Resolution
Protocol) which maps IP addresses to MAC addresses at Layer 2, and Default
Gateways which enable static routing across multiple subnets. Both concepts were
demonstrated practically in GNS3.
Task 1 – ARP (Address Resolution Protocol)
Aim
Observe how ARP allows devices to discover and track the MAC addresses of other
devices on the same LAN.
Network Used
The Setting-IP-12314739 project from Week 2 was used — 4 Alpine Linux hosts
connected to 1 Ethernet switch.
Step 1 – Empty ARP Table
Before any communication, viewed the ARP table on Host A (AlpineLinux-1):
ip neigh show
Result: Empty — Host A had not communicated with any other device, so no MAC
addresses had been learned yet.
Empty ARP Table Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 2
Step 2 – After Pinging Host B
Ran a ping from Host A to Host B:
ping -c 5 10.1.1.2
Then checked the ARP table again:
ip neigh show
Result:
10.1.1.2 dev eth0 lladdr 02:42:95:14:ef:00 ref 1 used 0/0/0 probes 4
REACHABLE
Host A learned Host B's MAC address (02:42:95:14:ef:00) and stored it with state
REACHABLE.
ARP Table After Ping to Host B
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 3
Step 3 – After Host C Pinged Host A
From Host C (AlpineLinux-3), ran a ping to Host A:
ping -c 5 10.1.1.1
Then checked Host A's ARP table again:
ip neigh show
Result:
10.1.1.3 dev eth0 lladdr 02:42:e3:31:0d:00 REACHABLE
10.1.1.2 dev eth0 lladdr 02:42:95:14:ef:00 STALE
Two entries now visible: Host C (10.1.1.3) is REACHABLE as it just communicated.
Host B (10.1.1.2) changed to STALE as it has not been used recently.
ARP Table After Host C Ping
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 4
ARP States Summary
State Meaning
REACHABLE Entry recently confirmed — MAC address is valid and host is
reachable
STALE Entry exists but has not been verified recently — may be removed
soon
FAILED Host did not respond — entry will be removed
Task 2 – Default Gateways
Aim
Use default gateways to enable static routing across 3 subnets using 2 routers.
Network Setup
Project: Default-Gateway-12314739
Device Interface IP Address Role
HOST1 eth0 10.1.1.1 Host (Subnet 1)
HOST2 eth0 10.1.1.2 Host (Subnet 1)
R1 eth0 10.1.1.254 Gateway for Subnet 1
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 5
R1 eth1 10.1.2.1 Link to R2
R2 eth0 10.1.2.2 Link to R1
R2 eth1 10.1.3.254 Gateway for Subnet 3
HOST3 eth0 10.1.3.1 Host (Subnet 3)
HOST4 eth0 10.1.3.2 Host (Subnet 3)
Network Topology Screenshot
Routing Tables
HOST1
default via 10.1.1.254 dev eth0
10.1.1.0/24 dev eth0 scope link src 10.1.1.1
R1
default via 10.1.2.2 dev eth1
10.1.1.0/24 dev eth0 scope link src 10.1.1.254
10.1.2.0/24 dev eth1 scope link src 10.1.2.1
R1 Routing Table Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 6
R2
default via 10.1.2.1 dev eth0
10.1.2.0/24 dev eth0 scope link src 10.1.2.2
10.1.3.0/24 dev eth1 scope link src 10.1.3.254
HOST3
default via 10.1.3.254 dev eth0
10.1.3.0/24 dev eth0 scope link src 10.1.3.1
Ping Test (HOST1 to HOST3 across 3 subnets)
ping 10.1.3.1
Metric Result
Packets Transmitted 23
Packets Received 23
Packet Loss 0%
Avg RTT 0.691 ms
TTL 62 (2 router hops)
TTL=62 confirms the packet traversed exactly 2 routers (R1 and R2). Default TTL is 64,
decremented by 1 at each router.
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 7
Ping Screenshot
Key Concepts Learned
• ARP resolves IP addresses to MAC addresses at Layer 2. When a device wants
to send a packet to an IP on the same subnet, it uses ARP to discover the
corresponding MAC address.
• ARP table entries are cached and have states: REACHABLE (recently
confirmed), STALE (not recently used), FAILED (unreachable). Entries expire
over time.
• Default gateway allows hosts to send traffic to subnets they are not directly
connected to. All unknown destinations are forwarded to the gateway.
• Multiple routers can be chained together to connect many subnets. Each router
should only be aware of the subnets that are directly connected to it and also the
default route for other subnets.
• TTL is a reliable method for counting hops – a router decreases the value of TTL
by 1. When TTL = 62, this implies that there are two hops (64 – 2 = 62).
• For an IP packet to be forwarded by a Linux routing node, IP Forwarding should
be enabled (ip_forward = 1).
Reflection
The ARP assignment has been very educational. While I already knew what ARP is,
actually observing the ARP table getting populated by just one ping command brought it
closer to reality. What has been quite impressive, too, is the fact that over time the state
goes from REACHABLE to STALE. This means that ARP records are not eternal and
need to be updated periodically.
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 8
As for the Default Gateway assignment, it has logically extended our previous
knowledge of routing from Week 4, adding the additional difficulty of using two routers
and three subnets in addition to the routing task. Seeing the TTL of 62 in the ping output
meant that packets really used 2 routers as expected.
To conclude, all these concepts combine to present a full story of communication in the
case when there is more than one subnet involved, namely ARP at Layer 2 and routing
/default gateways at Layer 3.
