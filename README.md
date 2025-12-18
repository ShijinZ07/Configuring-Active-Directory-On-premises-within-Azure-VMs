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

- Windows Server 2025 Datacenter: Azure Edition
- Windows 11 Pro, 25H2

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resources
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users

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
<p>
  Create Second VM for Client using the same resource group and Vnet used for DC client:
  <img src="https://i.imgur.com/uXzmcUe.png" height="75%" width="75%" alt="Client"/>
</p>
<p>
  Make sure both VM's are in the same Vnet by checking out the Topology tab:
  <img src="https://i.imgur.com/Scx92dU.png" height="75%" width="75%" alt="Topology"/>
<h3 align="center">Check for connectivity between DC and Client VM's</h3>
<br />
<p>
  Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t (perpetual ping):
  <img src="https://i.imgur.com/GlQ5R9Q.png" height="75%" width="75%" alt="Ping"/>
</p>
<p>
  Enable DC-1 VM to respond to pings from Client-1 VM on the Vnet:
  <img src="https://i.imgur.com/h9m2mUS.png" height="75%" width="75%" alt="Ping"/>
</p>
<p>
  Check back with Client-1 VM to see if ping is now successful:
  <img src="https://i.imgur.com/Q4YVJ0q.png" height="75%" width="75%" alt="Ping"/>
</p>
  
