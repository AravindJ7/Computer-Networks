
# 🧠 Network Interface Card (NIC) – Data Link Layer Notes

## 🔌 What is a NIC?

A **NIC (Network Interface Card)** is a hardware component that allows a device to connect to a network. It functions at both the **Data Link Layer (Layer 2)** and **Physical Layer (Layer 1)** of the OSI model.

---

## 🧩 Key Functions of NIC

- 🛂 **MAC Addressing**: Assigns a unique hardware address (MAC) for network identification.
- 📤 **Framing & Transmission**: Converts data into network frames for transmission.
- 📥 **Reception**: Receives incoming frames and sends them to the OS.
- 📶 **Signal Conversion**: Translates digital data to/from electrical, optical, or wireless signals.
- 🔌 **Media Access Control**: Controls how devices access the shared transmission medium.

---

## ⚙️ Components of a NIC

| Component           | Function |
|--------------------|----------|
| MAC Controller      | Controls access to medium & manages MAC address |
| Buffer Memory       | Temporarily holds incoming/outgoing data |
| Transceiver         | Converts between digital signals and physical medium |
| ROM (MAC Address)   | Stores the unique MAC address |
| Physical Connector  | RJ-45 port, antenna, etc. for network connection |

---

## 🌐 Types of NICs

| Type               | Description                         |
|--------------------|-------------------------------------|
| Ethernet NIC       | For wired LAN (RJ-45)               |
| Wi-Fi NIC          | For wireless LAN (built-in/USB)     |
| Bluetooth NIC      | For PAN (often in Wi-Fi combo)      |
| Fiber NIC          | For high-speed fiber optic networks |
| Virtual NIC (vNIC) | Software-based for VMs/VPNs         |

---

## 💻 Example: NICs in a Laptop

| Interface    | NIC Present | Notes                             |
|--------------|-------------|-----------------------------------|
| Ethernet     | ✅ Yes      | Built-in RJ-45 or USB adapter     |
| Wi-Fi        | ✅ Yes      | Internal or USB Wi-Fi adapter     |
| Bluetooth    | ✅ Yes      | Often combined with Wi-Fi card    |
| Virtual NIC  | ✅ Yes      | Used by VMs or VPNs               |
| USB Tethering| ✅ Yes      | Mobile phone creates temp NIC     |

---

## 🔍 How to Check Active NICs (Linux)

```bash
ip link show
```
Or:
```bash
ifconfig -a
```

Common interfaces:
- `eth0` → Ethernet
- `wlan0` → Wi-Fi
- `lo` → Loopback
- `bt0` → Bluetooth (optional)

---

## 🛡️ NIC Security Considerations

- **MAC Address Filtering**
- **Promiscuous Mode** (used for packet sniffing)
- **Disabling unused NICs**
- **Monitoring virtual NICs** for unauthorized access

---

## 🧠 Summary Table

| Topic           | Details |
|----------------|---------|
| OSI Layers      | Layer 2 (Data Link), Layer 1 (Physical) |
| MAC Address     | Unique 48-bit identifier burned into NIC |
| Multiple NICs?  | Yes – Ethernet, Wi-Fi, Bluetooth, etc. |
| Virtual NICs    | Created by software for VMs, VPNs       |

---

## 🔄 NIC vs IP Address

| Feature           | MAC Address (NIC)       | IP Address (Logical)     |
|------------------|-------------------------|---------------------------|
| Layer            | Data Link (Layer 2)      | Network Layer (Layer 3)   |
| Scope            | Local                    | Global                    |
| Assigned by      | Manufacturer             | Admin/DHCP/ISP            |
| Permanent?       | Usually fixed            | Dynamic or static         |

---
