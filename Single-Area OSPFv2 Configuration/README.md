# Packet Tracer: Single-Area OSPFv2 Configuration

## 📝 Project Description
This project involved implementing, optimizing, and troubleshooting a network topology using the **Single-Area OSPFv2 (Area 0)** dynamic routing protocol. The environment was divided into two distinct segments:
1. **Headquarters (HQ) Network:** Comprised point-to-point serial links interconnected using traditional network statements (`network`) and inverse masks (*wildcards*).
2. **Data Services Network:** A broadcast multiaccess network segmented through a central switch, where OSPF activation was configured directly at the physical interface level.

Strict security policies were enforced using passive interfaces, metric/cost reference bandwidth adjustments were applied to adapt the protocol for high-speed paths (Gigabit Ethernet), and timers (*Hello* and *Dead* intervals) were optimized for specific WAN links. Additionally, high availability was achieved by manipulating OSPF priorities to mandate the Designated Router (DR) election, and a static default route toward the ISP was implemented and dynamically injected across the entire OSPF domain.

---

## 🛠️ Key Technologies & Concepts
* **Single-Area OSPFv2:** Core dynamic link-state routing protocol.
* **DR/BDR Election:** Manipulation of interface priorities to enforce predictable multiaccess topology behavior.
* **Auto-Cost Reference Bandwidth:** Adjusting the metric formula to properly distinguish between Fast Ethernet and Gigabit Ethernet capabilities.
* **Passive Interfaces:** Securing user LANs and saving system resources by suppressing unnecessary OSPF Hello updates.
* **Default Route Propagation:** Dynamic injection (`default-information originate`) of the internet gateway to all upstream and downstream internal routers.

---

## 💻 Complete Command Reference (Step-by-Step)

### 1. Headquarters Configuration
OSPF process activation using legacy `network` statements paired with calculated wildcard masks for `/30`, `/24`, and `/28` prefixes.

#### **Router P2P-1**
```text
P2P-1> enable
P2P-1# configure terminal
P2P-1(config)# router ospf 10
P2P-1(config-router)# network 10.0.0.0 0.0.0.3 area 0
P2P-1(config-router)# network 10.0.0.8 0.0.0.3 area 0
P2P-1(config-router)# network 10.0.0.12 0.0.0.3 area 0
P2P-1(config-router)# auto-cost reference-bandwidth 1000
P2P-1(config-router)# interface Serial0/1/1
P2P-1(config-if)# ip ospf cost 50
P2P-1(config-if)# interface Serial0/2/0
P2P-1(config-if)# ip ospf hello-interval 20
P2P-1(config-if)# ip ospf dead-interval 80
P2P-1(config-if)# end
P2P-1# write

Router P2P-2
P2P-2> enable
P2P-2# configure terminal
P2P-2(config)# router ospf 10
P2P-2(config-router)# network 10.0.0.0 0.0.0.3 area 0
P2P-2(config-router)# network 10.0.0.4 0.0.0.3 area 0
P2P-2(config-router)# network 192.168.1.0 0.0.0.255 area 0
P2P-2(config-router)# network 192.168.2.0 0.0.0.255 area 0
P2P-2(config-router)# passive-interface GigabitEthernet0/0/0
P2P-2(config-router)# passive-interface GigabitEthernet0/0/1
P2P-2(config-router)# auto-cost reference-bandwidth 1000
P2P-2(config-router)# end
P2P-2# write

Router P2P-3
P2P-3> enable
P2P-3# configure terminal
P2P-3(config)# router ospf 10
P2P-3(config-router)# network 10.0.0.4 0.0.0.3 area 0
P2P-3(config-router)# network 10.0.0.8 0.0.0.3 area 0
P2P-3(config-router)# network 192.168.3.0 0.0.0.15 area 0
P2P-3(config-router)# passive-interface GigabitEthernet0/0/0
P2P-3(config-router)# auto-cost reference-bandwidth 1000
P2P-3(config-router)# end
P2P-3# write

2. Data Services Configuration
Direct interface-level OSPF mapping, custom static Router IDs implementation, and mandated DR role assignment.

Router BC-1 (Edge & Principal DR)
BC-1> enable
BC-1# configure terminal
BC-1(config)# ip route 0.0.0.0 0.0.0.0 Serial0/1/1
BC-1(config)# router ospf 10
BC-1(config-router)# router-id 6.6.6.6
BC-1(config-router)# default-information originate
BC-1(config-router)# auto-cost reference-bandwidth 1000
BC-1(config-router)# interface Serial0/1/0
BC-1(config-if)# ip ospf 10 area 0
BC-1(config-if)# ip ospf hello-interval 20
BC-1(config-if)# ip ospf dead-interval 80
BC-1(config-if)# interface GigabitEthernet0/0/0
BC-1(config-if)# ip ospf 10 area 0
BC-1(config-if)# ip ospf priority 255
BC-1(config-if)# end
BC-1# write

Router BC-2
BC-2> enable
BC-2# configure terminal
BC-2(config)# router ospf 10
BC-2(config-router)# router-id 5.5.5.5
BC-2(config-router)# passive-interface GigabitEthernet0/0/0
BC-2(config-router)# auto-cost reference-bandwidth 1000
BC-2(config-router)# interface GigabitEthernet0/0/0
BC-2(config-if)# ip ospf 10 area 0
BC-2(config-if)# interface GigabitEthernet0/0/1
BC-2(config-if)# ip ospf 10 area 0
BC-2(config-if)# end
BC-2# write

Router BC-3
BC-3> enable
BC-3# configure terminal
BC-3(config)# router ospf 10
BC-3(config-router)# router-id 4.4.4.4
BC-3(config-router)# passive-interface GigabitEthernet0/0/0
BC-3(config-router)# auto-cost reference-bandwidth 1000
BC-3(config-router)# interface GigabitEthernet0/0/0
BC-3(config-if)# ip ospf 10 area 0
BC-3(config-if)# interface GigabitEthernet0/0/1
BC-3(config-if)# ip ospf 10 area 0
BC-3(config-if)# end
BC-3# write

🔍 Verification Commands Applied
To validate successful network convergence, adjacent neighbor routing dependencies, and clean routing trees, the following verification execution tools were utilized:

show ip route (Ensures presence of standard internal networks alongside dynamic O*E2 internet gateway routes).

show ip route ospf (Isolates and prints out only specific routes gathered by OSPF).

show ip ospf neighbor (Verifies successful neighbor synchronization, showing complete Full/DR, Full/BDR, and Full/2-Way topology relationships).
