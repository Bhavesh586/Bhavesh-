Unit COIT12206 TCP/IP Principles and Protocols
Student Name Bhavesh Chaudhari
Student ID 12314739
Week Week 02
Overview
This week covered two tasks: setting static IP addresses on Linux hosts using three
different methods, and testing network connectivity using the ping command. A network
of 4 Alpine Linux hosts connected to an Ethernet switch was built in GNS3.
Network Topology Screenshot
Task 1 – Setting Static IP Addresses
Aim
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 2
Learn three different approaches to set a static IP address on a Linux host in GNS3.
Network Setup
Project:
Node IP Address Subnet Method Used
AlpineLinux-1 10.1.1.1 10.1.1.0 GNS3 Configure
menu
AlpineLinux-2 10.1.1.2 10.1.1.0 GNS3 Configure
menu
AlpineLinux-3 10.1.1.3 10.1.1.0 Edit interfaces in
console
AlpineLinux-4 10.1.1.4 10.1.1.0 ip address add
command
Method 1 – GNS3 Configure Menu (Hosts 1 & 2)
Right-clicked the node and selected Edit config before starting. Edited
/etc/network/interfaces:
auto eth0
iface eth0 inet static
 address 10.1.1.1
 netmask 255.255.255.0
 up sysctl net.ipv4.ip_forward=0
The IP is applied automatically when the node starts. This is the most reliable method
as the configuration persists after reboot.
Host 1 Console Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 3
Host 2 Console Screenshot
Method 2 – Edit interfaces file in Console (Host 3)
After starting the node, opened the console and used cat to write the config file directly:
cat > /etc/network/interfaces << EOF
auto eth0
iface eth0 inet static
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 4
 address 10.1.1.3
 netmask 255.255.255.0
 up sysctl net.ipv4.ip_forward=0
EOF
Then reloaded the interface using ifdown and ifup since the node was already running:
ifdown eth0
ifup eth0
Note: nano was not available on Alpine Linux, so the cat heredoc method was used
instead.
Host 3 Console Screenshot
Method 3 – ip address add Command (Host 4)
Used a single command to immediately assign an IP address:
ip address add 10.1.1.4/24 dev eth0
This method is immediate with no need to restart the interface, but is NOT persistent —
the IP is lost after a reboot.
Host 4 Console Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 5
Task 2 – Testing Network Connectivity with Ping
Aim
Learn the basics of ping to test if a device is reachable and to measure round-trip time
(RTT).
Test 1 – Basic Ping (Host A to Host B)
Run from AlpineLinux-1 console:
ping 10.1.1.2
Metric Result
Packets Transmitted 7
Packets Received 7
Packet Loss 0%
Min RTT 0.201 ms
Avg RTT 0.531 ms
Max RTT 1.660 ms
0% packet loss confirms both hosts are reachable across the switch. The first packet
had a higher RTT (1.660 ms) due to ARP resolution.
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 6
Basic Ping Screenshot
Test 2 – Ping to Non-Existent IP
Run from AlpineLinux-1 console:
ping 10.1.1.99
Metric Result
Packets Transmitted 31
Packets Received 0
Packet Loss 100%
100% packet loss — no device with IP 10.1.1.99 exists on the network, so no ICMP
Echo Reply messages were received.
Error Ping Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 7
Test 3 – Ping with Custom Options
Run from AlpineLinux-1 console:
ping -c 5 -s 100 -i 2 10.1.1.2
Option Value Meaning
-c 5 5 packets Limit to 5 packets total
-s 100 100 bytes Set data size per packet to 100 bytes
-i 2 2 seconds Set interval between packets to 2 seconds
Metric Result
Packets Transmitted 5
Packets Received 5
Packet Loss 0%
Avg RTT 0.280 ms
Custom Options Ping Screenshot
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 8
Key Concepts Learned
• Three methods to set a static IP: GNS3 Configure menu (persistent, before start),
editing interfaces file in console + ifdown/ifup (persistent, while running), and ip
address add command (temporary, immediate).
• The /etc/network/interfaces file in Linux is used to make persistent configuration
changes to the network settings. These changes take effect only upon boot
unless the ifdown/ifup command is executed.
• The ip address add command is fast and convenient for testing purposes but
does not persist beyond a reboot. It should therefore not be used for production
purposes.
• The operation of ping involves sending ICMP Echo Request messages from the
source. The receiver responds back using Echo Reply messages. RTT stands for
round trip time for packets.
• The first ping message will have a longer RTT because of ARP process required
to find out MAC address.
• Packet loss of 100% for a ping to an IP that does not exist means that no reply
was received, which confirms that IP address is indeed not being used.
• -ping options include -c(count), -s(size), -i(interval).
COIT12206 TCP/IP Principles and Protocols | Portfolio
Student: Bhavesh | ID: 12314739 | Page 9
Reflection
This week has provided me with hands-on knowledge of configuring static IPs using
three distinct methods. The main lesson I learned from this process is knowing the
differences between persistent and non-persistent settings. In the real-life application,
one should definitely use the interfaces file in order to keep the settings alive after the
reboot; however, there is an ip address add command for fast experiments.
Moreover, I faced another technical issue when working with the Linux distribution
called Alpine. By default, this particular version of Linux does not include the nano
editor, therefore, I was forced to use the cat heredoc approach to create an interfaces
file. It shows that different Linux distributions might lack some common utilities and a
good network engineer has to know how to adapt to such situations.
Finally, the ping tests were very illustrative since packet loss on a completely nonexistent IP address demonstrated what exactly happens when a host is unreachable. In
addition, I could clearly see increased RTT for the first packets due to the ARP
resolution, which we will cover in detail during Week 6.
