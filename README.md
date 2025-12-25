# SCC.333 Practical Exercise templates
This week we will focus on learning more about P4 and building our own switch! 

## Learning Objectives


# Task 1: Building a packet with P4
Before we dive into building our own switch, we need to take a closer look at packets — the fundamental units of communication in a network.
As we saw using Wireshark, hosts within a network communicate by sending and receiving packets. Each packet is made up of multiple headers, stacked on top of one another, followed by the actual data (payload) being transmitted.
Each header provides specific information about the packet, such as how it should be handled, where it’s coming from, and where it’s going. Understanding these headers is essential, because switches and routers rely on them to make forwarding decisions.

``` 
+-----------------------------+
| Ethernet Header (MAC)       |  ← Used by switches
+-----------------------------+
| IP Header (IP Address)      |  ← Used by routers
+-----------------------------+
| TCP / UDP Header            |  ← Used by applications
+-----------------------------+
| Payload (Actual Data)       |  ← Message, file, video, etc.
+-----------------------------+
```

## Packet headers
A network packet is built in layers, where each layer adds its own header. Not every packet contains all headers, but when present, each one serves a specific purpose.

### Ethernet Header (Layer 2 – Data Link)
- Purpose: Local delivery within a network
- Used by: Switches
- Contains:
    - Source MAC address – who sent the packet
    - Destination MAC address – who should receive it next
    - EtherType – indicates what protocol comes next (e.g., IPv4, IPv6, ARP)

> ‼️ This header is rewritten at every hop as packets move between networks.

### IP Header (Layer 3 – Network)
- Purpose: End-to-end delivery across networks
- Used by: Routers
- Contains:
    - Source IP address
    - Destination IP address
    - Protocol field (TCP, UDP, ICMP, etc.)
    - TTL (Time To Live) – prevents infinite looping
    - Packet length & fragmentation info

> ‼️ This header determines where the packet is going globally.

### Transport Layer Header (Layer 4)
This header contains information about how the packet is transmitted between applications on different hosts. It specifies the transport protocol being used (such as TCP or UDP) and includes the necessary details to manage communication, such as port numbers, delivery guarantees, and flow control.
#### TCP Header (Transmission Control Protocol)
- Purpose: Reliable, ordered delivery
- Used by: Web browsing, email, file transfer
- Contains:
    - Source & destination port numbers
    - Sequence & acknowledgment numbers
    - Flags (SYN, ACK, FIN)
    - Window size (flow control)

> ‼️ TCP ensures no data is lost or reordered.

#### UDP Header (User Datagram Protocol)
- Purpose: Fast, lightweight delivery
- Used by: Video streaming, gaming, VoIP
- Contains:
    - Source & destination port numbers
    - Packet length
    - Checksum

> ‼️ UDP is faster but does not guarantee delivery.

### ICMP Header (Control & Error Messages)
- Purpose: Network diagnostics and errors
- Used by: ping, traceroute
- Contains:
    - Type (e.g., echo request, destination unreachable)
    - Code (error details)

> ‼️ ICMP packets do not carry application data.

### Optional / Special Headers
Optional or special headers are not present in every packet. They are added only when specific network features or services are required. These headers allow networks to provide advanced functionality beyond basic packet forwarding, without changing the core packet structure.
#### VLAN Tag (802.1Q)
- Used for network segmentation
- Inserted between Ethernet header fields
#### MPLS Header
- Used in high-performance ISP networks
- Enables fast packet forwarding using labels
#### IPsec Headers
- Used for encryption and authentication
- Common in VPNs

### Application Layer Data (Payload)
- Purpose: Actual user data
- Examples:
    - HTTP request
    - Video stream
    - File contents
    - DNS query

> ‼️  Network devices usually do not inspect this, unless deep packet inspection is enabled.

<!-- ## Analogy: Sending a Letter
Think of a packet like mailing a letter:
- Ethernet header → Office mailroom instructions
- IP header → City and street address
- TCP/UDP header → Delivery instructions (urgent or careful)
- Payload → The letter itself

Each network device only reads the part it cares about! -->

## Why This Matters for P4 & SDN
Normally, switches and routers are like robots with fixed instructions:
> “I only understand Ethernet, IP, TCP… don’t ask me to do anything else.”
P4 changes that.

With P4, you get to say:
>“Hey switch, this is what a packet looks like, this is how I want you to read it, and this is what I want you to do with it.”

### In other words:
P4 = Programming Your Network Like a Game Character
- You define what headers exist
- You define how packets are processed
- You define what actions to take
- You control the rules of the game

### Why P4 Is Cool?
- You’re not stuck with predefined protocols
- You can build custom routers and switches
- You get full control over packet forwarding logic

### Analogy: A Custom Recipe
Traditional networking is like ordering from a fixed menu.

P4 lets you write your own recipe:
- Choose the ingredients (packet headers)
- Decide how to cook them (packet processing)
- Decide who gets served (forward, drop, modify)

## Getting started with P4

# Task 2: Building your very own switch with P4!
