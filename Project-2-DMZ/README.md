## Project 2: Enterprise DMZ & Edge Security

Architected a secure network topology featuring an Inside LAN, an Outside Internet connection, and a DMZ hosting a Linux Web Server. Cisco Extended ACLs were configured to block outside threats from the LAN while allowing access to the DMZ.

👉 **[Click here to view the full Project 2 Documentation, Diagram, and Configurations](./Project-2-DMZ/README.md)**

### 📌 Objective
Architected a secure enterprise network topology in GNS3 featuring an Inside LAN, an Outside Internet connection, and a DMZ (Demilitarized Zone). The goal was to securely host a public-facing Linux Web Server while protecting the internal corporate network from external threats.

### 🗺️ Topology Architecture
*   **Edge Router (R1):** Cisco 7200 series managing traffic between three zones.
*   **Inside LAN:** Switch1 connecting internal employee PCs (PC1, PC2).
*   **Outside (Internet):** GNS3 NAT Node simulating the public internet.
*   **DMZ:** Switch2 connecting to a Cloud Node, which bridges to a VirtualBox Alpine Linux Server acting as a Web Server.

  <img width="660" height="337" alt="pro2 drawio" src="https://github.com/user-attachments/assets/5139d7ef-2a8d-49dd-a77e-ed46cc5b47aa" />



### ⚙️ Key Configurations Implemented
*   **Linux Web Server (Alpine):** Deployed in VirtualBox and bridged to GNS3 using a Host-Only Adapter. Configured with a static IP (`192.168.20.10`) and default gateway (`192.168.20.1`) using Linux CLI (`ip addr`, `ip route`).
*   **Cisco Router Interfaces:** Configured `Fa0/0` as a DHCP client (WAN), `Fa1/0` (`192.168.10.1`) for the LAN, and `Fa2/0` (`192.168.20.1`) for the DMZ.
*   **NAT Overload (PAT):** Configured dynamic NAT to allow internal LAN PCs to access the internet while hiding their private IP addresses.
*   **Extended ACL (Firewall):** Created a strict Access Control List (ACL 100) on the WAN interface (`Fa0/0`) to enforce security:
    1.  `permit ip any host 192.168.20.10`: Allows outside traffic to reach the public DMZ Web Server.
    2.  `deny ip any 192.168.10.0 0.0.0.255`: **Blocks** all outside traffic from reaching the internal LAN.
    3.  `permit ip any any`: Allows all other outbound traffic.
 
    <img width="895" height="595" alt="image" src="https://github.com/user-attachments/assets/b111f0dd-e387-4cdb-82ae-445ad57232da" />


### 🧪 Verification & Security Testing

 <img width="895" height="595" alt="image" src="https://github.com/user-attachments/assets/814299f4-6de1-460f-8fdc-8c3600947a25" />


Simulated an external attacker using the router's WAN interface (`source f0/0`) to test the firewall.
*   **Test 1 (Attack LAN):** `ping 192.168.10.10 source f0/0` resulted in **0% success** (timeouts). The ACL successfully blocked the external attacker.
*   **Test 2 (Access DMZ):** `ping 192.168.20.10 source f0/0` resulted in **100% success**. The ACL successfully allowed external access to the public Web Server.
*   Verified hit counters using `show access-lists` to confirm the firewall was actively matching and dropping packets.

  ### 📦 Project Files
👉 **[Download the GNS3 Project File (Project2_DMZ_Security.zip)](Project2_DMZ_Security.zip)**
