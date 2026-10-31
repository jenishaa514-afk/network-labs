### Project 3: Multi-Area OSPF, ZBF & NAT
Architected an enterprise multi-area OSPF network featuring a Zone-Based Firewall, route summarization, and NAT Overload.
👉 **[View Project 3 Documentation and Files](./Project-3-OSPF/README.md)**

### 📌 Objective
Designed and deployed an enterprise multi-area OSPF network featuring a Zone-Based Firewall (ZBF), route summarization, NAT Overload, and a simulated external threat to verify firewall rules.

### 🗺️ Topology Architecture
*   **R1 (Edge Router):** Connects to the internet via a simulated ISP (NAT Node). Originates the default route into OSPF.
*   **R2 (ABR - Area Border Router):** Bridges Area 0 (Backbone) and Area 1 (Branch). Enforces ZBF security policies and performs route summarization.
*   **R3 (Branch Router):** Connects to Area 1, the local LAN, and simulated branch networks via Loopback interfaces.
*   **Hacker-PC:** A simulated external threat connected to the ISP network.

  <img width="797" height="298" alt="Proj3 drawio" src="https://github.com/user-attachments/assets/922b97c5-bf41-44e5-b793-eedccc5f16d1" />


### ⚙️ Key Configurations Implemented
*   **Multi-Area OSPF & Summarization:** Configured Area 0 and Area 1. R2 summarizes the branch loopback networks (`172.16.10.0/24` and `172.16.20.0/24`) into a single `172.16.0.0/16` route.
*   **Zone-Based Firewall (ZBF):** Configured `INSIDE` and `OUTSIDE` security zones on R2. Applied an inspect policy to block outside traffic from reaching the internal LAN.
*   **NAT Overload (PAT) & Default Route:** Configured R1 to translate internal private IPs to its public WAN IP and used `default-information originate` to push the default route down to R2 and R3 via OSPF.

---

### 📸 Proof of Working Configuration

#### 1. OSPF Route Summarization
R1 successfully receives the summarized `172.16.0.0/16` route from R2 (notice the `O IA` code), and the default route (`S* 0.0.0.0/0`).
<img width="895" height="595" alt="image" src="https://github.com/user-attachments/assets/f4e883bb-c713-45dd-b572-ed80464ddee4" />


#### 2. The Firewall Block (Hacker-PC Attack)
An external Hacker-PC (`192.168.42.100`) attempts to attack the internal PC (`192.168.100.10`). 
*   **Result:** The ping times out. The Zone-Based Firewall on R2 successfully dropped the malicious packets.
<img width="895" height="595" alt="image" src="https://github.com/user-attachments/assets/b8bf9fbe-554e-4909-8fb4-732557cc9890" />


#### 3. The Firewall Allow (Hacker-PC to Edge)
To prove the network is actually routing, the Hacker-PC successfully pings R1's WAN interface (`192.168.42.231`). The ZBF is specifically blocking access to the *inside* network, not the edge.
<img width="895" height="595" alt="image" src="https://github.com/user-attachments/assets/a1968dc4-ff27-4159-8f35-73c5da9fdaa2" />


#### 4. NAT Overload (PAT) Translation
Internal PC1 (`192.168.100.10`) successfully pings `8.8.8.8`. The `show ip nat translations` command on R1 confirms the private IP is being translated to R1's public IP.
<img width="895" height="595" alt="image" src="https://github.com/user-attachments/assets/212c0049-3e73-492e-91f8-daf0971b1a17" />


  ### 📦 Project Files
👉 **[View Project 3 Documentation and Files](./Project-3-OSPF/README.md)**
