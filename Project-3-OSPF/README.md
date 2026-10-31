
# Project 3: Multi-Area OSPF, Zone-Based Firewall & NAT

### 📌 Objective
Built an enterprise multi-area OSPF network featuring a Zone-Based Firewall (ZBF), route summarization, and NAT Overload for internet access. This project demonstrates advanced routing, security, and edge connectivity.

### 🗺️ Topology Architecture
*   **R1 (Edge Router):** Connects to the internet (NAT) and Area 0. Originates the default route into OSPF.
*   **R2 (ABR - Area Border Router):** Connects Area 0 and Area 1, performs route summarization, and enforces ZBF security policies.
*   **R3 (Branch Router):** Connects to Area 1, a local LAN, and simulates multiple branch networks via Loopback interfaces.

### ⚙️ Key Configurations Implemented
*   **Multi-Area OSPF:** Configured Area 0 (Backbone) and Area 1 (Branch) to dynamically share routes across all routers.
*   **Route Summarization:** Configured the ABR (R2) to summarize multiple branch loopback routes into a single `172.16.0.0/16` advertisement to optimize routing tables.
*   **Zone-Based Firewall (ZBF):** Configured security zones on R2 to inspect and block traffic from the OUTSIDE (Internet) to the INSIDE (LAN), while allowing internal users to access the internet.
*   **NAT Overload (PAT) & Default Routing:** Configured R1 to translate internal IPs to a public IP and propagate a default route via OSPF.

### 🧪 Verification & Screenshots
*(Drag and drop your screenshots here)*

*   **OSPF Summarization:** Verified on R1 using `show ip route`, showing the summarized `172.16.0.0/16` route.
*   **NAT Overload:** Verified internal PC (`192.168.100.10`) successfully pinging `8.8.8.8` and checked the NAT translation table.
*   **Firewall Block:** Simulated an external attack using `ping 192.168.100.10 source fa0/0` from R1, which was successfully dropped by the ZBF on R2.
