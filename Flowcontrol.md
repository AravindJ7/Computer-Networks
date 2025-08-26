# Flow Control & ARQ Protocols — In‑Depth Notes

> Clean, exam‑ready notes covering **Noiseless** and **Noisy** channel protocols: **Simplest**, **Stop‑and‑Wait**, **Stop‑and‑Wait ARQ**, **Sliding Window**, **Go‑Back‑N ARQ**, and **Selective Repeat ARQ**—with diagrams, examples, and quick formulas.

---

## 1) Big Picture

- **Flow control**: prevents a fast **sender** from overwhelming a slow **receiver**.
- **Error control**: detects and corrects transmission errors (loss, corruption, duplication, reordering).
- **ARQ (Automatic Repeat reQuest)**: receiver/sender cooperate so that **lost/corrupted frames are retransmitted**.

**Channel assumptions**  
- **Noiseless channel**: No errors, no losses. (Unrealistic, but good for understanding.)
- **Noisy channel**: Errors/losses possible → need ARQ + timers + sequence numbers.

---

## 2) Core Building Blocks & Terminology

- **Frame**: data unit at DLL; **Segment** at Transport layer (concepts apply similarly).
- **ACK**: acknowledgment that a frame (or sequence of frames) arrived correctly.
- **NAK/REJ**: negative acknowledgment (optional; not in every design).
- **Sequence number (seq)**: unique number mod \(2^m\) (with **m** bits) to identify frames and detect duplicates.
- **Timer**: started when a frame is sent; if it **times out** before ACK, sender **retransmits**.
- **Window**: range of seq numbers sender/receiver are **willing to use/accept** at any time (pipelining).
- **RTT**: round‑trip time = **propagation (one‑way)** × 2 + **processing delays**.
- **Transmission time** of a frame: \( T_{tx} = \frac{\text{frame size (bits)}}{\text{link rate (bps)}} \).
- **a (bandwidth‑delay product factor)**: \( a = \frac{T_{prop}}{T_{tx}} \). Heavily influences utilization.

---

## 3) Noiseless Channel Protocols

### 3.1 Simplest Protocol (Unidirectional, No Flow Control)
- Sender just **sends frames continuously**; receiver always ready (infinite buffer).
- No ACKs, no back‑pressure; **not practical**, but forms a baseline model.

### 3.2 Stop‑and‑Wait (Noiseless)
- **One frame at a time**: sender transmits a frame, then **waits** for receiver to confirm (implicit, since channel is noiseless in this model).
- Ensures receiver is not overwhelmed, but **no errors are considered** here.

**Key idea**: serialization of frames → **no pipelining** → often **low link utilization** over long paths.

---

## 4) Noisy Channel Protocols (with ARQ)

### 4.1 Stop‑and‑Wait ARQ
- **Rule**: *Send one frame, wait for its ACK before sending the next.*
- **Timer**: If ACK not received before timeout → **retransmit the same frame**.
- **Sequence numbers**: Usually **1 bit** (0/1, i.e., modulo‑2). Receiver uses it to **discard duplicates**.
- **ACK variants**: ACK0 / ACK1 indicate which sequence was received.

**What can go wrong & how it’s handled**
- **Frame lost/corrupted** → sender times out → retransmits.
- **ACK lost/corrupted** → sender times out → retransmits (receiver discards duplicate by seq number).
- **Duplicate frames** → receiver detects via seq and discards, but still ACKs the last correctly received seq.

**Pros**
- Very **simple** to implement; robust to loss/corruption.
  
**Cons**
- **Inefficient** on long‑delay or high‑bandwidth links (no pipelining).

**Utilization (idealized, no errors)**  
Let \( a = \frac{T_{prop}}{T_{tx}} \). Then approximate sender utilization  
\[
U_{\text{S\&W}} \approx \frac{1}{1 + 2a}
\]
(You spend one \(T_{tx}\) sending and roughly \(2T_{prop}\) waiting.)

---

### 4.2 Sliding Window ARQ — General Idea
- **Pipeline multiple frames** without waiting for individual ACKs.
- **Sender window (size W\_s)**: up to \(W_s\) **unacknowledged** frames may be **in flight**.
- **Receiver window (size W\_r)**: range of seq the receiver is **willing to accept** at a time.
- **ACKs** can be **cumulative** (acknowledge up to N) or **selective** (ack particular seq). 
- **Piggybacking**: a data frame can carry an ACK for the opposite direction (duplex links).

**Window sliding intuition**
```
[  left .................. right ]
^sent&ACKed      ^sent, awaiting ACK      ^not yet sent
window slides right as ACKs arrive
```

Two classic ARQ variants under Sliding Window:

