## 🖼️ Network Diagram
![Network Diagram](network-dagram.png)

Inter‑VLAN Routing (ROAS) + Multi‑Switch VLAN Network
📌 Project Overview
This project demonstrates a full Layer 2 + Layer 3 enterprise‑style network using:

VLAN segmentation

802.1Q trunking across multiple switches

Router‑on‑a‑Stick (ROAS) for inter‑VLAN routing

Multi‑switch VLAN propagation

End‑to‑end connectivity testing

This is a core CCNA‑level design and reflects real production network behaviour.

🖥️ Topology Summary
Code
PCs → SW1 (Access) → SW2 (Distribution) → Router (ROAS)
VLANs:

VLAN 10 – Marketing

VLAN 20 – Sales

Subnets:

VLAN 10 → 192.168.1.0/24

VLAN 20 → 192.168.5.0/24

Router Sub‑interfaces:

g0/0.10 → 192.168.1.30

g0/0.20 → 192.168.5.30

📡 Router Configuration (ROAS)
bash
interface GigabitEthernet0/0
 description Connected to SW2 trunk
 no ip address
 no shutdown

interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.1.30 255.255.255.0

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.5.30 255.255.255.0
🔀 SW2 – Distribution Switch
Trunk to Router
bash
interface GigabitEthernet1/0/7
 switchport mode trunk
 switchport trunk allowed vlan 10,20
Trunk to SW1
bash
interface GigabitEthernet1/0/12
 switchport mode trunk
 switchport trunk allowed vlan 10,20
VLANs
bash
vlan 10
 name Marketing
vlan 20
 name Sales
🔌 SW1 – Access Switch
Trunk to SW2
bash
interface GigabitEthernet1/0/11
 switchport mode trunk
 switchport trunk allowed vlan 10,20
Access Ports
bash
interface GigabitEthernet1/0/1
 switchport mode access
 switchport access vlan 10

interface GigabitEthernet1/0/3
 switchport mode access
 switchport access vlan 20
💻 PC IP Configuration
PC in VLAN 10
Code
IP Address: 192.168.1.2
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.30
PC in VLAN 20
Code
IP Address: 192.168.5.2
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.5.30
🧪 Connectivity Testing
Ping Tests
From VLAN 10 PC:

Code
ping 192.168.1.30   ✔️ (gateway)
ping 192.168.5.30   ✔️ (inter‑VLAN routing)
ping 192.168.5.2    ✔️ (PC in VLAN 20)
Result:  
Inter‑VLAN routing successful across multiple switches.

🛠️ Skills Demonstrated
VLAN design & segmentation

802.1Q trunking

Multi‑switch VLAN propagation

Router‑on‑a‑Stick (ROAS)

Subnetting & gateway planning

End‑to‑end troubleshooting (ARP, trunks, VLAN membership, routing)

Enterprise‑style Layer 2 + Layer 3 design

📘 Lessons Learned
All switches in the path must carry the same VLANs

ROAS requires sub‑interfaces + dot1Q tagging

Access ports must match the correct VLAN

PCs must have correct IP + gateway

First ping often fails due to ARP (normal)

🚀 Future Improvements
Add VLAN 30 and extend routing

Implement DHCP relay

Add ACLs to control inter‑VLAN traffic

Convert design to a Layer 3 switch (SVIs)

Add OSPF between multiple routers
