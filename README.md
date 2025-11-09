# SoftEther VPN and SCCM Remote Domain Lab

A virtual IT lab project using Windows Server 2022, SoftEther VPN, and SCCM for remote domain setup, client management, and connectivity testing.

Overview

🧩 Lab Overview Diagram

 Virtual Network Topology
 - Shows how the Domain Controller, VPN Server, and Remote Client are connected.

 SoftEther VPN and SCCM Workflow
 - Visual overview of how VPN and SCCM interact for remote client management.
 
 SoftEther VPN Server Configuration
- Displays Virtual Hub, Bridge, and connected VPN Client setup.


<p align="center">
  <img src="https://github.com/eanyanwu006/softether-ad-sccm-lab/raw/main/A_flowchart_infographic_titled_SoftEther_VPN.jpg" alt="SoftEther VPN and SCCM Workflow" width="800">
</p>



This project demonstrates how to build a remote management environment using **SoftEther VPN, Active Directory (AD), and System Center Configuration Manager (SCCM).  
It simulates how an IT administrator manages Windows clients from a corporate network while the clients connect securely over the internet through a VPN.

Objectives

- Set up a Windows Server 2022 domain controller.  
- Configure SoftEther VPN for remote access.  
- Join a remote Windows client to the domain.  
- Deploy and test SCCM client installation remotely.  
- Validate connectivity, file sharing, and management.

 Lab Environment

Platform: VMware Workstation  

Virtual Machines
1. DC01 (Domain Controller)
- OS: Windows Server 2022  
- Roles: AD DS, DNS, DHCP, SCCM  
- Domain: netfusion.internal  
- IP: 192.168.140.128  

2. VPN Server
- OS: Windows 10 / Server  
- Software: SoftEther VPN Server Manager  
- Features: Dynamic DNS, SecureNAT, Local Bridge to internal LAN  

 3. Remote Client
- OS: Windows 10 Enterprise  
- Connects via: SoftEther VPN Client  
- Domain Joined To: netfusion.internal  


Configuration Steps

1. Domain Setup
- Installed and configured **Active Directory Domain Services (AD DS).  
- Added DNS and **DHCP** roles.  
- Created domain users and groups.  

 2. SoftEther VPN Setup
- Installed SoftEther VPN Server.  
- Created a **Virtual Hub named `CprpVPN`.  
- Enabled SecureNAT and Dynamic DNS (`vpn303926760.softether.net`).  
- Bridged the Virtual Hub to the internal LAN adapter.  
- Created a VPN user and verified the connection from the remote client.  

3. Domain Join via VPN
- Verified connectivity to `DC01.netfusion.internal`.  
- Joined the external client to the domain successfully.  
- Logged in using domain credentials after restarting.  

4. File and Folder Access
- Accessed shared folders such as:  

\DC01.netfusion.internal\New_Boot

- Adjusted permissions to allow remote domain users access.  

5. SCCM Client Installation
- Installed and configured SCCM on `DC01`.  
- Pushed the SCCM client to the remote system.  
- Verified installation and site assignment.  

Troubleshooting Highlights

- Fixed the Virtual Hub does not exist error by correcting the hub name in SoftEther.  
- Resolved domain login issues by reconnecting to the VPN.  
- Adjusted share permissions for remote access.  
- Confirmed DNS resolution and connectivity before SCCM deployment.  

Tools and Technologies

- VMware Workstation  
- Windows Server 2022  
- Windows 10 / 11 Enterprise  
- SoftEther VPN  
- Active Directory / DNS / DHCP  
- System Center Configuration Manager (SCCM/MECM)  


Skills Demonstrated

- VPN configuration and NAT traversal  
- Windows Server administration  
- Active Directory and DNS management  
- SCCM client deployment and troubleshooting  
- Network connectivity and permission control  

Validation Checklist

| Task | Status |
|------|---------|
| VPN connection established | ✅ |
| Domain join successful | ✅ |
| File share access confirmed | ✅ |
| SCCM client deployed remotely | ✅ |
| Communication between client and DC verified | ✅ |


Screenshots (To Add Later)

You can upload your screenshots to a folder named `/images`, then reference them like this:

```markdown
![VPN Connection](images/vpn-connected.jpg)
![Domain Join](images/domain-join.jpg)
![SCCM Client Install](images/sccm-install.jpg)


Next Steps
	•	Add WSUS and Group Policy integration.
	•	Automate VPN connection setup using PowerShell.
	•	Expand the lab with additional site servers for redundancy.

Author
Emmanuel Anyanwu
GitHub Profile

  
