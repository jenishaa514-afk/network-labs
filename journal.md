Day 1 & 2 (GNS3 Setup & First Ping)
Goal: Install GNS3, resolve virtualization errors, and establish basic connectivity between a router and a PC.

✅ What I Accomplished Today:

Installed GNS3 and VirtualBox.

Successfully imported the Cisco c7200 IOS router image into GNS3 (allocated 3 FastEthernet slots).

Deployed my first lab topology (1 Router + 1 VPCS).

Configured the router's interface (FastEthernet 0/0) with IP 192.168.1.1.

Assigned IP 192.168.1.10 to PC1.

Successfully pinged the router from PC1! (End-to-end connectivity works).

🚧 Challenges Faced & Solutions:

VirtualBox VM Issues: GNS3 would not connect to the GNS3 VM. The client and server versions were mismatched (2.2.61 vs 2.2.58).

Solution: Uninstalled GNS3 and VirtualBox VM entirely. Switched to the GNS3 Local Server instead, which is much lighter and fixed all connection issues.

File Path Errors: GNS3 was looking for files on the D: drive.

Solution: Manually updated the paths in Preferences to C:\Program Files\GNS3\.

The Uppercase Bug: The server wouldn't start because the file was named gns3server.EXE.

Solution: Renamed it to gns3server.exe (lowercase).

🧠 Key Commands Learned:

Router config: enable, configure terminal, interface fa0/0, ip address 192.168.1.1 255.255.255.0, no shutdown.

VPCS config: ip 192.168.1.10/24, ping 192.168.1.1.

🎯 Plan for Tomorrow:

Add an Ethernet Switch between the router and PC.

Create VLAN 10 (Office) and VLAN 20 (Server).

Test that PCs in different VLANs cannot ping each other without routing.

Configure "Router-on-a-Stick" (sub-interfaces) to allow the VLANs to talk to each other.



