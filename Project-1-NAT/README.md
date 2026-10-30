# CCNA & Linux Practical Projects
Welcome to my network engineering portfolio! This repository contains my hands-on labs covering Cisco routing, switching, security, and Linux integration.
---

## Project 1: Enterprise Edge Routing, NAT & Hybrid Linux Integration

### 📌 Project Objective
The goal of this project was to build a fully functional enterprise edge network in GNS3. This lab demonstrates how to connect an internal Cisco network to the internet using **NAT (Network Address Translation)**. Additionally, a Linux server (Ubuntu) was deployed to simulate a centralized DHCP server for the internal network.


### 🗺️ Topology Overview
*   **Internal Network:** 3 Virtual PCs connected to a Cisco Switch, routed through a Cisco 7200 Router.
*   **Edge/WAN:** The router connects to a GNS3 NAT Cloud to simulate an ISP connection.
*   **Linux Server:** An Ubuntu VM acting as a DHCP Server for the `192.168.10.0/24` network.


<img width="613" height="408" alt="Project1_Topology" src="https://github.com/user-attachments/assets/61a55e71-1f1c-4b92-88f4-426f00ff046b" />


### ⚙️ Cisco Router Configurations Implemented
To achieve internet connectivity, the following configurations were applied to the Cisco Router (R1):

*   **WAN DHCP Client:** Configured the external-facing interface (`Fa0/0`) to dynamically obtain a public IP address from the simulated ISP.
*   **Access Control Lists (ACLs):** Created a standard ACL to identify and permit internal LAN traffic (`192.168.10.0/24`).
*   **NAT Overload (PAT):** Configured Port Address Translation, allowing multiple internal private IPs to share the single public WAN IP for internet access.
*   **Interface Roles:** Explicitly defined the LAN interface (`Fa1/0`) as `ip nat inside` and the WAN interface (`Fa0/0`) as `ip nat outside`.

<img width="895" height="594" alt="image" src="https://github.com/user-attachments/assets/1febdcc5-32bf-447c-a323-dd020bb5eee8" />


### 🧪 Verification & Testing
*   **Internal Connectivity:** Successfully verified that internal virtual PCs could communicate with each other and the default gateway.
*   **Internet Connectivity:** Executed a ping to Google's public DNS (`8.8.8.8`) from an internal PC (`192.168.10.10`). The successful replies prove that the Cisco router is correctly translating the private IP to the public IP. 

<img width="895" height="594" alt="image" src="https://github.com/user-attachments/assets/14bb5846-1ecb-43ed-910e-aa5e6643ffdf" />

*   **NAT Table Verification:** Ran the `show ip nat translations` command on the router. This output verifies that the router is actively mapping the internal private address to the public edge address.
*   <img width="895" height="594" alt="image" src="https://github.com/user-attachments/assets/3a672b05-1515-456d-b492-09992934ac5b" />



### 🐧 Linux Server Integration (Ubuntu)
To simulate a real-world enterprise setup, I installed and configured an Ubuntu Server to act as an ISC-DHCP-Server.

*   **Configuration:** Edited the `/etc/dhcp/dhcpd.conf` file to define the `192.168.10.0/24` subnet, IP ranges, default gateway, and DNS servers.
*   **Service Management:** Restarted the `isc-dhcp-server` and verified its active running status using `systemctl`.
*   


### 🚧 Challenges Overcame
*   **Virtualization Troubleshooting:** Encountered and resolved complex GNS3 VM connection issues, including VirtualBox Host-Only adapter failures, Windows Firewall blocks, and antivirus interference.
*   **Console Emulation:** Configured custom terminal applications (Solar-PuTTY) to fix missing console path errors in the GNS3 GUI.
*   **Cloud Node Limitations:** Successfully pivoted from relying on a flaky Cloud Node Wi-Fi bridge to a stable GNS3 NAT Node to ensure the final routing project was completed and verified.

*   ### 📦 Project Files
👉 **[Download the GNS3 Project File (NAT-test.zip)](NAT-test.zip)**
