# Line Coding – Comprehensive Notes

---

## 1. Introduction

**Line Coding** is the process of converting a stream of digital data (bits) into a form of digital signal suitable for transmission over a physical communication channel.

- **Encoding**: Assigning a pattern of voltage, current, or light pulses to represent bits (`0` and `1`).
- **Purpose**:
  1. Enable transmission over a specific medium.
  2. Maintain synchronization between transmitter and receiver.
  3. Reduce errors and DC components.
  4. Optimize bandwidth usage.

---

## 2. Types of Signals

1. **Digital Electrical Signal** – Represented by discrete voltage levels (e.g., +5V, 0V, -5V).
2. **Digital Optical Signal** – Represented by light pulses (e.g., ON = `1`, OFF = `0`).

---

## 3. Data Elements vs Signal Elements

- **Data Element**: The smallest unit of data (usually a single bit: `0` or `1`).
- **Signal Element**: The shortest unit of the transmitted signal corresponding to one or more data elements.

**Mapping Possibilities**:
- **Many-to-One**: Multiple data elements encoded as a single signal element.
- **One-to-Many**: A single data element encoded as multiple signal elements.

**Signal Element is also called a "Carrier"** because it carries the data.

---

## 4. Desirable Properties of Line Coding

1. **Synchronization** – Receiver must correctly align bit boundaries.
2. **DC Balance** – Average voltage close to zero to avoid baseline wander.
3. **Error Detection** – Some codes allow detection of bit errors.
4. **Bandwidth Efficiency** – Minimize required transmission bandwidth.
5. **Noise Immunity** – Better tolerance to noise and signal degradation.

---

## 5. Line Coding Techniques

### 5.1 Unipolar Techniques

#### 5.1.1 Unipolar NRZ (Non-Return-to-Zero)
- Uses only **one polarity**: `0V` for logical `0` and `+V` for logical `1`.
- **Bit Mapping**:
  - `1` → +V (constant for full bit duration)
  - `0` → 0V
- **Characteristics**:
  - **Advantages**: Very simple implementation.
  - **Disadvantages**: No DC balance, poor synchronization, high DC component, long runs of same bit cause loss of sync.
  - **Bandwidth**: Low (since signal changes only on bit changes).
- **DC Component**: High.

---

#### 5.1.2 Unipolar RZ (Return-to-Zero)
- Signal returns to `0V` in the middle of each bit period.
- **Bit Mapping**:
  - `1` → +V (first half), then 0V (second half)
  - `0` → 0V for entire bit duration
- **Advantages**: Better synchronization than NRZ.
- **Disadvantages**: Requires more bandwidth (twice transitions).
- **DC Component**: Still present.

---

### 5.2 Polar Techniques

#### 5.2.1 Polar NRZ-Level (NRZ-L)
- Two polarities: `+V` and `-V`.
- **Bit Mapping**:
  - `1` → +V
  - `0` → -V
- **Advantages**: Lower DC component compared to unipolar NRZ.
- **Disadvantages**: Long sequences of same bits cause sync issues.

---

#### 5.2.2 Polar NRZ-Inverted (NRZ-I)
- Polarity changes only on `1`s.
- **Bit Mapping**:
  - `1` → Transition at start of bit
  - `0` → No transition
- **Advantages**: Avoids issues with long runs of `0`s.
- **Synchronization**: Better than NRZ-L for certain data patterns.

---

#### 5.2.3 Polar RZ
- Returns to zero in middle of bit.
- **Bit Mapping**:
  - `1` → +V for first half, 0V for second half
  - `0` → -V for first half, 0V for second half
- **Advantages**: Excellent sync.
- **Disadvantages**: Doubles bandwidth requirement.

---

### 5.3 Bipolar Techniques

#### 5.3.1 AMI (Alternate Mark Inversion)
- `0` → 0V
- `1` → Alternates between +V and -V for each occurrence.
- **Advantages**: No DC component, allows long runs of `0`s, supports error detection.
- **Disadvantages**: Still loses sync if long run of `0`s.
- **DC Component**: Eliminated due to alternating marks.

---

#### 5.3.2 Pseudoternary
- Opposite of AMI:
  - `1` → 0V
  - `0` → Alternates between +V and -V.
- **Characteristics**: Same benefits as AMI but encoding is reversed.

---

### 5.4 Biphase Techniques

#### 5.4.1 Manchester
- Mid-bit transition **always occurs**.
- **Bit Mapping**:
  - `1` → High → Low transition (in middle)
  - `0` → Low → High transition (in middle)
- **Advantages**: Self-clocking, good synchronization.
- **Disadvantages**: Requires double bandwidth.

---

#### 5.4.2 Differential Manchester
- Always has a **mid-bit transition** for clocking.
- Bit value determined by presence/absence of transition at start of bit:
  - `1` → No transition at start
  - `0` → Transition at start
- **Advantages**: Immune to polarity inversion, good sync.
- **Bandwidth**: Double base rate.

---

## 6. Bandwidth and DC Component Summary

| Technique          | Voltage Levels | Bandwidth Usage   | DC Component | Self-Clocking | Long Run Problem |
|--------------------|----------------|-------------------|--------------|---------------|------------------|
| Unipolar NRZ       | 0, +V          | Low               | High         | No            | Yes              |
| Unipolar RZ        | 0, +V          | Medium            | High         | Yes           | Less             |
| Polar NRZ-L        | +V, -V         | Low               | Medium       | No            | Yes              |
| Polar NRZ-I        | +V, -V         | Low               | Medium       | Partial       | Less             |
| Polar RZ           | +V, 0, -V      | Medium            | Low          | Yes           | No               |
| AMI                | 0, ±V          | Medium            | None         | Partial       | Long zeros       |
| Pseudoternary      | 0, ±V          | Medium            | None         | Partial       | Long ones        |
| Manchester         | +V, -V         | High (2x)         | None         | Yes           | No               |
| Diff Manchester    | +V, -V         | High (2x)         | None         | Yes           | No               |

---

## 7. Visual Representation (Conceptual)

Although actual waveforms require diagrams, here’s a **symbolic mapping**:

Unipolar NRZ: 1:+V -------, 0: 0V _______
Unipolar RZ: 1:+V_|0V , 0: 0V_______
Polar NRZ-L: 1:+V ------- , 0: -V ______
Polar NRZ-I: 1:transition , 0:no change
Polar RZ: 1:+V_|0V , 0:-V_|0V
AMI: 1:+V/-V alt , 0:0V
Pseudoternary: 1:0V , 0:+V/-V alt
Manchester: 1:High→Low , 0:Low→High
Diff Manchester: 1:no start tx, 0:start tx

---

## 8. Key Takeaways

- **NRZ methods** are bandwidth-efficient but suffer from sync loss during long runs.
- **RZ methods** improve synchronization but double the required bandwidth.
- **Bipolar codes** eliminate DC component and can aid in error detection.
- **Manchester methods** are robust for synchronization but require higher bandwidth.
- **Differential encoding** (like Diff Manchester) adds robustness against polarity errors.

---
