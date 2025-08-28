# 📘 Networking Devices – Notes

Networking devices are hardware components used to connect computers, printers, phones, and other devices in a network. They operate across different **OSI model layers** and have specific purposes.

---

## 🔹 1. Hub
- **Layer**: Physical Layer (Layer 1)
- **Definition**: A central device that connects multiple computers in a LAN. It broadcasts incoming signals to all ports.
- **Types**:
  - **Passive Hub**: Forwards signals without amplification.
  - **Active Hub**: Amplifies and regenerates signals.
  - **Intelligent Hub**: Adds basic management functions.
- **Limitations**: Shared bandwidth, collisions, no traffic filtering.

---

## 🔹 2. Switch
- **Layer**: Data Link Layer (Layer 2), sometimes Network Layer (Layer 3).
- **Definition**: Connects devices in a LAN and forwards data only to the specific destination device using MAC addresses.
- **Features**:
  - Maintains a **MAC Address Table**.
  - Supports full-duplex communication.
  - Reduces collisions (each port = collision domain).
- **Types**:
  - Unmanaged Switch (plug & play)
  - Managed Switch (configurable, supports VLANs)
  - Layer 3 Switch (routing capabilities)

---

## 🔹 3. Router
- **Layer**: Network Layer (Layer 3).
- **Definition**: Connects multiple networks (e.g., home LAN to the Internet) and forwards data packets based on IP addresses.
- **Functions**:
  - Assigns IP addresses (via DHCP).
  - Performs NAT (Network Address Translation).
  - Routes packets between networks.

---

## 🔹 4. Modem
- **Layer**: Works between Physical & Data Link.
- **Definition**: Converts **digital signals ↔ analog signals** for transmission over telephone, cable, or fiber lines.
- **Types**:
  - Dial-up Modem
  - DSL Modem
  - Cable Modem
  - Fiber Modem (ONT)
  - Wireless Modem (4G/5G dongles)

---

## 🔹 5. Gateway
- **Layer**: Multiple layers (mainly Network & above).
- **Definition**: A "translator" that connects two networks using different protocols.
- **Functions**:
  - Protocol conversion
  - Default gateway: Path from LAN to external networks
- **Examples**:
  - Email Gateway (SMTP ↔ X.400)
  - VoIP Gateway (Telephone ↔ IP)
  - IoT Gateway (Zigbee ↔ Internet)

---

## 🔹 6. Access Point (AP)
- **Layer**: Data Link Layer (Layer 2).
- **Definition**: Extends a wired network into a **wireless LAN (Wi-Fi)**.
- **Functions**:
  - Provides Wi-Fi connectivity.
  - Acts as a bridge between wired and wireless networks.
  - Enables roaming in enterprise networks.
- **Types**:
  - Standalone AP
  - Controller-based AP
  - Repeater/Extender AP
  - Cloud-managed AP

---

## 🔹 7. Bridge
- **Layer**: Data Link Layer (Layer 2).
- **Definition**: Connects and filters traffic between two LAN segments.
- **Functions**:
  - Uses MAC addresses to forward/filter traffic.
  - Reduces network congestion.
- **Types**:
  - Transparent Bridge
  - Source Routing Bridge
  - Translational Bridge

---

## 🔹 8. Repeater
- **Layer**: Physical Layer (Layer 1).
- **Definition**: Regenerates and amplifies weak signals to extend network distance.
- **Example**: Ethernet repeaters, Wi-Fi range extenders.

---

## 🔹 9. Firewall
- **Layer**: Network/Transport (Layer 3/4), sometimes higher.
- **Definition**: Controls and filters traffic entering or leaving a network based on security rules.
- **Types**:
  - Hardware Firewall
  - Software Firewall
- **Functions**:
  - Blocks unauthorized access.
  - Allows safe traffic.

---

## 🔹 10. Proxy Server
- **Layer**: Application Layer.
- **Definition**: An intermediary between client and Internet.
- **Functions**:
  - Hides client identity (anonymity).
  - Caches web pages for speed.
  - Filters web content.

---

## 🔹 11. Load Balancer
- **Layer**: Network & Application Layers.
- **Definition**: Distributes traffic across multiple servers to balance workload.
- **Functions**:
  - Improves reliability and availability.
  - Prevents server overload.

---

## 🔹 12. Network Interface Card (NIC)
- **Layer**: Data Link Layer (Layer 2).
- **Definition**: Hardware that enables a device to connect to a network.
- **Types**:
  - Wired (Ethernet card)
  - Wireless (Wi-Fi adapter)

---

## 🔹 13. Access Server
- **Definition**: Provides remote access to a LAN via dial-up or VPN.
- **Use Case**: ISPs and corporate networks.

---

## 🔹 14. VoIP Devices
- **Definition**: Devices that enable voice calls over IP networks.
- **Examples**: IP phones, VoIP gateways.

---

## 🔹 15. IDS / IPS
- **IDS (Intrusion Detection System)**: Monitors network for malicious activity.
- **IPS (Intrusion Prevention System)**: Actively blocks suspicious traffic.
- **Example Tools**: Snort (IDS), Cisco Firepower (IPS).

---

## 🔹 16. CDN Edge Servers
- **Definition**: Distributed servers placed globally to deliver content closer to users.
- **Examples**: Cloudflare, Akamai.

---

## 🔹 17. Wireless Controller (WLC)
- **Definition**: Manages multiple Access Points in large networks.
- **Functions**:
  - Centralized authentication
  - Roaming support
  - Monitoring & control

---

# ✅ Summary
- **Basic Devices**: Hub, Switch, Router, Modem, Bridge, Repeater, Access Point.
- **Advanced Devices**: Firewall, Proxy, Load Balancer, IDS/IPS, Gateway, WLC, CDN.
