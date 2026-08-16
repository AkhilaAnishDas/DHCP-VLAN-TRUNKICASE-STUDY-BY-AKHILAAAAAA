# VLAN, DHCP & TRUNK NETWORK CONFIGURATION

A Cisco Packet Tracer networking project implementing **VLAN segmentation, DHCP address assignment, and 802.1Q trunking** using the topology instructed by the faculty.

---

## 📌 Project Overview

This project demonstrates the configuration of a VLAN-based network with four departments:

* **VLAN 10 — Sales**
* **VLAN 20 — Finance**
* **VLAN 30 — Manufacturing**
* **VLAN 99 — Guest Wi-Fi**

The network consists of:

* **4 Servers:** S1, S2, S3, S4
* **2 Switches:** SW1, SW2
* **4 Routers:** R1, R2, R3, R4

The connection between **SW1 and SW2** is configured as an **802.1Q trunk**, carrying VLANs 10, 20, 30 and 99.

---

## 🖧 Network Topology

```text
                         802.1Q TRUNK
                    VLAN 10,20,30,99
                     Native VLAN 999
                            │
                         ┌─────┐
                         │ SW1 │
                         └──┬──┘
                            ║
                            ║ TRUNK
                            ║
                         ┌──┴──┐
                         │ SW2 │
                         └──┬──┘
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
         R1                R2                R3                R4
          │                 │                 │                 │
         S1                S2                S3                S4
       VLAN 10           VLAN 20           VLAN 30           VLAN 99
        Sales            Finance         Manufacturing         Guest
```

---

## 🔌 Devices Used

| Device  | Type   | VLAN / Role             |
| ------- | ------ | ----------------------- |
| **S1**  | Server | VLAN 10 – Sales         |
| **S2**  | Server | VLAN 20 – Finance       |
| **S3**  | Server | VLAN 30 – Manufacturing |
| **S4**  | Server | VLAN 99 – Guest         |
| **SW1** | Switch | Main/Core Switch        |
| **SW2** | Switch | Second/Warehouse Switch |
| **R1**  | Router | VLAN 10 – Sales         |
| **R2**  | Router | VLAN 20 – Finance       |
| **R3**  | Router | VLAN 30 – Manufacturing |
| **R4**  | Router | VLAN 99 – Guest         |

---

## 📋 VLAN & IP Addressing

| VLAN | Name          | Network           | Gateway        | DHCP Range                       |
| ---: | ------------- | ----------------- | -------------- | -------------------------------- |
|   10 | SALES         | `192.168.10.0/24` | `192.168.10.1` | `192.168.10.50 – 192.168.10.200` |
|   20 | FINANCE       | `192.168.20.0/24` | `192.168.20.1` | `192.168.20.50 – 192.168.20.150` |
|   30 | MANUFACTURING | `192.168.30.0/24` | `192.168.30.1` | `192.168.30.50 – 192.168.30.220` |
|   99 | GUEST         | `192.168.99.0/24` | `192.168.99.1` | `192.168.99.50 – 192.168.99.250` |
|  999 | NATIVE        | —                 | —              | —                                |

---

## ⚙️ VLAN Configuration

The following VLANs were configured on **SW1 and SW2**:

```text
VLAN 10 → SALES
VLAN 20 → FINANCE
VLAN 30 → MANUFACTURING
VLAN 99 → GUEST
VLAN 999 → NATIVE
```

### Access Port Assignment

```text
Fa0/1 → VLAN 10
Fa0/2 → VLAN 20
Fa0/3 → VLAN 30
Fa0/4 → VLAN 99
```

### Verification Command

```bash
show vlan brief
```

---

## 🔗 Trunk Configuration

The **SW1–SW2** connection is configured as an **802.1Q trunk**.

### Allowed VLANs

```text
VLAN 10
VLAN 20
VLAN 30
VLAN 99
```

### Native VLAN

```text
VLAN 999
```

### Verification Command

```bash
show interfaces trunk
```

### Expected Verification

```text
Encapsulation: 802.1q
Status: trunking
Native VLAN: 999
Allowed VLANs: 10,20,30,99
```

---

## 🌐 DHCP Configuration

DHCP is configured separately for each VLAN so that the servers automatically receive IP addresses from their respective networks.

### R1 — VLAN 10

```text
Network: 192.168.10.0/24
Gateway: 192.168.10.1
```

### R2 — VLAN 20

```text
Network: 192.168.20.0/24
Gateway: 192.168.20.1
```

### R3 — VLAN 30

```text
Network: 192.168.30.0/24
Gateway: 192.168.30.1
```

### R4 — VLAN 99

```text
Network: 192.168.99.0/24
Gateway: 192.168.99.1
```

### DHCP Verification

Run the following command on each router:

```bash
show ip dhcp binding
```

This displays the IP addresses dynamically assigned to the servers.

---

## 🧪 Connectivity Testing

### S1 → R1

```bash
ping 192.168.10.1
```

### S2 → R2

```bash
ping 192.168.20.1
```

### S3 → R3

```bash
ping 192.168.30.1
```

### S4 → R4

```bash
ping 192.168.99.1
```

Expected result:

```text
Success rate is 100 percent
```

---

## 🔄 Reverse Connectivity Testing

