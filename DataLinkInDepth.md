
# 🌐 Data Link Layer - In-Depth Notes

---

## 4. 🔄 Flow Control

Flow control ensures that the **sender does not overwhelm the receiver** with data faster than it can be processed. This is crucial in preventing **data loss or buffer overflow**.

### 🔧 Techniques:

1. **Stop-and-Wait Protocol**:
   - Sender sends one frame at a time.
   - Waits for an acknowledgment (ACK) before sending the next frame.
   - If ACK is not received within a time, the frame is retransmitted.
   - ✅ Simple but inefficient for high-speed networks due to idle wait times.

2. **Sliding Window Protocol**:
   - Sender can send **multiple frames** before needing an ACK.
   - Both sender and receiver maintain a **window** of sequence numbers.
   - ACKs move the window forward, allowing new frames to be sent.
   - Variants:
     - Go-Back-N: Resends all frames after an error.
     - Selective Repeat: Only retransmits erroneous frames.
   - ✅ Efficient and scalable for fast or long-distance communication.

3. **Backpressure (in Wired Networks)**:
   - Receiver signals the sender to **slow down** or **pause** transmission if it’s overwhelmed.
   - Common in full-duplex links like Ethernet switches.

---

## 5. 🔐 Access Control (Media Access Control)

Access control ensures that **multiple devices sharing the same communication medium** (like Ethernet or Wi-Fi) do not transmit simultaneously, causing collisions.

### 🔧 Techniques:

1. **CSMA/CD (Carrier Sense Multiple Access with Collision Detection)**:
   - Used in traditional Ethernet.
   - Device listens before transmitting.
   - If the medium is idle, it transmits.
   - If collision is detected, all devices stop, wait random times, and retry.

2. **CSMA/CA (Collision Avoidance)**:
   - Used in Wi-Fi.
   - Device senses channel before sending.
   - Waits for acknowledgment from the receiver.
   - Uses **Request to Send (RTS)** and **Clear to Send (CTS)** mechanisms to avoid collision.

3. **Token Passing**:
   - A **token** (a special control frame) circulates in the network.
   - Only the device holding the token can send data.
   - Used in Token Ring networks.

4. **Polling**:
   - A central controller (master) polls devices (slaves) one by one to check if they have data to send.
   - Reduces collisions, but introduces **central point of failure**.

---

## 6. 📦 Hop-by-Hop Delivery

Unlike the Network Layer (which ensures end-to-end delivery), the Data Link Layer is responsible for **hop-by-hop delivery**.

- Involves the transmission of frames between **two directly connected devices** (e.g., PC ↔ Switch or Switch ↔ Router).
- Each intermediate device (like a switch) receives the frame, processes it, and forwards it to the next hop based on MAC addressing.
- Important in building larger networks using small, reliable data transfers between links.

---

## 7. ⚙️ Sub-layers of the Data Link Layer

The Data Link Layer is logically split into two sub-layers for modularity and standardization.

### a. LLC (Logical Link Control) – IEEE 802.2
- Manages communication between the Network Layer and MAC sub-layer.
- Identifies the protocol encapsulated in the frame (IP, ARP, etc.).
- Provides error detection and flow control.
- Ensures reliable logical links between devices.

### b. MAC (Media Access Control) – IEEE 802.3 (Ethernet), IEEE 802.11 (Wi-Fi)
- Manages **physical addressing** using MAC addresses.
- Responsible for **frame delimiting**, **media access**, and **collision handling**.
- Deals with how the frame is placed on the medium and how access to the medium is controlled.

---

## 🖥 Role of NIC (Network Interface Card) in DLL

The **NIC** is a hardware component inside each networked device and operates at Layer 2 (Data Link Layer).

### Key Responsibilities:
- Holds the **MAC address**.
- Converts **bits to frames** (encapsulation) and vice versa (decapsulation).
- Performs **CRC** to check integrity of incoming frames.
- **Buffers** incoming/outgoing frames.
- May implement offloading features (e.g., TCP checksum calculation, segmentation).

---

## 🧩 Example: Data Link Layer in Ethernet Transmission

Suppose PC A wants to send a file to PC B over Ethernet:

1. **Encapsulation**: Network Layer packet (IP packet) is encapsulated into an Ethernet frame by the Data Link Layer.
2. **MAC Addressing**: The frame includes:
   - Source MAC (PC A's NIC)
   - Destination MAC (PC B’s NIC)
3. **Transmission**: Frame sent from PC A to the switch.
4. **Switch Operation**:
   - Uses its MAC address table to determine the correct output port.
   - Forwards frame to PC B.
5. **Error Checking**: PC B’s NIC uses CRC to check for errors.
6. **Decapsulation**: Header and trailer are removed, and the IP packet is passed to the Network Layer.

---

## 🏁 Summary of Data Link Layer Functions

| Function         | Description                                                                          |
|------------------|--------------------------------------------------------------------------------------|
| Framing          | Breaks bit stream into manageable data units (frames).                              |
| MAC Addressing   | Local delivery using unique hardware MAC addresses.                                 |
| Error Detection  | Detects and handles transmission errors using CRC, parity bits, etc.                |
| Flow Control     | Regulates data flow so receiver isn't overwhelmed.                                  |
| Access Control   | Controls which device can transmit on shared medium to avoid collision.             |
| Hop-by-Hop       | Ensures reliable delivery between adjacent nodes (not end-to-end).                  |
