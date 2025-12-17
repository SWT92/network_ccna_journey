# 1. Layer 1 - Physical Layer
The physical layer is the lowest layer of the OSI model. It is responsible for the physical transmission of bits over the network medium (e.g., copper wire, fiber optic), as such we will be discussing the different type of medium used to transmit bits across the network.

# Table of Contents
- [1. Layer 1 - Physical Layer](#1-layer-1---physical-layer)
- [2. Types of Network Media](#2-types-of-network-media)
  - [2.1 Copper Cable](#21-copper-cable-electrical-signals)
  - [2.2 Fiber Optic](#22-fiber-optic-light-pulses)
  - [2.3 Wireless](#23-wireless-radio-frequency)
- [3. Layer 1 Devices](#3-layer-1-devices-legacy--modern)
- [4. Security at Layer 1](#4-security-at-layer-1-cybersec--physical-security)

---

# 2 Types of Network Media
The physical layer defines the hardware means of sending and receiving data on a carrier. There are three main ways to represent bits on a physical medium:

### 2.1 Copper Cable (Electrical Signals)
Uses **electrical pulses** (voltage changes) to represent data.
*   **Examples**: Unshielded Twisted Pair (UTP), Shielded Twisted Pair (STP), Coaxial.
*   **Pros**: Inexpensive, easy to install, widely used.
*   **Cons**: Susceptible to Electromagnetic Interference (EMI) and Crosstalk. Limited distance (typically 100m for UTP).
*   **Speed**: Ranges from 10 Mbps (Legacy Ethernet) to 40 Gbps (Cat 8).

#### 2.1.1 Twisted Pair (UTP & STP)
The most common cabling in LANs. It consists of 8 wires, twisted into 4 pairs.
*   **Why twist?** The twisting cancels out **EMI (Electromagnetic Interference)** and **Crosstalk** (signal bleeding from one wire to another) via a phenomenon called *cancellation*.

**Comparison: UTP vs. STP**
| Feature | UTP (Unshielded Twisted Pair) | STP (Shielded Twisted Pair) |
| :--- | :--- | :--- |
| **Shielding** | None. Relies solely on twists. | Foil wrapping around pairs/cable. |
| **Cost** | Low (Cheapest option). | Higher. |
| **Install** | Easy, flexible. | Harder (stiffer, requires grounding). |
| **Use Case** | Homes, Offices, Standard LANs. | Factories, MRI rooms, High-Interference areas. |

#### 2.1.2 Copper Categories & IEEE Standards
Not all copper is created equal. The "Category" determines the twist tightness and bandwidth.

| Category | IEEE Standard | Speed | Bandwidth | Max Distance | Application |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Cat 5e** | 802.3ab (1000BASE-T) | 1 Gbps | 100 MHz | 100m | Gigabit Ethernet |
| **Cat 6** | 802.3an (10GBASE-T) | 10 Gbps | 250 MHz | 55m | High-speed LANs |
| **Cat 6a** | 802.3an (10GBASE-T) | 10 Gbps | 500 MHz | 100m | Data Centers |
| **Cat 8** | 802.3bq (25G/40GBASE-T)| 40 Gbps | 2000 MHz | 30m | Switch-to-Server |

#### 2.1.3 Duplex Modes
Duplex refers to the direction and simultaneity of data transmission:
*   **Half-Duplex**: Device can send OR receive, but not simultaneously (like a walkie-talkie). Used with hubs and CSMA/CD.
*   **Full-Duplex**: Device can send AND receive simultaneously (like a telephone call). Standard on modern switches with dedicated point-to-point links.
*   **Auto-Negotiation**: Modern devices automatically negotiate the best speed and duplex mode when connecting.

#### 2.1.4 Auto-MDIX (Automatic Medium-Dependent Interface Crossover)
**Auto-MDIX** is a modern feature that automatically detects whether a straight-through or crossover cable is connected and adjusts internally.
*   **Benefit**: Eliminates the need for crossover cables; you can use any cable type and it will work.
*   **Status**: Enabled by default on most switches manufactured after 2004.
*   **Modern Context**: With Auto-MDIX, the distinction between cable types is largely irrelevant for end users.

#### 2.1.5 Wiring Standards & Cables
Copper cables use **RJ-45** connectors. The order of wires matters!
*   **TIA/EIA-568B**: Most common standard (Orange-White starts).
*   **TIA/EIA-568A**: Older standard, sometimes used in government (Green-White starts).

**Cable Types:**
1.  **Straight-Through**: Both ends are the same (B-to-B or A-to-A). Connects **different** devices (e.g., PC to Switch).
2.  **Crossover**: One end A, one end B. Connects **same** devices (e.g., Switch to Switch, PC to PC). *Note: Modern "Auto-MDIX" ports largely eliminate the need for this.*
3.  **Rollover (Console)**: Wires are completely reversed. Connects PC serial port to Router/Switch **Console Port** for configuration.

#### 2.1.6 Understanding Ethernet Naming Convention
Ethernet standards use a naming scheme to describe speed, signaling method, and medium. Examples: **1000BASE-T**, **10GBASE-SX**, **25GBASE-T**.

**Breakdown:**
- **First number (1000, 10G, 25G, etc.)**: Speed in Mbps or Gbps
  - 1000 = 1 Gbps (Gigabit Ethernet)
  - 10G = 10 Gbps
  - 25G = 25 Gbps
  - 40G = 40 Gbps

- **BASE**: Baseband signaling (entire cable bandwidth used for one signal)

- **Letter/Abbreviation**: Medium type
  - **T** = Twisted Pair (copper)
  - **SX** = Short wavelength multimode fiber (shorter distances, ~550m)
  - **LX** = Long wavelength single-mode fiber (~10km)
  - **LR** = Long Reach single-mode fiber (very long distances)
  - **ER** = Extended Reach (ultra-long distances, trans-oceanic)

**Examples:**
- `1000BASE-T` = 1 Gbps over twisted pair copper
- `10GBASE-SR` = 10 Gbps over short-range multimode fiber
- `10GBASE-LR` = 10 Gbps over long-range single-mode fiber

#### 2.1.7 Power over Ethernet (PoE)
Power over Ethernet allows Ethernet cables to carry both data and electrical power to devices, eliminating the need for separate power cables.

| Standard | IEEE | Max Power | Typical Use Case |
| :--- | :--- | :--- | :--- |
| **PoE (Type 1)** | 802.3af | 15.4W | IP phones, wireless access points, IP cameras |
| **PoE+ (Type 2)** | 802.3at | 25.5W | Higher-power WAPs, Pan-Tilt-Zoom (PTZ) cameras |
| **PoE++ (Type 3)** | 802.3bt | 60W | Video conferencing systems, thin clients |
| **PoE++ (Type 4)** | 802.3bt | 100W | Industrial devices, high-power LED lighting |

**How it Works:**
- In traditional twisted pair, 4 pairs of wires exist: one pair for transmit, one for receive, two unused.
- PoE delivers power over the unused pairs (phantom power).
- In Gigabit Ethernet (uses all 4 pairs for data), PoE injects power across multiple pairs simultaneously.
- The receiving device has a **Power over Ethernet Injector** or **PoE Switch** that adds power; the **PoE Splitter** on the device side separates power from data.

**Advantage**: Simplifies installation in locations without nearby power outlets (e.g., ceiling-mounted cameras, outdoor APs).

#### 2.1.8 Coaxial Cable
Legacy but still alive. Consists of a central copper core, insulation, metal shield, and outer jacket.
*   **Connectors**: BNC (old LANs), F-Type (Cable TV/Modems).
*   **Modern Use**: Broadband Cable Internet (DOCSIS) via RG-6 cable.
*   **Legacy Use**: 10BASE2 (Thinnet) and 10BASE5 (Thicknet) bus topologies.


### 2.2 Fiber Optic (Light Pulses)
Uses **pulses of light** (laser or LED) to represent data.
*   **IEEE Standards**: 
    *   **1000BASE-SX** (802.3z): Short-range MMF (up to 550m).
    *   **1000BASE-LX** (802.3z): Long-range SMF (up to 10km).
    *   **10GBASE-SR** (802.3ae): Short-range 10G over MMF.
    *   **10GBASE-LR** (802.3ae): Long-range 10G over SMF (up to 10km).

#### 2.2.2 Types of Fiber
*   **Single-Mode (SMF)**: Small core (~9 microns), uses **Laser**, for long distances (kms). Yellow Jacket.
*   **Multi-Mode (MMF)**: Larger core (~50 microns), uses **LED**, for shorter distances (within buildings). Orange/Aqua Jacket.

#### 2.2.3 Fiber Connectors
Fiber optic cables use different connector types depending on application and era. The connector choice affects compatibility and performance.

| Connector | Size | Type | Description | Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **LC** | Small | Push-pull | Small form-factor, most common in modern gear. | Data center, SFP/SFP+ modules, high-density installations. |
| **SC** | Medium | Push-pull | Larger square connector, older standard. | Legacy telecom, some enterprise installations. |
| **ST** | Medium | Bayonet (twist-lock) | Historic connector with twist-to-lock mechanism. | Older equipment, mostly phased out. |
| **MTP/MPO** | Large | Push-pull | Multi-fiber connector (12, 24, or 32 fibers in one). | High-speed deployments: 40G, 100G, 400G applications. |
| **APC** | Various | Angled Polish | Angled 8° polish reduces back-reflection. | High-performance/long-haul applications. |

**Compatibility Note**: Fiber connectors are **not interchangeable**. Mixing connector types requires adapters or patch cord changes.

#### 2.2.4 WDM (Wavelength Division Multiplexing)
WDM allows multiple data streams to travel over a *single* fiber strand by using different colors (wavelengths) of light.

| Feature | CWDM (Coarse WDM) | DWDM (Dense WDM) |
| :--- | :--- | :--- |
| **Channel Spacing** | Wide (20nm). Up to 18 channels. | Tight (0.8nm). Up to 80+ channels. |
| **Distance** | Shorter (up to 80km). | Very Long (Trans-oceanic with amplifiers). |
| **Cost** | Cheaper. | Expensive. |
| **Use Case** | Metro Ethernet, Campus Backbones. | ISP Backbones, Undersea Cables. |

### 2.3 Wireless (Radio Frequency)
Uses **electromagnetic waves** (radio frequencies) to represent data.

#### 2.3.1 IEEE 802.11 Wi-Fi Standards
Wi-Fi standards evolve to provide higher speeds and better efficiency.

| Standard | Name | Frequency | Max Speed (Theoretical) | Key Feature |
| :--- | :--- | :--- | :--- | :--- |
| **802.11b** | Wi-Fi 1 | 2.4 GHz | 11 Mbps | The original widely adopted standard. |
| **802.11g** | Wi-Fi 3 | 2.4 GHz | 54 Mbps | Backwards compatible with 'b'. |
| **802.11n** | Wi-Fi 4 | 2.4/5 GHz | 600 Mbps | Introduced **MIMO** (Multiple antennas). |
| **802.11ac** | Wi-Fi 5 | 5 GHz | 6.9 Gbps | **MU-MIMO**, wider channels. |
| **802.11ax** | Wi-Fi 6 | 2.4/5/6 GHz | 9.6 Gbps | **OFDMA** (Efficiency), Target Wake Time. |
| **802.11be** | Wi-Fi 7 | 2.4/5/6 GHz | 46 Gbps | Extremely high throughput (Future). |

#### 2.3.2 Wireless Frequencies
Wi-Fi operates primarily on two (now three) radio frequency bands.

| Frequency | Pros | Cons |
| :--- | :--- | :--- |
| **2.4 GHz** | **Range**: Better at penetrating walls. **Compatibility**: Supported by almost all devices. | **Interference**: Crowded by microwaves, Bluetooth, baby monitors. **Speed**: Slower. Only 3 non-overlapping channels (1, 6, 11). |
| **5 GHz** | **Speed**: Much faster. **Interference**: Less crowded, more channels (24+). | **Range**: Shorter range, struggles with solid walls. |
| **6 GHz** (Wi-Fi 6E) | **Capacity**: Massive amount of new spectrum. **Zero Legacy**: No slow older devices slowing it down. | **Range**: Shortest range. Requires new hardware. |

# 3. Layer 1 Devices (Legacy & Modern)
While most intelligence lives in higher layers, Layer 1 has its own hardware.

### 3.1 Hubs (The "Dumb" Device)
A hub is a legacy device that operates purely at Layer 1. It does not understand MAC addresses or IP addresses.
*   **Function**: When it receives bits on one port, it blindly regenerates and repeats those bits out **all other ports**.
*   **The Problem (Collision Domain)**: Because everyone talks at once on the same shared wire, **collisions** occur. If two devices transmit simultaneously, the signals crash and data is destroyed.
*   **Security Risk**: Since it broadcasts everything to everyone, a hacker plugging into a hub can sniff **all** traffic on the network (using tools like Wireshark) without any effort.

#### CSMA/CD on Shared Ethernet
To reduce collisions on hub-based Ethernet, devices use **Carrier Sense Multiple Access with Collision Detection (CSMA/CD)**:
*   **Carrier Sense**: A device listens and waits for an idle wire before transmitting.
*   **Multiple Access**: Many devices share the same physical medium.
*   **Collision Detection**: If a collision is detected, the device stops sending immediately.
*   **Backoff Algorithm**: The device waits a random time (exponential backoff) and retries.
*   **Half-Duplex Operation**: Devices cannot send and receive at the same time.
*   **Modern Context**: Full-duplex **switches** provide dedicated links per port, eliminating collisions and the need for CSMA/CD.

### 3.2 Repeaters
A repeater is a simple device used to regenerate a signal to extend its range.
*   **Use Case**: Placing a repeater every 100m to extend a copper cable run beyond its limit.
*   **Modern Context**: Hubs are essentially "multi-port repeaters".

### 3.3 Media Converters
Devices that convert one type of media to another at Layer 1.
*   **Example**: Copper-to-Fiber converter. Useful for connecting a copper switch port to a fiber run that goes to another building.

# 4. Security at Layer 1 (CyberSec & Physical Security)
In Cybersecurity, we often focus on firewalls and encryption, but if an attacker has physical access, they own the network.

### 4.1 Physical Threats
1.  **Wiretapping**:
    *   **Copper**: Attackers can physically tap into copper wires to eavesdrop on electrical signals.
    *   **Fiber**: Much harder to tap without breaking the light beam (which is detectable), making fiber more secure for high-security zones.
2.  **Jamming (Wireless)**:
    *   Attackers can blast radio noise on 2.4GHz or 5GHz frequencies to create a Denial of Service (DoS), rendering Wi-Fi unusable.
3.  **Rogue Access Points**:
    *   An employee plugging a cheap Wi-Fi router into a corporate wall jack, bypassing security and creating an open backdoor for attackers.

### 4.2 Mitigations
*   **Physical Access Control**: Locked server rooms, conduit for cables, and port locks.
*   **Port Security**: (Layer 2 feature) Configuring switches to shut down ports if unauthorized devices connect.
*   **Faraday Cages**: Shielding sensitive rooms to prevent wireless signals from leaking out.
*   **Fiber for High Security**: Use fiber for sensitive uplinks to prevent tapping.
