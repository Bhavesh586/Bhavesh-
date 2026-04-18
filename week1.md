COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 1
Week 01 – GNS3 Introduction
Field Details
Unit COIT12206 TCP/IP Principles and Protocols
Student Name Bhavesh Chaudhari
Student ID 12314739
Overview
This week focused on setting up the lab environment and getting familiar with GNS3, a
network simulation tool. I installed VirtualBox and GNS3, set up the GNS3 VM, and
completed my first hands-on network simulation task involving a single Linux host with a
static IP address.
Environment Setup
• Installed Oracle VirtualBox as the virtualisation platform
• Installed GNS3 2.2.58.1 and linked it to the GNS3 VM running inside VirtualBox
• Created a private GitHub repository.
• Added the Alpine Linux Docker appliance to GNS3 as the Linux Host node
• Ignored VMware-related error — not needed as VirtualBox is used
Task 1 – GNS3 Introduction
Aim
Get familiar with GNS3 by creating a project, adding a Linux host node, configuring a
static IP address, and verifying it via the console.
Activities
Project Created
Node Added
Alpine Linux (Docker-based Linux host) - dragged onto the canvas from the device
panel.
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 2
IP Address Selected
10.10.1.1 - configured as a static IP address on interface eth0.
Static IP Configuration
Configured in /etc/network/interfaces before starting the node:
auto eth0
iface eth0 inet static
 address 10.10.1.1
 netmask 255.255.255.0
 up sysctl net.ipv4.ip_forward=0
IP forwarding was disabled using 'up sysctl net.ipv4.ip_forward=0' to ensure the node
acts as a host, not a router.
Verification
After initializing the node and accessing the console, the command shown below was
executed:
ip address show
Output confirmed the IP address was correctly applied:
6: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
 inet 10.10.1.1/24 scope global eth0
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 3
Outputs
File Description
12314739.gns3project Exported GNS3 project file
12314739-network.png Screenshot of network topology with
annotations
12314739-ipaddress.png Screenshot of console showing IP address
Key Concepts Learned
• Real-life network topologies can be simulated by GNS3 using virtualized nodes.
The GNS3 VM operates under VirtualBox and supports Docker containers such as
Alpine Linux.
• In Linux, the /etc/network/interfaces file allows you to configure a static IP
address, which will remain unchanged even after rebooting – unlike dynamic IP
addresses that use DHCP.
• Forwarding determines whether IP packets are forwarded from one interface to
another in a Linux node. While hosts should disable IP forwarding (ip_forward=0),
routers should enable it (ip_forward=1).
• The 'ip address show' command outputs information about all network interfaces
and their associated IP addresses, both IPv4 and IPv6.
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 4
• The annotations in GNS3 allow you to add text-based notes containing important
topology details, such as IP addresses, project details, and student particulars.
Reflection
This was my first time working with GNS3, and it proved to be harder than anticipated.
Linking GNS3 VM with VirtualBox involved some issues. I got an error from VMware at
start-up, which I later found to be unrelated, since I was using VirtualBox, and continued
as usual.
It was fascinating to see how lightweight the Docker container can replicate a network
host by running the same code as any other OS would. IP configuration with the help of
the interfaces file was easy once I understood the syntax. The crucial part was realizing
that the configuration for /etc/network/interfaces should be made before launching the
container so that changes are applied automatically; otherwise, the process will require
manually running ifdown/ifup.
Now I know why it is better to have static IPs in lab workstations. They offer reliable,
stable, and constant IP addresses that remain the same after every reboot. In my case,
this will ensure reliable network communication between two or more hosts.
