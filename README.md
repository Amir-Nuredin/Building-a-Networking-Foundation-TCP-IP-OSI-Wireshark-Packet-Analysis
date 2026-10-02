# Building a Networking Foundation TCP IP OSI Wireshark Packet Analysis

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

The first thing to do before starting any lab work was to get a good idea of what my lab architecture currently looks like. This would be the foundation for the rest of my network lab configuration. I needed to document all my VMs and this information: 
- Interface
- IPv4 Address
- Subnet Mask
- MAC Address
- Gateway
- DNS
- DHCP/Static IP




































