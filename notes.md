
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

<pre>
Not needed
</pre>

# More IPv4
*Day 2 - Page 8*
<pre>
Maximum Number of hosts = number of bits in host part - 2
Very large blocks allocated to RIRs (e.g /8)
  - Divided into smaller blocks for business (e.g /17)
  - Divided into smaller blocks for local networks (e.g /26)
  - Each host geta a host address
Current system - classless, slash/netmask notation
Old System - Classes (A, B, C)
Class A - large, 8 bit network, 24 bit host
Class B - medium, 16 bits network, 16 bit host
Class C - small, 24 bit network, 8 bit host
CIDR - Classless Inter-Domain Routing (routing does not assume that class implies prefix length)
VLSM - Variable-Length Subnet Masks (routing does not assume all subnets are the same size)
Classless example:
  ISP get large bloack of addresses e.g. /16 (65536 addresses)
  Allocates smaller blocks to customers e.g. /22 (1024), /28 (16) etc
  An organisation recieving /22 divideds it into smaller blocks e.g. /26 (64), /27 (32) etc
Private addresses - not allocated to any particular organisation, internal use, not globally unique -> not internet routable
IPv4 addresse space running out - work arounds (NAT, dynamic allocation of less addresses than customers etc), IPv6 is real solution. Causes: WWW unforseen, created massive deman, networking of previously standalone devices, ineffcient allocation of initial address space
NAT (Network Address Translation) - Internal addresses are private, router translates private IP's to public ones, violates layer seperation
Dynamic Allocation - More customers than address, addresses allocated on demand, assumes not everyone online at the same time, most customers dont get static IP address
ARP (Address Resolution Protocol) - In local network, LAyer 7 apps need to connect to distributed Apps by IP address, LAN commuinication lower doesn needs also a MAC address to encapsulate and form an ethernet frame to go to wire, ARP maps these two and caches mapping to ARP table
DHCP (Dynamic Host Configuration Protocol) - Dynamic allocation of host addresses
  Host braodcasts UDP Datagram using port 67 with "this host" source address; 0.0.0.0
  DHCP server sends "offer"
  Host sends "Request"
  DHCP sever sends Ack
DNS (Domain Name System) - Maps human readable name to IP address
</pre>

# IPv6
*Day 2 - Page 18*

<pre>
Successor to IPv4
Expanded address space - Address length quadrupled to 16 bytes (128 bits)
Header format simplification - fixed length, optional headers are dasiy-chained
No checksum at the IP network layer

Add image of IPv4/IPv6 header comparison

IPv4 - 32 bits, 4,294,967,296 possible addresses
PIv6 - 128 bits, 3.4 x 10^38 addresses, 5 x 10^28 per person on the planet

Address Representation - 16 bit fields in case insensitive colon hexadecimal representation
Leading zeros in a field are optional
Succerssive feilds of 0 given by ::, but only once in an address
  - 0:0:0:0:0:0:0:1 -> ::1 (loopback address)
  - 0:0:0:0:0:0:0:0 -> :: (unspecified address)
In a URL it is enclosed in square brackets e.g. http://[2001:4860:b006::67]:80/index.html, cumbersome for users, mostly for diagnostic purposes, use FQDN (Fully Qualified Domain Name) instead

Global Unicast Addresses: Provider - Site - Host = 48 bits - 16 bits - 64 bits = 001 - Global Routing Prefix - Subnet-id - Interface ID

Address Allocation:
  IANA is allocating out of 2000::/3 for initial IPv6 unicast use
  Each registry gets a /12 prefix from the IANA
  Registry allocated a /32 prefix (or larger) to an IPv6 ISP
  Policy is that an ISP allocates a /48 prefix to each end customer
Registry - /12, ISP prefix - /32, Site prefix - /48, LAN prefix - /64

