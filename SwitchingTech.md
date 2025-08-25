# Switching Techniques (Deep, Exam‑ready Notes)

> Switched networks interconnect nodes (hosts, routers/switches) via links and forward traffic by **switching** decisions. “Switching” is a general concept used at multiple layers; in LANs, **Layer‑2 (Data Link Layer)** switches forward **frames** using MAC addresses, while at Layer‑3 routers forward **packets** using IP addresses. This note focuses on classic **Circuit Switching**, **Message Switching**, and **Packet Switching** (Datagram & Virtual‑Circuit styles), plus addressing and control.

---

## 1) Big Picture

* **Goal of switching:** Move data from a source to a destination through intermediate nodes efficiently.
* **Granularity:**

  * **Bit stream / circuit** (reserved resources) → *circuit switching*.
  * **Whole message** (store‑and‑forward the entire message) → *message switching*.
  * **Packets** (message split into smaller units) → *packet switching*.
* **Where it lives:**

  * **Data Link (L2):** LAN switches (Ethernet) learn/build MAC tables and switch frames within a broadcast domain.
  * **Network (L3):** Routers and packet/cell switches forward based on network‑layer addresses (IP) or labels (VCI/MPLS).

Key terms: **node/switch**, **link**, **frame/packet/message**, **throughput**, **latency** (propagation + transmission + processing + queuing), **QoS** (bandwidth, delay, jitter, loss).

---

## 2) Circuit Switching (CS)

**Idea:** Establish a **dedicated end‑to‑end path** (circuit) with reserved resources (bandwidth/time slots) before sending user data. Think classic telephone networks.

### Phases

1. **Connection setup (establishment):** Signaling messages reserve resources hop‑by‑hop. A circuit ID (time slot/frequency/wavelength or a local identifier) is chosen.
2. **Data transfer:** A continuous bit stream flows; intermediate switches forward based on the reserved circuit state. **No per‑packet addressing** in the data; the circuit context identifies the flow.
3. **Teardown:** Resources are explicitly released when either side ends the call.

### Characteristics

* **Connection‑oriented**, **resource reservation** per call (e.g., TDM/FDM/WDM).
* **Low and predictable delay** after setup (no queueing for bandwidth on that path).
* **In‑band vs out‑of‑band signaling:** Early systems carried signaling on the same channel; modern PSTN used SS7 out‑of‑band.
* **Blocking vs non‑blocking:** In blocking networks, calls can fail if no free path exists; non‑blocking designs guarantee a path if capacity exists.

### Pros

* **Minimal variable delay/jitter** → great for real‑time voice/video.
* **No addressing overhead per packet** once set up.
* **Strong QoS** by design due to dedicated resources.

### Cons

* **Setup latency** before any data flows.
* **Inefficient utilization** under bursty data (reserved capacity may sit idle).
* **Poor scalability/flexibility** vs packet networks; failures often tear down the call.

**Typical tech:** PSTN/ISDN, SONET/SDH, optical TDM, old leased lines.

---

## 3) Message Switching (MS)

**Idea:** Each message is treated as a **whole unit**. Intermediate nodes **store the entire message**, then forward it to the next hop (store‑and‑forward). No connection setup.

### Characteristics

* **Connectionless**; each message carries full **destination address**.
* **Potentially huge buffering** at each switch (must store whole messages).
* **High delay** and **variable latency** (especially for long messages).
* Can support **priority**, **scheduling**, **alternate routing**, and **retransmission** at switches.

### Pros

* **No setup/teardown overhead.**
* **Link utilization** can be good under heavy loads (links used on demand).
* Fits non‑real‑time, store‑and‑forward applications (historical telegraph, email relays).

### Cons

* **Long delays** (must receive the whole message before forwarding).
* **Large memory** requirements at switches; not suitable for real‑time.

**Status:** Mostly historical academically; ideas influenced modern store‑and‑forward systems.

---

## 4) Packet Switching (PS)

**Idea:** The source **splits the message into packets** (fixed or variable length). Switches forward packets **independently** (datagram) or along a **pre‑established path** (virtual circuit). Packets are stored just long enough to forward (store‑and‑forward per packet), enabling **pipelining** across multiple hops.

