<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" height="40%" width="70%"alt="Microsoft Active Directory Logo"/>
</p>

<h1>Configuring Active Directory(On Premises) Within Azure</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server
- Windows 

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resources
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users on Client-1
- Create a manu additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>
<br />
<br />
<h3 align="center">Setup Resources in Azure</h3>
<br />
<p>
  Setup a resource group called "RG-AD”:
  <img src="https://i.imgur.com/nYaaMsE.png" height="75%" width="75%" alt="resource group"/>
</p>
<p>
  Create a Virtual Nerwork(Vnet) so our VM's can talk to each other and call it "AD-VNet”:
  <img src="https://i.imgur.com/hVrFNnw.png" height="75%" width="75%" alt="Vnet"/>
</p>
<p>
  Create first VM first one being the domain controller and call it "DC-1":
  <img src="https://i.imgur.com/1ObiyJA.png" height="75%" width="75%" alt="DC1"/>
</p>
<p>
  After creating VM for domain controller were going to set the ip of it to static:
  <img src="https://i.imgur.com/DjSTChh.png" height="75%" width="75%" alt="Static"/>
</p>