64 bits reserved for the interface ID - possibility of 2^64 hosts on one network LAN
16 bits reserved for the end site - possibility of 2^16 networks at each end-site, 65536 subnets equivalent to a /12 in IPv4 (assuming 16 hosts per IPv4 subnet)

Benefits:
  Every link uses fe80::/64 for link-local stuff (hosts in isoloated networks automatically communicate)
  Router can announce global addresses, Router Advertisement (RA) ICMP packets e.g 2001:608:4:0::/64
  Clients will use all available /64 prefixes (compute the host part from their MAC address, EUI-64: Algorithm for computing 64-bit host paart from 48-bit (Ethernet) MAC address)
  
EUI-64 diagram but probably not needed
  
Migration towards IPv6:
  Problems:
    v4 host wanting to talk to v6 host
    v6 networks that are only connected by v4 infrastructure
  Migration techniques::
    Duel-stacked hosts/router (v4 + v6 IP stack on same machine)
    Duel-stacked proxies / application-level gateways
    Tunnelling
      Manually configured tunnels
      Automatic tunnelling (6to4, ISATAP, Teredo)
      Tunnels configured by tunnel broker

Duel Stack Image
</pre>

# Large Network Issues & Routers
*Day 2 - Page 25*

<pre>
Need for Packet Forwarding:
  Many small networks can be interconnected to make a larger internetwork
  A device on one network cannont send a packet directly to a device on another network
  The packet has to be forwarded from one network to another, through intermediate nodes, until it reaches its destination
  Intermediate nodes -> routers

IP Router:
  A device with more than one link-layer interface
  Different IP addresses (from different subnets) on different interfaces
  Recieves packets -> forwards them to get them one hop closer to destination
  Maintians forwarding tables

Action for each packet:
  Packet recieved on one interface
  Check if router is destination -> if so, pass to higher layers
  Decrement TTL (time to live) -> discard if TTL = 0
  Look up destination IP in forwarding table
  
Forwarding vs Routing:
  Forwarding -> moving packets from input to output (uses forwarding table and info in packet)
  Routing -> process which builds and maintains the forwarding table (one or more routing protocols, Procedures (algorithms) to convert routing info to forwarding table)
  Basic: 
    Routing = building maps + giving directions
    Forwarding = moving packets between interfaces based on directions

Each router tries to get the poacket one hop closer to destination
Each router makes an independent decision based on its own forwarding table
Routers talk routing protocols to each other, to help update routing and forwarding tables

Router Functions:
  Determin optimum routing path through network (lowest delay, highest reliability)
  Move packets through the network
  Interconnected router exchange routing tables in order to maintain a clear picture of the network
  In a large network -> routing table updates can consume a lot of bandwidth (update protocol required)

Forwarding Table Structure:
  Don't list every IP number on the Internet (would be huge)
  Contains prefixes (network numbers) -> "If the first /n bits matches this entry, send the datagram that way"
  Longest prefix wins (more specific route) if more than one
  0.0.0.0/0 is default route -> matches anything but only if no other prefix matches
  
</pre>

# Cisco Router Configuration - Basics
*Day 2 - Page 30*

<pre>
Is this really needed?
</pre>
------------------------

http://learn.lboro.ac.uk/pluginfile.php/979934/mod_resource/content/1/17coc190_Day3.pdf

# Concept Review
page 2

<pre>
Not needed
</pre>

# Static and Dynamic Routing
page 6

<pre>
Static routes can be entered into the forwarding table manually
  High degree of control
  Needs negular human intervention
  Not practical in large networks
Dynamic Routing -> routing protocols automatically maintain the forwarding table

Approaches:
  Distance Vector:
    Based on Bellman-Form algorthim
    Each router tells its IMMEDIATE NEIGHBOURS about all the prefixes it can reach and the cost
    Each router chooses leat-cost option for forwarding
  
  Link State:
    Dijkstras algorithm
    Routers tell all other routers in the network avout their direct links
    Routers for their own view of the network and select least-cost forwarding option
    
