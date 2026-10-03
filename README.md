# Building a Networking Foundation: TCP IP, OSI, Wireshark Packet Analysis

## Objective

This lab/project aims to develop practical skills in TCP/IP networking, the OSI model, network communication, routing, address resolution, transport protocols, and packet analysis. In this lab, I used a multi-VM VirtualBox homelab consisting of Windows and Linux systems to examine network configuration, routing tables, ARP/neighbor caches, listening ports, and active network connections. I also used Wireshark to capture and analyze network traffic at multiple layers, including ARP, ICMP, DNS, TCP, UDP, and TLS traffic.

To gain hands-on experience with how systems communicate across a network, I analyzed ARP address resolution, captured ICMP Echo Request and Reply traffic, examined DNS queries and responses, and observed the TCP three-way handshake. I compared TCP and UDP communication, examined the network information that remains visible when TLS encryption is used, and correlated active Windows TCP connections with their corresponding packets in Wireshark. Finally, I documented the homelab environment by creating a network diagram showing the systems, subnet, default gateway, and Internet path. This project gave me practical experience in network troubleshooting and packet analysis while connecting concepts such as MAC addresses, IP addresses, ports, routing, encapsulation, ARP, DNS, TCP/UDP, and network sockets to real communication in the homelab.

### Skills learned 

This project provided hands-on experience with the following networking, TCP/IP, and packet analysis skills:

- TCP/IP and OSI model concepts, including network layers, encapsulation, and the relationship between Ethernet frames, IP packets, and transport protocols
- Windows and Linux network configuration analysis using tools such as ipconfig, Get-NetIPConfiguration, ip addr, ip route, route print, arp, and ip neigh
- IPv4 addressing, subnetting, routing tables, default gateways, MAC addressing, and local versus remote network communication
- ARP analysis, including observing ARP requests and replies and understanding IPv4-to-MAC address resolution on a local network
- Wireshark packet capture and analysis using display filters to inspect ARP, ICMP, DNS, TCP, UDP, and TLS traffic

### Tools Used

## Tools Used

- Oracle VirtualBox
- Wireshark
- Windows 10
- Windows 11
- Ubuntu Linux
- Ubuntu (Wazuh)
- Kali Linux
- Windows Command Prompt
- Windows PowerShell
- Linux Terminal
- Windows networking utilities (`ipconfig`, `route`, `arp`, `netstat`, `nslookup`, `ping`)
- Linux networking utilities (`ip`, `ss`, `resolvectl`, `ping`)

## Current Lab Environment Architecture/Setup

Below is a screenshot of my current lab architecture (IM1). This is the starting point for my lab architecture and will grow as I learn and implement more VMs and networking components. 

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/d05fd3c8-b661-4660-b7e5-685a6a7fffe0" />

IM1: Current Lab Environment/Architecture

## Steps/Procedure

### Part 1: Baseline Network Documentation

Before starting any lab work, I first got a clear picture of my current lab architecture. This would be the foundation for the rest of my network lab configuration. I needed to document all my VMs and the following information: 
- Interface
- IPv4 Address
- Subnet Mask
- MAC Address
- Gateway
- DNS
- DHCP/Static IP

1. **WINDOWS INFORMATION.** To find all the network information for the Windows 10 VM, I used the command "ipconfig /all to find detailed information on the Windows VM's network configuration (IM2).
2. **LINUX INFORMATION.** To find the network information for the Linux VMs (Ubuntu server, Ubuntu Wazuh server, and Kali Linux). I used a couple of commands. First was "ip addr" to find basic information like the IP address and subnet mask. I also used "resolvectl status" to find DNS information for the VMs.
3. **SUMMARY TABLE.** Below is a table summarizing all the IP and network information for all four VMs (TB1).

| System | Interface | IPv4 | Prefix/Mask | MAC | Gateway | DNS | DHCP/Static |
|---|---|---|---|---|---|---|---|
| Ubuntu | `enp0s8` | `192.168.1.20` | `/24 (255.255.255.0)` | `08:00:27:53:e5:af` | `192.168.1.1` | `192.168.40.1` | Static |
| Ubuntu/Wazuh | `enp0s8` | `192.168.1.30` | `/24 (255.255.255.0)` | `08:00:27:b7:b1:dd` | `192.168.1.1` | `192.168.40.1` | Static |
| Kali | `eth1` | `192.168.1.40` | `/24 (255.255.255.0)` | `08:00:27:40:33:36` | `192.168.1.1` | `192.168.40.1` | Static |
| Windows 10 | `LabNet` | `192.168.1.50` | `/24 (255.255.255.0)` | `08:00:27:4C:32:66` | `192.168.1.1` | `192.168.40.1` | Static |

