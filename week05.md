Unit COIT12206 TCP/IP Principles and Protocols
Student Name Bhavesh Chaudhari
Student ID 12314739
Overview
This week covered Virtual LANs (VLANs) — a method of logically segmenting a network
at Layer 2 without needing separate physical switches. Two tasks were completed:
configuring VLANs on an OpenvSwitch to isolate hosts, and enabling inter-VLAN routing
using a Linux Router with sub-interfaces.
Task 1 – Setup VLANs on Switch
Aim
Learn to configure VLANs on a managed switch to logically isolate hosts into separate
broadcast domains.
Network Setup
Project: Vlan-Basics-12314739
VLAN IDs were based on the last 3 digits of student ID 12314739 = 739:
Node IP Address Switch Port VLAN
AlpineLinux-1 10.1.1.1 eth1 739
AlpineLinux-2 10.1.1.2 eth2 739
AlpineLinux-3 10.1.1.3 eth3 740
AlpineLinux-4 10.1.1.4 eth4 740
Network Topology Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 2
VLAN Configuration on OpenvSwitch
After starting all nodes, opened the OpenvSwitch console and ran:
ovs-vsctl set port eth1 tag=739
ovs-vsctl set port eth2 tag=739
ovs-vsctl set port eth3 tag=740
ovs-vsctl set port eth4 tag=740
Verified configuration using:
ovs-vsctl show
Switch Ports Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 3
Connectivity Testing
Tested ping between hosts to verify VLAN isolation:
Source Destination VLAN Result
Host1 (10.1.1.1) Host2 (10.1.1.2) Same (739) 0% packet loss
Host1 (10.1.1.1) Host3 (10.1.1.3) Different (739→740) 100% packet loss
Hosts in the same VLAN could communicate freely. Hosts in different VLANs could not
communicate — confirming that VLAN isolation was working correctly.
Ping Test Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 4
Task 2 – Setup VLANs on a Router (Inter-VLAN Routing)
Aim
Use a Linux Router with sub-interfaces to enable communication between VLANs — a
technique known as router-on-a-stick.
Network Setup
Project: Vlan-Router-12314739
Node IP Address Subnet VLAN
AlpineLinux-1 10.1.1.1 10.1.1.0 739
AlpineLinux-2 10.1.1.2 10.1.1.0 739
AlpineLinux-3 10.1.2.1 10.1.2.0 740
AlpineLinux-4 10.1.2.2 10.1.2.0 740
Router eth0.739 10.1.1.254 10.1.1.0 739
Router eth0.740 10.1.2.254 10.1.2.0 740
Network Topology Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 5
Switch Configuration
Access ports for hosts:
ovs-vsctl set port eth1 tag=739
ovs-vsctl set port eth2 tag=739
ovs-vsctl set port eth3 tag=740
ovs-vsctl set port eth4 tag=740
Trunk port for router (eth0 carries all VLANs):
ovs-vsctl set port eth0 trunks=739,740
Switch Ports Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 6
Router Sub-Interface Configuration
A single physical interface (eth0) was used with VLAN sub-interfaces. First removed the
conflicting IP from eth0:
ip address del 10.1.1.253/24 dev eth0
Created sub-interfaces for each VLAN:
ip link add link eth0 name eth0.739 type vlan id 739
ip link add link eth0 name eth0.740 type vlan id 740
ip link set eth0.739 up
ip link set eth0.740 up
ip address add 10.1.1.254/24 dev eth0.739
ip address add 10.1.2.254/24 dev eth0.740
This is the router-on-a-stick technique — one physical link carries tagged traffic for
multiple VLANs, and the router processes each VLAN on a separate sub-interface.
Connectivity Testing
Tested inter-VLAN ping from Host1 to Host3:
ping -c 3 10.1.2.1
Metric Result
Packets Transmitted 3
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 7
Packets Received 3
Packet Loss 0%
Avg RTT 6.225 ms
TTL 63 (1 router hop)
Inter-VLAN routing worked successfully. TTL=63 confirms the packet traversed the
router.
Inter-VLAN Ping Screenshot
Key Concepts Learned
• VLANs logically segment a network at Layer 2. Communication between devices
belonging to different VLANs is impossible without special means, even if they
are connected to the same physical switch.
• Access ports connect end hosts to a switch. Each access port is assigned a
single VLAN tag using ovs-vsctl set port ethX tag=VLAN_ID.
• Trunk ports carry traffic from multiple VLANs between a switch and a router. The
trunks=[] setting allows all VLANs on that port.
• Router-on-a-stick uses VLAN sub-interfaces (e.g. eth0.739) on a single physical
router interface to route between VLANs. Each sub-interface handles one VLAN
and gets its own IP address.
• OpenvSwitch is a software-defined switch that supports VLAN tagging, making it
ideal for virtual network labs.
• IP conflicts between eth0 and sub-interfaces must be avoided. The physical
interface eth0 should not have an IP in the same subnet as any sub-interface.
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 8
Reflection
VLANs have proven to be a difficult yet extremely exciting topic this week. It was quite
interesting to know that hosts from different VLANs cannot talk to each other, despite
being connected to the same physical switch, until I knew about VLANs forming
separate Layer 2 broadcast domains.
The router-on-a-stick setup for Task 2 proved to be quite intriguing. In practice, I
experienced a problem whereby ping did not work despite configuring sub-interfaces,
but its solution was removing the conflicting IP on eth0 that fell within the eth0.739 subinterface subnet range. It is a very important experience to learn how IP addresses can
create problems for routing setups.
One of the main reasons I can now understand why VLANs are very popular in
corporate environments is because one can divide his/her corporate network into logical
parts (segments), without necessarily having to change any hardware. Router-on-a-stick
setup offers one the ability to do that effectively.s