Desirable Characteristics of Dynamic Routing:
  Automatically detect and adapt to topology changes
  Provide optimal routing
  Scalability
  Robustness
  Simplicity
  Rapid convergence
  Some control of routing choices (e.g which links we prefer to use)
  
Convergence:
  When all routers have a stable view of the network
  Downtime when not converged
    Packets don't get to where they are supposed to go (black holes, routing loops)
  Occurs when there is a change in status of router or links
  
Internet Routing Hierarchy
  Composed of Autonomous Systems (AS)
  Each AS is an administrative entity that
    Uses Interior Gateway Protocols (IGPs) to determine routing within the AS
    Uses Exterior Gateway Protocols (EGPs) to interact with other ASs
 
IGPs (four well known):
  RIP
  EIGRP
  OSPF
  ISIS
  
EGPs (one single de-facto standard):
  BGP
  
</pre>

# Layer-2 Network Design
page 11

<pre>
Encapsulation & decapsulation:
  Lower layers add headers (and sometimes trailers) to data from higher layers (see previous layer images)

Hubs (repeaters):
  Not really used anymore
  Frame sent by one node is sent to all others
  
Switch:
  Learns the location of each node by looking at the source address of each incoming frame, nd builds a forwarding table
  Forwards each incoming frame to the port where the destination node is
    Reduces the collision domain
    Makes more efficient use of the wire
    Nodes don't waste time checking frames not destined to them
  Broadcasts frames when desintation address is not found, the frame is destined to the broadcast address (FF:FF:FF:FF:FF:FF), or when the frame is destined to a multicast ethernet address
  They do not reduce the broadcast domain
  
Routers more or less do with IP packets what switches do with Ethernet frames
Differences:
  IP packets travel inside ethernet frames
  IP networks can be logically segmented into subnets
  Switches do not usually know about IP, they only deal with ethernet frames

Level 2 Network Design:

A good network design is modular and hierarchical, with a clear seperation of functions
  Core: Resilient, few changes, few features, high bandwidth, CPU power
  Distribution: Aggregation, redundancy
  Access: Port density, affordability, security features, many adds, moves and changes
  
Traffic Domains:

Add traffic domain image

Try to  eliminate collision domains
  get rid of hubs
Try to keep broadcast domain limited to no more than 250 simultaneously connected hosts
  segment network using routers

Guidlines:
  Always connect hierarchically
    If multiple switches present in a building, use an aggregation switch
    Locate the aggregation switch close to the building entry point (e.g. fiber panel)
    Locate edge switches close to users (e.g. one per floor) (max length for Cat 5 is 100 meters)
  Minimize path between elements
  Build incrementally
  Do not dasiy-chain
  Connect buildings hierarchically
  
Virtual LANs (VLANs)
  Allow us to split switches into seperate (virtual) switches
  Only members of a VLAN can see that VLAN's traffic
    Inter-vlan traffic must go through a router
  VLANs seperate broadcast domains
  Assigning a host to the correct VLAN is a 2-step process
    Connect host to correct port on switch
    Assign host the correct IP address depending on the VLAN membership

VLAN Operation:
  As a device enters the network, it automatically assumes the VLAN membership of the port to which it is attached
  The default VLAN for evey port in the switch is VLAN 1 and cannot be deleted

VLANs vs Multiple Switches
  VLANS provide segmentation based on broadcast domains
  VLANSs logically segment switched networks based on the functions, project teams, or applications of the organization regardless of the physical location or connections to the network
  All workstations and servers used by a particular workgroup share the same VLA, regardless of the physical connection or location

Add VLAN Truncking image

VLANs increase complexity
  You can no longer just replace a switch 
    VLAN configuration needs to be maintained
    Field technicians need more skills
  Need to make sure that all switch-to-switch trunks are carrying all the necessary VLANs
    Keep in mind when adding/removing VLANs

