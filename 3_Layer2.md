# Layer 2: Data Link Layer

## What is Layer 2?
Layer 2 (Data Link Layer) is responsible for **local network communication** within the same network segment. While Layer 1 transmits raw bits over physical media, Layer 2 organizes those bits into **frames**, adds **MAC addresses** for local device identification, and handles error detection and frame forwarding.

**Key Functions:**
- Frame creation and formatting using MAC addresses
- Switch-based forwarding within a local network
- Network segmentation using VLANs (Virtual LANs)
- Loop prevention using Spanning Tree Protocol (STP)
- Media access control (determining when devices can transmit on shared media)

**Relationship to Other Layers:**
- **Layer 1 (Physical)**: Provides the physical transmission medium; Layer 2 structures data into frames for transmission.
- **Layer 3 (Network)**: Provides logical addressing (IP) for routing between networks; Layer 2 handles local delivery within the same subnet.

---

## Layer 2 Overview

**Devices:** Switches, Wireless Access Points (WAPs), Network Interface Cards (NICs)

**Protocols:** Ethernet, IEEE 802.11 (Wi-Fi), IEEE 802.1Q (VLAN Tagging), IEEE 802.1X (Port-Based Authentication), ARP (Address Resolution Protocol)

**Layer 2 Objectives:**
1. Forward data frames to the correct host based on MAC addresses
2. Avoid network congestion through intelligent switching
3. Prevent network loops via Spanning Tree Protocol
4. Segment networks into VLANs for security and performance
5. Provide error detection and frame validation

---

## Key Layer 2 Concepts

### Collision Domain
A network segment where simultaneous transmissions cause collisions (signals interfere and data is lost). 
- **On Hubs**: All ports share one collision domain (everyone competes to transmit).
- **On Switches**: Each port is its own collision domain (dedicated, collision-free links).

### Broadcast Domain
The set of all devices that receive broadcast frames when one device broadcasts.
- **Without VLANs**: All devices connected to a switch are in one broadcast domain.
- **With VLANs**: Each VLAN is a separate broadcast domain; routers separate broadcast domains.

### MAC Address Table (CAM Table)
The switch's dynamic memory table that maps MAC addresses to physical ports. Learned automatically as devices send frames.

---

## Why Are Switches Important?

1. **Collision Elimination**: Each physical link is its own collision domain, eliminating collisions unlike hubs where all ports share the same collision domain.

2. **Virtual Local Area Networks (VLANs)**: Switches can create multiple logical networks on a single physical switch.
   - In small networks, this may not be critical, but large networks benefit from reduced congestion and improved security.
   - When a broadcast or unknown unicast frame arrives, the switch floods it within the VLAN only (thanks to VLAN tagging), preventing broadcasts from reaching devices in other VLANs.

---

