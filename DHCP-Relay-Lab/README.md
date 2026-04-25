This project demonstrates a full enterprise‑style Layer 2 + Layer 3 network using:

VLAN segmentation

Multi‑switch 802.1Q trunking

Router‑on‑a‑Stick (ROAS)

Centralised DHCP server

DHCP relay using ip helper-address

End‑to‑end connectivity across VLANs and switches

This design reflects real production networks where DHCP servers live on a separate server network.

🖥️ Topology Summary
Devices
PCs → SW1 (Access) → SW2 (Distribution) → Router (ROAS)

DHCP Server connected to Router g0/1

VLANs
VLAN 10 – Marketing

VLAN 20 – Sales

Subnets
VLAN 10 → 192.168.1.0/24

VLAN 20 → 192.168.5.0/24

Router Sub‑interfaces
g0/0.10 → 192.168.1.30

g0/0.20 → 192.168.5.30

DHCP Server Network
Server IP → 10.10.10.5

Router g0/1 → 10.10.10.1

📡 Router Configuration (ROAS + DHCP Relay)
bash
interface GigabitEthernet0/0
 description Connected to SW2 trunk
 no ip address
 no shutdown

interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.1.30 255.255.255.0
 ip helper-address 10.10.10.5

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.5.30 255.255.255.0
 ip helper-address 10.10.10.5

interface GigabitEthernet0/1
 description Connected to DHCP Server
 ip address 10.10.10.1 255.255.255.0
 no shutdown
✔ Why this works
g0/0 is a trunk carrying VLAN 10 & 20

Sub‑interfaces provide inter‑VLAN routing

ip helper-address forwards DHCP broadcasts to the server

g0/1 provides L3 connectivity to the DHCP server

🖥️ DHCP Server Configuration (Packet Tracer)
Server → Desktop → IP Configuration
IP Address: 10.10.10.5

Subnet Mask: 255.255.255.0

Default Gateway: 10.10.10.1

Server → Services → DHCP
Pool for VLAN 10
Pool Name: VLAN10

Default Gateway: 192.168.1.30

DNS Server: 8.8.8.8

Start IP: 192.168.1.10

Subnet Mask: 255.255.255.0

Maximum Users: 50

Add

Pool for VLAN 20
Pool Name: VLAN20

Default Gateway: 192.168.5.30

DNS Server: 8.8.8.8

Start IP: 192.168.5.10

Subnet Mask: 255.255.255.0

Maximum Users: 50

Add

✔ Why this works
DHCP server provides IPs for both VLANs

Router relays DHCP requests from VLANs to server

Server gateway points back to router g0/1

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
💻 PC IP Configuration (DHCP)
PC in VLAN 10
Set to DHCP

Expected IP: 192.168.1.10–59

Gateway: 192.168.1.30

PC in VLAN 20
Set to DHCP

Expected IP: 192.168.5.10–59

Gateway: 192.168.5.30

🧪 Connectivity Testing
From VLAN 10 PC
Code
ping 192.168.1.30   ✔️ (gateway)
ping 192.168.5.30   ✔️ (router subinterface)
ping 192.168.5.2    ✔️ (PC in VLAN 20)
DHCP Tests
PC receives correct IP from server

Gateway matches router subinterface

DNS = 8.8.8.8

✔ Result
Inter‑VLAN routing works

DHCP relay works

Multi‑switch VLAN propagation works

🛠️ Skills Demonstrated
VLAN design & segmentation

Multi‑switch trunking

Router‑on‑a‑Stick

DHCP server configuration

DHCP relay (ip helper-address)

Subnetting & gateway planning

End‑to‑end troubleshooting

Enterprise‑style L2 + L3 design

🚀 Future Improvements
Add VLAN 30 + DHCP pool

Implement ACLs to restrict inter‑VLAN traffic

Convert to Layer 3 switch (SVIs)

Add OSPF between multiple routers

Add redundancy (HSRP, EtherChannel)
