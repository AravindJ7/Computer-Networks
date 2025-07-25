
# ❌ Error Detection and Control – Data Link Layer

## 🎯 Objective
Ensure data integrity across a noisy or unreliable transmission medium (e.g., cables, wireless) by detecting and possibly recovering from errors.

---

## 🧠 Responsibilities of Data Link Layer

| Task               | Description |
|--------------------|-------------|
| Error Detection     | Identifying whether the frame received is corrupted |
| Error Control       | Deciding how to handle or recover from that error |

---

## 🔍 Error Detection Techniques

### 1. Parity Bit
- Simple single-bit error detection mechanism.
- **Even parity**: Ensures total 1s is even.
- **Odd parity**: Ensures total 1s is odd.

**Example**:  
Data: `1010001` (3 ones) → Add 1 (even parity) → `10100011`

✅ Detects single-bit errors  
❌ Cannot detect multiple or burst errors

---

### 2. Checksum
- Common in IP, UDP.
- Adds all bytes (or words) and sends the sum as a checksum.
- Receiver adds data and checksum → Should result in 0.

✅ Detects many error types  
❌ Not reliable for all cases

---

### 3. CRC (Cyclic Redundancy Check)
- Most robust and commonly used (e.g., Ethernet, HDLC).
- Based on polynomial division.
- Sender divides data by a generator polynomial and appends the remainder (CRC value).
- Receiver re-divides data and checks remainder.

✅ Detects:
- Single, double, and odd-bit errors
- Burst errors up to certain length

---

## ✅ Error Control Mechanisms

### 1. Frame Discard
- If error is detected (e.g., CRC mismatch), discard the frame.
- No recovery is done at this layer in basic Ethernet.

---

### 2. ARQ (Automatic Repeat Request)
Used in PPP, HDLC, and sliding window protocols.

#### ARQ Types:
| Type               | Description |
|--------------------|-------------|
| Stop-and-Wait ARQ  | Sender waits for ACK before sending next frame |
| Go-Back-N ARQ      | Sender retransmits from erroneous frame onward |
| Selective Repeat ARQ | Only the corrupted frame is resent |

---

### 3. Acknowledgments
- **ACK**: Frame received correctly.
- **NACK**: Frame had error – request retransmission.
- If no ACK → timeout triggers resend.

---

## 🧬 Layer-wise Responsibility

| Layer             | Role in Error Handling |
|------------------|------------------------|
| Data Link Layer   | Detects errors, may request resend (ARQ) |
| Transport Layer (TCP) | Handles complete retransmission and ordering |
| Application Layer | May use checksums or hashes for final integrity |

---

## 🌐 Ethernet Frame Example with CRC

| Field           | Description |
|----------------|-------------|
| Preamble        | 7 bytes for sync |
| SFD             | 1 byte |
| Destination MAC | 6 bytes |
| Source MAC      | 6 bytes |
| Type/Length     | 2 bytes |
| Payload         | 46–1500 bytes |
| CRC (FCS)       | 4 bytes – Error detection (CRC-32) |

---

## 🧠 Summary

| Concept           | Description |
|------------------|-------------|
| Error Detection   | Find corrupted frames |
| Detection Methods | Parity, Checksum, CRC |
| CRC               | Most reliable, used in Ethernet |
| Error Control     | Drop or request resend |
| ARQ               | Automatic retransmission methods |
| TCP/Transport     | Ensures full recovery and reliability |

---
