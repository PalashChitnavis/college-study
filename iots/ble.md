# Bluetooth Low Energy (BLE) & IoT — Complete Study Notes

---

## Table of Contents

1. [Unit 1: Bluetooth Low Energy (BLE) Fundamentals](#unit-1-bluetooth-low-energy-ble-fundamentals)
2. [Unit 2: BLE Architecture & Stack](#unit-2-ble-architecture--stack)
3. [Unit 3: Physical Layer & Communication Basics](#unit-3-physical-layer--communication-basics)
4. [Unit 4: Link Layer & Communication States](#unit-4-link-layer--communication-states)
5. [Unit 5: BLE Roles & Operations](#unit-5-ble-roles--operations)
6. [Unit 6: Advertising & Scanning](#unit-6-advertising--scanning)
7. [Unit 7: Connection Management](#unit-7-connection-management)
8. [Unit 8: Data Transport & Reliability](#unit-8-data-transport--reliability)
9. [Unit 9: Advanced Advertising](#unit-9-advanced-advertising)
10. [Unit 10: Upper Layers](#unit-10-upper-layers)
11. [Unit 11: GATT Design (Real System)](#unit-11-gatt-design-real-system)
12. [Unit 12: BLE Security](#unit-12-ble-security)
13. [Unit 13: BLE Vulnerabilities & Attacks](#unit-13-ble-vulnerabilities--attacks)
14. [Unit 14: 6LoWPAN (IoT Integration)](#unit-14-6lowpan-iot-integration)

---

## Unit 1: Bluetooth Low Energy (BLE) Fundamentals

### Introduction to Bluetooth & BLE

- **Bluetooth** → Wireless cable replacement technology

#### Types of Bluetooth

| Type | Full Form | Use Case | Power |
|------|-----------|----------|-------|
| **Bluetooth Classic (BR/EDR)** | Basic Rate / Enhanced Data Rate | High data transfer (audio, files) | High power |
| **BLE** | Bluetooth Low Energy | Sensors, IoT, Wearables | Low power |

- BLE was introduced in **Bluetooth 4.0 (2010)**
- Current version: **Bluetooth 5.0**
- Both operate in the **2.4 GHz ISM (Industrial, Scientific and Medical) band**

> ❗ **Important:** Classic Bluetooth and BLE devices **cannot directly communicate** with each other.

---

### BLE Technical Specifications

| Parameter | Value |
|-----------|-------|
| Frequency Range | 2.400 – 2.4835 GHz |
| Channels | 40 channels (2 MHz each) |
| Data Rate | 2 Mbps (Bluetooth 5) |
| Range | 10–30 meters (typical) |
| Peak Power Consumption | < 15 mA |
| Security Algorithm | AES-CCM (Advanced Encryption Standard – Counter with CBC-MAC), 128-bit key |

- Security is **optional**
- Designed for **low bandwidth applications**
- **Backward compatible** — works across versions, though some features may be limited

---

### Bluetooth Classic vs BLE

| Feature | Bluetooth Classic | BLE |
|---------|-------------------|-----|
| Use Case | Audio, file transfer | Sensors, IoT |
| Power Consumption | High | Low |
| Data Rate | ≈ 3 Mbps | 2 Mbps |
| Channels | 79 channels | 40 channels |
| Discovery Channels | 2 channels | 3 channels |
| Data Pattern | Continuous data | Small, intermittent data |

---

### Advantages of BLE

- **Low power consumption** — radio is OFF most of the time
- Optimized for **IoT and sensor** applications
- **Low cost** modules and chipsets available
- **Free specification** — available on the Bluetooth website
- Available in **most modern smartphones**
- Key advantage: **High energy efficiency**

---

### Limitations of BLE

- **150 µs gap** between packets
- **Packet overhead** — header reduces efficiency
- **Empty packets required** even when there is no data to send
- **Retransmission** of lost packets adds overhead
- **Interference** in 2.4 GHz band (caused by walls, metal, human body)
- **Device orientation** affects signal quality
- Key limitation: **Lower efficiency and reliability** compared to high-speed systems

---

## Unit 2: BLE Architecture & Stack

### BLE Architecture (Host + Controller + HCI)

BLE architecture is divided into three main components:

#### Components

| Component | Role |
|-----------|------|
| **Host** | Main CPU, software stack — handles GATT, ATT, security, and applications |
| **Controller** | Radio hardware — handles radio communication and packet transmission |
| **HCI (Host Controller Interface)** | Connects Host and Controller |

#### HCI Communication Flow

- **Commands** → Host → Controller
- **Events** → Controller → Host

> **Key Idea:** Separation of Host and Controller improves **flexibility and efficiency**.

---

### BLE Stack Layers

```
┌────────────────────────────────────────┐
│         GAP (Generic Access Profile)   │  ← Discovery & Connection
├────────────────────────────────────────┤
│        GATT (Generic Attribute Profile)│  ← Services & Characteristics
├────────────────────────────────────────┤
│         ATT (Attribute Protocol)       │  ← Client-Server Data Exchange
├────────────────────────────────────────┤
│    SMP (Security Manager Protocol)     │  ← Pairing & Encryption
├────────────────────────────────────────┤
│  L2CAP (Logical Link Control &         │  ← Multiplexing & Segmentation
│         Adaptation Protocol)          │
├────────────────────────────────────────┤
│  HCI (Host Controller Interface)       │  ← Host–Controller Interface
├════════════════════════════════════════╡
│         Link Layer                     │  ← Communication Control
├────────────────────────────────────────┤
│     PHY (Physical Layer)               │  ← Radio Transmission
└────────────────────────────────────────┘
```

#### Division of Layers

| Part | Layers Included |
|------|----------------|
| **Controller** | PHY + Link Layer |
| **Host** | L2CAP, SMP, ATT, GATT, GAP |

#### Data Flow Direction

- **Transmit:** Top → Bottom (application down to radio)
- **Receive:** Bottom → Top (radio up to application)

---

## Unit 3: Physical Layer & Communication Basics

### PHY (Physical Layer)

- Handles **radio transmission and reception**
- Converts **digital data ↔ radio signals**
- Operates in **2.4 GHz ISM band**
- Uses **40 channels (2 MHz each)**
- Uses **FHSS (Frequency Hopping Spread Spectrum)**

#### PHY Functions

- Modulation
- Frequency selection
- Channel usage

> **Key Point:** PHY deals with **signals**, not the meaning of data.

---

### Modulation (GFSK)

- **Modulation** — converts digital data into a radio signal
- BLE uses **GFSK (Gaussian Frequency Shift Keying)**

#### How GFSK Works

| Bit | Frequency |
|-----|-----------|
| `1` | Higher frequency |
| `0` | Lower frequency |

- **Gaussian filtering** — smooths signal transitions and reduces interference
- **Frequency deviation:** ±185 kHz or ±370 kHz

#### Advantages of GFSK

- Low power
- Noise resistant
- Efficient

---

### PHY Variants (1M, 2M, Coded PHY)

| PHY Variant | Data Rate | Range | Notes |
|-------------|-----------|-------|-------|
| **1M PHY** | 1 Mbps | Medium | Mandatory, balanced |
| **2M PHY** | 2 Mbps | Shorter | Faster, shorter range |
| **Coded PHY** | Lower | Longer | Uses error correction (FEC) |

#### Transmit Power Range

- Maximum: **+20 dBm** (BLE 5)
- Minimum: **-20 dBm**

> **Key Concept:** Speed ↑ → Range ↓ | Range ↑ → Speed ↓

---

## Unit 4: Link Layer & Communication States

### Link Layer Functions

The Link Layer manages communication over the air and works directly above the PHY layer.

#### Functions

- MAC (Medium Access Control)
- Error control
- Connection establishment
- Flow control

#### Device States

| State | Description |
|-------|-------------|
| **Standby** | No transmission or reception |
| **Advertising** | Sends advertising packets |
| **Scanning** | Listens for advertisers |
| **Initiating** | Establishes a connection |
| **Connected** | Active data exchange |

#### Roles in Connected State

- **Master** → Initiator of connection
- **Slave** → Advertiser that accepted connection

> **Key Role:** Link Layer ensures **reliable communication**.

---

### Packet Structure

#### Uncoded PHY (1M, 2M) Packet Fields

1. Synchronization
2. Link identification
3. Payload
4. CRC (Cyclic Redundancy Check)
5. Direction finding

#### Coded PHY Packet Fields

1. Synchronization
2. Link identification
3. Coding indicator
4. FEC (Forward Error Correction) block info
5. Payload
6. CRC (Cyclic Redundancy Check)
7. End indicator

> **Key Point:** CRC is used for **error detection**.

---

### Bluetooth Addressing

- Bluetooth address is a **48-bit identifier** (similar to a MAC address)

#### Address Types

| Type | Description |
|------|-------------|
| **Public Address** | Fixed, factory programmed, IEEE registered |
| **Random Address** | No registration required, more popular |

#### Random Address Sub-types

| Sub-type | Description |
|----------|-------------|
| **Static** | Remains the same during the device's lifetime |
| **Private – Non-resolvable** | Temporary, changes periodically |
| **Private – Resolvable** | Uses IRK (Identity Resolving Key), changes periodically |

> **Key Concept:** Used for **privacy and tracking prevention**.

---

## Unit 5: BLE Roles & Operations

### Central vs Peripheral Devices

| Role | Function |
|------|----------|
| **Peripheral** | Sends advertising packets, accepts connections |
| **Central** | Scans for devices, initiates connections |

**Example:**
- Smartphone → Central
- Sensor → Peripheral

**Key Flow:** Advertising → Scanning → Connection

---

### Broadcaster & Observer

| Role | Function |
|------|----------|
| **Broadcaster** | Sends advertising packets; no connection allowed, no receiver |
| **Observer** | Receives advertising packets; cannot initiate a connection |

- Both roles use **no bidirectional communication**
- Require **reduced hardware and software**

> **Key Concept:** Used for **connectionless communication**.

---

### Multi-role Devices

- A BLE device can act in **multiple roles simultaneously**

**Example:**
- Acts as **Central** → monitors sensors
- Acts as **Peripheral** → advertises to a smartphone

**Functions:**
- Scan + Connect
- Advertise + Accept connections

> **Key Concept:** Enables **flexible and complex BLE systems**.

---

## Unit 6: Advertising & Scanning

### Advertising Concepts

- **Advertising** — broadcasting packets without establishing a connection
- **Advertising packets** — contain useful data about the device
- **Advertising interval** — fixed time between transmissions
- **Advertising event** — multiple packets sent together

**Advertising Channels:** 37, 38, 39

**Purpose:** Device discovery

---

### Advertising Events & Types

An **advertising event** consists of multiple packets sent on channels 37, 38, and 39.

#### 7 Types of Advertising Events

| # | Type | Description |
|---|------|-------------|
| 1 | **Connectable & Scannable Undirected** | Allows both connections and scan requests from any device |
| 2 | **Connectable Undirected** | Allows connections from any device |
| 3 | **Connectable Directed** | Allows connections from a specific device |
| 4 | **Non-connectable & Non-scannable Undirected** | No connections, no scan requests; broadcast only |
| 5 | **Non-connectable & Non-scannable Directed** | Same as above but to a specific device |
| 6 | **Scannable Undirected** | Allows scan requests from any device; no connections |
| 7 | **Scannable Directed** | Allows scan requests from a specific device |

**Key Concepts:**
- **Connectable** → allows connection
- **Scannable** → allows scan request
- **Directed** → targets a specific device

---

### Advertising Parameters (Interval & Data)

#### Advertising Interval

| Parameter | Value |
|-----------|-------|
| Range | 20 ms to 10.24 s |
| Step size | 625 µs |

- Affects **battery life** and **connectivity speed**

#### Advertising Data

- Stored in **PDU (Protocol Data Unit)**
- Uses **TLV (Type-Length-Value) format**

#### TLV Structure

| Field | Description |
|-------|-------------|
| **Length** | Size of the data |
| **Type** | Type of data |
| **Value** | Actual data content |

#### Common AD (Advertising Data) Types

- **Local Name** — device name
- **Tx Power Level / RSSI (Received Signal Strength Indicator) Value**
- **Flags** — device capabilities and mode
- **Service UUIDs (Universally Unique Identifiers)** — services offered
- **Appearance** — device type (e.g., phone, sensor)

---

### Scanning Types & Parameters

- **Scanning** — listening for advertising packets

#### Scan Types

| Type | Description |
|------|-------------|
| **Active Scanning** | Sends a scan request to get more info |
| **Passive Scanning** | Only listens; does not send any request |

#### Scan Parameters

| Parameter | Description |
|-----------|-------------|
| **Scan Type** | Active or Passive |
| **Scan Window** | Duration for which scanning is active |
| **Scan Interval** | Frequency of scanning |

> **Key Relation:** Scan Window occurs **within** the Scan Interval.

---

## Unit 7: Connection Management

### Connection Events

- A **connection event** is when Master and Slave exchange packets alternately
- **Process:** Master sends → Slave responds → repeat
- Ends when: **no data is left to send**

> **Key Concept:** BLE communication is **event-based**.

---

### Connection Parameters

| Parameter | Description | Range / Step |
|-----------|-------------|--------------|
| **Connection Interval** | Time between connection events | 7.5 ms – 4 s, step: 1.25 ms |
| **Slave Latency** | Allows slave to skip connection events to save power | — |
| **Supervision Timeout** | Detects connection loss | — |
| **MTU (Maximum Transmission Unit)** | Max data size per packet | Effective = minimum of both devices |

#### Supervision Timeout Formula

```
Timeout > (1 + Slave Latency) × Connection Interval × 2
```

- **Connection Interval** is set by the **Central**
- **Slave Latency** saves power by allowing skipped events

---

### Channel Hopping

- **Channel hopping** — switching channels between connection events
- **Channel map** — defines which channels are used; sent in the connection request
- **Hop increment** — determines the next channel to use

> **Key Point:** Each connection event may use a **different channel**.

---

## Unit 8: Data Transport & Reliability

### Data Transport Architecture

| Mode | Reliability | Direction | Transport Type |
|------|-------------|-----------|----------------|
| **Connected mode** | Reliable | Bi-directional | Uses ACL (Asynchronous Connection-oriented Logical transport) |
| **Advertising mode** | Unreliable | Uni-directional | Uses ADVB (Advertising Broadcast) |

#### Channels Used

- **Connected mode** → Piconet channel
- **Advertising mode** → Advertising channel

---

### Logical Transports

#### LE-ACL (Asynchronous Connection-oriented Logical Transport)

- Connection-oriented
- Bi-directional
- Used in **connected mode**

#### ADVB (Advertising Broadcast)

- Connectionless
- Uni-directional
- Used in **advertising mode**

> **Key Point:** Logical transports define **how data is transferred logically**.

---

### Packet Ordering & Acknowledgment

**LE-ACL ensures:**
- **Ordered data** delivery
- **Acknowledgement** of received packets
- **Retransmission** of lost packets

#### Reliability-related Packet Fields

| Field | Full Form | Purpose |
|-------|-----------|---------|
| **SN** | Sequence Number | Identifies packet order |
| **NESN** | Next Expected Sequence Number | Tells sender what to send next |
| **MD** | More Data | Indicates more data is pending |

- **CRC (Cyclic Redundancy Check)** — used for error detection
- Lost packets are **retransmitted**

---

## Unit 9: Advanced Advertising

### ADVB (Broadcast Mode)

- **ADVB** — connectionless communication in broadcast mode
- **Unidirectional** — no connection required

#### Types

| Type | Description |
|------|-------------|
| **Legacy** | Uses ADV_IND — 31-byte payload limit |
| **Extended** | Supports larger data using additional PDUs |

**Components:**
- **PDU (Protocol Data Unit)** — the packet
- **Payload** — actual data
- **Channels:** 37, 38, 39

**Use:** Device discovery and IoT communication

---

### Extended Advertising

Extended Advertising supports **larger data** using additional PDUs (Protocol Data Units).

#### Channels

| Channel Type | Channel Numbers | Purpose |
|--------------|-----------------|---------|
| **Primary channels** | 37, 38, 39 | Device discovery |
| **Secondary channels** | Remaining channels | Data transmission |

#### Packet Types

| Packet | Description |
|--------|-------------|
| **ADV_EXT_IND** | Points to the next packet (pointer) |
| **AUX_ADV_IND** | First data packet on secondary channel |
| **AUX_CHAIN_IND** | Remaining data (chained packets) |

- **AuxPtr (Auxiliary Pointer)** — points to the next packet in the chain

> **Key Concept:** Large data is split via **data fragmentation across packets**.

---

### Packet Chaining

- **Packet chaining** — splits large data into multiple smaller packets
- **Fragmentation** — dividing data into smaller parts
- **Maximum data:** Up to **1650 bytes**

#### Chaining Sequence

1. **AUX_ADV_IND** — first packet
2. **AUX_CHAIN_IND** — remaining packets

- **AuxPtr** points to the next packet in the sequence
- Data is transmitted on **secondary advertising channels**

---

### Periodic Advertising (PADVB)

- **PADVB** — Periodic Advertising Broadcast
- Provides a **reliable, synchronized broadcast**
- **Non-connectable** and **Non-scannable**

#### Key Concept: Advertising Train

| Packet | Purpose |
|--------|---------|
| **AUX_ADV_IND** | Indicates availability of periodic data |
| **AUX_SYNC_IND** | Carries periodic data |
| **AUX_CHAIN_IND** | Carries large chained data |

#### Sync Mechanism

- Receiver **wakes at fixed intervals** to receive data

---

## Unit 10: Upper Layers

### HCI (Host Controller Interface)

- **HCI** — standard interface between Host and Controller

#### Functions

- Bi-directional communication
- Configuration and feature control

#### Communication

| Direction | Type |
|-----------|------|
| **Host → Controller** | Commands |
| **Controller → Host** | Events |

**Example use:** Enabling features like AoA (Angle of Arrival) and AoD (Angle of Departure)

---

### L2CAP (Logical Link Control and Adaptation Protocol)

#### Functions

- **Protocol multiplexing** — handles multiple protocols over one link
- **Flow control** — manages data rate
- **Segmentation & Reassembly** — splits and rebuilds large data

#### Terms

| Term | Description |
|------|-------------|
| **SDU (Service Data Unit)** | Original data passed from upper layers |
| **Segments** | Smaller pieces of the SDU |

> **Role:** Manages data exchange **between layers**.

---

### ATT (Attribute Protocol)

- **ATT** — client–server model for data exchange

#### Attribute Structure

| Field | Description |
|-------|-------------|
| **Handle** | Unique identifier for the attribute |
| **Type (UUID)** | Defines the type of data |
| **Value** | Actual data content |

#### Roles

| Role | Function |
|------|----------|
| **Client** | Sends requests |
| **Server** | Stores and responds with data |

#### Operations

- Request
- Response
- Command
- Notification

> **Key Concept:** ATT is the **basic data exchange protocol** in BLE.

---

### GATT (Generic Attribute Profile)

- **GATT** — built on top of ATT; organizes data into a structured format

#### Structure

| Element | Description |
|---------|-------------|
| **Service** | Group of related data |
| **Characteristic** | A value with properties (read, write, notify) |
| **Descriptor** | Additional information about a characteristic |

#### Roles

| Role | Function |
|------|----------|
| **Server** | Stores data |
| **Client** | Accesses data |

#### Operations

- Read
- Write
- Notify
- Indicate

> **Key Concept:** GATT **defines data organization** in BLE.

---

### GAP (Generic Access Profile)

- **GAP** — defines how devices interact and are discovered

#### Functions

- Device discovery
- Connection establishment
- Role definition

#### Discovery Process

- Advertising + Scanning

#### Roles Defined by GAP

- Central
- Peripheral
- Broadcaster
- Observer

> **Key Concept:** GAP **controls how devices connect**.

---

## Unit 11: GATT Design (Real System)

### Home Automation Example

GATT design maps a **real-world system** to a BLE data structure.

**Steps:**
1. Define services
2. Define characteristics
3. Define descriptors
4. Define access methods

**Example:**
- Light control → **ON/OFF Characteristic**

**Roles:**
- **Server** → stores data
- **Client** → accesses data

> **Key Concept:** Functionality maps to **service + characteristic**.

---

### Step-by-Step GATT Design

| Step | Action |
|------|--------|
| 1 | Identify requirements |
| 2 | Define services |
| 3 | Define characteristics |
| 4 | Assign UUIDs (Universally Unique Identifiers) |
| 5 | Define properties (read / write / notify) |
| 6 | Add descriptors |
| 7 | Implement client-server architecture |
| 8 | Test the system |

> **Key Concept:** Map application logic → BLE structure.

---

## Unit 12: BLE Security

### Security Overview

BLE security protects data and controls access.

#### Key Security Components

| Component | Description |
|-----------|-------------|
| **Pairing** | Establishes trust and exchanges keys |
| **ECDH** | Elliptic Curve Diffie-Hellman — secure key exchange |
| **Bonding** | Stores keys for future use |
| **Encryption** | Protects data in transit |

> **Key Concept:** Secure communication between devices.

---

### Pairing Process

**Purpose:** Establish trust and enable secure communication

#### Phases of Pairing

1. **Feature exchange** — devices share capabilities
2. **Key generation** — a shared key is generated
3. **Key distribution** — keys are exchanged between devices

> **Key Concept:** Pairing **enables encryption**.

---

### Secure Connections (ECDH)

- **Secure Connections** uses **ECDH (Elliptic Curve Diffie-Hellman)**
- ECDH uses a **public + private key system** to generate a shared secret

#### Process

1. Exchange public keys
2. Compute shared secret key independently

> **Key Point:** Prevents **eavesdropping** during key exchange.

---

### Bonding & Key Storage

- **Bonding** — the storage of security keys after pairing

**Purpose:**
- Enables future secure communication
- Allows faster reconnection without re-pairing

#### Relation Between Pairing and Bonding

| Stage | Action |
|-------|--------|
| **Pairing** | Generates keys |
| **Bonding** | Stores those keys |

> **Key Point:** Avoids repeated pairing on reconnection.

---

### Encryption

**Purpose:**
- Ensures **confidentiality** of data
- Prevents **unauthorized access**

#### Process

1. Pairing → key generation
2. Encryption → uses generated keys to protect data

> **Key Concept:** Data is **unreadable without the key**.

---

## Unit 13: BLE Vulnerabilities & Attacks

### Weaknesses in Pairing

The pairing process can have weaknesses that expose BLE devices to attacks.

#### Common Attacks

| Attack | Description |
|--------|-------------|
| **Eavesdropping** | Captures data transmitted over the air |
| **MITM (Man-In-The-Middle)** | Intercepts and modifies data between two devices |
| **DoS (Denial of Service)** | Disrupts communication to make device unavailable |

> **Key Concept:** Security strength **depends on the pairing process**.

---

### Attacks on Devices

| Attack | Description |
|--------|-------------|
| **Spoofing** | Faking the identity of a trusted device |
| **Replay Attack** | Resending previously captured data to trick a device |
| **Unauthorized Access** | Illegally controlling or reading a device |

#### Cause

- Weak security configuration

#### Prevention

- Strong pairing methods
- Encryption

---

## Unit 14: 6LoWPAN (IoT Integration)

### Introduction to 6LoWPAN

- **6LoWPAN** — IPv6 over Low-Power Wireless Personal Area Network
- **Purpose:** Enable IPv6 communication in low-power, constrained devices

#### Features

- IPv6 support
- Low power operation
- Suitable for IoT communication

> **Key Concept:** Adapts IPv6 for **constrained devices**.

---

### IPv6 Basics

- **IPv6** — Internet Protocol version 6 (latest Internet Protocol)
- **Address size:** 128-bit
- **Format:** Written in hexadecimal, separated by colons

**Why IPv6?**
- **IPv4 (Internet Protocol version 4)** has a limited address space (4.3 billion addresses)
- IPv6 provides a vastly **larger address space** and **more efficient routing**

---

### 6LoWPAN Architecture

6LoWPAN uses an **adaptation layer** between the data link and network layers.

#### Protocol Stack

```
┌──────────────────────────────┐
│        Application           │
├──────────────────────────────┤
│      Transport (UDP)         │  ← User Datagram Protocol
├──────────────────────────────┤
│      Network (IPv6)          │  ← Internet Protocol version 6
├──────────────────────────────┤
│  Adaptation Layer (6LoWPAN)  │  ← Header compression, Fragmentation, Forwarding
├──────────────────────────────┤
│         Data Link            │
├──────────────────────────────┤
│         Physical             │
└──────────────────────────────┘
```

#### Adaptation Layer Functions

- **Header compression** — reduces packet size
- **Fragmentation** — splits large IPv6 packets into smaller ones
- **Packet forwarding** — routes packets across the network

> **Key Concept:** Enables IPv6 in **low-power networks**.

---

### Protocol Stack Comparison

| Feature | BLE Stack | 6LoWPAN Stack |
|---------|-----------|---------------|
| **Layers** | GATT, ATT, L2CAP, Link Layer | IPv6, UDP, Adaptation Layer |
| **Protocol type** | Non-IP based | IP-based |
| **Use case** | Short-range communication | IoT networks |

---

### Packet Structure (6LoWPAN)

A 6LoWPAN packet consists of:
1. **Compressed IPv6 header**
2. **Fragmentation header** (if applicable)
3. **Payload**

#### Fragmentation Header Fields

| Field | Description |
|-------|-------------|
| **Datagram size** | Total size of original data |
| **Tag** | Identifies all fragments of the same datagram |
| **Offset** | Position of this fragment within the original data |

> **Key Concept:** Reduce packet size for **low-power networks**.

---

### Security in 6LoWPAN

#### Security Goals

- **Confidentiality** — data is not readable by unauthorized parties
- **Integrity** — data is not tampered with in transit
- **Authentication** — verifying the identity of communicating devices

#### Security Mechanisms

- Encryption
- Authentication

#### Challenge

- Devices have **limited power and processing capability**

> **Key Concept:** 6LoWPAN requires **lightweight security** solutions.

---

*End of Notes*
