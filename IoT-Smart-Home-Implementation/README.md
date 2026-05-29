# Packet Tracer - Smart Home IoT Implementation

## 📋 Lab Description
In this advanced Cisco Networking Academy IoT simulation lab, I explored an existing smart home infrastructure and deployed additional wired and wireless Internet of Things (IoT) sensors and actuators. The lab focuses on configuring network profiles, handling hardware module hot-swaps, and integrating digital cross-connections between components to enable automated physical monitoring.

---

## 🌐 Smart Home IoT Architecture
![Smart Home Architecture](./iot_topology.png)

---

## 🛠️ Implemented Objectives & Tasks

### Part 1: Exploring the Pre-built Smart Home Network
* **Infrastructure Discovery:** Analyzed the core **Home Gateway** central hub which serves as the local DHCP server, Wi-Fi Access Point, and IoT local Registration Server.
* **Network Parameters Audited:**
  * **LAN Default Gateway IP Address:** As provisioned in the configuration console (`192.168.25.1` / default range depending on configuration).
  * **Wireless SSID:** `HomeNetwork`
  * **Security Type:** WPA2-PSK (Pre-Shared Key authentication standard).
* **Remote Ecosystem Management:** Utilized the client **Tablet** to access the gateway's web portal interface (`http://<Gateway-IP>`) using administrator credentials (`admin/admin`). Verified and interacted with active appliances like the `Smart Fan`, `Garage Door`, and `Smart Lamp`.

### Part 2: Deploying Wireless IoT Components
* **Wind Detector Integration:** * Deployed a wireless `Wind Detector` into the logical workspace environment.
  * Configured the global settings to redirect its environmental telemetry logs directly to the **Home Gateway** IoT server registry.
  * Configured the `Wireless0` adapter with `WPA2-PSK` authentication alongside the matching network pass phrase.
  * Verified successful over-the-air (OTA) pairing and validated its real-time operational status on the Tablet dashboard.

### Part 3: Deploying Wired IoT Components & Machine-to-Machine (M2M) Control
* **Smart Sprinkler Deployment:**
  * Placed a `Lawn Sprinkler` onto the topology canvas.
  * Enabled **Advanced Settings** to access the structural **I/O Config** layer.
  * Performed a network interface card (NIC) hot-swap, replacing the default adapter with a **PT-IOT-NM-1CFE** FastEthernet port to allow copper termination.
  * Wired the physical interface using a **Copper Straight-Through** cable from `FastEthernet0` into an available local interface on the Home Gateway hub.
  * Altered the display profile to `Smart Sprinkler`, toggled the IP setup to `DHCP` for seamless dynamic binding, and pointed the IoT Registration Server to the Home Gateway.
* **Smart Water Meter & Hardware Interaction:**
  * Added a `Water Level Monitor` and updated its host parameters to read `Water Meter` with pointing directed to the Home Gateway.
  * Attached its wireless interface to the gateway using the correct `SSID` and `WPA2-PSK` authentication.
  * Navigated to **I/O Config** to allocate `1 Digital Slot` and adjusted its native logic profile to run as a **Component** sensor rather than a standalone endpoint.
  * Established an analytical Machine-to-Machine (M2M) cross-connection by running an **IoT Custom Cable** directly from the Smart Sprinkler's digital input/output `D0` interface into the Water Meter's `D0` pin.
* **Ecosystem Verification:** Logged into the automated home dashboard utilizing the **Smartphone's** integrated web browser tool to ensure both the `Wind Detector` and `Water Meter` telemetry values were reporting normally.

---

## 📊 Core Network Configuration Reference

Below are the baseline specifications tracked throughout the smart appliance deployment and verification phases:

| Metric / Parameter | Configured Value / Specification | Functional Role in Topology |
| :--- | :--- | :--- |
| **IoT Server Target** | `Home Gateway` | Acts as the local registration directory and control plane for metrics. |
| **Wireless SSID** | `HomeNetwork` | Broadcast domain for smart environment appliances. |
| **Security Mode** | `WPA2-PSK` | Encryption algorithm to prevent rogue node injection. |
| **IP Protocol** | `DHCP (IPv4)` | Automates host node tracking within the residential subnet pool. |
| **M2M Interface Link** | `IoT Custom Cable (D0 to D0)` | Physical transmission link for serial digital signaling between assets. |

---

## 🧠 Technical Key Takeaways
1. **IoT Node Registration & Management:** IoT devices utilize lightweight application-layer protocols to check in with a registration broker (the Home Gateway). This architectural decoupling allows users to safely query status states and alter mechanical outputs (e.g., turning on a sprinkler or fan) from any authenticated host on the subnet via standard HTTP/HTTPS channels.
2. **Modular Interfaces (NIC Swapping):** Industrial IoT deployment simulations demonstrate that legacy endpoints require modular adaptors (such as the `PT-IOT-NM-1CFE` transceiver) to adapt traditional physical copper mediums to modern Ethernet processing lines.
3. **Machine-to-Machine (M2M) Interoperability:** Linking the `Smart Sprinkler` directly to the `Water Meter` using an **IoT Custom Cable** models low-level micro-controller loop setups. Activating the sprinkler changes digital high/low logic values across pin `D0`, which instantly feeds electrical telemetry directly into the meter to alter sensor graphs without requiring centralized computing assistance.

---
*Laboratory workbook completed as part of the Cisco Networking Academy curriculum.*
