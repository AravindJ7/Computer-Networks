# 📘 Multiplexing – In-depth Notes

---

## 1. Definition
Multiplexing is a technique in communication systems where **multiple signals (analog or digital) are combined and transmitted over a single communication channel/medium**, to make efficient use of available bandwidth.  

At the receiver end, a **demultiplexer** separates these signals back to their original form.  

- **Multiplexer (MUX):** Combines input signals → single composite signal  
- **Demultiplexer (DEMUX):** Splits composite signal → original individual signals  

---

## 2. Need for Multiplexing
- Communication channels (like fiber, copper, radio spectrum) are costly and limited  
- Users may need simultaneous transmission (voice, data, video)  
- Multiplexing allows **resource sharing** → many signals, one medium  

---

## 3. Classification of Multiplexing
There are **two major categories**:  

### **A. Analog Multiplexing**
Used for analog signals. Common techniques:
1. **Frequency Division Multiplexing (FDM)**
2. **Wavelength Division Multiplexing (WDM)** (a special form of FDM in optics)

### **B. Digital Multiplexing**
Used for digital signals. Common techniques:
1. **Time Division Multiplexing (TDM)**
   - Synchronous TDM
   - Statistical (Asynchronous) TDM
2. **Code Division Multiplexing (CDM / CDMA)**  

---

## 4. Types of Multiplexing (Detailed)

---

### 4.1 Frequency Division Multiplexing (FDM)
- **Concept:** The available bandwidth of a channel is divided into **non-overlapping frequency bands**, each carrying a separate signal  
- **Requires:** Guard bands (small unused frequency gaps) to avoid overlap  
- **Applications:** Radio/TV broadcasting, cable TV, first-generation analog telephony  

**Advantages:**
- Simple implementation for analog signals  
- Continuous transmission, low delay  

**Disadvantages:**
- Inefficient use of bandwidth (guard bands waste space)  
- Susceptible to crosstalk and interference  

---

### 4.2 Wavelength Division Multiplexing (WDM)
- **Concept:** Used in **optical fiber communication**. Multiple optical signals are transmitted on the same fiber using **different wavelengths (colors of light)**  
- **Types:**  
  - **CWDM (Coarse WDM):** Wide spacing (20 nm), fewer channels, cheaper  
  - **DWDM (Dense WDM):** Tight spacing (0.8–1.6 nm), many channels, expensive but supports terabits/sec  

**Applications:**  
- Internet backbone, submarine communication cables, long-distance fiber optic links  

**Advantages:**
- Extremely high capacity  
- Scalable (add wavelengths to increase bandwidth)  
- Long-distance transmission with optical amplifiers  

**Disadvantages:**
- Expensive equipment  
- Complex synchronization  

---

### 4.3 Time Division Multiplexing (TDM)
- **Concept:** The entire channel bandwidth is allocated to **one signal at a time**, but signals take turns in **time slots**.  
- **Two Types:**  
  1. **Synchronous TDM:** Each input gets a fixed time slot in a repeating cycle, even if idle  
  2. **Statistical (Asynchronous) TDM:** Time slots are dynamically assigned based on demand  

**Applications:** Digital telephony, computer networks, GSM  

**Advantages:**
- Efficient for digital signals  
- Simple demultiplexing (synchronized clocks)  

**Disadvantages:**
- Synchronous TDM wastes bandwidth if users are idle  
- Requires precise timing and synchronization  

---

### 4.4 Code Division Multiplexing (CDM / CDMA)
- **Concept:** Multiple signals are transmitted simultaneously over the **same frequency band**, but each is assigned a **unique spreading code**.  
- Receivers use the same code to separate signals.  
- Example: Mobile networks (3G CDMA).  

**Advantages:**
- High security and privacy (codes are unique)  
- Efficient spectrum usage (all users share full bandwidth)  
- Resistant to interference  

**Disadvantages:**
- Complex design and synchronization  
- Limited by the number of unique codes  

---

## 5. Comparison of Multiplexing Techniques

| Technique | Domain | Key Idea | Applications |
|-----------|--------|----------|--------------|
| **FDM**  | Frequency | Divide channel into frequency bands | Radio, cable TV |
| **WDM**  | Optical | Different wavelengths of light in fiber | Fiber internet, submarine cables |
| **TDM**  | Time | Divide channel into time slots | Telephony, GSM |
| **CDM**  | Code | Assign unique code per user | 3G, secure comms |

---

## 6. Real-World Applications
- **Telephony:** Multiple calls over one cable using TDM or FDM  
- **Radio/TV:** Different stations broadcast at different frequencies (FDM)  
- **Internet Backbone:** Fiber optics with DWDM carrying terabits of data  
- **Wireless Networks:** CDMA for mobile communication, OFDM in LTE/5G  

---

## 7. Summary
- **Multiplexing** allows multiple signals to share a single channel efficiently  
- **Analog methods:** FDM, WDM  
- **Digital methods:** TDM, CDM  
- Choice depends on **application, bandwidth availability, cost, and efficiency requirements**  

---
