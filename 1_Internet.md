# Table of contents
- [1. Understanding the Internet](#1-understanding-the-internet)
  - [Physical Infrastructure](#physical-infrastructure)
- [2. Organizations Overseeing Networking Frameworks](#2-organizations-overseeing-networking-frameworks)
  - [A. Standards Governed by IEEE (Layer 1 & 2)](#a-standards-governed-by-ieee-layer-1--2)
  - [B. Standards Governed by IETF (Protocols)](#b-standards-governed-by-ietf-protocols)
  - [C. Standards Governed by IANA (Addressing)](#c-standards-governed-by-iana-addressing)
- [3. TCP/IP and OSI Model](#3-tcpip-and-osi-model)
  - [OSI Model (7 Layers)](#osi-model-7-layers)
  - [TCP/IP Model (4 Layers)](#tcpip-model-4-layers)
  - [Comparison](#comparison)
  - [3.1 Encapsulation](#31-encapsulation)
  - [3.2 Host to Host (Transmission)](#32-host-to-host-transmission)
  - [3.3 Deencapsulation](#33-deencapsulation)
  - [3.4 Advantages](#34-advantages)

---

# 1. Understanding the Internet
The Internet is a global system of interconnected computer networks. It is the "network of networks" that allows devices worldwide to communicate.

## Physical Infrastructure
The internet isn't a cloud. It's tangible hardware connecting the world:
- **Undersea Cables**: Massive fiber-optic cables on the ocean floor carrying ~99% of intercontinental data.
- **Fiber Optic Cables**: The high-speed backbone of land-based networks connecting cities and countries.
- **Satellites & Wireless**: Providing connectivity to remote areas (e.g., ships, planes, rural locations).
- **Internet Exchange Points (IXPs)**: Physical locations where different networks (ISPs, content providers) interconnect to exchange traffic directly, improving speed and reducing costs.

> **Explore the Map:**
> - [Submarine Cable Map](https://www.submarinecablemap.com/) - View the underwater arteries of the internet.
> - [Internet Exchange Map](https://www.internetexchangemap.com/) - See where different networks physically connect (IXPs).
> - [Satellite Map](https://satellitemap.space/) - Visualize the constellation of internet satellites (like Starlink).

---

# 2. Organizations Overseeing Networking Frameworks
**There will be a deeper dive into these topic as we continue on the journey.** 

In the early days of networking, vendors (like IBM and Cisco) created proprietary communication frameworks. If a company used Cisco devices and wanted to connect with IBM devices, they could not communicate.

**Layered models** (like OSI and TCP/IP) solved this by ensuring:
- **Interoperability**: Devices from any vendor can communicate seamlessly by adhering to universal standards.
- **Troubleshooting**: Engineers can isolate problems to specific layers (e.g., "The network layer is failing, so it's a routing issue, not a cable issue").
- **Protocol Development**: Developers can build new features for one layer without redesigning the entire stack.

## A. Standards Governed by IEEE (Layer 1 & 2)
The **Institute of Electrical and Electronics Engineers (IEEE)** creates the standards for the physical hardware we use to connect. Without them, your WiFi card wouldn't talk to your router, and your ethernet cable wouldn't fit in the port.

| Standard | Name | Description | Key Focus |
| :--- | :--- | :--- | :--- |
| **IEEE 802.3** | Ethernet | Defines wired connections, cabling specs, and speed. | Wired LAN Access |
| **IEEE 802.11** | Wi-Fi | Defines standards for Wireless LANs. | Wireless LAN Access |
| **IEEE 802.1Q** | VLAN Tagging | Mechanism for tagging frames with VLAN info on trunk links. | Trunking & VLANs |

## B. Standards Governed by IETF (Protocols)
The **Internet Engineering Task Force (IETF)** defines the "language" of the internet. They create the voluntary standards (protocols) that allow different software and operating systems to understand each other.

### Request for Comments (RFCs)
The IETF publishes documents called **RFCs**. These contain the technical specifications and policies for the internet. If you want to know exactly how a protocol like IP or TCP works down to the bit level, you read its RFC.

| Protocol | RFC | Description |
| :--- | :--- | :--- |
| **IP** | RFC 791 | Internet Protocol - Defines addressing and routing. |
| **TCP** | RFC 793 | Transmission Control Protocol - Defines reliable delivery. |
| **DNS** | RFC 1035 | Domain Name System - Defines name resolution. |
| **HTTP** | RFC 9110 | Hypertext Transfer Protocol - Defines web data transfer. |

## C. Standards Governed by IANA (Addressing)
The **Internet Assigned Numbers Authority (IANA)** is the "bookkeeper" of the internet. They manage unique global identifiers to ensure every device and service has a unique address and no conflicts occur.

### 1. IP Addresses (Public vs. Private)
IANA defines specific blocks of IP addresses for private use (non-routable on the internet).
- **Class A**: `10.0.0.0` - `10.255.255.255`
- **Class B**: `172.16.0.0` - `172.31.255.255`
- **Class C**: `192.168.0.0` - `192.168.255.255`

> **Historical Note**: The terms "Class A, B, C" refer to the original classful addressing scheme from the early internet. This system is now obsolete and replaced by **CIDR (Classless Inter-Domain Routing)**, which provides more flexible IP address allocation. However, the private IP ranges defined above are still valid and widely used.

### 2. Port Numbers
IANA divides TCP/UDP ports into three categories to identify services (like Web, Mail, DNS).
- **Well-Known Ports (0-1023)**: Reserved for system processes (e.g., HTTP: 80, HTTPS: 443).
- **Registered Ports (1024-49151)**: Registered by companies (e.g., MySQL: 3306).
- **Dynamic/Private Ports (49152-65535)**: Used temporarily by client applications.

---

# 3. TCP/IP and OSI Model
These models provide the "blueprint" for how network communications happen.

## OSI Model (7 Layers)
The **Open Systems Interconnection (OSI)** model is a conceptual framework used to understand and troubleshoot networks.

| Layer # | Layer Name | Description | Key Protocols/Devices |
| :--- | :--- | :--- | :--- |
| 7 | **Application** | Interface for end-user processes. | HTTP, DNS, SMTP |
| 6 | **Presentation** | Data formatting and encryption. | SSL/TLS, JPEG |
| 5 | **Session** | Manages sessions between applications. | RPC, NetBIOS |
| 4 | **Transport** | Reliable delivery and error correction. | TCP, UDP |
| 3 | **Network** | Routing and logical addressing (IP). | IP, Routers, MPLS |
| 2 | **Data Link** | Physical addressing (MAC) and switching. | Ethernet, Switches |
| 1 | **Physical** | Transmission of raw bits over media. | Cables, Hubs |

## TCP/IP Model (4 Layers)
The **TCP/IP** model is the practical implementation used in the real world (the Internet). It condenses the OSI layers.

| TCP/IP Layer | Corresponds to OSI | Description |
| :--- | :--- | :--- |
| **Application** | App, Pres, Session | Handles high-level data and user interface. |
| **Transport** | Transport | Handles connection and reliability (TCP/UDP). |
| **Internet** | Network | Handles routing and addressing (IP). |
| **Network Access** | Data Link, Physical | Handles physical hardware and framing. |

## Comparison
**Why Two Models?**

- **OSI Model**: A theoretical framework created by ISO (International Organization for Standardization) for understanding and teaching networking. It provides granular separation with 7 distinct layers, making it ideal for troubleshooting and academic study.

- **TCP/IP Model**: The practical model that the actual Internet runs on, developed by DARPA. It combines OSI layers for simplicity, reflecting how protocols are actually implemented in software.

**Key Differences:**

| Aspect | OSI Model | TCP/IP Model |
| :--- | :--- | :--- |
| **Layers** | 7 layers | 4 layers |
| **Development** | Theoretical (ISO) | Practical (DARPA/IETF) |
| **Usage** | Teaching & Troubleshooting | Real-world Implementation |
| **Top Layers** | Separate App/Presentation/Session | Combined into Application |
| **Bottom Layers** | Separate Data Link/Physical | Combined into Network Access |

**In Practice**: Network engineers use OSI terminology for troubleshooting ("It's a Layer 3 issue") but design networks using TCP/IP protocols.

---
# 3.1 Encapsulation
**Encapsulation** is the process of adding headers and trailers to data as it moves down the protocol stack. Think of it like putting a letter in an envelope, then putting that envelope in a box, then putting that box in a shipping container.

### Visualizing the Flow
As data moves down from the Application layer to the Physical layer, each layer adds its own "envelope" (Header/Trailer).

```text
Layer 4: Transport (Segment)
+------------------+-------------------------------+
| Transport Header |       Application Data        |
+------------------+-------------------------------+
       |
       v
Layer 3: Network (Packet)
+-----------+------------------+-------------------------------+
| IP Header | Transport Header |       Application Data        |
+-----------+------------------+-------------------------------+
       |
       v
Layer 2: Data Link (Frame)
+-----------------+-----------+------------------+-------------------------------+------------------+
| Ethernet Header | IP Header | Transport Header |       Application Data        | Ethernet Trailer |
+-----------------+-----------+------------------+-------------------------------+------------------+
       |
       v
Layer 1: Physical (Bits)
01101010010101011101010010101010101010101...
```

### What happens at each layer?
1.  **Layer 7 (Application)**: You generate data (e.g., an HTTP request).
2.  **Layer 4 (Transport)**: Adds a **Transport Header** (TCP/UDP) with Source & Destination Ports. The data is now a **Segment**.
3.  **Layer 3 (Network)**: Adds an **IP Header** with Source & Destination IP Addresses. The segment becomes a **Packet**.
4.  **Layer 2 (Data Link)**: Adds an **Ethernet Header** (Source/Dest MAC addresses) and a **Trailer** (FCS - Frame Check Sequence for error checking). The packet becomes a **Frame**.
5.  **Layer 1 (Physical)**: The frame is converted into **Bits** (0s and 1s) and transmitted over the wire or air.

# 3.2 Host to Host (Transmission)
Once the data is fully encapsulated and converted into **Bits** (Layer 1), it begins its journey from the source device to the destination across the physical medium (cables, fiber, or airwaves).

### The Journey Between Source and Destination
The path from sender to receiver isn't always direct. Data typically passes through multiple network devices, each operating at different layers:

1. **Local Network (Same Subnet)**
   - If the destination is on the same local network, the **Switch** (Layer 2 device) reads the **Ethernet Frame**.
   - The switch examines the **Destination MAC Address** (Media Access Control - the unique hardware address) in the frame header and forwards it out the correct port.
   - The frame arrives directly at the destination device.
   - *Note: The source device uses **ARP (Address Resolution Protocol)** to discover the destination's MAC address from its IP address when they're on the same network.*

2. **Remote Network (Different Subnet)**
   - If the destination is on a different network (e.g., across the internet), the frame must first reach a **Router** (Layer 3 device).
   - The router strips off the Layer 2 frame, examines the **Destination IP Address** in the **Packet** header, and determines the next hop.
   - The router then re-encapsulates the packet in a new Layer 2 frame (potentially with different MAC addresses) and forwards it toward the destination.
   - This process may repeat through multiple routers until the packet reaches the destination network.

3. **Final Delivery**
   - Once the packet arrives at the destination network, the local switch forwards the frame to the correct host using the MAC address.
   - The destination device receives the frame and begins **Deencapsulation**.

### Visualizing the Path
```text
[Source PC] 
    |
    | (Encapsulation: Data → Segment → Packet → Frame → Bits)
    |
    v
[Switch] ← Reads Layer 2 (MAC Address)
    |
    v
[Router] ← Reads Layer 3 (IP Address), makes routing decision
    |
    v
[Internet] ← May pass through multiple routers
    |
    v
[Router] ← Final router near destination
    |
    v
[Switch] ← Local switch at destination network
    |
    v
[Destination PC]
    |
    | (Deencapsulation: Bits → Frame → Packet → Segment → Data)
```

### Key Takeaways
- **Switches** operate at Layer 2 and use **MAC addresses** to forward frames within a local network.
- **Routers** operate at Layer 3 and use **IP addresses** to route packets between different networks.
- Data may be **re-encapsulated** at each router hop (new Layer 2 headers) while the Layer 3 packet and above remain intact.

*Note: The complex details of how Switches learn MAC addresses and how Routers build routing tables will be covered in later sections on Layer 2 (Switching) and Layer 3 (Routing).*

---

# 3.3 Deencapsulation
**Deencapsulation** is the process of removing headers and trailers as data moves up the protocol stack. At each layer, the receiving device processes and strips away the relevant header before passing the data up to the next layer.

### What Happens at Each Layer?

1. **Layer 1 (Physical)**: The network interface card (NIC) receives electrical signals, light pulses, or radio waves and converts them back into **Bits** (0s and 1s).

2. **Layer 2 (Data Link)**: 
   - The NIC assembles bits into a complete **Frame**.
   - Validates the **FCS (Frame Check Sequence)** in the Ethernet Trailer to detect transmission errors.
   - Checks if the Destination MAC address matches its own (or is a broadcast/multicast).
   - If valid, strips the Ethernet Header and Trailer, passing the **Packet** to Layer 3.

3. **Layer 3 (Network)**: 
   - The operating system's network stack examines the **IP Header**.
   - Verifies the Destination IP Address matches the device's IP.
   - Checks for fragmentation and reassembles if needed.
   - Strips the IP Header and passes the **Segment** to Layer 4.

4. **Layer 4 (Transport)**: 
   - Examines the **Transport Header** (TCP or UDP).
   - Uses the **Destination Port Number** to determine which application should receive the data (e.g., port 80 → web server).
   - For TCP: Performs error checking, acknowledges receipt, and handles reordering.
   - Strips the Transport Header and delivers the **Application Data** to Layer 7.

5. **Layer 7 (Application)**: The application (web browser, email client, etc.) receives the raw data and processes it according to its protocol (HTTP, SMTP, etc.).

### Visualizing the Flow
```text
Layer 1: Physical (Bits)
01101010010101011101010010101010101010101...
       |
       v
Layer 2: Data Link (Frame)
+-----------------+-----------+------------------+-------------------------------+------------------+
| Ethernet Header| IP Header | Transport Header |       Application Data        | Ethernet Trailer |
+-----------------+-----------+------------------+-------------------------------+------------------+
       |
       v
Layer 3: Network (Packet)
+-----------+------------------+-------------------------------+
| IP Header | Transport Header |       Application Data        |
+-----------+------------------+-------------------------------+
       |
       v
Layer 4: Transport (Segment)
+------------------+-------------------------------+
| Transport Header |       Application Data        |
+------------------+-------------------------------+
       |
       v
Layer 7: Application (Data)
+-------------------------------+
|       Application Data        |
+-------------------------------+
```


# 3.4 Advantages of Layered Models
Using a layered approach (like OSI or TCP/IP) provides critical benefits for network engineering:

1.  **Interoperability**: Different vendors (Cisco, Juniper, Apple, Microsoft) can communicate because they all speak the same "standard" at each layer.
2.  **Troubleshooting**: It's easier to fix a problem when you can isolate it to a layer.
    *   *Example*: If you can ping a server (Layer 3 works) but can't load the web page (Layer 7 issue), you know the cables and IP addressing are fine.
3.  **Modular Development**: Developers can write code for an application (Layer 7) without needing to worry about how the bits are physically transmitted (Layer 1).

---

# Summary
We have covered the fundamental structure of the internet, the organizations that govern its standards, and the layered models that ensure communication happens reliably. This foundation sets the stage for diving deeper into each layer, starting with the Physical Layer.
