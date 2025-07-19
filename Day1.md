
📘 Computer Networking - Comprehensive Notes

🔹 What is a Computer Network?
--------------------------------
A computer network is a collection of interconnected devices (computers, servers, switches, etc.) that can:
- Communicate and share resources like files, printers, and internet connections
- Be linked via cables, fiber optics, or wireless media
- Share data using standard protocols such as TCP/IP

🔹 What is the Internet?
-------------------------
The Internet is a global network of millions of devices connected together using TCP/IP protocols.
- It is a "network of networks"
- Supports services like: Email, Web Browsing, File Transfer, VoIP, Streaming
- Relies on components like: ISPs, routers, servers, fiber optic cables

🔹 What is a Client?
---------------------
A client is any device or software that requests services/data from a server.
Example: A web browser requesting a webpage from a web server.

🔹 What is a Server?
---------------------
A server is a powerful computer or program that provides services to clients.
Example: Google’s web server sending search results to your browser.

🔹 Network Devices and Their Types
-----------------------------------
| Device        | Layer     | Function                                 | Smartness    | Use Case                    |
|---------------|-----------|------------------------------------------|--------------|-----------------------------|
| Router        | Layer 3   | Connects different networks (LAN ↔ WAN)  | High         | Connect LAN to Internet     |
| Switch        | Layer 2   | Connects devices within the same LAN     | High         | Office or lab connectivity  |
| Hub           | Layer 1   | Broadcasts data to all connected devices | Low          | Obsolete technology         |
| Access Point  | Layer 2   | Provides wireless connectivity (Wi-Fi)   | Medium       | Wireless access in buildings|
| Modem         | Layer 1   | Converts digital ↔ analog signals        | Low          | DSL/Fiber internet access   |
| Firewall      | L3–L7     | Filters traffic based on security rules  | Very High    | Network security            |
| Gateway       | All       | Protocol conversion between networks     | Very High    | VoIP, SMS, Cloud services   |
| Bridge        | Layer 2   | Connects two LAN segments                | Medium       | Legacy LAN expansion        |

🔹 Key Concepts
----------------
📌 ARP (Address Resolution Protocol)
- Maps IP address to MAC address within a LAN
- Operates at Layer 2 (Data Link Layer)
- Example: "Who has IP 192.168.1.1? Tell me your MAC address"

📌 NAT (Network Address Translation)
- Translates private IPs ↔ public IPs
- Used by routers to allow multiple devices to share one public IP
- PAT (Port Address Translation) is used to track individual internal devices

🔹 Client-Server Model vs Peer-to-Peer
---------------------------------------
| Model           | Description                          | Example              |
|-----------------|--------------------------------------|----------------------|
| Client-Server   | Clients request, servers respond     | Web browsing, Email |
| Peer-to-Peer    | All nodes act as client and server   | Torrents, File Share|

🔹 Network Topologies (Basic)
------------------------------
- Bus: Single backbone cable, all devices tap into it
- Star: Devices connected to a central switch or hub
- Ring: Devices connected in a closed loop
- Mesh: Devices interconnected with every other device
- Hybrid: Combination of two or more topologies

🔹 Types of Networks
---------------------
- LAN (Local Area Network): Small area like a home, office, lab
- MAN (Metropolitan Area Network): Medium area like a city or campus
- WAN (Wide Area Network): Large area; connects countries (e.g., Internet)