The corresponding router can also be used to test connectivity to its server.

### R1 → S1

```bash
ping <S1-DHCP-IP>
```

### R2 → S2

```bash
ping <S2-DHCP-IP>
```

### R3 → S3

```bash
ping <S3-DHCP-IP>
```

### R4 → S4

```bash
ping <S4-DHCP-IP>
```

Expected result:

```text
!!!!!
Success rate is 100 percent (5/5)
```

---

## 🔍 Verification Commands

### Check VLAN Configuration

```bash
show vlan brief
```

### Check Trunk Configuration

```bash
show interfaces trunk
```

### Check DHCP Bindings

```bash
show ip dhcp binding
```

### Check Interface Status

```bash
show ip interface brief
```

---

## ✅ Verification Results

| Verification                    | Status |
| ------------------------------- | ------ |
| VLAN 10 – Sales                 | ✅ PASS |
| VLAN 20 – Finance               | ✅ PASS |
| VLAN 30 – Manufacturing         | ✅ PASS |
| VLAN 99 – Guest                 | ✅ PASS |
| SW1–SW2 Trunk                   | ✅ PASS |
| 802.1Q Encapsulation            | ✅ PASS |
| Native VLAN 999                 | ✅ PASS |
| VLANs 10, 20, 30 and 99 Allowed | ✅ PASS |
| DHCP – VLAN 10                  | ✅ PASS |
| DHCP – VLAN 20                  | ✅ PASS |
| DHCP – VLAN 30                  | ✅ PASS |
| DHCP – VLAN 99                  | ✅ PASS |
| S1 → R1                         | ✅ PASS |
| S2 → R2                         | ✅ PASS |
| S3 → R3                         | ✅ PASS |
| S4 → R4                         | ✅ PASS |

---

## 💬 Discussion Questions

### 1. Why does the link between the core switch and the warehouse switch need to be a trunk instead of an access port?

The link needs to be a **trunk** because it carries traffic from multiple VLANs simultaneously. An access port carries traffic for a single VLAN, whereas a trunk allows VLANs 10, 20, 30 and 99 to travel through one physical connection.

### 2. What would happen to Guest Wi-Fi traffic if the trunk's allowed VLAN list did not include VLAN 99?

If VLAN 99 is not included in the allowed VLAN list, **Guest VLAN traffic cannot cross the trunk**. Therefore, Guest devices connected through the other switch would not be able to communicate across that trunk.

### 3. Why is it useful for each VLAN to have its own DHCP scope rather than one shared scope for the whole company?

Each VLAN represents a separate network and requires IP addresses from its own subnet. Separate DHCP scopes ensure that devices receive the correct IP address range and gateway for their respective VLAN.

For example:

```text
VLAN 10 → 192.168.10.x
VLAN 20 → 192.168.20.x
VLAN 30 → 192.168.30.x
VLAN 99 → 192.168.99.x
```

### 4. What risk does changing the native VLAN to an unused ID (999) help reduce?

Using an unused VLAN such as **VLAN 999** as the native VLAN helps reduce the risk of unwanted or unauthorized traffic using the default/native VLAN. It provides additional separation between normal user VLANs and native VLAN traffic.

---

## 📸 Screenshots Included

The project documentation includes screenshots of:

1. Final Packet Tracer topology
2. SW1 – `show vlan brief`
3. SW2 – `show vlan brief`
4. SW1 – `show interfaces trunk`
5. SW2 – `show interfaces trunk`
6. R1 – `show ip dhcp binding`
7. R2 – `show ip dhcp binding`
8. R3 – `show ip dhcp binding`
9. R4 – `show ip dhcp binding`
10. S1 → R1 ping
11. S2 → R2 ping
12. S3 → R3 ping
13. S4 → R4 ping
14. R1 → S1 ping
15. R2 → S2 ping
16. R3 → S3 ping
17. R4 → S4 ping

---

## 🎯 Expected Outcome

The completed network demonstrates:

* Separate VLANs for each department.
* Automatic IP address assignment using DHCP.
* Successful server-to-router connectivity.
* A working 802.1Q trunk between SW1 and SW2.
* VLANs 10, 20, 30 and 99 carried across the trunk.
* Native VLAN 999 configured on the trunk.
* Organized IP addressing according to VLANs.

---

## 📁 Project Structure

```text
VLAN-DHCP-TRUNK-ASSIGNMENT/
│
├── README.md
├── Documentation.pdf
├── Network-Topology.pkt
│
└── Screenshots/
    ├── Final-Topology.png
    ├── SW1-VLAN.png
    ├── SW2-VLAN.png
    ├── SW1-Trunk.png
    ├── SW2-Trunk.png
    ├── R1-DHCP.png
    ├── R2-DHCP.png
    ├── R3-DHCP.png
    ├── R4-DHCP.png
    ├── S1-R1-Ping.png
    ├── S2-R2-Ping.png
    ├── S3-R3-Ping.png
    ├── S4-R4-Ping.png
    ├── R1-S1-Ping.png
    ├── R2-S2-Ping.png
    ├── R3-S3-Ping.png
    └── R4-S4-Ping.png
```

---

## 👩‍💻 Author

**Akhila Anish Das**
**B.Tech CSE**
**Computer Networking & Cyber Security**
