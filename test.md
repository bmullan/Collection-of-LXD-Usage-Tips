<center>
#CIAB
#*Secure Full Mesh VPN*
#*Virtual Data Center Solution*
by Brian Mullan   
(c) CIAB 2017-2025
</center>
<br>
<br>
### Table of Contents

---
<br>
###Pre-reqs:  
1. A **Registered DOMAIN NAME**   
2. DNS has been configured to point that *DOMAIN NAME* to the *IP Address* of your *Controller's Server*   
3. A Server for the **self-hosted Netbird VPN Controller**  
4. One Physical/Cloud  Instance, VM, or System Container (using Incus)   
5. *with a* ***Public IPv4*** *address* & minimum of 2 vcpu & 2-4GB Memory  
6. - Make the *Server Host Name* the ***root*** of the DOMAIN NAME  
<br>
> **Example:**    
> *If the* **DOMAIN NAME** is **ciab-meshvpn.org** then *name the HOST Server* **ciab-meshvpn**   

---
<br>
###**References**   
<br>
**[Netbird's own Self-hosting quickstart guide](https://docs.netbird.io/selfhosted/selfhosted-quickstart)**    
<br>
**[3rd Party Guide to Install & Setup a Self-Hosted Netbird Wireguard System - includes Youtube video](https://wiki.opensourceisawesome.com/books/netbird-with-wireguard/page/install-and-setup-the-netbird-wireguard-system)**   
<br>
---
<br>
###Open Source Software utilized and utilized      
<br>  
***Ubuntu LTS*** version of Linux installed with...  
1.  Incus - a next generation ***System*** and ***Application***  (re Docker) Containers, and **Virtual Machine** (VM) manager.  
2. Installed & Configured *Self-Hosted* **Netbird** *VPN Controller*  
3. **FRRouting  (FRR)** a free, open source Internet routing protocol suite for Linux and Unix platforms
<br>
---
<br>
###Terminology *as used in the Guide*

| ***Term*** |***Definition***  |
|--|--|
| **SERVER** | **can be a Physical Server, a Cloud Instance or VM** |
| **NODE** | **can be any** ***Server*** |
| **VM** | **Virtual Machine on physical server or Cloud Instance** |
| **BGP** | **Border Gateway Protocol**|
| **VTEP** | **Virtual Tunnel End Point** |
| **MANO** | **MANagement And Operations** |
| **CONTROLLER**| **in this document "controller" means the server configured as a Self-Hosted Netbird VPN Controller** |
| **PEER** | **Any NODE that is configured as a Member of the CIAB-MeshVPN** | 
| **UserID**| **Linux User Name** 

<br>
###Compute/Networking environment used
<br>
- Digital Ocean "Droplets" (ie compute instances)  
- Incus - "System" Containers, Docker Application Containers and VMs  
- All Node/Servers & Containers/VMs used Ubuntu 24.04 LTS was the OS  

CIAB MeshVPN supports the concept of ==***multi-tenants*** (***refer to Figure 1 below***)==.    

Each CIAB Mesh VPN Node/Server, also called a ==Host Node==  
- implements a "Compute" Environment consisting of any number of Incus System Containers and/or VMs   
- 1 LInux Bridge dedicated for each Tenant/Client to be supported by "that" Node.  
- Supports any number of different/distinct Tenants/Clients that will be referred to as Tenant1, Tenant2, Tenant3 etc  
- All of TenantX's Containers and VMs are attached to the TenantX Linux Bridge



Each CIAB Mesh VPN Node
dedicated to one or more Tenants/Clients.


### ==<<<<< Insert Architecture Image Here >>>>>==


<br>
###Installation and Configuration of the MANO CIAB-MeshVPN NODE  
<br>
The following steps are to be performed on the Server, Cloud Instance or VM you plan to utilize for your Mesh VPN Controller.
<br>
###Step 1    
*Create/Configure your Controller*  

**Login to your Server as Root**and execute the following:    
- **apt update && apt upgrade -y**  
- **apt install nala nano wget curl ufw jq wireguard terminator -y**   

*Create a new User* and give them sudo privileges   
**adduser UserID adm**    
**adduser UserID sudo**  
**su UserID**  
---
###Step 2  
Guides to Install and Configure Self-Hosted Netbird:  
1. **[Netbird's own Installation and Setup Guide for Self-Hosted Controller](https://docs.netbird.io/selfhosted/selfhosted-quickstart)**  
2. **[AwesomeSoftware's version of the above Guide](https://wiki.opensourceisawesome.com/books/netbird-with-wireguard/page/install-and-setup-the-netbird-wireguard-system)**

<br>
###Step 3  
 <TBD>
 
###Step -X  
*After the MANO-CIAB-meshvpn Netbird has been setup & Peers Configured successfully*, the following
steps need to be Netbird ROUTE config needs to be done for ***each Peer*** 

Config Netbird Route to HAWKS1'S default incus bridge network interface 
incusbr0's IP network CIDR.

- this will allow communication across the mesh to Hawks1 Incus Containers  
  and VMs from any Mesh PeerX  
- if that PeerX has a ROUTE set to its own Incus bridge network interface   
  CIDR then  
  -  HAWKS Containers/VMs can communicate to PeerX Containers/VMs


As CIAB-meshvpn admin create a SETUP KEY that can be used to start Netbird on any NODE  
and connect that NODE to the CIAB-MeshVPN
  
  Once the SETUP KEY is created then each PEER NODE just needs to execute:
  
> **$ netbird up --setup-key=ciab-meshvpn**

 --- 
###Step 5  
  ---
###Step 6  

###Step 7

###Step 8
