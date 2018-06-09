
[Day 1](http://learn.lboro.ac.uk/pluginfile.php/976792/mod_resource/content/1/coc_2017_day1.pdf)

# IP and Networking Basics
*Day 1 - Page 2*

**Parallel**: multiple bits are transferred at once.
**Serial**: bits are transferred one after another.

**Circuit Switching**: The actual network is mutated to provide two end points direct connection to one another. Then data can be transferred directly from one end to the other.
**Packet Switching**: The entire network is connected simultaneously. Data is split into packets. Packets are then routed through the network based upon their own destination addresses. When received packets are reassembled in the proper order to create the original message.

> History of the internet: page 8-10

The internet is arranged into a network of networks with a rough hierarchy
- at center "Tier 1" ISPs
  - national coverage
  - e.g. Level 3, Cogent, NTT
- Tier 2 ISPS
  - usally smaller
  - regional
  - connect to one of more tier-1
  - possibly connet to other tier-2 ISPs
- Tier 3
  - last hop (closest to end systems)


![Structure of the internet](images/structureofinternet.png)

- IP has no "connection"
  - packets do not garuntee order
  - each packet finds its own way
  - no error detection
  - no congestion control (beyond "drop")
- TCP helps for connection-oriented applications
  - error correction by retransmission

# OSI Stack & TCP/IP Architecture
*Day 1- Page 16*

Edge vs Core (end-systems vs routers)
- Core
  - contains routers and dumb systems
  - connectionless
- Edge
  - **connection oriented**
    - Achieved by **TCP**
  - contains hosts, computers, servers
  - run application programs
  - peer to peer

## Connection Orientated - TCP
- handshake
- setup state
- flow control
  - slow down sending rate if receiver becomes overwhelmed
- congestion control
  - slow down sending when the network is congested

## Connectionless service - UDP
- Unreliable
- no flow control
- no congestion control

## Layers
![OSI model](images/layerstructure.png)

Important layers to know are:
- Layer 7: Applicaition - Netscape navigator
- Layer 4: Transport (**segment**) - TCP, UDP
- Layer 3: Network (**datagram**)- Ip Addresses
- Layer 2: Data Link (**frame**) - Ethernet - Mac Addresses
- Layer 1: Physical - Binary

Each layer is unaware of the mechanics of the other layers.

The application layer communicates with other application layers, seemingly, directly. It simply passes the data to the next layer down and it arrives intact and complete at the destinations layer 7. No need to know what the other layers do.

![Layer Encapsulation](images/layerencapsulation.png)

## Layer 2 - Ethernet frame
![ethernet frame structure](images/ethernetframestructure.png)
- Destination and source are 48-bit Mac addesses
- Type of `0x0800` means the data portion contains an IPv4 datagram
- Type of `0x0806` is ARP
- Type of `0x86DD` is IPv6
- The data part contains a layer 3 datagram

## Layer 3 - IPv4 datagram
![ipv4 datagram structure](images/ipv4datagramstructure.png)
- Version = 4
- If no options specified:
  - IHL = 5
  - Source and Destination addresses are 32bit IPv4 addresses
- Protocol of `6` means the data contains TCP
- Protocol of `17` means the data contains UDP

## Layer 4 - TCP segment
![tcp segment structure](images/tcpsegmentstructure.png)

- Source and Destination are 16-bit TCP port numbers (ip address implied by the IP header)
- if no options:
  - Data Offset = 5 (meaning 20 octets)

# IP Addressing
*Day 1- Page 29*
- Unique ID from source and destination
  - sometimes used for security or policy-based filtering
- Moving a machine between networks involves changing IP address
- Assigned in a heirarchical fashion
  1. IANA (Intenet assigned number authority)
  2. IANA to RIRs (AfriNIC, ARIN, RIPE, APNIC, LACNIC)
  3. ISPS and Large organizations
  4. End users

IP addresses are divided into *network* and *host* parts
2 computers on the same network will have the same network parts but different host parts.
The boundry between the network and host parts can be anywhere along the 32 bits.

**Network masks** help differentiate between host and network parts by containing all 1s in the network part and 0s in the host part

```javascript
             NETWORK                  | HOST
1111 1111   1111 1111   1111 1111   11|00 0000  netmask
1100 1101   0010 0101   1100 0001   10|00 0000  ip address
```

The slash notation shows where the divide is:

```javascript
112.0.0.34/24  => 24 bits of network and 8 of host
```

## Special Addresses
If the host part contains all **0s** then it represents the **network** address
All **1s** in the host part is the **broadcast** address

**127.0.0.0/8** is a loopback address (127.0.0.1)
**0.0.0.0** has various special purposes

---------------------------

http://learn.lboro.ac.uk/pluginfile.php/976842/mod_resource/content/0/coc190_2017_day2.pdf

# Concept Review
*Day 2 - Page 2*

# More IPv4
*Day 2 - Page 8*

# IPv6
*Day 2 - Page 18*
# Large Network Issues & Routers
*Day 2 - Page 25*

# Cisco Router Configuration - Basics
*Day 2 - Page 30*

------------------------

http://learn.lboro.ac.uk/pluginfile.php/979934/mod_resource/content/1/17coc190_Day3.pdf

# Concept Review
page 2

# Static and Dynamic Routing
page 6
# Layer-2 Network Design
page 11
# Link Aggregation
page 27

# Switching Loops
page 29

------------------------

http://learn.lboro.ac.uk/pluginfile.php/981753/mod_resource/content/0/coc190_2017_Day4.pdf

# Concept Review
page 2

# Address Resolution Protocol (ARP)
page 10

# Routing
page 14

http://learn.lboro.ac.uk/pluginfile.php/981754/mod_resource/content/0/coc190_2017_day4a.pdf

# Dynamic Routing
page 1

------------------------

http://learn.lboro.ac.uk/pluginfile.php/985655/mod_resource/content/0/coc190_2017_Day5.pdf


# Concept Review
# Address Resolution Protocol (ARP)
page 2


http://learn.lboro.ac.uk/pluginfile.php/985654/mod_resource/content/0/coc190_2017_day5a.pdf


# Distance Vector Worked Example
page 1

# IS-IS
page 10

------------------------

http://learn.lboro.ac.uk/pluginfile.php/990708/mod_resource/content/0/coc190_2017_day6.pdf

# Concept Review
page 2
- Link state protocols
- Dijsktra's Algorithm
- OSPF
- IS-IS

# Autonomous Systems
page 13

# BGP
page 14

# BGP Part 2
page 24

------------------------

http://learn.lboro.ac.uk/pluginfile.php/994793/mod_resource/content/0/coc190_2017_day7.pdf


BGP Protocol Basics
Small ISP with one upstream provider
Keeping Local Traffic Local
A Large ISP with more than one upstream provider
Terminology: “Policy”

------------------------

http://learn.lboro.ac.uk/pluginfile.php/996302/mod_resource/content/0/coc190_2017_day8.pdf

# DNS
page 10

# NAT
page 32

------------------------
