## Project 2: Enterprise DMZ & Edge Security (GNS3)

### Day 7: Architecture Design & Linux Server Deployment
*   **Goal:** Build a 3-zone enterprise network (Inside LAN, Outside Internet, and a DMZ) to practice firewall security.
*   **What I Did:**
    *   Designed the new topology in GNS3: R1 connecting to a NAT node (WAN), Switch1 (LAN), and Switch2 (DMZ).
    *   Deployed an **Alpine Linux VM** in VirtualBox to act as the public-facing Web Server inside the DMZ.
    *   Solved the previous VirtualBox bridging issues by binding the GNS3 Cloud Node to the **VirtualBox Host-Only Adapter**. This created a stable, error-free virtual cable between GNS3 and the Linux VM.
*   **Challenges & Solutions:**
    *   *Issue:* The Linux server lost its IP address after rebooting the VM.
    *   *Solution:* Manually re-applied the static IP (`192.168.20.10`) and default gateway (`192.168.20.1`) using the `ip addr add` and `ip route add` commands.

### Day 8: Cisco Router Configuration & Extended ACLs
*   **Goal:** Configure the Cisco router to route traffic between all three zones and implement a strict firewall.
*   **What I Did:**
    *   Configured interfaces: `Fa0/0` (WAN DHCP), `Fa1/0` (LAN `192.168.10.1`), and `Fa2/0` (DMZ `192.168.20.1`).
    *   Re-applied NAT Overload (PAT) to allow internal PCs to access the internet.
    *   **The Firewall:** Created an Extended Access Control List (ACL 100) and applied it inbound on the WAN interface (`Fa0/0`).
    *   **The Rules:** 
        *   Permit traffic to the DMZ Web Server (`192.168.20.10`).
        *   Deny traffic to the Internal LAN (`192.168.10.0/24`).
        *   Permit all other traffic.

### Day 9: Security Testing & Verification
*   **Goal:** Prove that the DMZ architecture protects the internal network from external threats.
*   **What I Did:**
    *   Used the router to simulate an external attack (`ping 192.168.10.10 source f0/0`). 
    *   **Result:** 0% success rate. The ACL successfully blocked the simulated hacker from reaching the internal LAN.
    *   Simulated external access to the public server (`ping 192.168.20.10 source f0/0`).
    *   **Result:** 100% success rate. The ACL correctly allowed traffic to the DMZ.
    *   Ran `show access-lists` to verify the hit counters, proving the firewall was actively matching and dropping packets.
    *   Created a professional network diagram in Draw.io documenting the entire architecture.

*   **Key Takeaway:** Learned the importance of the DMZ architecture in enterprise security. By isolating public-facing servers from the internal LAN, a company can prevent a compromised web server from being used as a stepping stone to attack internal employee PCs.