Good reasons to use VLANS:
  Reduced need for seperate physical switches when seperate subnets are desirable
  Seperating network traffic by function
  Security of Network Managment Functions
  Segregating a network within an organisation by staff function rather than physical location
  
Do not build "VLAN spaghetti":
  Extending a VLAN to multiple buildings across trunk ports
  Bad idea because:
    Broadcast traffic is carried across all trunks from one end of the network to another
    Broadcast storm can spread across the extent of the VLAN
</pre>

# Link Aggregation
page 27

<pre>
Also known as port bundling/link building
You can use multiple links in parallel as a single, logical link
  For increased capacity
  For redundancy (fault tolerance)
LACP (Link Aggregation Control Protocol) is a standardized method of negotiating these bundled links between switches

LACP Operation:

Add LACP example image

Switches A and B are connected to each other using two sets of Fast Ethernet ports
LACP is enabled and the ports are turned on
Switches start sending LACPDUs, then negotiate how to set up the aggregation
The result is an aggregated 200 Mbps logical link
The link is also fault tolerant:
  If one of the member links fail, LACP will automaticallt take that link off the bundle, and keep sending traffic over the remaining link

</pre>

# Switching Loops
page 29

<pre>
If there is more than one path between two switches:
  Forwarding tables become unstable
    Source MAC addresses are repeatedly seen coming from different ports
  Switches will broadcast each other's broadcasts
    All available bandwidth is utilized
    Switch processors cannot handle the load
    Known as a broadcast storm
    
Add image of broadcast storm

Loops can be taken advantage of:
  Redundant paths improve resilience when a switch breaks or wiring fails

Enableing the Spanning Tree Protocol (STP) on all switches can achieve redundancy without creating dangerous traffic loops

Spanning Tree:

"Given a connected undirected graph, a spanning tree of that graph is a subgraph which is a tree and connects all the verticies together"

A single graph can have many different spanning trees

Spanning Tree Protocol:
  The purpose of the protocol is to have bridges dunamically discover a subset of the topology that is loop-free (a tree) and yet has just enough connectivity so that where physically possible, there is a path between every switch
  
Several flavors:
  Traditional Spanning Tree (802.1d)
  Rapid Spanning Tree or RSTP (802.1w)
  Multiple Spanning Tree or MSTP (802.1s)
  
