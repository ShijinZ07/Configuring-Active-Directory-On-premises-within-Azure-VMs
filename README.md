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
- Ensure Connectivity between the Domain Controller VM and client VM 
- Install Active Directory
- Create an Admin user and Normal Users Account in AD
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users on Client-1
- Generate additional users using a powershell script and attempt to log into client-1 with one of those users

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
  Enable DC-1 VM to respond to pings from Client-1 VM on the Vnet via the CLI:
  <img src="https://i.imgur.com/h9m2mUS.png" height="75%" width="75%" alt="Ping"/>
</p>
<p>
  Check back with Client-1 VM to see if ping is now successful:
  <img src="https://i.imgur.com/Q4YVJ0q.png" height="75%" width="75%" alt="Ping"/>
</p>
<br />
<h3 align="center">Setup Active Directroy in DC-1 VM</h3>
<br />
<p>
  RDP into DC-1 and install Active Directory:
  <img src="https://i.imgur.com/AF5Z00r.png" height="75%" width="75%" alt="AD"/>
</p>
<p>
  Promote to Domain Controller:
  <br />
  <img src="https://i.imgur.com/lgoWhOK.png" height="75%" width="75%" alt="Promote"/>
</p>
<p>
  Add a new forest(can be named whatever you want):
  <br />
  <img src="https://i.imgur.com/9DVGcBa.png" height="75%" width="75%" alt="Forest"/>
</p>
<p>
 The VM will restart and you will have to relog using the root domain name \yourusername:
  <br />
  <img src="https://i.imgur.com/QwD5a68.png" height="75%" width="75%" alt="Relog"/>
</p>
<h3 align="center">Creating an admind and a normal user in Active Directory</h3>
<br />
<p>
 Were going to go to Active Directo Users and Computers right-click our forest and create 2 organizational units _ADMINS and _EMPLOYEES :
  <br />
  <img src="https://i.imgur.com/tq6B4Jo.png" height="75%" width="75%" alt="OU's"/>
</p>
<p>
 Create a new user:
  <br />
  <img src="https://i.imgur.com/4tKEoHR.png" height="75%" width="75%" alt="New User"/>
</p>
<p>
 Add the user we created to Domain Admins Group:
  <br />
  <img src="https://i.imgur.com/KAL377w.png" height="75%" width="75%" alt="Domain Admin"/>
</p>
<p>
 Disconnect and relog-in with new created admin:
  <br />
  <img src="https://i.imgur.com/MsyugAr.png" height="75%" width="75%" alt="New Admin"/>
</p>
<br />
<h3 align="center">Joing Client-1's VM to Domain</h3>
<br />
<p>
 Change Vnets DNS to DC-1 VM's static IP:
  <br />
  <img src="https://i.imgur.com/UtuEytq.png" height="75%" width="75%" alt="VnetStatic"/>
</p>
<p>
 Were going to RDP into our Client-1 VM and join our domain using our new admin Credentials we created:
  <br />
  <img src="https://i.imgur.com/Yi5zv51.png" height="75%" width="75%" alt="JoinDomain"/>
</p>
<p>
 RDP into our DC-1 VM and verify Client-1 shows up in Active Directory Users and Computers(ADUC) inside the “Computers” folder on the root of the domain. Create a new OU named “_CLIENTS” and move Client-1 into there:
  <br />
  <img src="https://i.imgur.com/yYfKbth.png" height="75%" width="75%" alt="VerifyClient1"/>
</p>
<h3 align="center">Setup Remote Desktop for non-admin users on Client-1</h3>
<br />
<p>
 Log into Client-1 as DC1ActiveDirectory\shijin_admin and search up remote desktop settings ->remote desktop users -> add -> type-in domain users and check name and click ok:
  <br />
  <img src="https://i.imgur.com/3TYgecW.png" height="75%" width="75%" alt="DomainUsersRemote"/>
</p>
<h3 align="center">Generating a ton of users using a powershell script</h3>
<br />
<p>
 Log into Client-1 as shijin_admin and Open PowerShell_ise as an admin -> Create a new Script and copy and paste the contents of this script that generates random username and saves it to our _EMPLOYEES directory (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1):
  <br />
  <img src="https://i.imgur.com/PQ3lcZ9.png" height="75%" width="75%" alt="Script"/>
  </p>
<p>
Check the users added by the script are now in Active Directory:
  <br />
  <img src="https://i.imgur.com/eBxPv08.png" height="75%" width="75%" alt="AddedUsers"/>
</p>
<p>
Log-in Client-1 using the creditials created by the script to see if everything was done correctly(note the password is in the script):
  <br />
  <img src="https://i.imgur.com/2ONqM4W.png" height="75%" width="75%" alt="userlogin"/>
  <img src="https://i.imgur.com/OJXQ3TX.png" height="75%" width="75%" alt="userlogin!"/>
</p>
<p>
CLEAN UP AZURE ENVIRONMENT to make sure we dont incur charges. Delete everything created in this tutorial
</p>