<img width="693" height="836" alt="Screenshot 2026-09-29 135536" src="https://github.com/user-attachments/assets/f0e413e7-c14a-47e6-8d40-546ba7b349fa" />

IM2: Windows IP and Network configuration

<img width="830" height="336" alt="Screenshot 2026-09-29 140350" src="https://github.com/user-attachments/assets/6be73a7a-cdac-4637-a465-85682277a06f" />
<img width="646" height="342" alt="Screenshot 2026-09-29 140630" src="https://github.com/user-attachments/assets/4a3d7d7d-4e4b-44d8-8107-6f8f3cabde88" />
<img width="817" height="366" alt="Screenshot 2026-09-29 141020" src="https://github.com/user-attachments/assets/3145a534-2464-47e9-839e-20b6e2bc76ec" />
<img width="619" height="393" alt="Screenshot 2026-09-29 141350" src="https://github.com/user-attachments/assets/72c0fd09-9616-4418-8fc9-458fe9506789" />

IM3-6: Linux VM IP and Network Configurations


### Part 2: Route tables

The next information I needed was how the VMs were routing packets and other IP information. I used the "route print" command on Windows 10 to see what routes were currently being used and to where (IM7). I also did the same on the Ubuntu VM as well using the command "ip route" (IM8). 

<img width="617" height="340" alt="Screenshot 2026-09-29 142347" src="https://github.com/user-attachments/assets/5f1955dd-f97b-4b4f-8c00-e0844e95c8b1" />

IM7: Finding route information for Windows 10 VM

<img width="587" height="135" alt="Screenshot 2026-09-29 142813" src="https://github.com/user-attachments/assets/ec2bf1c0-cf02-47e0-9bcc-1b5ef68edd3d" />

IM8: Finding route information for Ubuntu Server VM

### Part 3: Inspecting ARP 

Now I wanted to look at ARP and what the current cache looked like. Address Resolution Protocol is a network protocol that maps IP addresses to MAC addresses so that when sending packets over the network, it doesn't have to repeat the address resolution every time. On Windows 10, I used the command "arp -a" to inspect the current cache (IM9). The current cache showed which IP addresses were associated with specific MAC addresses. 

<img width="453" height="331" alt="Screenshot 2026-09-29 143231" src="https://github.com/user-attachments/assets/ce35bc2d-b8ee-404e-9d9c-960a933d62f9" />

IM9: Current ARP Cache on Windows 10 VM

### Part 4: Exploring Wireshark

Next, I went into Wireshark to start inspecting a few packets and understand what is flowing through my network (IM10). I started a capture, and the packets started flowing in (IM11). I tested a couple of filters to see how display filters were used as well (IM12&13).

<img width="627" height="482" alt="Screenshot 2026-09-29 144902" src="https://github.com/user-attachments/assets/b00a9800-4557-48e0-a04f-e31b1ae9eac9" />

IM10: Network Interfaces on Wireshark

<img width="1898" height="487" alt="Screenshot 2026-09-29 144922" src="https://github.com/user-attachments/assets/cbe21ed8-31e8-4bde-8c26-192c5dfcde39" />

IM11: Sample Wireshark Packet Capture

<img width="1918" height="448" alt="Screenshot 2026-09-29 145115" src="https://github.com/user-attachments/assets/9c187c77-b957-45d3-b266-1ebbec2116f7" />
<img width="1919" height="428" alt="Screenshot 2026-09-29 145152" src="https://github.com/user-attachments/assets/964abd89-777e-4861-b1a2-93f7c5a1f76e" />

IM12&13: Using Display Filters in Wireshark

### Part 5: Capturing ARP Packets

Now I wanted to capture a few different kinds of network packets within Wireshark to see how the different protocols were working with each other. The first of these is ARP. I planned to start a fresh capture, filter for ARP, then ping the Ubuntu Server VM to generate ARP captures. When I first checked the ARP cache, the Ubuntu Server VM was not in there. I started the capture, filtered for ARP (IM14), then pinged the Ubuntu Server VM. I found a packet that had the Ubuntu Server IP and inspected it. I first looked for the Ethernet header. It showed the destination MAC (broadcast) and Source MAC (Windows) (IM15). The ARP header also showed the destination and source IP addresses (IM15). The reply packet showing the MAC address of the Ubuntu Server VM was essentially flipped when it came to the source and destination MAC and IP addresses (IM16-17). Now, running the arp -a command again, I saw the Ubuntu VM MAC address in the ARP cache (IM18).

<img width="949" height="201" alt="Screenshot 2026-09-30 152925" src="https://github.com/user-attachments/assets/4684eb00-bcec-4c3b-a01d-15ae11e8763c" />

IM14: Using ARP Filter in Wireshark

<img width="494" height="85" alt="Screenshot 2026-09-30 153026" src="https://github.com/user-attachments/assets/87a75db3-598f-4d34-a6b5-6e426d99f8a0" />

