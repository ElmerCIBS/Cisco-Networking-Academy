# Packet Tracer: Single-Area OSPFv2 Basic Configuration and Optimization

## 📝 Project Description
This project involved the staging, basic initial configuration, and advanced traffic optimization of a corporate network topology running **Single-Area OSPFv2 (Area 0)**. The goal was to establish reliable end-to-end routing between an enterprise internal LAN (Web Server) and a branch office wireless/wired network (Laptop). 

Following the initial basic deployment, network optimization strategies were implemented to reduce routing protocol overhead traffic across the backbone link and to systematically ensure that the main router (R1) maintains strict primary topology path control (Designated Router designation). Furthermore, internal local area network structures were optimized by substituting dynamic intra-area updates with localized static default gateway mapping and dynamic route propagation methods.

---

## 🛠️ Key Technologies & Concepts
* **OSPFv2 Routing Instance 56:** Multi-device link-state topology mapping.
* **DR/BDR Priority Overrides:** Elevating interface priority counters to deterministic states to eliminate arbitrary Router ID ties.
* **Custom OSPF Hello & Dead Timers:** Tuning interval timers ($30\text{s} / 120\text{s}$) to curb unnecessary update flooding and resource utilization.
* **External Metric Type-2 (E2) Propagation:** Utilizing `default-information originate` to dynamically broadcast a static stub interface path as a low-cost seeding metric (`1`) throughout the autonomous system.
* **Auto-Cost Reference Bandwidth Tuning:** Modifying OSPF base bandwidth configurations to align with modern $1\text{ Gbps}$ speeds, preventing improper scaling on high-bandwidth links.

---

## 💻 Complete Command Reference (Step-by-Step)

### 1. Global Parameters & Device Hardening
Initial baseline setup on all network components, covering custom hostnames, privileged level credentials (`class`), execution line passwords (`cisco`), interactive warning banners, and localized service encryption.

#### **Router R1**
```text
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# no ip domain-lookup
R1(config)# enable secret class
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
R1(config-line)# exit
R1(config)# line vty 0 4
R1(config-vty)# password cisco
R1(config-vty)# login
R1(config-vty)# exit
R1(config)# service password-encryption
R1(config)# banner motd "UNAUTHORIZED ACCESS IS STRICTLY PROHIBITED!"
R1(config)# end
R1# write

##Router R2##
Router> enable
Router# configure terminal
Router(config)# hostname R2
R2(config)# no ip domain-lookup
R2(config)# enable secret class
R2(config)# line console 0
R2(config-line)# password cisco
R2(config-line)# login
R2(config-line)# exit
R2(config)# line vty 0 4
R2(config-vty)# password cisco
R2(config-vty)# login
R2(config-vty)# exit
R2(config)# service password-encryption
R2(config)# banner motd "UNAUTHORIZED ACCESS IS STRICTLY PROHIBITED!"
R2(config)# end
R2# write

##Switches S1 & S2 (Repeated for both devices)##
Switch> enable
Switch# configure terminal
Switch(config)# hostname S1  *(Change to S2 on the second device)*
S1(config)# no ip domain-lookup
S1(config)# enable secret class
S1(config)# line console 0
S1(config-line)# password cisco
S1(config-line)# login
S1(config-line)# exit
S1(config)# line vty 0 15
S1(config-vty)# password cisco
S1(config-vty)# login
S1(config-vty)# exit
S1(config)# service password-encryption
S1(config)# banner motd "UNAUTHORIZED ACCESS IS STRICTLY PROHIBITED!"
S1(config)# end
S1# write

##2. Interface Addressing & Basic OSPFv2 Routing
Router R1 Base Configuration##

R1> enable
R1# configure terminal
R1(config)# interface GigabitEthernet0/0/0
R1(config-if)# ip address 172.16.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# interface GigabitEthernet0/0/1
R1(config-if)# ip address 10.53.0.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# router ospf 56
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 10.53.0.0 0.0.0.255 area 0
R1(config-router)# network 172.16.1.0 0.0.0.255 area 0
R1(config-router)# end
R1# write

##Router R2 Base Configuration##
R2> enable
R2# configure terminal
R2(config)# interface GigabitEthernet0/0/0
R2(config-if)# ip address 192.168.1.1 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# interface GigabitEthernet0/0/1
R2(config-if)# ip address 10.53.0.2 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# exit
R2(config)# router ospf 56
R2(config-router)# router-id 2.2.2.2
R2(config-router)# network 10.53.0.0 0.0.0.255 area 0
R2(config-router)# network 192.168.1.0 0.0.0.255 area 0
R2(config-router)# end
R2# write

##3. Advanced Topology Optimization
Optimizing R1 (DR Status, Custom Timers, Reference Bandwidth & Route Injection)##
R1> enable
R1# configure terminal
R1(config)# interface GigabitEthernet0/0/1
R1(config-if)# ip ospf priority 50
R1(config-if)# ip ospf hello-interval 30
R1(config-if)# ip ospf dead-interval 120
R1(config-if)# exit
R1(config)# ip route 0.0.0.0 0.0.0.0 GigabitEthernet0/0/0
R1(config)# router ospf 56
R1(config-router)# no network 172.16.1.0 0.0.0.255 area 0
R1(config-router)# default-information originate
R1(config-router)# auto-cost reference-bandwidth 1000
R1(config-router)# end
R1# write

##Optimizing R2 (Matching Hello/Dead Intervals & Reference Bandwidth)##
R2> enable
R2# configure terminal
R2(config)# interface GigabitEthernet0/0/1
R2(config-if)# ip ospf hello-interval 30
R2(config-if)# ip ospf dead-interval 120
R2(config-if)# exit
R2(config)# router ospf 56
R2(config-router)# auto-cost reference-bandwidth 1000
R2(config-router)# end
R2# write

##Forcing Database Recalculation (Executed on BOTH routers)##

R1# clear ip ospf process
Reset ALL OSPF processes? [no]: yes

