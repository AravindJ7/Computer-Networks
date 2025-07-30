# 🌐 Network Layer – Deep Dive Notes with Example Scenario

---

## 📘 Overview

The **Network Layer** is **Layer 3** in the OSI model, primarily responsible for:

* **Logical addressing** using IP
* **Routing and forwarding** packets across networks
* **Fragmentation** and **reassembly**
* **Error handling** via control messages (e.g., ICMP)

The layer enables data to move from **source to destination** even when the devices are on **different networks**.

---

## 🧱 Core Functions

### 1. Logical Addressing

* Every device gets a **unique IP address**.
* IP addresses allow communication **beyond local networks**.

### 2. Routing

* Finds the **best path** to the destination.
* Routers use **routing tables** and **protocols** (e.g., OSPF, BGP).

### 3. Packet Forwarding

* Routers forward packets based on destination IP.

### 4. Fragmentation and Reassembly

* Large packets are **fragmented** if they exceed MTU.
* Fragments are **reassembled** at the destination.

### 5. Error Reporting

* **ICMP** is used to report errors (e.g., destination unreachable).

---

## 📦 IPv4 Packet Format (Header)

| Field               | Size    | Description                                 |
| ------------------- | ------- | ------------------------------------------- |
| Version             | 4 bits  | IP version (usually 4)                      |
| Header Length       | 4 bits  | Length of header                            |
| Type of Service     | 8 bits  | QoS / Differentiated Services               |
| Total Length        | 16 bits | Total length of packet                      |
| Identification      | 16 bits | Used for fragmentation                      |
| Flags               | 3 bits  | Control fragmentation                       |
| Fragment Offset     | 13 bits | Position of fragment                        |
| TTL                 | 8 bits  | Max hops before discard                     |
| Protocol            | 8 bits  | Next protocol (e.g., TCP=6, UDP=17, ICMP=1) |
| Header Checksum     | 16 bits | Error check for header                      |
| Source IP Address   | 32 bits | Sender's IP                                 |
| Destination IP Addr | 32 bits | Receiver's IP                               |

---

## 🔁 Packet Lifecycle – End-to-End Example

### 🧪 Scenario

Two networks:

* **Network 1** (192.168.1.0/24) with PC1 (192.168.1.10, MAC: AA\:AA\:AA)
* **Network 2** (192.168.2.0/24) with PC3 (192.168.2.30, MAC: DD\:DD\:DD)
* Routers: R1 (192.168.1.1, MAC: BB\:BB\:BB), R2 (192.168.2.1, MAC: CC\:CC\:CC)

### Step-by-Step Transmission

#### **Step 1: PC1 Creates Packet**

* Source IP: `192.168.1.10`
* Destination IP: `192.168.2.30`
* Destination MAC: `BB:BB:BB` (R1's MAC via ARP)

**Ethernet Frame (Layer 2):**

```
+-------------------------+
| Dest MAC: BB:BB:BB:BB  |
| Src MAC:  AA:AA:AA:AA  |
+-------------------------+
| IP Header (L3):        |
| Src IP: 192.168.1.10   |
| Dst IP: 192.168.2.30   |
+-------------------------+
| TCP Header / Data      |
+-------------------------+
```

#### **Step 2: Router R1 Forwards Packet**

* R1 strips Ethernet frame.
* Uses routing table → forwards to R2.
* New Ethernet frame created:

  * Source MAC: R1 (BB\:BB\:BB)
  * Destination MAC: R2 (CC\:CC\:CC)

#### **Step 3: Router R2 Delivers to PC3**

* IP header still has:

  * Src: `192.168.1.10`
  * Dst: `192.168.2.30`
* R2 ARPs for PC3's MAC → gets `DD:DD:DD`
* New Ethernet frame:

  * Source MAC: CC\:CC\:CC
  * Destination MAC: DD\:DD\:DD

#### **Step 4: PC3 Receives Packet**

* Matches destination IP and MAC.
* Processes packet at Transport Layer.

---

## 🛡️ Security Risks at Network Layer

| Attack Type             | Description                                      |
| ----------------------- | ------------------------------------------------ |
| IP Spoofing             | Attacker fakes source IP to disguise identity    |
| ICMP Flood              | Overwhelms host with ICMP (ping) requests        |
| Smurf Attack            | Spoof broadcast ping to flood victim             |
| Routing Table Poisoning | Injects false routes into routing protocol       |
| MITM (Layer 3)          | Intercepts/modifies traffic using IP redirection |

---

## 🧠 IPv4 vs IPv6

| Feature       | IPv4                       | IPv6                    |
| ------------- | -------------------------- | ----------------------- |
| Address Size  | 32-bit                     | 128-bit                 |
| Format        | Decimal (e.g. 192.168.1.1) | Hex (e.g. 2001\:db8::1) |
| Header Size   | Variable                   | Fixed                   |
| Fragmentation | By routers and sender      | Only by sender          |
| IPsec         | Optional                   | Mandatory               |

---

## 📘 Key Network Layer Protocols

| Protocol | Purpose                              |
| -------- | ------------------------------------ |
| IP       | Core protocol for addressing/routing |
| ICMP     | Control and diagnostic (e.g., ping)  |
| IGMP     | Multicast group management           |
| IPsec    | Secure IP communication (encryption) |

---

## 📋 Sample Routing Table (Router R1)

```
Destination      Gateway        Interface
0.0.0.0/0        10.0.0.1       eth1      # Default route
192.168.1.0/24   0.0.0.0        eth0      # Directly connected
192.168.2.0/24   10.0.0.2       eth1      # Route to Network 2
```

---

## ✅ Summary

* Network layer uses **IP addresses** for routing.
* **Routers** operate at this layer.
* It enables communication **between networks**, not just within a LAN.
* **MAC addresses are irrelevant beyond local network**.
* Attacks at this layer exploit weaknesses in **routing and IP management**.

---
