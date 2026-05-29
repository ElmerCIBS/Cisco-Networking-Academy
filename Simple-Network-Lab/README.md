# Packet Tracer - Creating a Simple Network

## 📋 Lab Description
In this Cisco Networking Academy hands-on lab activity, I designed, implemented, and verified a basic Local Area Network (LAN) topology within the Packet Tracer Logical Workspace. The network establishes hybrid (wired and wireless) connectivity from local end devices out to a simulated remote server in the cloud (`cisco.srv`).

---

## 🌐 Network Topology
![Network Topology](./topology.png)

---

## 🛠️ Implemented Objectives & Tasks

### Part 1: Building the Physical Network
* **Device Deployment:** Deployed a PC, a Laptop, and a Cable Modem (WAN Emulation) into the workspace to simulate hardware interaction with an Internet Service Provider (ISP).
* **Cabling & Interfaces:** * Connected a Copper Straight-Through cable from the PC's `FastEthernet0` port to the Wireless Router's `Ethernet 1` interface.
  * Connected a Copper Straight-Through cable from the Wireless Router's `Internet` port to `Port 1` on the Cable Modem.
  * Connected a Coaxial cable from `Port 0` on the Cable Modem to the Internet cloud's `Coaxial 7` interface.

### Part 2: End Device Configuration & Connectivity Verification
* **PC Configuration (Wired Network):** Verified automatic IPv4 address allocation through **DHCP** issued dynamically by the wireless router.
* **Laptop Hardware Migration (Wireless Network):** 1. Power-cycled the laptop device off within the simulation.
  2. Removed the default wired copper Ethernet interface module.
  3. Installed the **WPC300N** wireless network interface card module.
  4. Powered the laptop back on and successfully associated it with the local `HomeNetwork` SSID.
* **Connectivity Diagnostic Testing:** * Executed the `ipconfig /all` command inside the Command Prompt to audit DHCP server configurations.
  * Performed an ICMP utility check via `ping cisco.srv` to test end-to-end packet transmission.
  * Opened the integrated Web Browser on the Laptop and successfully retrieved the remote HTTP webpage on `cisco.srv`.

---

## 📊 IPv4 Addressing Documentation

Using the network configurations captured via `ipconfig`, the following internal parameters were registered within the local `192.168.0.x` subnet pool:

| Device | IPv4 Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- |
| **PC** | `192.168.0.2` * | `255.255.255.0` | `192.168.0.1` |
| **Laptop** | `192.168.0.3` * | `255.255.255.0` | `192.168.0.1` |

> *\*Note: Dynamic host IP addresses may scale anywhere between `192.168.0.2` and `192.168.0.254` depending on DHCP server lease orders.*

---

## 🧠 Technical Reflections
* **Subnet Mask Functionality:** The `/24` prefix (`255.255.255.0`) logically segments the net-ID portion (`192.168.0.`) from the host-ID section, setting a strict domain constraint that isolates and allows up to 253 unique computing nodes to talk to each other locally on the same subnet.
* **Default Gateway Purpose:** Acts as the designated exit intersection for local traffic. Any data packets generated on the `192.168.0.x` network bound for an external street (the internet/WAN) are natively routed to the Wireless Router interface (`192.168.0.1`), which safely proxies the packets out to the Cable Modem and the ISP.

---
*Laboratory workbook completed as part of the Cisco Networking Academy curriculum.*