# Table of Contents
- [Layer 2: Data Link Layer](#layer-2-data-link-layer)
  - [What is Layer 2?](#what-is-layer-2)
  - [Key Layer 2 Concepts](#key-layer-2-concepts)
- [1.1 MAC Address](#11-mac-address)
- [1.2 Ethernet Frame](#12-ethernet-frame)
- [1.3 Address Resolution Protocol (ARP)](#13-address-resolution-protocol-arp)
- [1.4 How Switches Forward Frames](#14-how-switches-forward-frames)
- [1.5 VLAN Tagging](#15-vlan-tagging)
- [1.6 Spanning Tree Protocol (STP)](#16-spanning-tree-protocol-stp)
- [1.7 STP Toolkit](#17-stp-toolkit)
- [1.8 Rapid Spanning Tree Protocol (RSTP)](#18-rapid-spanning-tree-protocol-rstp)
- [1.9 MSTP](#19-mstp)
- [1.10 Link Aggregation (EtherChannel)](#110-link-aggregation-etherchannel)
- [1.11 802.1X Port-Based Authentication](#111-802-1x-port-based-authentication)
- [1.12 Multicast and IGMP Snooping](#112-multicast-and-igmp-snooping)
- [1.13 Layer 2 Security](#113-layer-2-security-risks-and-mitigations)

---

# 1.1 MAC Address

## What is a MAC Address?
- **Media Access Control Address**
- **48-bit address** (often expressed as 12 hexadecimal characters)
- **Unique identifier** assigned to each network interface controller (NIC)
- **Used to identify** the source and destination of data frames on a local network
- **Scope**: Layer 2 (Data Link) - used for local delivery within the same network segment

## How Does a MAC Address Work?

A MAC address is divided into two parts:

1. **OUI (Organizationally Unique Identifier)** - First 24 bits (3 bytes)
   - Assigned by IEEE to manufacturers
   - Identifies the manufacturer of the NIC
   - Example: Apple devices start with `00:1A:A0`, Cisco with `00:1B:D5`

2. **Device Identifier** - Last 24 bits (3 bytes)
   - Unique to each NIC produced by that manufacturer
   - Ensures no two NICs have the same MAC address globally

**Result**: Each MAC address is theoretically globally unique and identifies a specific network interface.

## MAC Address Format
- **12 hexadecimal digits** (0-9, A-F)
- **Separated by colons** (most common) or **hyphens**
- **Example**: `00:1A:2B:3C:4D:5E` or `00-1A-2B-3C-4D-5E`

## MAC Address Types

### Unicast MAC Address
- Represents a **single device**
- First octet has **even last bit** (e.g., 00, 02, 04... in hex)
- Example: `00:1A:2B:3C:4D:5E`
- **Usage**: Sending data to a specific host

### Broadcast MAC Address
- Represents **all devices** on a network segment
- Address: `FF:FF:FF:FF:FF:FF`
- When a device sends to this address, **all devices receive** it
- **Usage**: ARP requests, DHCP discovery

### Multicast MAC Address
- Represents **a group of devices**
- First octet has **odd last bit** (e.g., 01, 03, 05... in hex)
- Range: `01:00:5E:00:00:00` to `01:00:5E:7F:FF:FF` (IPv4 multicast)
- **Usage**: Video streaming, online gaming, stock tickers to interested subscribers

## Viewing MAC Addresses on Your Device

**Windows:**
```cmd
ipconfig /all
```
Look for "Physical Address"

**macOS:**
```bash
ifconfig
```
Look for "ether"

**Linux:**
```bash
ip addr show
```
Look for "link/ether"

---

# 1.2 Ethernet Frame

## Ethernet Frame Structure (802.3)

The Ethernet frame is the fundamental "package" of data at Layer 2. Here's its complete structure:

```
+----------+-----+-------------+-------------+------+---------+-----+
| Preamble | SFD | Dest MAC    | Source MAC  | Type | Payload | FCS |
| 7 bytes  | 1B  | 6 bytes     | 6 bytes     | 2B   | 46-1500B| 4B  |
+----------+-----+-------------+-------------+------+---------+-----+
```

**With Optional 802.1Q VLAN Tag:**
```
+----------+-----+-------------+-------------+--------+------+---------+-----+
| Preamble | SFD | Dest MAC    | Source MAC  | 802.1Q | Type | Payload | FCS |
| 7 bytes  | 1B  | 6 bytes     | 6 bytes     | 4 bytes| 2B   | 46-1500B| 4B  |
+----------+-----+-------------+-------------+--------+------+---------+-----+
```

---

## Field Descriptions

### Preamble (7 bytes)
- **Purpose**: Synchronization
- **Pattern**: `01010101 01010101 01010101 01010101 01010101 01010101 01010101`
- **Function**: Tells the receiver "data is coming" and allows it to synchronize its clock with the sender's clock
- **Analogy**: Like the clicking sound before a song on a record

### Start Frame Delimiter (SFD) (1 byte)
- **Pattern**: `10101011` (unique break in the preamble pattern)
- **Purpose**: Marks the exact start of the Ethernet frame
- **Function**: Receiver knows "real data starts NOW"

### Destination MAC Address (6 bytes)
- **Purpose**: Identifies the intended recipient
- **Content**: MAC address of the receiving device
- **Scope**: Used by switches to decide which port to forward the frame out
- **Example**: `FF:FF:FF:FF:FF:FF` (broadcast to all)

### Source MAC Address (6 bytes)
- **Purpose**: Identifies the sender
- **Content**: MAC address of the sending device
- **Important**: Switches learn MAC addresses by examining this field
- **Security Risk**: Can be spoofed (MAC spoofing)

### VLAN Tag [Optional] (4 bytes) - Only in tagged frames
- **Standard**: IEEE 802.1Q
- **Content**: 
  - TPID (Tag Protocol ID): 0x8100 (identifies this as an 802.1Q tag)
  - PCP (Priority Code Point): 3 bits for Quality of Service (QoS)
  - DEI (Drop Eligible Indicator): 1 bit
  - VLAN ID: 12 bits (0-4094 usable VLANs)
- **Purpose**: Tells the switch which VLAN this frame belongs to
- **Usage**: Only on trunk ports carrying multiple VLANs

### Type/Length Field (2 bytes)
- **Dual Purpose**:
  - **If value ≤ 1500**: Field indicates **payload length** in bytes
  - **If value > 1500**: Field indicates **protocol type** (EtherType)

**Common EtherType Values:**
| Value (Hex) | Protocol | Description |
| :--- | :--- | :--- |
| `0x0800` | IPv4 | Internet Protocol v4 |
| `0x0806` | ARP | Address Resolution Protocol |
| `0x8100` | 802.1Q | VLAN Tag |
| `0x86DD` | IPv6 | Internet Protocol v6 |
| `0x0835` | RARP | Reverse ARP |

### Payload (46–1500 bytes)
- **Content**: The actual data being transmitted (from Layer 3 and above)
- **Minimum**: 46 bytes (padded with zeros if needed to meet minimum frame size)
- **Maximum**: 1500 bytes (called **MTU - Maximum Transmission Unit**)
- **If data < 46 bytes**: Frame is **padded** with zeros to reach minimum
- **If data > 1500 bytes**: Data is **fragmented** into multiple frames by Layer 3

### Frame Check Sequence (FCS) (4 bytes)
- **Purpose**: **Error detection** (NOT error correction)
- **Method**: CRC-32 (Cyclic Redundancy Check)
- **How it works**:
  1. Sender calculates CRC on all previous fields and appends FCS
  2. Receiver calculates CRC on received frame
  3. If calculated FCS ≠ received FCS → **Frame is corrupted and discarded**
  4. If FCS matches → Frame is accepted and passed to Layer 3

**Important**: FCS only detects errors, doesn't fix them. Corrupted frames are discarded.

---

## Ethernet Frame Size

- **Minimum**: 64 bytes (including preamble and SFD, but often not counted)
- **Maximum**: 1518 bytes (traditional)
- **Jumbo Frames**: Up to 9000 bytes (used in data centers for efficiency)

**If payload is too small**: The frame is **padded with zeros** to reach 64 bytes minimum.

---

# 1.3 Address Resolution Protocol (ARP)

## The Problem

Devices know **IP addresses** (from Layer 3), but Ethernet frames require **MAC addresses** (Layer 2) to be delivered locally. How does a device find the MAC address corresponding to an IP address?

**Example**: Your PC wants to send data to the gateway (router) at IP `192.168.1.1`, but doesn't know its MAC address.

---

## What is ARP?

**ARP (Address Resolution Protocol)** is a Layer 2 protocol that **maps IP addresses to MAC addresses** within a local network (same subnet/VLAN).

**Key Points:**
- Operates at **Layer 2** (though it uses Layer 3 addresses)
- Used on **local networks only** (doesn't cross routers)
- **Automatic**: Works transparently in the background
- **Dynamic**: Mappings are learned and cached temporarily

---

## How ARP Works: Step-by-Step

**Scenario**: Host A (192.168.1.5) wants to send data to Host B (192.168.1.10).

```
Step 1: Host A checks its ARP table
   - "Do I have Host B's MAC address cached?"
   - NO → Go to Step 2

Step 2: Host A sends ARP Request (Broadcast)
   - Destination MAC: FF:FF:FF:FF:FF:FF (broadcast)
   - Type: ARP Request
   - Message: "Who has IP 192.168.1.10? Tell 192.168.1.5 (00:11:22:33:44:55)"
   - ALL devices on the network receive this

Step 3: Devices check the request
   - Host B: "That's me! I'll respond"
   - Other devices: "Not me, ignore"

Step 4: Host B sends ARP Reply (Unicast)
   - Destination MAC: 00:11:22:33:44:55 (directly to Host A)
   - Type: ARP Reply
   - Message: "I have 192.168.1.10, my MAC is AA:BB:CC:DD:EE:FF"
   - ONLY Host A receives this

Step 5: Host A updates ARP cache
   - ARP Table Entry: 192.168.1.10 → AA:BB:CC:DD:EE:FF
   - TTL (Time To Live): Typically 2-10 minutes

Step 6: Communication proceeds
   - Host A sends data to AA:BB:CC:DD:EE:FF (directly, no ARP needed)
```

---

## ARP Cache

### Viewing Your ARP Table

**Windows:**
```cmd
arp -a
```

**macOS/Linux:**
```bash
arp -n
```
or
```bash
ip neigh show
```

**Output Example:**
```
? (192.168.1.1) at 00:1a:2b:3c:4d:5e [ether] on eth0
? (192.168.1.100) at aa:bb:cc:dd:ee:ff [ether] on eth0
```

### ARP Entry States

| State | Meaning |
| :--- | :--- |
| **Reachable** | Valid entry; being actively used |
| **Stale** | Entry expired; next frame will trigger ARP refresh |
| **Permanent** | Manually configured; never ages out |
| **Incomplete** | ARP request sent, waiting for reply |

---

## Security Risk: ARP Spoofing

**The Attack:**
An attacker sends fake ARP replies to associate their MAC with another device's IP.

**Example**:
- Attacker sends: "I have 192.168.1.1 (gateway), my MAC is AA:AA:AA:AA:AA:AA"
- Your PC believes it and updates its ARP table
- Your traffic meant for the gateway is redirected to the attacker
- **Result**: Man-in-the-Middle (MITM) attack

**Mitigation** (see Section 1.13):
- **Dynamic ARP Inspection (DAI)**: Switch validates ARP messages against DHCP snooping bindings
- **Static ARP entries**: Manually configure critical ARP mappings
- **ARP monitoring**: Watch for suspicious ARP activity

---

# 1.4 How Switches Forward Frames

## MAC Learning: Building the MAC Address Table

When a switch receives a frame, it learns the **source MAC address** and updates its MAC Address Table (CAM Table).

```
Frame arrives on port Gi0/1 from source MAC 00:11:22:33:44:55
  ↓
Switch: "I learned that 00:11:22:33:44:55 is on port Gi0/1"
  ↓
MAC Address Table Entry Created:
   MAC: 00:11:22:33:44:55
   Port: Gi0/1
   VLAN: 1
   Age: 0 (timer starts)
  ↓
Every few seconds, timer resets when traffic from that MAC is seen
After ~300 seconds (5 minutes) of inactivity: Entry is aged out and deleted
```

---

## Forwarding Decisions: Four Frame Types

Once learned, a switch makes forwarding decisions based on the **destination MAC**:

### 1. Known Unicast
**Condition**: Destination MAC is in the MAC Address Table (same VLAN)

**Action**: Forward the frame **only to the learned port**

```
Frame arrives on Gi0/1, destination: AA:BB:CC:DD:EE:FF
Switch looks up: AA:BB:CC:DD:EE:FF → Port Gi0/5 (same VLAN)
Forward frame OUT: Gi0/5 only (efficient, no flood)
```

**Benefit**: Reduces unnecessary network traffic

### 2. Unknown Unicast
**Condition**: Destination MAC is NOT in the MAC Address Table

**Action**: **Flood the frame to all ports in the VLAN** (except ingress port)

```
Frame arrives on Gi0/1, destination: XX:XX:XX:XX:XX:XX
Switch looks up: XX:XX:XX:XX:XX:XX → NOT FOUND
Flood to: Gi0/2, Gi0/3, Gi0/4, Gi0/5, Gi0/6... (all ports except Gi0/1)

Why? The switch doesn't know where XX:XX:XX:XX:XX:XX is,
so it sends a copy to everywhere.

When the destination responds, the switch learns its MAC from the reply.
```

**Security risk**: If attacker floods with many unknown unicast frames, the switch floods excessively (DoS).

### 3. Broadcast
**Condition**: Destination MAC is `FF:FF:FF:FF:FF:FF`

**Action**: **Flood to all ports in the VLAN** (except ingress port)

```
Frame arrives on Gi0/1, destination: FF:FF:FF:FF:FF:FF
Flood to: Gi0/2, Gi0/3, Gi0/4, Gi0/5, Gi0/6... (all ports except Gi0/1)

Example broadcasts:
- ARP requests
- DHCP discovery
- Routing protocol hellos
```

**Important**: Broadcasts are **contained within a VLAN** (Layer 3 routers separate broadcast domains).

### 4. Multicast
**Condition**: Destination MAC starts with `01:00:5E` (IPv4 multicast)

**Action**: **Flood to all ports** (unless IGMP Snooping limits it)

```
WITHOUT IGMP Snooping:
Frame arrives on Gi0/1, destination: 01:00:5E:01:01:01 (multicast group)
Flood to: All ports except Gi0/1 (same as broadcast)

WITH IGMP Snooping:
Switch listens to IGMP Join messages to learn which ports want the multicast group
Forward only to those interested ports
Reduces unnecessary flooding
```

---

## Per-VLAN MAC Tables

**Critical Concept**: The MAC Address Table is **maintained separately per VLAN**.

```
VLAN 1 MAC Table:
  00:11:22:33:44:55 → Gi0/1
  AA:BB:CC:DD:EE:FF → Gi0/2

VLAN 10 MAC Table:
  00:11:22:33:44:55 → Gi0/5  (same MAC, different VLAN, different port!)
  XX:XX:XX:XX:XX:XX → Gi0/6

Forwarding never crosses VLAN boundaries!
```

---

## Ingress Filtering (Prevent Loops)

**Rule**: Frames are **never sent back out the port they arrived on**

```
Frame arrives on Gi0/1 → Forward to Gi0/2, Gi0/3, Gi0/4... 
                       BUT NOT back to Gi0/1
```

**Why**: Prevents immediate loops and unnecessary traffic.

---

## Hands-On: Viewing the MAC Address Table

**On a Cisco Switch:**
```
Switch# show mac address-table
          Mac Address Table
-------------------------------------------
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0011.2233.4455    DYNAMIC     Gi0/1
   1    aabb.ccdd.eeff    DYNAMIC     Gi0/2
  10    1122.3344.5566    DYNAMIC     Gi0/5
  10    aabb.ccdd.eeff    DYNAMIC     Gi0/6
```

**Key Points:**
- `DYNAMIC`: Learned automatically (ages out)
- `STATIC`: Manually configured (never ages out)
- Same MAC (`aabb.ccdd.eeff`) appears in different VLANs with different ports

---

# 1.5 VLAN Tagging

## Virtual Local Area Network (VLAN)

A **VLAN** is a logical network created on a physical switch, allowing multiple isolated networks on one device.

**Benefits:**
- **Network Segmentation**: Separate users/departments logically
- **Security**: Restrict communication between VLANs (requires a router)
- **Performance**: Reduce broadcast domains and flooding
- **Flexibility**: Move devices to VLANs without rewiring

---

## Access vs. Trunk Ports

### Access Ports
- **Carries**: **One VLAN only**
- **Frame format**: Frames are **untagged** on the wire
- **Switch behavior**: The switch assigns frames to the configured VLAN internally
- **Typical use**: End devices (PCs, printers, IP phones)
- **Configuration**:
```
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
```

**Scenario**:
```
PC (Access VLAN 10) sends frame → Switch receives (untagged) 
  → Switch: "This came from port Gi0/1, which is VLAN 10"
  → Tags frame internally as VLAN 10
  → Forwards only to other VLAN 10 ports
```

### Trunk Ports (802.1Q)
- **Carries**: **Multiple VLANs** simultaneously
- **Frame format**: Frames are **802.1Q tagged** (include VLAN ID)
- **Switch behavior**: Examines VLAN tag to make forwarding decisions
- **Typical use**: Switch-to-switch, switch-to-router links
- **Configuration**:
```
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 1,10,20,30
```

**Scenario**:
```
Switch A (VLAN 10) → Trunk Link → Switch B
  → Frame tagged with VLAN 10: 01:00:5E... | VLAN=10 | ...
  → Switch B examines tag: "VLAN 10, forward to Gi0/5"
```

---

## 802.1Q VLAN Tag Format

An 802.1Q tag is 4 bytes inserted between Source MAC and Type/Length:

```
+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
| TPID (16 bits)  | PCP |DEI|    VLAN ID (12 bits)  |
|   = 0x8100      |(3) |(1)|         (0-4094)       |
+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
```

| Field | Bits | Description |
| :--- | :--- | :--- |
| **TPID** | 16 | Tag Protocol ID = 0x8100 (identifies as 802.1Q) |
| **PCP** | 3 | Priority Code Point (0-7); used for QoS |
| **DEI** | 1 | Drop Eligible Indicator (set if frame can be dropped during congestion) |
| **VLAN ID** | 12 | VLAN number (0-4094 usable; 0 and 4095 reserved) |

---

## Native VLAN

**Definition**: The VLAN on a trunk port that is sent **untagged**.

**Default**: VLAN 1

**Why it matters**:
- Devices on legacy equipment that don't understand 802.1Q tags communicate on the native VLAN
- All untagged frames on a trunk are assigned to the native VLAN

**Best Practice**: **Use a different VLAN as native** (not VLAN 1 or production VLANs)
```
Switch(config-if)# switchport trunk native vlan 999
```

---

## Inter-VLAN Routing

**Problem**: Devices in VLAN 10 **cannot communicate** with devices in VLAN 20 (Layer 2 blocks it).

**Solution**: Use a **Layer 3 device** (router) to route between VLANs.

**Two methods:**

### Method 1: Router-on-a-Stick
A router with a single physical connection to a trunk port:
```
VLAN 10 PCs → Switch (trunk) → Router (subinterfaces) → Switch → VLAN 20 PCs
  Gi0/1.10   Trunk Link       Gi0/1.10, Gi0/1.20        Gi0/2
```

### Method 2: Switched Virtual Interface (SVI)
The switch itself routes between VLANs (simpler, more modern):
```
VLAN 10 PCs → Switch (native routes) → VLAN 20 PCs
   SVI 10                    SVI 20
```

---

# 1.6 Spanning Tree Protocol (STP)

## The Problem: Layer 2 Loops

**Why redundancy matters**: Network designers add backup links for fault tolerance.

**The danger**: Without protection, multiple paths create **broadcast storms**.

### Loop Scenario
```
        [Switch A]
           / \
          /   \
         /     \
   [Switch B]—[Switch C]

Device sends BROADCAST:
  → Switch A floods to B and C
  → B forwards to C, C forwards to A, A forwards to B...
  → INFINITE LOOP!

Network collapses in seconds. Every device is flooded with duplicates.
```

### Symptoms of a Loop
- Network becomes unusable
- Switch CPU at 100%
- Traffic counts skyrocket
- "Storm" of duplicate frames seen in packet captures

---

## What is STP?

**Spanning Tree Protocol (STP)** is a Layer 2 protocol that:
1. **Detects redundant paths**
2. **Blocks some ports** to break loops
3. **Keeps backup paths available** for failover
4. **Reconverges** when topology changes

**Result**: One active path between any two switches (tree topology), with loops blocked but preserved as backups.

---

## STP Operation: Three Phases

### Phase 1: Root Bridge Election
Switches exchange **BPDUs (Bridge Protocol Data Units)** to elect a **Root Bridge**.

- The **Root Bridge** has the **lowest Bridge ID**
- **Bridge ID** = Priority (default 32768) + MAC Address
- All other switches find the best path toward the root

**Default election**:
```
Switch A: Priority 32768, MAC 00:00:00:00:00:01
Switch B: Priority 32768, MAC 00:00:00:00:00:02
Switch C: Priority 32768, MAC 00:00:00:00:00:03

Winner: Switch A (lowest MAC address) becomes Root Bridge
```

### Phase 2: Root Port & Designated Port Selection
Each switch selects:
- **Root Port**: Port closest to Root Bridge (lowest path cost)
- **Designated Port**: Port on a link that forwards toward the root

**Example**:
```
[Root Bridge]
  |
  | (Cost 4) - Link 1
  |
Switch B (Root Port: facing root, Cost 4)
  |
  | (Cost 4) - Link 2
  |
Switch C (Designated Port faces B, Root Port faces B via Link 2)
```

### Phase 3: Port State Assignment
Non-root, non-designated ports are placed in **Blocking state** (discarding frames).

```
[Root]
 |  \ 
 ✓   ✗ (Blocking - breaks the loop)
```

---

## STP Port States (802.1D)

Ports transition through states as the network starts up or topology changes:

| State | Purpose | Duration | Traffic | MAC Learning |
| :--- | :--- | :--- | :--- | :--- |
| **Blocking** | Receives BPDUs only; detects topology. | Default | ✗ None | ✗ No |
| **Listening** | Processes BPDUs; prepares to forward. | ~15 sec | ✗ None | ✗ No |
| **Learning** | Learns MAC addresses from traffic. | ~15 sec | ✗ None | ✓ Yes |
| **Forwarding** | Normal operation; forwards frames. | ∞ | ✓ Yes | ✓ Yes |
| **Disabled** | Admin shutdown or port failure. | ∞ | ✗ None | ✗ No |

**Typical Transition** (30 seconds total):
```
Blocking (15s) → Listening (15s) → Learning (15s) → Forwarding
```

**Why so slow?** To prevent temporary loops during topology changes.

---

## STP Operation Example: Two Connected Switches

```
Initial state: Both switches think they are Root Bridge
  ↓
Switch A sends BPDU: "I'm Root, Priority 32768, MAC 00:00:00:00:00:01"
Switch B sends BPDU: "I'm Root, Priority 32768, MAC 00:00:00:00:00:02"
  ↓
Each receives the other's BPDU
  ↓
Switch A: "Received BPDU with Bridge ID lower than mine" → "A is Root"
Switch B: "Received BPDU with Bridge ID higher than mine" → "A is Root"
  ↓
Switch A remains Root
Switch B designates its port facing A as Root Port
  ↓
Both can forward traffic without loop
```

---

# 1.7 STP Toolkit

Modern STP implementations include tools to optimize convergence and prevent misconfigurations:

## PortFast
**Purpose**: Immediately transitions **access ports** to Forwarding state (skips Listening/Learning).

**Why**: End devices don't cause loops; waiting 30 seconds for a PC to get network access is slow.

**Configuration**:
```
Switch(config-if)# spanning-tree portfast
```

**Warning**: Use **only on access ports**. Never on trunk ports connecting to switches.

---

## BPDU Guard
**Purpose**: Shuts down a **PortFast port** if it receives BPDUs (indicating a switch, not an end device).

**Why**: Prevents accidental connection of switches to access ports.

**Configuration**:
```
Switch(config-if)# spanning-tree bpduguard enable
```

**Effect**:
```
PC plugged into access port with PortFast → Port stays in Forwarding ✓
Switch plugged into access port with PortFast → BPDUs received → Port disabled ✗
```

---

## Root Guard
**Purpose**: Prevents a port from becoming Root Bridge or receiving better BPDUs.

**Why**: Protects the intended Root Bridge from topology manipulation attacks.

**Configuration**:
```
Switch(config-if)# spanning-tree guard root
```

---

## Loop Guard
**Purpose**: Keeps **non-designated ports** blocked if BPDUs stop arriving.

**Why**: Prevents loops if a unidirectional link failure occurs (one direction broken).

**Configuration**:
```
Switch(config-if)# spanning-tree guard loop
```

---

## BPDU Filter
**Purpose**: Suppresses sending/receiving BPDUs on edge ports.

**Warning**: Use cautiously; can cause loops if misconfigured.

**Better alternative**: Use **BPDU Guard** on PortFast ports instead.

---

## Storm Control
**Purpose**: Limits broadcast, multicast, and unknown unicast traffic rates.

**Why**: Prevents broadcast storms (intentional or accidental) from consuming bandwidth.

**Configuration**:
```
Switch(config-if)# storm-control broadcast level 10
Switch(config-if)# storm-control multicast level 10
Switch(config-if)# storm-control unknown-unicast level 10
```

---

# 1.8 Rapid Spanning Tree Protocol (RSTP) & RPVST+

## RSTP (802.1w)

RSTP improves upon classic STP with **faster convergence** and **better port roles**.

### Key Improvements

**Faster Convergence**: 3 seconds vs. 30 seconds
- Proposed/Agreed (P/A) handshake replaces Listen state
- Ports transition to Forwarding much faster

**More Port Roles**:
| Role | Description |
| :--- | :--- |
| **Root** | Faces the Root Bridge (same as STP) |
| **Designated** | Forwards on a segment (same as STP) |
| **Alternate** | Blocked backup to Root Port |
| **Backup** | Blocked backup to Designated Port |

### RSTP Port States
- **Discarding**: (replaces Blocking/Listening) Doesn't forward frames
- **Learning**: Learns MACs but doesn't forward frames
- **Forwarding**: Forwards frames (normal operation)

### Edge Ports
- Automatically transitions to Forwarding (like PortFast)
- Topology change doesn't trigger reconvergence on edge ports

**Configuration**:
```
Switch(config)# spanning-tree mode rapid-pvst
```

---

## RPVST+ (Rapid Per-VLAN Spanning Tree)

- Per-VLAN version of RSTP
- Faster convergence than PVST+
- **Modern standard** for most deployments

**Configuration**:
```
Switch(config)# spanning-tree mode rapid-pvst
```

---

# 1.9 MSTP (Multiple Spanning Tree Protocol)

## Problem with Per-VLAN STP

If you have 100 VLANs, you have 100 separate spanning trees running:
- High CPU overhead
- Complex to manage

---

## Solution: MSTP (802.1s)

**MSTP** maps multiple VLANs to a few **Spanning Tree Instances**.

```
VLAN 1, 2, 3, 4, 5     → Instance 1
VLAN 10, 11, 12, 13    → Instance 2
VLAN 20, 21, 22        → Instance 3
```

**Benefit**: Run 3 instances instead of 20 (dramatically reduces overhead).

**Trade-off**: Loss of per-VLAN path optimization (but acceptable in most networks).

---

# 1.10 Link Aggregation (EtherChannel / LACP)

## Purpose

Combine multiple physical links into **one logical link** for:
- **Redundancy**: If one link fails, others carry the load
- **Increased throughput**: Multiple links effectively act as one fatter pipe

---

## Protocols

### LACP (Link Aggregation Control Protocol) - IEEE 802.3ad
- **Open standard** (interoperable)
- **Recommended**: Use this
- Negotiates automatically with other devices

### PAgP (Port Aggregation Protocol)
- **Cisco proprietary**
- Use when connecting to older Cisco equipment only

---

## Configuration Requirements

Member links must match:
- **Speed**: All 1 Gbps or all 10 Gbps (not mixed)
- **Duplex**: All full-duplex (or all half-duplex, though rare)
- **VLAN settings**: All must be in the same VLAN or all trunks
- **Port roles**: All should be identical (Access or Trunk)

---

## Load Balancing

Traffic is distributed across member links using a hash algorithm:
```
Hash = (Source IP + Dest IP + Source Port + Dest Port) mod (number of links)

Flow 1: 10.1.1.1:5000 → 10.2.2.2:80  → Hash = 0 → Link 1
Flow 2: 10.1.1.1:5001 → 10.2.2.2:80  → Hash = 2 → Link 3
Flow 3: 10.1.1.2:5000 → 10.2.2.2:80  → Hash = 1 → Link 2
```

---

## STP and EtherChannel

- **EtherChannel to STP**: Appears as a single link
- **If one member fails**: Remaining links continue forwarding
- **If all fail**: EtherChannel is down (STP reconverges)

---

# 1.11 802.1X Port-Based Authentication

## Purpose

Prevent unauthorized devices from accessing the network by **requiring authentication before port access**.

**Use case**: Corporate WiFi, secure wired networks

---

## Components

| Component | Role |
| :--- | :--- |
| **Supplicant** | The device trying to access the network (laptop, phone) |
| **Authenticator** | Switch or WAP enforcing authentication (intermediary) |
| **Authentication Server** | RADIUS server validating credentials |

---

## Authentication Flow

```
Device connects to switch port
  ↓
Port starts in "Unauthorized" state
  ↓
Device submits credentials (username/password) via EAPoL (Extensible Authentication over LAN)
  ↓
Switch (Authenticator) forwards to RADIUS server
  ↓
RADIUS validates credentials
  ↓
IF valid: Port moves to "Authorized", device gains network access
IF invalid: Port remains "Unauthorized", limited/no access
```

---

## Port Access States

### Unauthorized Port
- **Allows**: EAPoL frames only (for authentication)
- **Blocks**: All other traffic (DHCP, HTTP, etc.)
- **Purpose**: Forces device to authenticate before network use

### Authorized Port
- **Allows**: All traffic (normal operation)
- **Device**: Fully on the network

---

# 1.12 Multicast and IGMP Snooping

## Multicast Overview

**Problem**: Broadcast floods to everyone on a VLAN (inefficient).

**Solution**: Multicast allows one sender to many interested receivers.

**Use cases**: Video streaming, online gaming, stock tickers, webinars

---

## IGMP (Internet Group Management Protocol)

Devices use **IGMP** to tell the network "I want to receive this multicast group".

```
Host: "I want to join multicast group 239.1.1.1"
  → Sends IGMP Join message
  ↓
Switch learns: "Port Gi0/5 wants 239.1.1.1"
```

---

## IGMP Snooping

**Without IGMP Snooping**: Switch floods all multicast to all ports (like broadcast).

**With IGMP Snooping**: Switch **listens to IGMP** to learn which ports want which multicast groups.

**Result**: Forward multicast only where wanted (reduces flooding).

```
Without: Multicast 239.1.1.1 → Floods to Gi0/1, Gi0/2, Gi0/3, Gi0/4, Gi0/5
With IGMP: Multicast 239.1.1.1 → Only to Gi0/2, Gi0/5 (ports that joined)
```

---

# 1.13 Layer 2 Security Risks and Mitigations

## Security Risks

### 1. MAC Flooding (CAM Overflow)

**Attack**: Attacker sends frames with thousands of fake MAC addresses, overwhelming the switch's MAC address table (CAM).

**Effect**: Switch CAM fills up; unable to learn new MACs → Reverts to **flooding mode** (acts like a hub) → Attacker can sniff all traffic.

**Mitigation**:
```
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 10
Switch(config-if)# switchport port-security violation restrict
```

---

### 2. VLAN Hopping

**Attack**: Attacker tricks the switch into forwarding their traffic to a different VLAN.

**Method 1 - Double Tagging**:
```
Attacker sends: [Outer 802.1Q: VLAN 10] [Inner 802.1Q: VLAN 20] [Data]
Switch removes Outer tag → Sees Inner tag → Forwards to VLAN 20 (unauthorized)
```

**Method 2 - Trunk Negotiation Abuse**:
```
Attacker enables DTP (Dynamic Trunking Protocol) negotiation on an access port
Switch negotiates and becomes trunk
Attacker now sees traffic from all VLANs
```

**Mitigation**:
```
# Force access mode (disable trunk negotiation)
Switch(config-if)# switchport mode access
Switch(config-if)# switchport nonegotiate

# Use non-default native VLAN
Switch(config-if)# switchport trunk native vlan 999
```

---

### 3. Rogue DHCP Servers

**Attack**: Attacker plugs in a DHCP server, assigns IPs pointing to attacker's gateway.

**Effect**: Devices get attacker's gateway IP → Traffic routed through attacker → Man-in-the-middle.

**Mitigation** - DHCP Snooping:
```
Switch(config)# ip dhcp snooping
Switch(config-if)# ip dhcp snooping trust (on uplink ports to real DHCP server)
Switch(config-if)# ip dhcp snooping untrust (on access ports)

Result: Rogue DHCP responses are dropped on untrusted ports
```

---

### 4. ARP Spoofing / ARP Poisoning

**Attack**: Attacker sends fake ARP replies to associate their MAC with a victim's IP or gateway IP.

**Effect**: Traffic meant for gateway is sent to attacker (MITM).

**Mitigation** - Dynamic ARP Inspection (DAI):
```
Switch(config)# ip arp inspection vlan 10

Result: Switch validates ARP messages against DHCP snooping bindings
```

---

### 5. STP Manipulation (Rogue Root Bridge)

**Attack**: Attacker connects a switch with lower Bridge ID, becomes the new Root Bridge.

**Effect**: All traffic flows through attacker; MITM position achieved.

**Mitigation** - BPDU Guard + Root Guard:
```
Switch(config-if)# spanning-tree bpduguard enable (on access ports)
Switch(config-if)# spanning-tree guard root (on uplink ports)

Result: Unauthorized BPDU sources are blocked
```

---

### 6. Unused Ports Providing Access

**Risk**: Active unused ports allow unauthorized devices to connect.

**Mitigation**:
```
# Option 1: Disable unused ports
Switch(config-if)# shutdown

# Option 2: Place in unused VLAN
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 999 (fake, unused VLAN)
```

---

## Layer 2 Security Best Practices

| Security Feature | Configuration | Benefit |
| :--- | :--- | :--- |
| **Port Security** | `switchport port-security` | Prevent MAC flooding |
| **DHCP Snooping** | `ip dhcp snooping` | Prevent rogue DHCP |
| **Dynamic ARP Inspection** | `ip arp inspection vlan X` | Prevent ARP spoofing |
| **BPDU Guard** | `spanning-tree bpduguard` | Prevent rogue root bridge |
| **Force Access Mode** | `switchport mode access` | Prevent VLAN hopping |
| **Disable DTP** | `switchport nonegotiate` | Prevent trunk abuse |
| **Unused VLAN** | Assign unused ports to VLAN 999 | Prevent unauthorized access |
| **802.1X** | `authentication port-control` | Enforce port authentication |
| **Storm Control** | `storm-control broadcast level 10` | Prevent broadcast storms |

---

# Summary

Layer 2 is the "local delivery" layer. Switches forward frames based on MAC addresses, segment networks into VLANs, prevent loops with STP, and provide security features to protect against attacks. Understanding Layer 2 is crucial for network design, troubleshooting, and security.

**Key Takeaways:**
- **MAC addresses** identify devices locally; **IP addresses** identify them globally
- **Switches** learn MAC addresses and forward frames intelligently
- **VLANs** provide logical segmentation and security
- **STP** prevents loops while maintaining redundancy
- **Layer 2 security** requires multiple overlapping technologies to be effective
