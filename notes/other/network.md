---
layout: post
title: Computer Networking - My Learnings

---

<style>
  .twitter-tweet {
    width: 30%;  /* Adjust the width as needed */
    margin: 0 auto;  /* Center the blockquote horizontally */
  
  }

    .twitter-tweet p {
    font-size: 14px !important; /* Change the font size */
  }

</style>

This article or note documents my exploration into seeing how computers work at a low level. I think it's good to even have a basic understanding because every time you write or a computer executes code, you can connect it back to your understanding.  So, just by pressing the right buttons and sending the right signals to a computer, you can get it to manipulate information for you! That is crazy to think about.

I make heavy use of the semi-deterministic framework (see my post on metacognition: thinking about thinking). The idea is to look at abstractions as input-output. And the lowest level of abstraction closest to hardware without worrying about the circuitry of the processor is: machine code or assembly. These notes were from Hussein Nasser's OS course and ChatGPT 4o. I'm not a developer but a quant/ds.

# Big Picture

## Central Problem of Networking

* Central Problem
  * We want to pass information from computer A to B in a node of networks, constrained by laws of physics/info, as fast as possible, reliably as possible. Problem: what path should A take to B?
  * Message passes between nodes, taking a path from source to dest. At each node, node will use info of PDU when needed to determine where to go next.
* Bits
  * Protocol = how you interpret bits. That's it.
* Client Server
  * Server is a specialized machine that does work (instructions) for clients, only has one purpose.
* Encapsulation/Decapsulation
  * Each PDU (protocol data unit) has a header and data. 
  * At each level of encapsulation, additional headers are added to the bit stream (left to right or top down visual).
  * Data is transformed or augmented at each level.
  * At each node, the data is decapsulated to the extent to which that node (router) knows where to send it next.
* (Physical) Connectivity Type 
  * Wifi, ethernet, LTE, fiber. Different methods of 'connecting', but one standard protocol. 
* Receiving Data
  * Any raw info (bits, frame, packet, datagram etc) is received by NIC (network interface controller), specialized hardware, passed to network drivers in kernel space, then handed off to user space.

## OSI Model

* **Application** - How the message is formatted. HTTP, FTP, SSH, DNS, RDP. How do we communicate at a high level?
* **Presentation** - Serialization. ASCII, TIFF, GIF, JPEG, MPEG, MP4. How do we interpret the bits of the actual data? Text, images, videos, audio, etc.
* **Session** - Info needed to establish a 'connection' between client and server. TCP connection
* **Transport** - Ports, application specific. One server could receive many diff messages for diff apps, all orthogonal.
* **Network** - IP range, so many different networks, which network does it go to?
* **Data Link** - MAC, within a network, so many diff nodes. Which specific computer does it go to?
* **Physical** - How do we encode our data physically? Wi-fi, fiber (light), ethernet, etc.  

# Application

## HTTP

* Key Ideas - Requests sent by client to server, receive response. Any data 
* Get - The main request. Addressed via URL.
* Response - The payload, and status codes (1xx, 2xx, 3xx, 4xx, 5xx). For example, 2xx is success. 4xx is client error. 5xx is server error.


## DNS

## SOAP



# Presentation

# Session/Transport

## TCP

* Use Case - Transmission control. 20-60 byte header. Handshake + resend segment if corrupted. Use when data loss is unacceptable. E.g remote shell, DB connections, text messaging, etc.
* Example - MS Teams, for message, use TCP. For video/audio, use UDP, as data loss is acceptable.
* Segment Structure - Source port, dest port, sequence number, acknowledgement number, header length, control flags (URG, ACK, PSH, RST, SYN, FIN), window size, checksum, urgent pointer.
* Handshake - On initial connection, a 3 way handshake (SYN, SYN-ACK, ACK) must be established. So receiver must acknowledge source first. Quite complex but helps in security (e.g spoofing IP address breaks the 3 way handshake)
* Retransmission (Error Handling) - Payload checksum is computed and stored in the checksum field (high condition number). The receiver takes the segment, computes the checksum. If 'different', knows corruption occurs. Request retransmission.

## UDP

* Use Case - Simple process-to-process communication, but trade correctness/reliability/security for latency (low). 4 byte header = speed.  Used when packet loss is not important (e.g video, audio, images)
* Datagram - The PDU of UDP. 64 bit header size, source and destination ports (16 bits each = 65535 ports), length (length of data bytes), checksum (error correction). Port = process specific.

# Network (IP)

## Routing Intuition

* Complexity - Ridiculously complex. Don't even bother. Lots of applied graph algorithms.
* Greedy and Local - The PDU goes through various nodes or routers. At each step, routers make an independent decision, no global end-to-end planning. 
* Stateless - Routers don't maintain info about individual PDUs. Once they send, it's gone.
* Dynamic - Routers constantly communicate to update their routing tables as network topology changes.


## IP Packet Structure

* IP Address - Assigned by DHCP from DNS and ISP. Private (available only to other nodes in your network, e.g your fellow colleague's computer in office wifi) and public (available to public Internet). 
* Subnet Mask - Data used to determine how many individual addresses are allocated to individual nodes in a network.
* Important fields - Version, IHL, DSCP, ECN, TL, ID, Flags, Fragment offset, TTL (number of hops packet exist in network before discard), protocol, source IP address, destination IP address, etc then data. Pretty complex. 

## Other Sub Protocols

* Internet Control Message Protocol (ICMP) - Check that network devices have success or failure when communicating with another IP address. Traceroute and clever use of TTL bits in IP header.
* Address Resolution Protocol (ARP) - Checking MAC in a network. Why need MAC? IP, even local, can be dynamic. If reassigned, problem. Need more granularity. Check local IP broadcast to all computers in network, get MAC, cache.

# Data Link

# Physical