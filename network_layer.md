# Network Layer — Deep, Engineer-Level Reference

## 1. Network Layer PDU: Packets
- The **Protocol Data Unit (PDU)** at the network layer is called a **Packet**.  
- Responsibilities:
  - Logical addressing (IP addresses).
  - Routing packets between source and destination across multiple networks.
  - Encapsulation of transport-layer segments into packets.
  - Error handling and diagnostics (e.g., ICMP).

---

## 2. IP Address: Device Identification
- Every device in a network is identified uniquely by an **IP address**.
- An **IP address** is:
  - A numerical label assigned to each device (host, router, switch with L3 capabilities).
  - Provides two critical pieces of information:
    - **Network ID**: Identifies the network segment.
    - **Host ID**: Identifies the specific device within the network.

---

## 3. IPv4
### 3.1 Basics
- **IPv4**: Internet Protocol version 4.
- **32-bit address space** (binary number).
- Written in **dotted decimal notation**: `8bit.8bit.8bit.8bit`
- Example: `192.168.1.1`
- Each octet ranges from `0–255`.
- Total addresses: `2^32 ≈ 4.29 billion` unique addresses.

### 3.2 Limitations of IPv4
- Exhaustion of public IPv4 addresses due to the internet boom.
- Workarounds: Subnetting, NAT, Private IP addresses.

---

## 4. IPv6
### 4.1 Basics
- **128-bit address space**.
- Total addresses: `2^128` (virtually inexhaustible).
- Written in **hexadecimal notation** separated by colons (`:`).
- Example: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Leading zeros are **not allowed** and can be compressed:
  - Example: `2001:db8:85a3::8a2e:370:7334`

### 4.2 Address Types
- **Unicast**: One-to-one communication (single sender → single receiver).
- **Multicast**: One-to-many communication (single sender → multiple receivers).
- **Anycast**: One-to-nearest communication (delivers to the nearest node out of a group).

---

## 5. IPv4 Address Classes
IPv4 addresses are divided into classes based on the **first octet**.

| Class | First Octet Range | Starting Bits | Usage              |
|-------|------------------|---------------|--------------------|
| A     | 0 – 127          | 0             | Unicast (Large networks) |
| B     | 128 – 191        | 10            | Unicast (Medium networks) |
| C     | 192 – 223        | 110           | Unicast (Small networks) |
| D     | 224 – 239        | 1110          | Multicast          |
| E     | 240 – 255        | 1111          | Experimental       |

### Details:
- **Class A**:
  - Network bits: 8, Host bits: 24.
  - Supports: `16 million hosts per network`.
- **Class B**:
  - Network bits: 16, Host bits: 16.
  - Supports: `65,534 hosts per network`.
- **Class C**:
  - Network bits: 24, Host bits: 8.
  - Supports: `254 hosts per network`.

---

## 6. Subnetting & Supernetting
### 6.1 Subnetting
- Process of dividing a large network into smaller sub-networks.
- Benefits:
  - Efficient IP usage.
  - Improved security and isolation.
  - Reduced broadcast traffic.
- Example:
  - Network: `192.168.1.0/24`
  - Subnet mask: `255.255.255.192`
  - Divides into 4 subnets, each with 64 IPs.

### 6.2 Supernetting
- Combining smaller networks into a bigger network (route aggregation).
- Benefits:
  - Simplifies routing tables.
  - Reduces overhead in backbone routers.

---

## 7. NAT (Network Address Translation)
### 7.1 Purpose
- Solves IPv4 exhaustion.
- Allows multiple devices in a private network to share a single public IP.

### 7.2 Types of NAT
- **Static NAT**: One-to-one mapping between private and public IP.
- **Dynamic NAT**: Maps private IPs to public IPs from a pool.
- **PAT (Port Address Translation)** / NAT Overload:
  - Multiple private IPs share one public IP using different port numbers.

### 7.3 Why Ports are Changed?
- To uniquely identify individual private connections.
- Enables multiple devices to communicate using the same public IP.

### 7.4 Private IP Ranges (RFC 1918)
- Class A: `10.0.0.0 – 10.255.255.255`
- Class B: `172.16.0.0 – 172.31.255.255`
- Class C: `192.168.0.0 – 192.168.255.255`

---

## 8. IPv4 Header (20–60 bytes)
### 8.1 Important Fields
- **Version (4 bits)**: IPv4 = 4.
- **IHL (Header Length)**: Size of header in 32-bit words.
- **TOS (Type of Service)**: QoS/priority indication.
- **Total Length**: Entire packet length (header + data).
- **Identification**: Helps in reassembling fragmented packets.
- **Flags**: Control fragmentation (DF = Don’t Fragment, MF = More Fragments).
- **Fragment Offset**: Position of fragment in original packet.
- **TTL (Time to Live)**: Prevents infinite looping of packets.
- **Protocol**: Indicates upper layer (e.g., TCP=6, UDP=17, ICMP=1).
- **Header Checksum**: Error detection for the header only.
- **Source Address** & **Destination Address**.
- **Options**: Security, timestamp, etc.
- **Data**: Payload.

### 8.2 MTU (Maximum Transmission Unit)
- Largest size a packet can be transmitted without fragmentation.
- Example:
  - Ethernet MTU = 1500 bytes.

---

## 9. IPv6 Enhancements
### 9.1 Advantages
- Vast address space (`2^128`).
- Simplified header (fixed size 40 bytes).
- No need for NAT (end-to-end connectivity).
- Better support for mobile and IoT devices.

### 9.2 IPv6 Header (40 bytes fixed)
- **Version (4 bits)**: IPv6 = 6.
- **Traffic Class (8 bits)**: Similar to TOS in IPv4.
- **Flow Label (20 bits)**: Identifies flow for QoS.
- **Payload Length**: Length of payload (excluding header).
- **Next Header**: Indicates upper layer protocol (similar to Protocol field in IPv4).
- **Hop Limit**: Like TTL in IPv4.
- **Source Address** & **Destination Address**.

---
