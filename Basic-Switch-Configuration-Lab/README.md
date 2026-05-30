# Packet Tracer - Basic Switch Configuration

## 📋 Lab Description
This Cisco Networking Academy laboratory exercise focuses on establishing baseline security, administrative identification, line access control, and logical IPv4 reachability across a loop-redundant switched local network infrastructure. The topology interconnects four 2960 series switches (`S2`, `S3`, `S4`, `S5`) supporting interconnected local host computing nodes.

---

## 🌐 Network Topology
![Switch Topology](./switch_topology.png)

---

## 📊 Addressing Matrix

Every infrastructure Virtual Interface (SVI) and host Network Interface Card (NIC) is mapped onto the static `192.168.100.0/24` subnet:

| Device Name | Interface | IPv4 Address | Subnet Mask |
| :--- | :--- | :--- | :--- |
| **S2** | VLAN 1 | `192.168.100.2` | `255.255.255.0` |
| **S3** | VLAN 1 | `192.168.100.3` | `255.255.255.0` |
| **S4** | VLAN 1 | `192.168.100.4` | `255.255.255.0` |
| **S5** | VLAN 1 | `192.168.100.5` | `255.255.255.0` |
| **PC6** | FastEthernet0 | `192.168.100.6` | `255.255.255.0` |
| **PC7** | FastEthernet0 | `192.168.100.7` | `255.255.255.0` |
| **PC8** | FastEthernet0 | `192.168.100.8` | `255.255.255.0` |
| **PC9** | FastEthernet0 | `192.168.100.9` | `255.255.255.0` |

---

## 🛠️ Cisco IOS Configuration Script

The following structured IOS command blocks were successfully deployed via the console terminal layer across the infrastructure assets to fulfill all administrative identity and security requirements:

### 🖥️ Switch 2 (S2) Configuration
```ios
enable
configure terminal
hostname S2
enable secret Cisco2
line console 0
password Consola2
login
exit
line vty 0 15
password Telnet2
login
exit
service password-encryption
banner motd # AUTHORIZED ACCESS ONLY #
interface vlan 1
ip address 192.168.100.2 255.255.255.0
no shutdown
exit
end
copy running-config startup-config
 🖥️ Switch 3 (S3) Configuration
enable
configure terminal
hostname S3
enable secret Cisco3
line console 0
password Consola3
login
exit
line vty 0 15
password Telnet3
login
exit
service password-encryption
banner motd # AUTHORIZED ACCESS ONLY #
interface vlan 1
ip address 192.168.100.3 255.255.255.0
no shutdown
exit
end
copy running-config startup-config
🖥️ Switch 4 (S4) Configuration
enable
configure terminal
hostname S4
enable secret Cisco4
line console 0
password Consola4
login
exit
line vty 0 15
password Telnet4
login
exit
service password-encryption
banner motd # AUTHORIZED ACCESS ONLY #
interface vlan 1
ip address 192.168.100.4 255.255.255.0
no shutdown
exit
end
copy running-config startup-config
🖥️ Switch 5 (S5) Configuration
enable
configure terminal
hostname S5
enable secret Cisco5
line console 0
password Consola5
login
exit
line vty 0 15
password Telnet5
login
exit
service password-encryption
banner motd # AUTHORIZED ACCESS ONLY #
interface vlan 1
ip address 192.168.100.5 255.255.255.0
no shutdown
exit
end
copy running-config startup-config
🧠 Technical Key Takeaways
Password Encryption Service (service password-encryption): By default, local system lines (console and virtual terminal lines) store passwords within the startup configuration registry in plain text. Activating this global service passes clear-text strings through a Cisco proprietary Type-7 reversible hashing algorithm, preventing unauthorized visual shoulder-surfing vectors.

Layered Access Security Control: The deployment splits administrative permissions into discrete control planes:

Line Console 0: Hardens physical out-of-band serial connections to the physical terminal chassis.

Line VTY 0 15: Establishes remote Telnet/SSH line allocation parameters for virtual in-band network socket management.

Enable Secret: Encrypts user EXEC scaling up to privileged configuration access states utilizing secure MD5/SHA algorithms.

Spanning Tree Protocol (STP) Behavior Observation: In the logical diagram, the connection link between S2 and S4 showcases an orange/amber interface indicator instead of the standard green. This provides clear visual confirmation of STP executing automatically in the background. It dynamically blocks a duplicate path to eliminate layer-2 broadcast storms while keeping the link ready for automatic failover processing if an active link fails.

Laboratory workbook completed as part of the Cisco Networking Academy curriculum.