#### (A) Go‑Back‑N (GBN) ARQ
- **Receiver rule**: accept **only the next expected** sequence number; **drop** out‑of‑order frames.
- **ACKs are cumulative**: ACK(k) means “received everything up to k correctly”.
- **Sender timers**: commonly **one timer** for the **oldest unACKed** frame.
- **On timeout**: **retransmit** the **timed‑out frame and all subsequent frames** in the window (“go back”).
- **Seq space/window size**: if **m‑bit** seq numbers → **window size \(W_s \le 2^m - 1\)**.

**GBN example (mod‑8, \(W_s=4\))**
```
Send: 0 1 2 3 | 4 5 ...
Assume frame 1 lost.
Receiver only accepts 0, then expects 1; drops 2,3 as out-of-order.
ACKs seen by sender stay at ACK0. Timer for 1 expires → retransmit 1,2,3.
```

**Pros**: simpler receiver (no buffering of out‑of‑order).  
**Cons**: **wasted retransmissions** when a single frame is lost.

#### (B) Selective Repeat (SR) ARQ
- **Receiver rule**: accept **out‑of‑order** frames and **buffer** them.
- **Selective ACKs (SACK)**: ACK each frame **individually**; receiver may also send **NAKs** for missing ones.
- **Sender timers**: typically **per‑frame** timers; on timeout, **retransmit only missing frames**.
- **Seq space/window size**: with **m‑bit** seq numbers, to avoid ambiguity, require  
  \[
W_s,\, W_r \le 2^{m-1}
\]
(i.e., **window size ≤ half of sequence space**).

**SR example (mod‑8, \(W_s=W_r=4\))**
```
Send: 0 1 2 3 | 4 5 ...
Assume 1 lost; 2 and 3 arrive and are buffered. Receiver SACKs 2,3 and (optionally) NAKs 1.
Sender retransmits only frame 1. Once 1 arrives, receiver delivers 1,2,3 in order.
```

**Pros**: best bandwidth efficiency on noisy links (no needless retransmission).  
**Cons**: more complex (receiver buffering, per‑frame state).

---

## 5) Visual Window Intuition

### 5.1 Sender Window (conceptual)
```
Sequence numbers (mod M):  ... 5 6 7 0 1 2 3 4 5 6 ...
                           [--- window size W_s --->]

Left edge: oldest unACKed (Sf)
Right edge: next seq available to send (Sn = Sf + W_s)
As ACKs arrive, left edge moves right → window slides.
```

### 5.2 Receiver Behavior
- **GBN**: keeps a **single “expected”** number; discards others.
- **SR**: maintains a **bitmap/buffer** for all seq within \(W_r\) and delivers in order when gaps fill.

---

## 6) Quick Math & Performance Intuition

- **Stop‑and‑Wait (no errors)**: \( U \approx \frac{1}{1+2a} \).  
  - For long links where \(a \gg 1\), utilization is **very low**.
- **Sliding Window (no errors)**: roughly \( U \lesssim \min\left(1, \frac{W_s}{1+2a}\right) \).  
  - By increasing \(W_s\), you can **fill the pipe** (bandwidth‑delay product) and approach 100% utilization.
- **With errors**: 
  - **GBN** wastes more bandwidth** when a loss occurs (retransmits a burst).
  - **SR** localizes retransmissions, so it **outperforms GBN** at higher loss rates.

**Tiny Example**  
- Link: **1 Mb/s**, Frame: **1 KiB = 8192 bits** → \(T_{tx} = 8.192\,ms\).  
- One‑way propagation \(T_{prop} = 50\,ms\) → \(a \approx 50/8.192 \approx 6.1\).  
- **Stop‑and‑Wait** utilization \( \approx \frac{1}{1+2a} \approx \frac{1}{1+12.2} \approx 7.6\% \).  
- With **Sliding Window**, choose \(W_s \approx 2a \approx 12\) to keep the pipe full.

---

## 7) Compact Pseudocode (Exam‑Ready)

### 7.1 Stop‑and‑Wait ARQ (sender)
```text
seq := 0
loop:
  wait for data
  send(DATA, seq); start_timer()
  wait until (ACK with ack_seq == seq) OR (timeout)
  if timeout:            // ACK lost or frame lost
    resend(DATA, seq); restart_timer(); goto wait
  else:                  // correct ACK received
    stop_timer(); seq := 1 - seq; // modulo-2
```

### 7.2 Stop‑and‑Wait ARQ (receiver)
```text
expected := 0
upon frame(DATA, s) arrival:
  if s == expected and frame_ok:
    deliver(DATA); send(ACK, s); expected := 1 - expected
  else:
    // duplicate or corrupted; (re)send ACK for last in-order seq
    send(ACK, 1 - expected)
```