### Delay model (intuition)

* Per hop: **transmission** (L/R), **propagation**, **processing**, **queueing**.
* Across N hops: pipeline makes steady‑state throughput higher than message switching; initial packets experience store‑and‑forward per hop.

### 4.1 Datagram Packet Switching (Connectionless)

* **No reservation**, **no setup**. Each packet carries the **full destination network address**.
* **Routing table** at each router/switch decides next hop independently; packets may take **different paths** → **reordering** possible.
* **Best‑effort** delivery (loss, duplication possible) unless higher layers ensure reliability (e.g., TCP).

**Pros:**

* **Robust and scalable**; adapts to failures/congestion by rerouting.
* **Efficient under bursty traffic**; pay overhead only when sending.

**Cons:**

* **Variable delay/jitter**, possible **loss/reorder**.
* **Larger headers** (each packet carries addressing and control fields).

**Examples:** IP networks (the Internet), Ethernet within a LAN (frame switching at L2 with MAC addresses but conceptually connectionless forwarding).

### 4.2 Virtual‑Circuit Packet Switching (Connection‑oriented)

* **Hybrid of circuit & datagram ideas.**
* **Phases:**

  1. **Setup:** A signaling exchange chooses a path. Each switch creates a **forwarding entry** that maps *(incoming port, incoming VCI)* → *(outgoing port, new VCI)*.
  2. **Data transfer:** Packets carry a short **VC identifier (VCI)** instead of full destination address; switches use their **VC tables** (state) for fast lookup.
  3. **Teardown:** A teardown message releases the state/resources.
* **Addressing:**

  * **Global address** (e.g., network address of source/destination) is used **only during setup** to find a route.
  * **VCI** is **locally significant** (changes at each hop), identifies the circuit on that link.
* **QoS:** Can reserve bandwidth/buffers per VC; supports traffic engineering.

**Pros:**

* **Smaller data headers**, **in‑order delivery** on a single VC.
* **Can guarantee QoS** (if resources reserved) and reduce per‑packet processing.

**Cons:**

* **State in the network** (scaling, failure handling).
* **Setup/teardown overhead**; path failure may break the VC until re‑established.

**Examples:** X.25, Frame Relay (**DLCI** labels), ATM (**VPI/VCI** cells), and MPLS label‑switched paths (conceptually similar label switching at L2.5).

#### Virtual‑Circuit Tables (at intermediate switches)

A switch maintains entries like:

| In Port | In VCI | Out Port | Out VCI | State       |
| ------: | -----: | -------: | ------: | :---------- |
|       3 |     17 |        1 |       5 | Established |

* **Setup** fills these tables during the **call request/ack** handshake.
* **Teardown** removes the entries when a **disconnect message** is received.

---

## 5) Addressing Recap

* **Global (network) addressing:** Identifies **end hosts** (source/destination). Required for routing path discovery (e.g., IP addresses, NSAPs). Used in datagram forwarding and in **VC setup**.
* **Local identifiers:**

  * **VCI (Virtual Circuit Identifier):** Short label used **per link** to select the outgoing interface/label in VC networks. **Locally significant** only.
  * **Related terms:** **DLCI** (Frame Relay), **VPI/VCI** (ATM), **MPLS label**.
* **Data Link (L2) addressing:** **MAC addresses** for frame switching within a LAN (learned in a switch’s MAC table).

---

## 6) When is addressing needed?

* **Circuit Switching:** After setup, **no per‑packet addressing**; the circuit ID/time slot identifies the flow.
* **Message Switching:** **Full destination address** carried in each message.
* **Datagram PS:** **Full destination address** per packet.
* **VC PS:** **Global address** during setup; **VCI/label** per packet in data phase.

---

## 7) Quick Comparative Summary

