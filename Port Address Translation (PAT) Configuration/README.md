Cisco CCNA Lab: Port Address Translation (PAT)
This repository demonstrates how to configure Port Address Translation (PAT) on Cisco routers using two different methods: a Dynamic IP Pool and a WAN Interface IP.

Markdown
![Network Topology]((PAT)topology.png)

1. R1: PAT with a Public IP Pool
Translates the internal network (172.16.0.0/16) to a specific pool of public IP addresses. When one IP exhausts its ports, it automatically uses the next IP in the pool.

Cisco CLI
enable
configure terminal

access-list 1 permit 172.16.0.0 0.0.255.255
ip nat pool PAT_POOL 209.165.200.233 209.165.200.234 netmask 255.255.255.252
ip nat inside source list 1 pool PAT_POOL overload

interface serial 0/1/0
 ip nat outside
interface gigabitEthernet 0/0/0
 ip nat inside
interface gigabitEthernet 0/0/1
 ip nat inside
2. R2: PAT using Interface IP
Translates the internal network (172.17.0.0/16) directly to the single public IP assigned to the outbound WAN interface. Ideal for SOHO environments.

Cisco CLI
enable
configure terminal

access-list 2 permit 172.17.0.0 0.0.255.255
ip nat inside source list 2 interface serial 0/1/1 overload

interface serial 0/1/1
 ip nat outside
interface gigabitEthernet 0/0/0
 ip nat inside
interface gigabitEthernet 0/0/1
 ip nat inside
✅ Verification
Generate HTTP traffic from the internal PCs to the external server (209.165.201.5) and run the following commands on R1 and R2 to verify the translations:

Cisco CLI
show ip nat translations
show ip nat statistics

## 🧠 Core Concept: What is a Socket?

A **Socket** is the unique endpoint of a communication link between two devices on a network. It is formed by combining an **IP Address** and a **Port Number**:

$$\text{Socket} = \text{IP Address} : \text{Port Number}$$

*   **IP Address:** Identifies the specific host on the network (like a building's street address).
*   **Port Number:** Identifies the specific application, process, or session running on that host (like an apartment number inside that building).

### How PAT Uses Sockets
PAT (Port Address Translation) relies completely on sockets. When multiple internal devices (PC1, PC2, etc.) share a single public IP address, the router dynamically changes the source port for each outgoing connection. 

This creates a unique **Public Socket** for every single traffic flow, allowing the router to precisely track where to return the traffic, even if thousands of devices are sharing the exact same public IP.
