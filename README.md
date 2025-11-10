# SoftEther VPN and SCCM Remote Domain Lab

A virtual IT lab project using **Windows Server 2022**, **SoftEther VPN**, and **SCCM** for remote domain setup, client management, and connectivity testing.

---

## Overview

<p align="center">
  <img src="https://github.com/eanyanwu006/softether-ad-sccm-lab/raw/main/A_flowchart_infographic_titled_SoftEther_VPN.jpg" alt="SoftEther VPN and SCCM Workflow" width="800">
</p>

This project demonstrates how I built a remote management environment using **SoftEther VPN**, **Active Directory (AD)**, and **System Center Configuration Manager (SCCM)**.  
It simulates how an IT administrator manages Windows clients from a corporate network while clients connect securely over the internet through a VPN.

---

## Objectives

- Set up a Windows Server 2022 domain controller  
- Configure SoftEther VPN for remote access  
- Join a remote Windows client to the domain  
- Deploy and test SCCM client installation remotely  
- Validate connectivity, file sharing, and management  

---

## Lab Environment

**Platform:** VMware Workstation  

### Virtual Machines

#### 1. DC01 (Domain Controller)
- **OS:** Windows Server 2022  
- **Roles:** AD DS, DNS, DHCP, SCCM  
- **Domain:** netfusion.internal  
- **IP:** 192.168.140.128  

#### 2. VPN Server
- **OS:** Windows 10  
- **Software:** SoftEther VPN Server Manager  
- **Features:** Dynamic DNS, SecureNAT, Local Bridge to internal LAN  

#### 3. Remote Client
- **OS:** Windows 10 Enterprise  
- **Connects via:** SoftEther VPN Client  
- **Domain Joined To:** netfusion.internal  

---

## Configuration Steps

### 1. Domain Setup
- Installed and configured **Active Directory Domain Services (AD DS)**  
- Added **DNS** and **DHCP** roles  
- Created domain users and groups  

### 2. SoftEther VPN Setup and Architecture

#### How I Set Up the SoftEther VPN Server
1. **Installed the VPN Server**  
   - Downloaded the SoftEther VPN Server package from the official site  
   - Installed it on a Windows 10 VM that served as the VPN gateway  

2. **Created a Virtual Hub**  
   - Opened *SoftEther VPN Server Manager* and connected to *localhost*  
   - Created a new Virtual Hub named `CprpVPN`  
   - Added a VPN user (`vpnuser`) and set a strong password  

3. **Enabled SecureNAT and Dynamic DNS**  
   - Turned on *SecureNAT* for internal routing  
   - Enabled *Dynamic DNS* with hostname:  
     `vpn303926760.softether.net`  

4. **Created a Local Bridge Connection**  
   - Selected the internal network adapter connected to my lab (for example `VMnet1`)  
   - Bridged the Virtual Hub to the internal LAN to allow domain traffic  

5. **Allowed Remote Access**  
   - Verified ports *UDP 500*, *UDP 4500*, and *TCP 443* were open  
   - Confirmed *NAT Traversal* was active  

6. **Tested the Connection**  
   - Installed *SoftEther VPN Client* on my external PC  
   - Connected using the DDNS hostname and verified connectivity  
   - Successfully pinged the Domain Controller and accessed shared folders  

---

### Understanding My VPN Architecture

After configuring SoftEther, I reviewed how each component worked:

- **Virtual Hub**  
  Acts as a virtual switch that handles VPN sessions and routes traffic between connected clients  

- **Local Bridge**  
  Connects the Virtual Hub to the LAN where the Domain Controller is hosted  
  Allows VPN clients to appear as if they’re physically connected to the same network  

- **SecureNAT**  
  Provides internal routing and DHCP when no physical router is available  

- **Dynamic DNS (DDNS)**  
  Keeps the VPN reachable through a static hostname like `vpn303926760.softether.net`, even when the IP changes  

- **NAT Traversal**  
  Enables connections through firewalls and routers without needing manual port forwarding  

---

### VPN Architecture Diagram

<p align="center">
  <img src="https://github.com/eanyanwu006/softether-ad-sccm-lab/raw/main/A_diagram_titled_Understanding_My_VPN_Architecture.jpg" alt="Understanding My VPN Architecture" width="800">
</p>

**Diagram:**  
This diagram shows how the SoftEther VPN connects remote clients to the internal domain network.  
The Virtual Hub, Local Bridge, SecureNAT, DDNS, and NAT Traversal features all work together to create a seamless connection between remote systems and the domain controller.

---

### 3. Domain Join via VPN
- Verified connectivity to `DC01.netfusion.internal`  
- Joined the external client to the domain successfully  
- Logged in using domain credentials after restart  

<p align="center">
  <img src="https://github.com/eanyanwu006/softether-ad-sccm-lab/raw/main/A_digital_screenshot_displays_a_Domain_Join_operation.jpg" alt="External Client Domain Join" width="700">
</p>

---

### 4. File and Folder Access
- Accessed shared folders such as `\\DC01.netfusion.internal\New_Boot`  
- Adjusted permissions for remote users  

### 5. SCCM Client Installation
- Installed and configured **SCCM** on DC01  
- Pushed SCCM client to the remote machine  
- Verified installation and client communication  

<p align="center">
  <img src="https://github.com/eanyanwu006/softether-ad-sccm-lab/raw/main/A_digital_diagram_illustrates_SCCM_communication_b.jpg" alt="SCCM Communication Over VPN" width="800">
</p>

---

## Troubleshooting Highlights

- Fixed **“Virtual Hub does not exist”** by correcting the hub name  
- Reconnected VPN to resolve domain login issues  
- Adjusted share permissions for remote access  
- Verified DNS resolution and SCCM connectivity  

---

## Tools and Technologies

- VMware Workstation  
- Windows Server 2022  
- Windows 10 / 11 Enterprise  
- SoftEther VPN  
- Active Directory / DNS / DHCP  
- System Center Configuration Manager (SCCM)  

---

## Skills Demonstrated

- VPN setup and NAT traversal  
- Windows Server administration  
- AD and DNS management  
- SCCM client deployment and troubleshooting  
- Secure remote network configuration  

---

## Validation Checklist

| Task | Status |
|------|---------|
| VPN connection established | ✅ |
| Domain join successful | ✅ |
| File share access confirmed | ✅ |
| SCCM client deployed remotely | ✅ |
| Communication between client and DC verified | ✅ |

---

## Next Steps

- Add WSUS and Group Policy for patching  
- Automate VPN setup with PowerShell  
- Add more site servers and clients for scalability  

---

## Author

**Emmanuel Anyanwu**  
[GitHub Profile](https://github.com/eanyanwu006)
