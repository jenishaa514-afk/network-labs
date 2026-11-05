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

### 📝 Cisco IOS Configuration (R1)
Here is the core routing and NAT configuration applied to the edge router to establish internet connectivity.

```text
! 1. Configure the WAN Interface (DHCP Client)
interface FastEthernet0/0
 ip address dhcp
 ip nat outside
 no shutdown
 exit

! 2. Configure the LAN Interface
interface FastEthernet1/0
 ip address 192.168.10.1 255.255.255.0
 ip nat inside
 no shutdown
 exit

! 3. NAT Overload (PAT) Configuration
access-list 1 permit 192.168.10.0 0.0.0.255
ip nat inside source list 1 interface FastEthernet0/0 overload

! 4. Save Configuration
write memory

<img width="895" height="595" alt="image" src="https://github.com/user-attachments/assets/9f5a547c-fa7d-4f3a-a140-f045e242ac39" />




### 🧪 Verification & Testing
*   **Internal Connectivity:** Successfully verified that internal virtual PCs could communicate with each other and the default gateway.
*   **Internet Connectivity:** Executed a ping to Google's public DNS (`8.8.8.8`) from an internal PC (`192.168.10.10`). The successful replies prove that the Cisco router is correctly translating the private IP to the public IP. 

```text
PC1> ip 192.168.10.10/24 192.168.10.1
Checking for duplicate address...
PC1 : 192.168.10.10 255.255.255.0 gateway 192.168.10.1

PC1> ping 8.8.8.8
84 bytes from 8.8.8.8 icmp_seq=1 ttl=109 time=187.520 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=109 time=135.935 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=109 time=121.728 ms
84 bytes from 8.8.8.8 icmp_seq=4 ttl=109 time=123.839 ms
84 bytes from 8.8.8.8 icmp_seq=5 ttl=109 time=124.008 ms

<img width="895" height="595" alt="image" src="https://github.com/user-attachments/assets/5156bdf1-f955-4cdb-affe-cea0ab1b8709" />


*   **NAT Table Verification:** Ran the `show ip nat translations` command on the router. This output verifies that the router is actively mapping the internal private address to the public edge address.

R1# show ip nat translations
Pro Inside global      Inside local       Outside local      Outside global
icmp 192.168.42.231:1  192.168.10.10:1    8.8.8.8:1          8.8.8.8:1
icmp 192.168.42.231:2  192.168.10.10:2    8.8.8.8:2          8.8.8.8:2
icmp 192.168.42.231:3  192.168.10.10:3    8.8.8.8:3          8.8.8.8:3

<img width="895" height="595" alt="image" src="https://github.com/user-attachments/assets/a7f3a473-1a68-4126-a613-0132ebee4f60" />




### 🐧 Linux Server Integration (Ubuntu)
To simulate a real-world enterprise setup, I installed and configured an Ubuntu Server to act as an ISC-DHCP-Server.

*   **Configuration:** Edited the `/etc/dhcp/dhcpd.conf` file to define the `192.168.10.0/24` subnet, IP ranges, default gateway, and DNS servers.
*   **Service Management:** Restarted the `isc-dhcp-server` and verified its active running status using `systemctl`.

**DHCP Scope Configuration:**
```text
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.100 192.168.10.200;
  option routers 192.168.10.1;
  option domain-name-servers 8.8.8.8;
}
linux-srv:~# systemctl status isc-dhcp-server
● isc-dhcp-server.service - ISC DHCP IPv4 server
   Loaded: loaded (/lib/systemd/system/isc-dhcp-server.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2026-11-05 10:00:00 UTC; 1h ago


### 🚧 Challenges Overcame
*   **Virtualization Troubleshooting:** Encountered and resolved complex GNS3 VM connection issues, including VirtualBox Host-Only adapter failures, Windows Firewall blocks, and antivirus interference.
*   **Console Emulation:** Configured custom terminal applications (Solar-PuTTY) to fix missing console path errors in the GNS3 GUI.
*   **Cloud Node Limitations:** Successfully pivoted from relying on a flaky Cloud Node Wi-Fi bridge to a stable GNS3 NAT Node to ensure the final routing project was completed and verified.

*   ### 📦 Project Files
👉 **[Download the GNS3 Project File (NAT-test.zip)](NAT-test.zip)**
