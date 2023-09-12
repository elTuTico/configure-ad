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
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/226c3d7b-aec2-4227-a257-76f2e6823858" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To start off, utilize Microsoft Azure to create two virtual machines. One will function as a Window’s Server called DC and the other as  Client that will access DC while running Active Directory. DC will be a Windows Server with 2 vcpus, 16GB memory, and an automatic vnet. Client will be a Windows PC with 2 vcpus, 16 GB memory, and the vnet of DC. 
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/50ceebcd-58ee-465e-863a-c4a81480222a" height="80%" width="80%" alt="timers"/>
</p>
<p>
Once both VMs are functioning, change the Domain Controller’s (DC) NIC Private IP address to static. By navigating to the DC’s “networking tab”, clicking “Network Interface”, and finally opening up “IP configuration”. From there change the IP address from dynamic to static. 
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/3fafefbd-8a27-4ca9-b2a9-9468900c92c8" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/e107642e-1374-45e8-bb7c-5b1fc097290c" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Remote Desktop into DC-1 and open up the Server Manager from the Start Menu. From there begin installing Active Directory by clicking on “Add roles and features”. Follow the prompts through the installation process ensuring to click “Active Directory Domain Services” upon the “Server Roles” page. 
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/a2924f36-bb75-48bf-905e-e5b2040c183a" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/9df5118e-18f7-4c43-bd67-b1de4a442ed4" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once Active Directory Certificate Services has installed, create the Domain Controller by configuring the domain. Click on the flag on the top right hand corner of the screen with a caution sign on it, followed by “Promote this server to a domain controller”, then “add a new forest” with the Root Domain server name being “mydomain.com”. Next through all of the prompts and click on the "Install" option to finalize the new domain.  
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/01d6cfd7-2983-4208-a852-a2b2a45d149c" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
This process will disconnect your remote desktop, which is normal due to the server becoming a DC. Reconnect back to DC, but now as a user utilizing the new domain just created: mydomain.com\DCvm(as the username). 
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/27dc04c7-1a33-4415-9438-92fb5ea313ee" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/c575c514-9b38-4b9b-9e11-ea2ae4c5874d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From the “Tools” drop down menu in the Server Manager click on “Active Directory Users and Computers”. This will open up the DC’s Active Directory UI. From here create two organizational units by right clicking on “mydomain”, hovering over “New”, and selecting “Organizational Units”. Name one “_EMPLOYEES” and another “_ADMINS”.
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/1d67477a-1df0-4896-8887-b7331f05fb6c" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Within the _ADMINS organizational unit create a new “User”. This will be the account primarily used to access the DC. Ensure to uncheck the “User must change password at new logon” to avoid having to hassle with too many differnet credentials.
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/e8af4cd6-7faf-4ca2-a870-c947874531c0" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now right click on the new user that was just created and open up “Properties”. From there navigate to “Member Of” to grant this new user access to the “Domain Admins” group, this will grant the new user overall access and permissions on the Active Diretory.  
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/002931d8-191a-4340-9133-e8e14c630e9d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Utilizing the new user that was registered into Domain Admins log back onto the DC; username: mydomain.com\alex_admin password: Password1. This will be the main way that changes are made to DC instead of the original local user used at the beginning of the excercise. 
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/37172d37-51ab-4365-99d7-e9109d4d0764" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
"Client" VM will be the way that the DC is logged into through other users, but to do so the DNS settings for Client must be changed from the automatically assigned Azure DC to the virtual DC just created.  That way when mydomain.com is used as a username it knows where within the DC to grant access to from the Active Directory. Change the Client’s DNS settings to the DC’s Private IP address through the Azure Portal. After changing the DNS settings, restart Client before logging back into it. 
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/ef7277df-51a8-4ee0-acdd-d9440b105d8d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/0b802214-c139-4c65-bca2-a030ef02c2fc" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log back into Client through the original credentials and right click on the start Menu to open up “Systems''. From there click on “Rename this PC”, followed by “change”, then under “domain” type in “mydomain.com”. This will grant you access to a Windows Security popup that will ask for username: mydomain.com\alex_admin password: Password1.  If this was done correctly the Client VM should have to restart. 
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/465ec34f-4f8a-4310-a8ad-51c036fae303" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log onto Client with the username: mydomain.com\alex_admin password: password: Password1. To allow all users to utilize the Client VM changes must be made to the Remote Desktop accessibility from only Domain Admins to All Users in the AD.
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/88f8e9e2-1419-4151-89b9-e79d04f25633" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/842112f8-bbbf-4cc8-b0f5-01d12bd6a37c" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Right click on the Menu to open up “Systems”, click on “Remote Desktop” on the right hand side of the screen, and lastly “Select users that can remotely access this PC”. Within the search box type “Domain Users” to grant access to All Users under that Organizational Unit access to Client remotely. 
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/f1eb93d4-c934-49c7-97f9-def2156227b6" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To test if the Client has successfully been granted acess to All Users utilize PowerShell to create the Users with their own individual credientials by logging back onto the DC with the admin account: mydomain.com\alex_admin. Open up the Menu, search up “PowerShell_ise”, and right click it to run as an administrator. 
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/0b5988f8-1c35-4328-a000-3e06f8ae90a8" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/1cdf8645-2e52-4241-9985-7a47f43ca062" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click on “create a new file” on the top left hand, paste in the <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">script</a>, and run it to create up to 10000 new Users that will be made into the “_EMPLOYEES” Organizational Unit to remotely log into Client. These new Users will be automatically generated by the script to make any random combination of names with the same password: Password1. 
</p>
<br />

<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/71ed9284-51dc-4ee0-8e47-1572a0e0ac7c" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/c52338a3-c827-4524-98bb-eef34a7343cc" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://github.com/elTuTico/configure-ad/assets/137955237/fbfec47c-d0dd-400b-899b-6f67afc97077" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
With the script running in the background on the DC, try to utilize one of the users made from the Organizational Unit “_EMPLOYEES” to log onto Client. There should be a network of users now that can utilize the Active Directory running off of the Domain Controller. 
</p>
<br />