Traditional Spanning Tree (802.1d):
  Switches exchange messages that allow them to ocmpute the Spanning Tree
    Messages are called BPDUs (Bridge Protocol Data Units)
    Two Types of BPDUs:
      Configuration
      Topology Change Notification (TCN)
  
  First Step:
    Decide on a point of refernece: the ROOT BRIDGE
    The election process is based on the Bridge ID, composed of:
      The Bridge Priority: A two-byte value that is configurable
      The MAC address: A unique, hardcoded address that cannot be changed
  Each switch starts by send out BPDUs with a Root Bridge ID equal to its own Bridge ID
  Recieved BPDUs are checked to see if a lower Root Bridge ID is being announced
    If so, each switch replaces the value of the advertised Root Bridge ID with this new lower ID
  Eventually, they all agree on who the Root Bridge is
  
  Root Port Selection: 
  
  Each switch needs to determine its Root Port
    Find the port with the lowest Root Path Cost (cumulative cost of all the links leading to the Root Bridge)
  
  N.B Path cost is inversely proportional to the link speed e.g. the faster the link, the lower the cost
  
  Root Path Cost:
    The accumulation of a links Path Cost and the Path Costs learned from neighboring switches
  
  1. Root Bridge sends out BPDUs with a Root Path Cost value of 0
  2. Neighbor recieves BPDU and adds port's Path Cost to Root Path Cost recieved
  3. Neighbor sends out BPDUs with new cumlative value as Root Path Cost
  4. Other neighbor's down the line keep adding in the same fashion
  
  On each switch, the port where the lowest Root Path Cost was recieved becomes the Root Port
    This is the port with the best path to the Root Bridge
  
  Electing Designated Ports (802.1d):
  
  Each network segment needs to have only one switch forwarding traffic to and from that segment
  Switches need to identify one Designated Port per link or segment
  Theone with the lowest cumulative Root Path Cost to the Root Bridge
  Two or more ports in a segment having identical Root Path Costs is possible, which results in a tie condition
  
  All STP decision are based on the following swquence of conditions:
    Lowest Root Bridge ID
    Lowest Root Path Cost to Root Bridge
    Lowest sender Bridge ID
    Lowest Sender Port ID
    
  Any port that is not elected as either a Root Port, nor a Designated Port is put into the Blocking State
  
  Spanning Tree Protocol States:
    Disabled - Port is shut down
    Blocking - Not forwarding frames, receiving BPDUs
    Listening - Not forwarding frames, sending and receiving BPDUs
    Learning - Not forwarding frames, sending and receiving BPDUs, learning new MAC addresses
    Forwarding - forwarding frames, sending and receiving BPDUs, learningnew MAC addresses
    
  STP Topology Changes:
    Switches will recalculate if:
      A new switch is introduced (It could be the new Root Bridge)
      A switch fails
      A link fails
    
  Root Bridge Placement:
    Using default STP parameters maight result in an undesired situation
    Traffic will flow in non-optimal ways
    An unstable or slow switch might become the root
    You need to plan your assignmnet of bridge priorities carefully
  
  802.1d Convergence Speeds:
    Moving from the Blocking state to the Forwarding State takes atleast 2 x Forward Delay time units (~ 30 secs)
    Some vendors have added enhancments such as ProtFast, which will reduce this time to a minimum for edge ports
    Topology changes typicallyt take 30 seconds
      This can be unacceptable in a production network
  
  Protecting the STP Topology:
    Some vendors have included features that protect the STP topology e.g:
      Root Guard - Prevents device attempting to become the Root (Used on switch to switch links)
      BPDU Guard - Prevents ports attempting to join the STP process
      
  STP Design Guidlines:
    ENABLE SPANNING TREE even if you don't have redundant paths
    Always plan an set bridge priorities
      Make the root choice deterministic
      include an alternative root bridge
    If possible, do not accept BPDUs on end user ports

Rapid Spanning Tree (802.1w)
  Convergence is MUCH faster - communication between switches is more interactive
  Edge ports don't participate - edge ports transition to forwarding state immediately, if BPDUs are received on an edge port it becomes a non-edge port to prevent loops
  
Multiple Spanning Tree (802.1s)
  Allows seperate spanning trees per VLAN group
    Different topologies allow for load balancing between links
    each group of VLANs are assigned to an "instance" of MST
  Compatible with STP and RSTP

Selecting Switches:
  Minimum feature:
    Standards compliance
    Encrypted managment (SSH/HTTPS)
    VLAN truncking
    Spanning Tree (RSTP at least)
    SNMP
      At least v2 (v3 has better security)
      Traps
  Other recommended features:
    DHCP Snooping
      Prevent end-users from running a rogue DHCP server - Happens a lot with little wireless routers (Netgear, Linksys, etc) plugged in backwards
      Uplink ports towards the legitimate DHCP server are defined as "trusted". If DHCPOFFERSs are seen coming from any untrusted port, they are dropped
    Dynamic ARP inspection
      A malicious host can perform a man-in-the-middle attach by sending gratuitous ARP responses, or responding to requests with bogus info
      Switches can look inside ARP packets and discard gratuitous and invaild ARP packets

Documentation:
  Document where your switches are located - name switch after building name e.g building1-sw1
  Keep files with physical location - floor, cloest number etc
  Document your edge port connections - room number, jack number, server name

</pre>

------------------------

http://learn.lboro.ac.uk/pluginfile.php/981753/mod_resource/content/0/coc190_2017_Day4.pdf

