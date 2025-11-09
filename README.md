# SoftEther VPN and SCCM Remote Domain Lab

A virtual IT lab project using Windows Server 2022, SoftEther VPN, and SCCM for remote domain setup, client management, and connectivity testing.

---

## Overview

This project shows how to build a remote management setup using:

- SoftEther VPN  
- Active Directory (AD)  
- System Center Configuration Manager (SCCM)

The goal is to manage a Windows client from a “corporate” network while the client connects over the internet by VPN.

---

## Lab Overview Diagram

### Virtual Network Topology

- Shows how the **Domain Controller**, **VPN Server**, and **Remote Client** are connected.

### SoftEther VPN and SCCM Workflow

- Visual overview of how VPN and SCCM work together for remote client management.

### SoftEther VPN Server Configuration

- Virtual Hub, local bridge, and connected VPN client.

(Images are in the repo as `.jpg` files.)

---

## Objectives

- Set up a Windows Server 2022 domain controller  
- Configure SoftEther VPN for remote access  
- Join a remote Windows client to the domain  
- Deploy and test SCCM client installation remotely  
- Confirm connectivity, file sharing, and management end-to-end  

---

## Lab Environment

**Platform:** VMware Workstation

### Virtual Machines

1. **DC01 (Domain Controller)**  
   - OS: Windows Server 2022  
   - Roles: AD DS, DNS, DHCP, SCCM  
   - Domain: `netfusion.internal`  
   - IP: `192.168.140.128`  

2. **VPN Server**  
   - OS: Windows 10 / Windows Server  
   - Software: SoftEther VPN Server Manager  
   - Features: Dynamic DNS, SecureNAT, local bridge to internal LAN  

3. **Remote Client**  
   - OS: Windows 10 Enterprise  
   - Connects with: SoftEther VPN Client  
   - Domain joined to: `netfusion.internal`  

---

## How to Reproduce This Lab

### 1. Build the VMs

- Create three VMs in VMware Workstation  
- Place DC01 and the VPN server on the same internal network (for example `VMnet1`)  
- Give the VPN server a second NIC bridged to your physical network for internet access  

### 2. Configure the Domain Controller (DC01)

- Install **Active Directory Domain Services**  
- Promote DC01 to a new forest: `netfusion.internal`  
- Add **DNS** and **DHCP** roles  
- Create at least one domain user account for testing  

### 3. Set Up SoftEther VPN Server

On the VPN server VM:

- Install **SoftEther VPN Server**  
- Create a **Virtual Hub** named `CprpVPN`  
- Enable:
  - Dynamic DNS (for example: `vpn303926760.softether.net`)  
  - SecureNAT  
- Create a **Local Bridge** from the Virtual Hub to the internal LAN adapter that reaches DC01  
- Create a VPN user (for example `vpnuser`) with a strong password  

### 4. Configure the Remote Client

On the external client PC (outside the lab network):

- Install **SoftEther VPN Client**  
- Add a new connection that points to your DDNS name  
  - Host name: `vpn303926760.softether.net`  
  - Port: `443`  
  - VPN type: “Direct TCP/IP Connection”  
  - User: `vpnuser`  
- Connect and confirm:
  - You can ping `192.168.140.128`  
  - You can ping `DC01.netfusion.internal` by name  

### 5. Join the Client to the Domain

On the remote client:

- While VPN is connected:
  - Change computer name and domain to `netfusion.internal`  
  - Use domain admin credentials to join  
- Restart and log in with a domain account  
- Confirm:
  - `whoami` shows `netfusion\username`  
  - `ping DC01.netfusion.internal` still works  

### 6. File and Folder Access

On DC01:

- Create or use an existing share (for example `\\DC01.netfusion.internal\New_Boot`)  
- Grant permissions to the domain user or a group  

On the remote client:

- While on VPN, open `\\DC01.netfusion.internal\New_Boot`  
- Confirm you can read/write as expected  

### 7. SCCM Client Installation

On DC01:

- Install and configure SCCM (single primary site in lab mode)  
- Add the remote client as a device resource  
- Use **“Install Client”** from the SCCM console to push the client

On the remote client:

- Confirm the **Configuration Manager** control panel applet appears  
- Check that the client is assigned to the correct site and is active  

---

## Troubleshooting Highlights

- **Virtual Hub does not exist**  
  - Fixed by using the correct Virtual Hub name (`CprpVPN`) in the VPN client settings  

- **Domain login not available**  
  - Solved by reconnecting the VPN so the client could reach DC01 during logon  

- **Access denied on file share**  
  - Resolved by adjusting NTFS and share permissions on `\\DC01\New_Boot`  

- **SCCM client not showing up**  
  - Checked DNS, firewalls, and pushed the client again from the SCCM console  

---

## Tools and Technologies

- VMware Workstation  
- Windows Server 2022  
- Windows 10 / 11 Enterprise  
- SoftEther VPN  
- Active Directory / DNS / DHCP  
- System Center Configuration Manager (SCCM / MECM)  

---

## Skills Demonstrated

- VPN setup and NAT traversal  
- Windows Server and domain services  
- AD and DNS design and troubleshooting  
- Remote client management with SCCM  
- Network testing, naming, and permission control  

---

## Validation Checklist

| Task                                    | Status |
|----------------------------------------|--------|
| VPN connection established             | ✅     |
| Domain join successful                 | ✅     |
| File share access confirmed            | ✅     |
| SCCM client deployed to remote client  | ✅     |
| Communication between client and DC    | ✅     |

---

## Screenshots

### SCCM Client Install

Image: `A_diagram_showcases_a_virtual_network_lab_topology.jpg`

### Domain Join

Image: `A_diagram_in_the_image_illustrates_the_SoftEther_VPN.jpg`

### VPN Connection

Image: `A_screenshot_of_SoftEther_VPN_Server_Manager_application.jpg`

(These are stored in the repo and can be embedded or viewed directly.)

---

## Next Steps

- Add WSUS and Group Policy for patching and policy control  
- Script VPN connection setup with PowerShell  
- Add more site servers and clients to grow the lab  

---

## Author

**Emmanuel Anyanwu**  
[GitHub Profile](https://github.com/eanyanwu006)
