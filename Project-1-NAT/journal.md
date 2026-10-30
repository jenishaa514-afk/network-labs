# 📓 Network Engineering Lab Journal

## Project 1: Enterprise Edge Routing & NAT (GNS3)

### Day 1 & 2: GNS3 Setup & Virtualization Troubleshooting
*   **Goal:** Install GNS3, configure VirtualBox, and establish the first network topology.
*   **What I Did:** 
    *   Installed GNS3, VirtualBox 7.0.26, and imported the GNS3 VM. 
    *   Imported the Cisco `c7200` IOS router image.
    *   Built a basic topology (1 Router + 1 VPCS) and successfully pinged the router interface.
*   **Challenges & Solutions:**
    *   *Issue:* Persistent `HAXM acceleration not installed` and QEMU image crashes.
    *   *Solution:* Unchecked "Require hardware acceleration" in QEMU settings and switched to software emulation. 
    *   *Issue:* VirtualBox network adapter (`vboxnet0`) missing, preventing the VM from starting.
    *   *Solution:* Manually created the Host-Only adapter via VirtualBox Host Network Manager.
    *   *Issue:* GNS3 GUI refused to connect to the Local Server (`WinError 10053`).
    *   *Solution:* Disabled the Local Server entirely and forced GNS3 to use the GNS3 VM via Remote Server (`192.168.56.101`).

### Day 3 & 4: Linux Server Deployment (Ubuntu / Alpine)
*   **Goal:** Deploy a Linux VM to act as a centralized DHCP server for the Cisco network.
*   **What I Did:**
    *   Attempted to deploy Alpine Linux in QEMU (encountered multiple file missing and bootloader errors).
    *   Pivoted to deploying an **Ubuntu Server VM** inside VirtualBox on a secondary laptop.
    *   Configured Netplan (`/etc/netplan/00-installer-config.yaml`) to assign a static IP (`192.168.31.150`).
    *   Installed `isc-dhcp-server` and created a DHCP scope for `192.168.10.0/24`.
*   **Challenges & Solutions:**
    *   *Issue:* The GNS3 Cloud Node consistently dropped packets when bridged to the host Windows Wi-Fi adapter, preventing the Cisco routers from reaching the Linux VM.
    *   *Solution:* Documented the Linux DHCP configuration as a standalone service. (Plan to integrate natively in future projects using GNS3 VM host-only adapters instead of the physical Wi-Fi bridge).

### Day 5: Console Application Fixes
*   **Goal:** Fix terminal issues preventing access to Cisco devices.
*   **What I Did:**
    *   Encountered the "Could not start telnet console Solar-PuTTY" error.
    *   Downloaded Solar-PuTTY directly and manually mapped the executable path in GNS3 Preferences (`C:\...\Solar-PuTTY.exe --telnet --hostname %h --port %p --name "%d"`).
    *   Successfully opened the Cisco IOS command-line interface.

### Day 6: NAT Overload (PAT) & Real Internet Connectivity
*   **Goal:** Connect the virtual Cisco network to the real internet using a GNS3 NAT Node.
*   **What I Did:**
    *   Added a NAT Node to the topology and connected it to the Router's `FastEthernet 0/0`.
    *   Configured the WAN interface as a DHCP client (`ip address dhcp`), successfully obtaining an IP (`192.168.42.x`).
    *   Configured an Access Control List (ACL) to permit the internal `192.168.10.0/24` network.
    *   Configured **NAT Overload (PAT)** to translate internal private IPs to the router's public WAN IP.
    *   Applied `ip nat inside` and `ip nat outside` to the correct interfaces.
*   **The Big Win:** 
    *   Successfully pinged Google's public DNS (`8.8.8.8`) from PC1 (`192.168.10.10`).
    *   Verified translations using `show ip nat translations`.
    *   Created the final network diagram in Draw.io and documented the project in `README.md`.
    *   Exported the `.zip` project file and uploaded it to GitHub.