# Concept Review
page 2

<pre>
Not needed
</pre>

# Address Resolution Protocol (ARP)
page 10

<pre>
Ethernet/IP Address Resolution:
  Internet Address:
    Unique Worldwide
    Independent of Physical Network technology
  Ethernet Address:
    Unique worldwide
    Ethernet Only
    "Attached" to physical interface
  Need to map from higher layer to lower (i.e IP to Ethernet, using ARP)
  
Ethernet Reminder:
  Ethernet is a broadcast medium
  Structure of Ethernet Frame:
    Preamble - Dest - Source - Type - Data - CRC
  Entire IP packet makes data part of Ethernet frame

Address Resolution Protocol:
  ARP is only used in IPv4 (ND replaces ARP in IPv6)
  Check ARP cache for matching IP address
  If not found, broadcast packet with IP address to every host on Ethernet
  Owner of the IP address responds
  Response cached in ARP table fro future use
  Old cache entries removed by timeout

Types of ARP Messages:
  ARP request - who is IP addr x.x.x.x tell IP addr y.y.y.y
  ARP reply - IP addr x.x.x.x is Ethernet Address hh:hh:hh:hh:hh:hh

ARP Procedure
  ARP Cache is checked on first comp
  ARP Request is sent using broadcast to all other comps
  ARP Entry is added by target comp
  ARP Reply is sent unicast by target to first
  ARP Entry is added to first comp


ARP Table:
  IP Address - Hardware Address - Age (Sec)

</pre>

# Routing
page 14

<pre>

Routing's 3 Aspects:
  Acquisition of information about the IP subnets that are reachable through an internet
    Static routing configuration information
    Dynamic routing info protocols (e.g. BGP4, OSPF, RIP, ISIS)
    Each mechanism/protocol constructs a Routing Information Base (RIB)
  Construction of a Forwarding Table
    Synthesis of a single table from all the Routing Information Bases (RIBs)
    Info about a destination subnet may be acquired multiple ways
    A precedence is defined among the RIBs to arbitrate conflicts on the same subnet
    Also called a Forwarding Information Base (FIB)
  Use of a forwarding table to forward individual packets
    Selection of the next-hop router and interface
    Hop-by-hop, each router makes an independent decision

Routing Tables Feed the Forwarding Table:

                  |BGP 4 Routing Table
FIB  <-- RIBs <---|ISIS - Link State Database
                  | Static Routes
                  
RIB Construction:
  Each routing protocol builds its own RIB
  Each protocol has its own "view" of "costs"
    e.g. ISIS is administrative weights
         BGP4 is Autonomous System path length

FIB Construction
  THERE IS ONLY ONE FORWARDING TABLE  
  An algorithm is used to choose one next-hop toward each IP destination known by any routing protocol
    The set of IP destinations present in any RIB are collected
    If a particular IP destination is present in only one RIB, that RIB determines the next hop forwarding path for that destination
    If a particular IP destination is present in multiple RIBs, then a precedence is defined to select which RIB entry determines the next-hop forwarding path for that destination
  There are no standards for this; it is an implementation (vendor) decision

FIB Contents:
  IP subnet and mask (or length) of destinations - can be the "default" IP subnet
  IP address of the "next hop" toward that IP subnet
  Interface ID of the subnet associated with the next hop
  Optional: cost metric associated with this entry in the forwarding table

IP Routing:
  Default Route:
    Where to send packets if there is no entry for the destination in the routing table
    Most machines have a single default route
    Often referred to as a default gateway
  0.0.0.0/0 - matches all possible destinations, but is usually not the longest match
  
IP Route Lookup: Longest Match Routing
  Most specific/longest match always wins - MANY PEOPLE FORGET THIS, EVEN EXPERIENCES ISP ENGINEERS
  Default route is 0.0.0.0/0  
    Can handle it using the normal longest match algorithm, matches everything, always the shortest match

</pre>

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
