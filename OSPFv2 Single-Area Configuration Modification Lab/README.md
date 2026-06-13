OSPFv2 Single-Area Configuration Modification Lab

This lab focused on modifying and optimizing an existing OSPFv2 single-area network in Cisco Packet Tracer. Connectivity between all PCs and the web server was verified before making any changes. The OSPF Hello and Dead timers were then modified on the link between R1 and R2, causing the neighbor relationship to fail until matching timer values were configured on both routers, restoring OSPF adjacency.

The lab also demonstrated how OSPF selects routes based on cost. By changing the bandwidth of the Serial0/0/0 interface on R1 to 64 Kbps, the OSPF metric was increased, causing the routing protocol to recalculate paths and select an alternative route through R3 instead of the direct path through R2.

Finally, connectivity tests confirmed that all devices could communicate successfully and that OSPF routing updates were functioning correctly throughout the network.

##Key Commands Used##
#
ip ospf hello-interval 15
ip ospf dead-interval 60
bandwidth 64
show ip ospf neighbor
show ip ospf interface
show ip route ospf
ping
tracert

Skills Demonstrated

* OSPFv2 configuration and optimization
* OSPF timer management
* OSPF neighbor troubleshooting
* Route cost manipulation
* Dynamic route recalculation
* Interface bandwidth configuration
* Network verification and connectivity testing
* Cisco Packet Tracer network simulation
