# 🌐 Packet Tracer — Examining NAT on a Wireless Router

> A hands-on Cisco Packet Tracer lab simulating Network Address Translation (NAT) in a Small Office/Home Office (SOHO) environment, where multiple local devices share a single public IP address to access the internet.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Objectives](#objectives)
- [Lab Walkthrough](#lab-walkthrough)
  - [Part 1 — Gateway & External Connectivity](#part-1--gateway--external-connectivity)
  - [Part 2 — Internal Network (LAN) Configuration](#part-2--internal-network-lan-configuration)
  - [Part 3 — Scaling the Network](#part-3--scaling-the-network)
  - [Part 4 & 5 — Visualizing NAT in Simulation Mode](#part-4--5--visualizing-nat-in-simulation-mode)
- [Key Technical Concepts](#key-technical-concepts)
- [How to Run the Lab](#how-to-run-the-lab)
- [Tools Used](#tools-used)

---

## Overview

This project demonstrates how **Network Address Translation (NAT)** works in a typical SOHO router setup. By using Cisco Packet Tracer's Simulation Mode, we can visually inspect how IP packet headers are modified as traffic crosses the boundary between a private LAN and the public internet.

---

## Objectives

- ✅ Configure a wireless router to provide dynamic IP addresses via DHCP
- ✅ Differentiate between **Private (internal)** and **Public (external)** IP address spaces
- ✅ Analyze the NAT translation process by inspecting packet headers in **Simulation Mode**

---

## Lab Walkthrough

### Part 1 — Gateway & External Connectivity

In this phase, we verify the router's connection to the ISP.

| Action | Discovery |
|--------|-----------|
| Connected a PC and accessed the router's web GUI at the default gateway address | Identified the **Internet Connection IP** under the Status menu |

> **Key Question:** Is this address public or private?
>
> **Answer:** This is a **Public IP address**. It is assigned by the ISP and is globally routable on the internet.

---

### Part 2 — Internal Network (LAN) Configuration

Next, we examined how the router communicates with internal devices.

| Action | Discovery |
|--------|-----------|
| Navigated to the Local Network settings in the GUI | Reviewed the DHCP server settings — the range of addresses handed out to laptops, phones, and PCs |

> **Key Question:** Are these addresses public or private?
>
> **Answer:** These are **Private IP addresses** (typically in the `192.168.x.x` range). They are used for internal communication only and **cannot exist on the public internet**.

---

### Part 3 — Scaling the Network

- Added **3 additional PCs** to the network and configured them for DHCP.
- Validated by running `ipconfig /all` in the Command Prompt on each device.

**Expected results for every device:**

```
IPv4 Address:     192.168.0.x    (unique per device)
Subnet Mask:      255.255.255.0
Default Gateway:  192.168.0.1    (same for all)
```

---

### Part 4 & 5 — Visualizing NAT in Simulation Mode

This is where the core NAT mechanism is revealed. We created a **Complex PDU** (an HTTP request) from a local PC targeting an external server (`ciscolearn.nat.com`) and tracked the packet's full journey.

#### 🔄 NAT Translation Process

```
[Local PC]                  [Wireless Router]               [External Server]
192.168.0.100  ──────────►  Src: 192.168.0.100              
                             ↕  NAT Header Swap
                             Src: <Public WAN IP>  ────────►  203.0.113.50

                             ◄────────────────────────────  Reply to Public IP
192.168.0.100  ◄────────────  NAT Table Lookup → forward to correct private IP
```

| Stage | Source IP | Destination IP |
|-------|-----------|----------------|
| **LAN → Router (Inbound)** | `192.168.0.100` (private) | External server |
| **Router → WAN (Outbound)** | `<Public WAN IP>` (public) | External server |
| **Return trip** | External server | `<Public WAN IP>` → router checks **NAT Table** → forwards to `192.168.0.100` |

> The router maintains a **NAT Translation Table** mapping each internal device's IP/port to the public IP — this is how it knows where to return incoming replies.

---

## Key Technical Concepts

| Concept | Description |
|---------|-------------|
| **NAT** (Network Address Translation) | Conserves the limited supply of IPv4 addresses by allowing one public IP to represent hundreds of private ones |
| **DHCP** (Dynamic Host Configuration Protocol) | Automatically assigns IP addresses, subnet masks, and gateways to devices on the network |
| **Default Gateway** | The "exit door" of the local network — the internal IP of the router (e.g., `192.168.0.1`) |
| **Private IP Range** | Addresses such as `192.168.x.x`, `10.x.x.x`, and `172.16–31.x.x` — not routable on the internet |
| **Public IP** | A globally unique address assigned by the ISP — used to communicate on the internet |

---

## How to Run the Lab

1. **Download** the `.pkt` file from this repository.
2. **Open** it in [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer).
3. Follow the instructions in the internal **Activity** window to verify configurations.
4. Switch to **Simulation Mode** and create a Complex PDU to observe the NAT process in real time.

---

## Tools Used

- **Cisco Packet Tracer** — Network simulation and visualization
- **Simulation Mode** — Packet-level inspection and header analysis
- **Router Web GUI** — DHCP and NAT configuration interface

---

> 📌 *This lab is part of my hands-on networking portfolio, completed as part of my IT studies and Cisco certification preparation (CCST IT Support / CCST Cybersecurity).*
