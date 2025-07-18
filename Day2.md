
📘 Networking Basics - Study Notes

🌐 What is the Internet?
-------------------------
The Internet is a global network of interconnected computers and devices that communicate using standardized protocols.
It allows users to access and share information, services, and communicate with others across the world.

🖥️ What is a Computer Network?
-------------------------------
A computer network is a group of interconnected devices (computers, printers, phones, etc.) that can share data and resources.
Networks can be:
- LAN (Local Area Network)
- WAN (Wide Area Network)
- MAN (Metropolitan Area Network)
- PAN (Personal Area Network)

👨‍💻 What is a Client?
------------------------
A client is a device or software that requests services or data from another device (usually a server) over a network.
Example: A web browser on your PC requesting a website.

🗃️ What is a Server?
----------------------
A server is a computer or software that provides services or data to clients.
Example: A web server hosting a website.

📨 What is Internet Protocol (IP)?
----------------------------------
Internet Protocol (IP) is a set of rules that allows devices to communicate over the internet or any IP-based network.
It is responsible for:
- Assigning IP addresses
- Routing packets
- Ensuring delivery of data

🧠 IP Analogy:
If the internet is like a postal system,
- IP is the addressing and routing rules.
- Every device has a unique IP address.

✉️ What does IP do?
--------------------
1. Breaks data into smaller parts called packets.
2. Adds sender and receiver IP addresses to each packet.
3. Routes packets through routers.
4. Delivers them to the destination device.

📦 IP Packet Structure:
------------------------
| Field             | Purpose                           |
|------------------|-----------------------------------|
| Source IP         | Who is sending the data           |
| Destination IP    | Who should receive it             |
| Payload           | The actual data (message, etc.)   |
| TTL (Time to Live)| Prevents packets from looping     |

🔢 IP Versions:
---------------
| Version | Name         | Address Example                 | Details                              |
|---------|--------------|----------------------------------|--------------------------------------|
| IPv4    | Internet Protocol v4 | 192.168.1.1         | 32-bit address (4.3 billion addresses) |
| IPv6    | Internet Protocol v6 | 2001:0db8::7334      | 128-bit address (trillions of devices) |

🌍 Real Life Example:
----------------------
When you visit www.google.com:
1. Your browser asks DNS for Google’s IP address.
2. Your device sends a request packet with:
   - Source IP (your device)
   - Destination IP (Google)
3. Routers pass the packet along to Google’s server.
4. Google responds and the packet is sent back to you.

📌 Summary:
-----------
- The Internet is a vast global network.
- A client requests data; a server provides it.
- IP manages addressing and delivery of data.
- IPv4 and IPv6 are the two versions of IP used to identify devices uniquely.
