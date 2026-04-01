Packet Tracer: Examining NAT on a Wireless Router
🌐 Project Overview
This repository contains a Cisco Packet Tracer lab focused on Network Address Translation (NAT). The project simulates a common Small Office/Home Office (SOHO) environment where multiple local devices share a single public IP address to access the internet.

🎯 Objectives
Configure a wireless router to provide dynamic IP addresses via DHCP.

Differentiate between Private (internal) and Public (external) IP address spaces.

Analyze the NAT translation process by inspecting packet headers in Simulation Mode.

🛠️ Detailed Lab Walkthrough
Part 1: Gateway & External Connectivity
In this phase, we verify the router's connection to the Internet Service Provider (ISP).

The Action: We connected a PC and accessed the router's web-based GUI (at the default gateway address).

The Discovery: Under the Status menu, we identified the Internet Connection IP address.

Key Question: Is this address public or private?

Answer: This is a Public IP address. It is assigned by the ISP and is globally routable on the internet.

Part 2: Internal Network (LAN) Configuration
Next, we looked at how the router communicates with the devices inside the building.

The Action: We navigated to the Local Network settings in the GUI.

The Discovery: We reviewed the DHCP server settings, which define the range of addresses handed out to laptops, phones, and PCs.

Key Question: Are these addresses public or private?

Answer: These are Private IP addresses (typically starting with 192.168.x.x). They are used for internal communication only and cannot exist on the public internet.

Part 3: Scaling the Network
The Action: We added 3 additional PCs and configured them for DHCP.

Validation: By running ipconfig /all in the Command Prompt, we verified that every device received a unique private IP, a subnet mask, and the same default gateway.

Part 4 & 5: Visualizing NAT (Simulation Mode)
This is where the "magic" of NAT is revealed. By creating a Complex PDU (an HTTP request) from a local PC to an external server (ciscolearn.nat.com), we tracked the packet's journey.

What happened at the Router?
When the packet moved from the LAN to the WAN, the router performed a header swap:

Inbound (Inside the Router): The packet arrives with a Source IP of the internal PC (e.g., 192.168.0.100).

Outbound (Outside the Router): The router changes the Source IP to its own Public WAN IP.

The Return Trip: When the server replies, it sends the data to the router's Public IP. The router checks its NAT Translation Table, sees which internal PC requested that data, and passes it back to the correct private IP.

🧠 Key Technical Concepts
NAT (Network Address Translation): Conserves the limited supply of IPv4 addresses by allowing one public IP to represent hundreds of private ones.

DHCP (Dynamic Host Configuration Protocol): Automatically assigns IP addresses to devices so users don't have to configure them manually.

Default Gateway: The "exit door" of the local network (the internal IP of the router).

🚀 How to Run the Lab
Download the .pkt file from this repository.

Open it in Cisco Packet Tracer.

Follow the instructions provided in the internal "Activity" window to verify the configurations.