IM15: Ethernet Header in ARP packet

<img width="497" height="202" alt="Screenshot 2026-09-30 153104" src="https://github.com/user-attachments/assets/bac34359-000f-484b-959b-b0b01262680f" />

IM16: ARP Protocol Header in ARP Packet

<img width="823" height="15" alt="Screenshot 2026-09-30 153216" src="https://github.com/user-attachments/assets/4bc64e4f-6cf4-4a32-8852-16be483fdf63" />
<img width="494" height="98" alt="Screenshot 2026-09-30 153308" src="https://github.com/user-attachments/assets/e27ee2f9-528e-4f2d-92da-28f3151ab643" />

IM16&17: ARP Packet and Headers of Ubuntu Server VM Reply

<img width="430" height="182" alt="Screenshot 2026-09-30 153459" src="https://github.com/user-attachments/assets/a2980dcb-85ee-4977-afba-49672f2d31d6" />

IM18: Updated ARP Cache with Ubuntu Server Mapped in Cache (192.168.1.20)

### Part 6: Capturing ICMP Packets

Next up, I wanted to inspect some ICMP packets. I started another fresh capture, filtered for ICMP (IM19), then pinged the Ubuntu Server VM from Windows 10. I then looked for the Echo Request and Echo Reply packets and inspected each of them. For the Echo Request, we can see the Ethernet and IP headers, each with the source and destination MAC Addresses (IM20&21). The ICMP header contained the type of Echo and whether a response has been seen (IM22). 

<img width="992" height="185" alt="Screenshot 2026-09-30 154030" src="https://github.com/user-attachments/assets/10713896-c6be-4cd1-81f6-22836ade79a1" />

IM19: Using ICMP Filter in Wireshark

<img width="493" height="85" alt="Screenshot 2026-09-30 154058" src="https://github.com/user-attachments/assets/b499ba30-4f9c-4389-a407-4e77b6de21ae" />
<img width="493" height="255" alt="Screenshot 2026-09-30 154131" src="https://github.com/user-attachments/assets/942ace97-efed-44e2-80eb-2cdad1036c77" />

IM20&21: Ethernet and IP Header Within ICMP Packet

<img width="499" height="196" alt="Screenshot 2026-09-30 154212" src="https://github.com/user-attachments/assets/fd145901-2557-43fd-83be-fd22fc4ab719" />

IM22: ICMP Header Within ICMP Packet. 

### Part 7: Capturing DNS Packets

The last kind of packet I captured was a DNS packet. To work through this, I started a fresh capture, then ran the command "nslookup example.com" to run a DNS query and generate DNS activity in Wireshark (IM23). I then filtered for DNS packets (IM24). In the IPv4 header, it showed the source and destination addresses (192.168.40.1 for the DNS Server) (IM25). In the DNS header, it showed what kind of query it was and the domain name as well (IM26). 

<img width="348" height="178" alt="Screenshot 2026-09-30 155530" src="https://github.com/user-attachments/assets/152c972b-56d1-41a7-95ac-c9706261d8be" />

IM23: Using the nslookup Command to Generate DNS Packets in Wireshark

<img width="993" height="224" alt="Screenshot 2026-09-30 155539" src="https://github.com/user-attachments/assets/a93c6042-2fd5-4ff1-96f5-85a59deeca34" />

IM24: Using DNS Filter in Wireshark

<img width="493" height="253" alt="Screenshot 2026-09-30 155627" src="https://github.com/user-attachments/assets/30c5e28a-68a5-4e24-a7e7-3b4cfcc187a8" />

IM25: IP Header in DNS packet

<img width="494" height="179" alt="Screenshot 2026-09-30 155727" src="https://github.com/user-attachments/assets/97a64dda-7ff1-44a4-9ee5-66e36eee3095" />

IM26: DNS Header in DNS Packet

## Conclusion

## Conclusion

This lab successfully demonstrated the core concepts of TCP/IP networking, the OSI model, network communication, routing, address resolution, transport protocols, and packet analysis. Throughout the project, I used Windows and Linux systems within my VirtualBox homelab to examine network configurations, routing tables, ARP caches, network sockets, and active connections. I also used Wireshark to capture and analyze ARP, ICMP, DNS, TCP, UDP, and TLS traffic, allowing me to observe how network communication occurs across multiple layers.

Overall, this project strengthened my understanding of MAC and IP addressing, ARP, default gateways, DNS resolution, TCP and UDP communication, ports, the TCP three-way handshake, network sockets, encapsulation, and encrypted network traffic. I also gained experience correlating operating system connection information with packet captures and documenting my homelab through a network diagram. The hands-on experience gained from this lab provides a strong networking foundation for future work in system administration, network security, security monitoring, incident investigation, and enterprise cybersecurity.




























































