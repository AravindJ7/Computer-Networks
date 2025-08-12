# Error Control — In-depth Notes

> Comprehensive explanation of error detection and correction, including theory, techniques, algorithms, and examples.

---

## Overview: Error Detection vs Error Correction

- **Error Detection** → Determine if an error occurred in received data.
- **Error Correction** → Locate and correct errors without retransmission.

Approaches:
- **ARQ (Automatic Repeat reQuest)** → Detect errors and retransmit.
- **FEC (Forward Error Correction)** → Detect and correct using redundant bits.

---

## Types of Errors

1. **Single-bit error** → Exactly one bit changes.
2. **Multiple isolated errors** → Several single-bit errors at different positions.
3. **Burst error** → Two or more consecutive bits in error.  
   - *Burst length* = from first to last erroneous bit inclusive.
4. **Random errors** → Independent bit flips (Binary Symmetric Channel model).

---

## Theory: Hamming Distance & Correction Capability

- **Hamming Distance (`d_min`)**: Minimum differing bit positions between any two codewords.
  - Detects up to `d_min - 1` errors.
  - Corrects up to `t = floor((d_min - 1) / 2)` errors.
- **Code Rate (`R`)**: `k / n` where:
  - `k` = data bits, `n` = total bits (data + redundancy).
- **Redundancy**: `n - k` bits.

---

## Error Detection Techniques

### 1. Parity Bit (VRC — Vertical Redundancy Check)
- Add 1 parity bit to make total `1`s even (even parity) or odd (odd parity).
- **Even parity** → total `1`s must be even.
- **Odd parity** → total `1`s must be odd.

**Properties**:
- Detects all odd-number-of-bit errors.
- Fails for even-number-of-bit errors.
- Overhead: 1 bit.

**Example**:  
Data: `1011001` → Even parity bit = `1`.

---

### 2. Longitudinal Redundancy Check (LRC)
- Compute parity column-wise across a data block.
- Append a parity row (LRC row).
- Often used with row parity (VRC) → **2D parity**.

**Properties**:
- Detects many burst errors.
- Can correct single-bit errors by locating intersection.

---

### 3. Checksum (One’s Complement)
**Steps**:
1. Split data into fixed-size words (e.g., 16-bit).
2. Add using one’s-complement arithmetic (wrap-around carry).
3. One’s complement the result → checksum.
4. Append checksum.
5. Receiver sums including checksum → result should be all `1`s.

**Properties**:
- Good for random errors.
- Weaker for burst errors than CRC.
- Used in TCP, UDP, IP.

---

### 4. Cyclic Redundancy Check (CRC)
- Treat data as a binary polynomial.
- Choose generator polynomial `G(x)` of degree `r`.
- Append `r` zeros to data.
- Divide by `G(x)` using XOR.
- Remainder (length `r`) = CRC bits.
- Append CRC to data.
- Receiver repeats division → remainder should be `0`.

**Properties**:
- Detects all burst errors ≤ `r` bits.
- High probability of catching longer errors.
- Used in Ethernet, storage, etc.

**Example**:
- Data: `1101011011`
- Generator: `10011` (deg=4)
- Append zeros: `11010110110000`
- Divide → remainder: `1000`
- Transmit: `11010110111000`

---

## Error Correction (FEC)

### 1. Hamming Codes
- `(n, k)` code, e.g., `(7,4)` → 4 data bits, 3 parity bits.
- Minimum distance = 3 → corrects 1 bit, detects 2 bits.
- Parity bits at positions 1, 2, 4.

**Encoding Example (Even parity)**:
- Data: `d1=1, d2=0, d3=1, d4=1`
- `p1 = d1 ⊕ d2 ⊕ d4 = 0`
- `p2 = d1 ⊕ d3 ⊕ d4 = 1`
- `p3 = d2 ⊕ d3 ⊕ d4 = 0`
- Codeword: `0110011`

**Decoding**:
- Compute syndrome bits from received word.
- Syndrome as binary number = error position.
- Flip that bit.

---

### 2. Reed–Solomon Codes
- Symbol-based, not bit-based.
- RS(n, k) corrects `(n − k) / 2` symbol errors.
- Excellent for burst errors.
- Used in CDs, DVDs, QR codes, storage.

---

### 3. Convolutional Codes
- Shift-register based; output depends on current + previous bits.
- Decoded using Viterbi algorithm.
- Used in wireless, satellite.

---

### 4. LDPC & Turbo Codes
- **LDPC** → Sparse parity-check matrix, iterative decoding.
- **Turbo** → Parallel concatenated convolutional codes, iterative decoding.
- Near Shannon-limit performance.
- Used in 5G, Wi-Fi, DVB-S2.

---

## ARQ Protocols (Detection + Retransmission)
- **Stop-and-Wait**: One frame at a time.
- **Go-Back-N**: Multiple frames; retransmit from error onward.
- **Selective Repeat**: Retransmit only erroneous frames.

---

## Comparison Table

| Technique   | Overhead | Detects                   | Corrects   | Use Cases                  |
|-------------|----------|---------------------------|------------|----------------------------|
| Parity (VRC)| 1 bit    | Odd number of bit errors   | No         | Simple, low-cost checks    |
| LRC         | m+n bits | Burst errors               | Single-bit | File blocks, tapes         |
| Checksum    | 16/32bit | Many random errors         | No         | TCP/IP, UDP                |
| CRC         | r bits   | Burst ≤ r, most others     | No         | Ethernet, storage, comms   |
| Hamming(7,4)| 3 bits   | ≤ 2-bit errors             | 1-bit      | Memory ECC                  |
| Reed–Solomon| Variable | Symbol errors, bursts      | Multi-sym  | CDs, QR, storage           |
| Convolutional| Variable| Many random/burst patterns | Several    | Wireless, streaming        |
| LDPC/Turbo  | Variable | Excellent                  | Excellent  | 5G, modern satellite       |

---

## Pseudocode

### Parity (Even)
```text
parity = 0
for bit in data:
    parity ^= bit
append parity
sum = 0
for word in data_words:
    sum += word
    if sum > 0xFFFF:
        sum = (sum & 0xFFFF) + 1
checksum = ~sum & 0xFFFF
append checksum
append r zeros to data
for i in range(original_length):
    if data[i] == 1:
        for j in range(r+1):
            data[i+j] ^= generator[j]
remainder = last r bits of data
append remainder
 
 ---