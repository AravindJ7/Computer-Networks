# 🌐 Deep-Dive Notes: Ports, Protocols, Headers & TCP Handshakes  

---

## 1. 🔑 Ports and Their Ranges
Ports are **logical communication endpoints** used by **Transport Layer protocols (TCP/UDP)** to identify specific processes/services on a host.

- **Port Number Size:** 16 bits → range **0 – 65,535**  

### 📌 Three Ranges
1. **Well-Known Ports (0–1023)**  
   - Assigned by **IANA (Internet Assigned Numbers Authority)**  
   - Reserved for common services  
   - Examples:  
     - 20/21 → FTP  
     - 22 → SSH  
     - 25 → SMTP  
     - 53 → DNS  
     - 80 → HTTP  
     - 443 → HTTPS  

2. **Registered Ports (1024–49151)**  
   - Assigned by IANA to vendors/applications on request  
   - Not as strictly controlled as well-known ports  
   - Examples:  
     - 3306 → MySQL  
     - 1433 → MS SQL Server  
     - 3389 → RDP  

3. **Ephemeral / Dynamic Ports (49152–65535)**  
   - Not registered with IANA  
   - Used **temporarily by clients** when initiating connections  
   - Example:  
     - A client connecting to a web server may use **port 54732** to reach server’s **TCP 443**  

⚠️ Note: The ephemeral port range **can vary by OS** (e.g., Linux often 32768–60999).  

---

## 2. 🔁 Transport Layer Protocols: TCP vs UDP  

### A. **UDP (User Datagram Protocol)**
- **Connectionless**, lightweight, no handshake  
- No guarantees of delivery, order, or reliability  
- Used where **speed > reliability** (e.g., VoIP, DNS, streaming)  
- **No congestion control**  

### B. **TCP (Transmission Control Protocol)**
- **Connection-oriented**, reliable  
- Provides:  
  - **Segmentation & Reassembly**  
  - **Acknowledgments (ACKs)**  
  - **Flow Control** (Sliding Window)  
  - **Congestion Control** (AIMD, slow start, etc.)  
  - **Ordered Delivery**  

---

## 3. 📜 Header Structures  

### UDP Header (8 bytes total)
| Field             | Size (bits) | Description |
|-------------------|-------------|-------------|
| Source Port       | 16          | Sending process identifier |
| Destination Port  | 16          | Receiving process identifier |
| Length            | 16          | Header + Data size in bytes |
| Checksum          | 16          | Error-checking for header + data |

➡️ Very small overhead — good for speed.  

---

### TCP Header (20–60 bytes)
| Field                 | Size (bits) | Description |
|------------------------|-------------|-------------|
| Source Port            | 16          | Sending process |
| Destination Port       | 16          | Receiving process |
| Sequence Number        | 32          | Order tracking |
| Acknowledgment Number  | 32          | Confirms received data |
| Data Offset (Header Len)| 4          | Size of TCP header |
| Reserved               | 3          | Future use |
| Flags (Control Bits)   | 9          | **URG, ACK, PSH, RST, SYN, FIN** |
| Window Size            | 16          | Flow control |
| Checksum               | 16          | Integrity check |
| Urgent Pointer         | 16          | Points to urgent data |
| Options (if any)       | Variable    | MSS, Window scaling, etc. |
| Data                   | Variable    | Application payload |

---

## 4. 🤝 TCP Handshakes  

### **Connection Establishment: 3-Way Handshake**
1. **SYN** → Client sends `SYN` with initial sequence number.  
2. **SYN-ACK** → Server replies with its own `SYN` + acknowledgment.  
3. **ACK** → Client acknowledges server’s sequence number.  

➡️ Now both sides agree on **initial sequence numbers (ISN)** and connection is established.  

---

### **Connection Termination: 4-Way Handshake**
1. **FIN** → Client sends `FIN` to close connection (no more data).  
2. **ACK** → Server acknowledges `FIN`.  
3. **FIN** → Server sends its own `FIN` once done sending data.  
4. **ACK** → Client acknowledges server’s `FIN`.  

➡️ Connection closed gracefully.  

⚠️ **TIME_WAIT state**:  
- After closing, the client may enter **TIME_WAIT** (typically 2 × MSL = Maximum Segment Lifetime)  
- Ensures delayed or duplicate packets are discarded safely.  

---

## 5. 📝 Key Notes for Engineers
- **TCP is reliable, but slower** due to handshakes and ACKs.  
- **UDP is faster, but unreliable**, suited for real-time apps.  
- Port ranges define how apps interact; ephemeral ports are crucial for client-server models.  
- TCP headers are **heavier** than UDP (20–60 bytes vs 8 bytes).  
- Always consider **NAT and firewalls**: they track connections based on **5-tuple (src IP, src port, dst IP, dst port, protocol)**.  

---