### 7.3 Go‑Back‑N ARQ (sender)
```text
Sf := 0                 // first unACKed
Sn := 0                 // next seq to send
W  := W_s               // window size
start no timer

when data available and Sn < Sf + W:
  send(DATA, Sn); if Sf == Sn: start_timer()
  Sn := Sn + 1

upon ACK(k):            // cumulative: ack up to k (inclusive)
  Sf := k + 1
  if Sf == Sn: stop_timer() else restart_timer()

upon timeout:
  // resend from oldest unACKed
  for i in [Sf .. Sn-1]: resend(DATA, i)
  restart_timer()
```

### 7.4 Go‑Back‑N ARQ (receiver)
```text
expected := 0
upon frame(DATA, s):
  if s == expected and frame_ok:
    deliver(DATA); expected := expected + 1
    send(ACK, s)     // cumulative ACK for s
  else:
    // out-of-order or corrupted
    send(ACK, expected-1)  // repeat last cumulative ACK
```

### 7.5 Selective Repeat ARQ (sender)
```text
Sf := 0; Sn := 0; W := W_s
for all seq in [0..M-1]: timer[seq] := stopped

when data available and Sn < Sf + W:
  send(DATA, Sn); start timer[Sn]
  Sn := Sn + 1

upon SACK(k):
  stop timer[k]
  mark k as ACKed
  while Sf is ACKed: Sf := Sf + 1  // slide left edge

upon timeout for seq t:
  resend(DATA, t); restart timer[t]
```

### 7.6 Selective Repeat ARQ (receiver)
```text
Wr := W_r
maintain buffer for seq in receiver window
expected_base := 0

upon frame(DATA, s):
  if s within receiver window and frame_ok:
    buffer[s] := DATA; send SACK(s)
    while buffer[expected_base] exists:
      deliver(buffer[expected_base]); remove it
      expected_base := expected_base + 1
  else if corrupted:
    // optionally NAK expected_base to speed recovery
```

---

## 8) Differences — Quick Table

| Feature | Stop‑and‑Wait ARQ | Go‑Back‑N ARQ | Selective Repeat ARQ |
|---|---|---|---|
| Pipelining | ❌ No | ✅ Yes | ✅ Yes |
| Receiver buffers out‑of‑order | ❌ | ❌ | ✅ |
| ACK type | Per‑frame (seq 0/1) | **Cumulative** | **Selective (SACK)** (+optional NAK) |
| Timers | 1 per outstanding (effectively 1) | Usually **1** (oldest) | **Per‑frame** |
| Retransmission scope | Single frame | From first missing onward | Only missing frames |
| Complexity | **Low** | **Medium** | **High** |
| Best use | Short delay links | Moderate loss, simpler impl. | Higher loss or long fat links |

---

## 9) Common Exam Nuggets (Don’t Miss)

- Stop‑and‑Wait ARQ uses **mod‑2** seq numbers; duplicates detected via seq + ACK mirroring.
- **GBN**: window size with **m‑bit seq** ⇒ \(W_s \le 2^m - 1\).  
- **SR**: to avoid ambiguity, require \(W_s, W_r \le 2^{m-1}\).
- **Cumulative ACK**: a single ACK confirms receipt of **all prior frames** (GBN, TCP).  
- **Selective ACK (SACK)**: acknowledges **specific** frames (SR; TCP has SACK option).
- **Piggybacking** reduces overhead by carrying ACKs inside data frames in the reverse direction.
- **Throughput tuning**: pick \(W_s\) near the **bandwidth‑delay product** to “fill the pipe”.

---

## 10) Mini Glossary

- **BDP (Bandwidth‑Delay Product)**: “amount of data the link can hold”. Guides window sizing.
- **In‑order delivery**: app expects data in sequence; SR buffers out‑of‑order until gaps fill.
- **Duplicate ACK**: repeated ACK for the same seq (signals a gap to the sender).
- **Timeout**: absence of expected ACK within timer duration → triggers retransmission.
- **Retransmission**: resend lost/corrupted data to ensure reliability.

---

## 11) One‑Slide Summary (Memory Hook)

- **S&W ARQ**: 1 frame, wait ACK, **simple but slow**.  
- **GBN**: pipeline; on loss → **resend this + all after it** (cumulative ACKs, no OOO buffering).  
- **SR**: pipeline + **retransmit only what’s missing** (selective ACKs, receiver buffers OOO).  
- **Window size rules**: GBN \(W \le 2^m-1\); SR \(W \le 2^{m-1}\).  
- **Utilization**: S&W \( \sim \frac{1}{1+2a} \) → small for big \(a\); **increase W** to fill the pipe.
```