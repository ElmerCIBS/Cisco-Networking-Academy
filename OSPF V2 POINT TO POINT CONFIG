OSPFv2 Single-Area Point-to-Point Configuration Lab

This lab focused on configuring and verifying OSPFv2 single-area routing in a point-to-point network topology using Cisco Packet Tracer. Three routers (R1, R2, and R3) were configured to exchange routing information dynamically within Area 0.

Objectives Completed

* Configured the OSPF routing process using Process ID 10.
* Assigned manual Router IDs to each router:
    * R1 → 1.1.1.1
    * R2 → 2.2.2.2
    * R3 → 3.3.3.3
* Advertised networks using different OSPF configuration methods:
    * Network statements with wildcard masks on R1.
    * Network statements with host addresses and 0.0.0.0 wildcard masks on R2.
    * Interface-based OSPF configuration on R3.
* Configured passive interfaces on LAN-facing ports to prevent unnecessary OSPF hello traffic.
* Verified OSPF neighbor relationships, routing advertisements, and passive interface settings.

#####Key Commands Used#####

Router ID Configuration
router ospf 10
router-id 1.1.1.1

###Network Statements with Wildcard Masks (R1)###
network 192.168.10.0 0.0.0.255 area 0
network 10.1.1.0 0.0.0.3 area 0
network 10.1.1.4 0.0.0.3 area 0

###Network Statements with Quad-Zero Wildcard Masks (R2)###
network 192.168.20.1 0.0.0.0 area 0
network 10.1.1.2 0.0.0.0 area 0
network 10.1.1.9 0.0.0.0 area 0

###Interface-Based OSPF Configuration (R3)###
interface g0/0/0
 ip ospf 10 area 0

interface s0/1/0
 ip ospf 10 area 0

interface s0/1/1
 ip ospf 10 area 0

###Passive Interface Configuration###
router ospf 10
passive-interface g0/0/0

###Verification Commands###
show ip protocols
show ip ospf interface brief
show ip route ospf
show ip ospf neighbor
show running-config | section ospf

$$Skills Demonstrated$$

* OSPFv2 configuration and deployment
* Router ID assignment
* Wildcard mask calculation
* Network advertisement methods
* Interface-based routing configuration
* Passive interface implementation
* OSPF verification and troubleshooting
* Dynamic routing in a single-area OSPF environment

This project demonstrates practical experience configuring and validating OSPF routing in a multi-router topology using Cisco Packet Tracer.