| Feature                      | Circuit Switching               | Message Switching          | Packet Switching (Datagram)          | Packet Switching (VC)                           |
| ---------------------------- | ------------------------------- | -------------------------- | ------------------------------------ | ----------------------------------------------- |
| Connection Setup             | **Yes** (reserve resources)     | No                         | No                                   | **Yes** (install VC state)                      |
| Resource Allocation          | Dedicated (TDM/FDM)             | On demand                  | On demand                            | On demand / Reserved QoS                        |
| Unit forwarded               | Bit stream                      | Entire message             | Packet                               | Packet                                          |
| Per‑packet Addressing        | No                              | Destination in message     | Destination in every packet          | **VCI/label** only                              |
| Ordering                     | Preserved                       | Preserved                  | May reorder                          | Preserved within VC                             |
| Delay                        | Low after setup; minimal jitter | High (store whole message) | Variable (store‑and‑forward per hop) | Moderate; smaller headers; predictable with QoS |
| Reliability                  | High once set                   | Can buffer/retry           | Best‑effort; handled by end hosts    | Better control; can police/shape                |
| Efficiency under bursty data | Poor                            | Moderate                   | **High**                             | High                                            |
| Typical use                  | Telephony, leased lines         | Historical                 | Internet (IP)                        | ATM/Frame Relay/MPLS VPNs                       |

---

## 8) Worked Intuition (Delays)

Let a message of size **M** be split into packets of size **P** over **N** hops with link rate **R**.

* **Circuit Switching:** after setup delay **S**, data flows at reserved rate → total ≈ **S + M/R + propagation**.
* **Message Switching:** each hop stores the **whole M** → ≈ **N·(M/R) + propagation** (plus queueing).
* **Packet Switching:** first packet incurs store‑and‑forward per hop, but pipeline reduces total: ≈ **(M/R) + (N−1)·(P/R) + propagation** (plus queueing). Smaller **P** ⇒ better pipelining but more header overhead.

---

## 9) Control Messages & Teardown (VC Networks)

* **Setup (Call Request):** carries source/destination **global addresses** and QoS hints. Each switch selects an outgoing port and assigns a **local VCI**; it installs a table entry and forwards the request.
* **Connect (Ack):** travels back confirming the chosen VCIs; after this, data can flow with short VCIs.
* **Teardown:** sender/receiver transmits a **disconnect/teardown** control frame; each switch removes the VC entry and releases any reserved resources.

---

## 10) Security & Reliability Notes (Exam/Interview Friendly)

* **Datagram PS:** susceptible to **congestion**, **DoS**, **spoofing**; mitigated via ACLs, rate limiting, anti‑spoofing (uRPF), and L4/L7 controls.
* **VC/Label switching:** mis‑provisioned labels/VCIs can leak traffic; use control‑plane authentication and separation (e.g., MPLS VPN best practices).
* **Circuit:** predictable paths but single failures can drop the circuit; protection switching (1+1, 1:1) mitigates.

---

## 11) High‑Yield Exam Pointers

* Define each technique with **unit switched**, **setup**, **addressing**, **ordering**, **delay**.
* Contrast **datagram vs virtual circuit** clearly (setup, headers, QoS, reordering).
* Emphasize **VCI is locally significant**; **global addresses** used during **setup**.
* For circuit switching, list **phases** and note **no per‑packet addressing** during data.
* For message switching, stress **store‑entire‑message** and high delay.
* Draw a small table like §7 in an answer.

---

## 12) Mini Glossary

* **VC/VCI:** Virtual Circuit / Virtual Circuit Identifier (per‑link label).
* **DLCI:** Data‑Link Connection Identifier (Frame Relay label).
* **VPI/VCI:** ATM Virtual Path/Channel identifiers (hierarchical labels).
* **MPLS Label:** Short, fixed‑length label enabling label switching (VC‑like).
* **SS7:** Out‑of‑band signaling system for PSTN circuit setup/teardown.

---

### One‑page Recap

* **Circuit:** setup → reserved path → stream → teardown; low jitter; inefficient for bursts.
* **Message:** store whole message → forward; no setup; high delay.
* **Datagram:** per‑packet addressing; flexible, scalable; variable delay.
* **VC:** setup tables with VCIs; small headers; possible QoS; teardown frees state.
