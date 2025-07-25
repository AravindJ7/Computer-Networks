# 🧠 Data Link Layer (Layer 2) – In-Depth Notes

The **Data Link Layer (DLL)** is the **second layer** in the OSI model. It provides reliable communication between two directly connected nodes (hop-by-hop delivery).

---

## 🔰 Primary Responsibilities

- Framing
- Physical Addressing
- Error Detection and Control
- Flow Control
- Access Control (MAC)
- Hop-by-Hop Delivery
- Media Access Control & Logical Link Control

---

## 1. 🧱 Framing

**Framing** involves encapsulating Network Layer data into manageable units called frames.

### Generic Frame Structure:
+---------+-------------+-----------+----------+----------+
| Preamble| Header | Payload | Trailer | CRC |
+---------+-------------+-----------+----------+----------+

shell
Copy
Edit

### Ethernet Frame Format:
+--------+--------+--------+--------+--------+--------+--------+
|Preamble|Dest MAC|Src MAC |Type/Len|Payload |CRC/FCS |Padding |
+--------+--------+--------+--------+--------+--------+--------+

- **Header**: Source/Destination MAC addresses, type, control info.
- **Payload**: Actual data (e.g., IP packet).
- **Trailer/FCS**: CRC for error detection.

---

## 2. 🏷 Physical Addressing

- Uses **MAC Address** (48-bit) for local addressing.
- Format: `AA:BB:CC:DD:EE:FF`
- Managed by the **NIC (Network Interface Card)**.
- Switches use MAC addresses for frame forwarding.

---

## 3. ❌ Error Detection and Control

### Error Detection Methods:
- Parity Bit
- Checksums
- **CRC (Cyclic Redundancy Check)** – most common in Ethernet

### Error Control:
- Frames with errors (CRC mismatch) are discarded.
- Retransmission (e.g., ARQ) may be handled in higher layers or specific protocols (e.g., PPP).

---

## 4. 🔄 Flow Control

Regulates frame transmission speed to avoid receiver overload.

### Techniques:
- **Stop-and-Wait**
- **Sliding Window Protocol**
- **Backpressure**

---

## 5. 🔐 Access Control (MAC)

Controls how devices share the medium.

### Techniques:
- **CSMA/CD** (Ethernet)
- **CSMA/CA** (Wi-Fi)
- **Token Passing** (Token Ring)
- **Polling** (Centralized control)

---

## 6. 📦 Hop-by-Hop Delivery

- DLL ensures delivery between **directly connected devices** (not end-to-end).
- Each hop receives the frame, processes it, and forwards accordingly.

---

## 7. ⚙️ Sub-layers of the Data Link Layer

### a. Logical Link Control (LLC) – IEEE 802.2
- Identifies Network Layer protocol.
- Provides flow/error control.

### b. Media Access Control (MAC) – IEEE 802.3, 802.11
- Handles physical addressing, media access, and framing.

---

## 🖥 Network Interface Card (NIC)

- Operates at Layer 2.
- Stores MAC address.
- Generates/receives frames.
- Performs CRC checks.
- May handle offloading (checksum, segmentation).

---

## 🧩 Example: Ethernet Communication

1. IP packet from PC A is encapsulated into Ethernet frame.
2. Destination MAC = PC B’s MAC.
3. Frame sent to switch.
4. Switch uses MAC table to forward.
5. NIC checks CRC.
6. Payload passed to Network Layer.

---

## 🏁 Summary Table

| Function            | Description                                                                 |
|---------------------|-----------------------------------------------------------------------------|
| **Framing**         | Breaks stream into manageable frames.                                       |
| **MAC Addressing**  | Local addressing using hardware MACs.                                       |
| **Error Detection** | Detects transmission errors using CRC or parity.                            |
| **Flow Control**    | Regulates frame flow between sender and receiver.                           |
| **Access Control**  | Determines which device can access the shared medium.                       |
| **Hop-by-Hop**      | Ensures reliable communication between two directly connected devices.      |

---

## 📌 Protocols at Data Link Layer

- **Ethernet (IEEE 802.3)**
- **Wi-Fi (IEEE 802.11)**
- **PPP (Point-to-Point Protocol)**
- **HDLC (High-Level Data Link Control)**
- **Token Ring**
- **FDDI**

---

## 📚 Additional Notes

- **Switches** operate at Layer 2.
- **Bridges** also function at this layer.
- DLL plays a crucial role in **LAN technologies**, where reliable and fast local communication is key.

---
