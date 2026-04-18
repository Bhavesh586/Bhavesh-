Unit COIT12206 TCP/IP Principles and Protocols
Student Name Bhavesh Chaudhari
Student ID 12314739
Overview
This week has been about getting an insight into IP routing between subnets. A network
has been created having two subnets linked to one another using a router, and the
process of IP forwarding has been done.
Task 1 – Viewing Routing Tables
Aim
Find out how to display routing tables and activate IP forwarding on a router node.
Network Setup
Project: View-Routes-12314739
Device Interface IP Address Forwarding
AlpineLinux-1 eth0 10.1.1.1 Disabled (0)
AlpineLinux-2 eth0 10.1.1.2 Disabled (0)
Router eth0 10.1.1.254 Enabled (1)
Router eth1 10.1.2.254 Enabled (1)
AlpineLinux-3 eth0 10.1.2.1 Disabled (0)
Network Topology Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 2
Node Configurations
Hosts (AlpineLinux-1, 2, 3)
auto eth0
iface eth0 inet static
 address 10.1.1.1
 netmask 255.255.255.0
 gateway 10.1.1.254
 up sysctl net.ipv4.ip_forward=0
Router
auto eth0
iface eth0 inet static
 address 10.1.1.254
 netmask 255.255.255.0
 up sysctl net.ipv4.ip_forward=1
auto eth1
iface eth1 inet static
 address 10.1.2.254
 netmask 255.255.255.0
Routing Tables
AlpineLinux-1 and AlpineLinux-2
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 3
default via 10.1.1.254 dev eth0
10.1.1.0/24 dev eth0 scope link src 10.1.1.1
AlpineLinux-2 Routing Table Screenshot
AlpineLinux-3
default via 10.1.2.254 dev eth0
10.1.2.0/24 dev eth0 scope link src 10.1.2.1
AlpineLinux-3 Routing Table Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 4
Router
10.1.1.0/24 dev eth0 scope link src 10.1.1.254
10.1.2.0/24 dev eth1 scope link src 10.1.2.254
Router Routing Table Screenshot
Ping Test (Cross-Subnet)
Ran from AlpineLinux-1 to AlpineLinux-3:
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 5
ping 10.1.2.1
Metric Result
Packets Transmitted 14
Packets Received 14
Packet Loss 0%
Avg RTT 0.873 ms
TTL 63 (1 router hop)
TTL=63 confirms the packet traversed exactly 1 router (default TTL 64, decremented by
1).
Ping Screenshot
Task 2 – Dynamic Routing with OSPF
Status
The OSPF template (OSPF-Basics-Template.gns3project) was not available this week
as it needs to be provided by the tutor. This task will be completed when the template is
made available.
Key OSPF Concepts (Theory)
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 6
OSPF (Open Shortest Path First) is a link-state dynamic routing protocol. Key features
include:
• Automatically discovers neighbouring routers and exchanges routing information
• Calculates shortest paths using Dijkstra's algorithm
• Adapts automatically when a network link fails, rerouting traffic via alternate
paths
• Uses FRRouting (FRR) software on Linux nodes to implement OSPF
Key FRR commands for viewing routing information:
show ip ospf neighbor # view neighbouring routers
show ip ospf route # view OSPF-learned routes
show ip route # view full routing table
traceroute is used to observe the actual path packets take:
traceroute <destination_ip>
Key Concepts Learned
• Routing tables determine where a device sends packets. Each entry has a
destination network and a next-hop (gateway) address.
• Default gateway is the router a host sends packets to when the destination is not
on its local subnet. Without it, hosts can only communicate within their own
subnet.
• IP forwarding must be enabled on routers (ip_forward=1) so they forward packets
between interfaces. For hosts, this needs to be set to false (ip_forward = 0).
• TTL is reduced by 1 for every router hop. By inspecting the TTL value in the ping
command output, we can determine the total number of routers in transit.
• The two interfaces present on a router are aware of both the subnets; hence, no
configuration is required to create direct routes.
• Open Shortest Path First (OSPF) is an example of dynamic routing protocol that
requires no route configuration manually.
Reflection
Setting up the network consisting of two subnets helped me comprehend the routing
process thoroughly. In previous weeks, I did not realize the concept of subnets and
considered networks as a unified structure. Now I am aware that subnets cannot
interact without a router that will forward traffic from one subnet to another.
It was really exciting to see both subnets registered as directly connected interfaces on
the router. Besides, hosts require only the information about the default gateway rather
than the list of all subnets they can be routed via.
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 7
I particularly enjoyed seeing the TTL value equal to 63, as it confirmed the idea that only
one router was used in the connection process. I am looking forward to accomplishing
the OSPF task as automatic routing that responds to link breaks is an awesome
function since there is no need to manually configure the routes on routers.
