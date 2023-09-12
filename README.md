# configure-ad
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup a Domain Controller (DC) and Client VM
- Install Acitve Directory (AD)
- Configure AD Accounts
- Join the Client VM to the AD domain
- Configure Remote Desktop within AD
- Create Users within the AD

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/ecdba355-660e-47ea-a329-5dd1cc2e0b41" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/58447002-8418-4e70-963c-95f007717db1" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To start off, utilize Microsoft Azure to create two virtual machines. One will function as a Window’s Server called DC and the other as  Client that will access DC while running Active Directory. DC will be a Windows Server with 2 vcpus, 16GB memory, and an automatic vnet. Client will be a Windows PC with 2 vcpus, 16 GB memory, and the vnet of DC. 
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/1bad6165-c53a-4867-be1e-cc237722186e" src="https://github.com/elTuTico/configure-ad/assets/137955237/9cf1e3ee-9fdb-48c9-a1ad-3cfdd273af52" height="80%" width="80%" alt="timers"/>
</p>
<p>
--Once both VMs are functioning, change the Domain Controller’s (DC) NIC Private IP address to static. By navigating to the DC’s “networking tab”, clicking “Network Interface”, and finally opening up “IP configuration”. From there change the IP address from dynamic to static. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Remote Desktop into DC-1 and open up the Server Manager from the Start Menu. From there begin installing Active Directory by clicking on “Add roles and features”. Follow the prompts through the installation process ensuring to click “Active Directory Certificate Services” upon the “Server Roles” page. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once Active Directory Certificate Services has installed, create the Domain Controller by configuring the domain. Click on the flag on the top right hand corner of the screen with a caution sign on it, followed by “Promote this server to a domain controller”, then “add a new forest” with the Root Domain server name being “mydomain.com”. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
This process will disconnect your remote desktop, which is normal due to the server becoming a DC. Reconnect back to DC, but now as a user utilizing the new domain just created: mydomain.com\VM-PC(as the username). 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From the “Tools” drop down menu in the Server Manager click on “Active Directory Users and Computers”. This will open up the DC’s Active Directory UI. From here create two organizational units by right clicking on “mydomain”, hovering over “New”, and selecting “Organizational Units”. Name one “_EMPLOYEES” and another “_ADMINS”.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Within the _ADMINS organizational unit create a new “User”. This will be the account primarily used to access the DC. Ensure to uncheck the “User must change password at new logon” to avoid having to hassle with too many differnet credentials.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now right click on the new user that was just created and open up “Properties”. From there navigate to “Member Of” to grant this new user access to the “Domain Admins” group. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Utilizing the new user that was registered into Domain Admins log back onto the DC; username: mydomain.com\alex_admin password: Password1. This will be the main way that changes are made to DC instead of the original local user used at the beginning of the excercise. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Client-1 will be the way that the DC is logged into through other users, but to do so the DNS settings for Client-1 must be changed from the manually assigned Azure DC to the virtual DC just created.  That way when mydomain.com is used as a username it knows where within the DC to grant access to from the Active Directory. Change the Client-1’s DNS settings to the DC’s Private IP address through the Azure Portal. After changing the DNS settings, restart Client-1 before logging back into it. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log back into Client-1 through the original credentials and right click on the start Menu to open up “Systems''. From there click on “Rename this PC”, followed by “change”, then under “domain” type in “mydomain.com”. This will grant you access to a Windows Security popup that will ask for username: mydomain.com\alex_admin password: Password1.  If this was done correctly the Client-1 VM should have to restart. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log onto Client-1 with the username: mydomain.com\alex_admin password: password: Password1. To allow all users to utilize the Client-1 VM changes must be made to the Remote Desktop accessibility from only Domain Admins to all users in the AD.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Right click on the Menu to open up “Systems”, click on “Remote Desktop” on the right hand side of the screen, and lastly “Select users that can remotely access this PC”. Within the search box type “Domain Users” to grant access to all users under that Organizational Unit access to Client-1 remotely. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log back onto the DC with the admin account: mydomain.com\alex_admin. Open up the Menu, search up “PowerShell_ise”, and right click it to run as an administrator. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click on “create a new file” on the top left hand, paste in the script, and run it to create up to 1000 new users that will be made into the “_EMPLOYEES” Organizational Unit to remotely log into Client-1. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
With the script running in the background on the DC, try to utilize one of the users made from the Organizational Unit “_EMPLOYEES” to log onto Client-1. There should be a network of users now that can utilize the Active Directory running off of the Domain Controller. 
</p>
<br />
