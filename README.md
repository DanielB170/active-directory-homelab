# Active Directory Homelab (Windows Server 2019)
Step-by-step documentation of a Windows Server 2019 Active Directory homelab built using VirtualBox.

# Overview
This homelab demonstrates how to:

- Install and configure Windows Server 2019
- Promote a server to a Domain Controller (DC)
- Set up Active Directory Domain Services (AD DS)
- Create and manage users, groups, and OUs
- Join a Windows 10 Pro client machine to the domain
- Practice basic domain administration tasks

# Server Setup

1. Create the Server VM
- Use Virtual Box and to create a New VM with two network adapters
- Select Windows 2019 as the OS 
- Mount the required ISO file found here: https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019
- Ensure you select Desktop Experience during installation

<img width="583" height="316" alt="image" src="https://github.com/user-attachments/assets/e1261d5d-6c65-4510-90c5-ffa1eb94db03" />




2. Initial Admin configuration
- Create Admin Password
- Login


<img width="802" height="473" alt="Screenshot 2026-02-21 162532" src="https://github.com/user-attachments/assets/6b6b3870-ae6b-43f8-a338-89fe373b451a" />

3. Install Guest Additions (optional)
- Install the Guest Additions CD image to improve VM compatibility and screen resolution
- On VirtualBox click devices and Guest Additions CD image
- Click on the new file added to this PC
- Run amd64 and reboot

<img width="774" height="582" alt="Screenshot 2026-02-21 175533" src="https://github.com/user-attachments/assets/f0c718e3-54d0-4d84-86fe-2655fe1eff8e" />

4. Configure Network Adapters
- Go to Settings > Network & Internet > Ethernet > Change adapter options
- Rename each network adapter according to its IP/role (e.g. INTERNET, INTERNAL)

<img width="1119" height="213" alt="Screenshot 2026-02-21 182248" src="https://github.com/user-attachments/assets/6d65d082-b32f-42aa-a510-615d8860ec77" />

5. Configure the Internal Network Adapter
- Right-click the internal adapter > Properties > IPv4
- Set the example static IP address and subnet mask
- Leave the default gateway empty — the Domain Controller will act as the gateway
- Set the Preferred DNS to the loopback address (127.0.0.1)

<img width="398" height="453" alt="Screenshot 2026-02-21 182757" src="https://github.com/user-attachments/assets/744fc828-5ab0-4b99-90b1-01e51c13cf3c" />


6. Rename the Server
- Go to Settings > System > About
- Rename the PC to something identifiable
- Restart the server to apply the change

<img width="677" height="293" alt="Screenshot 2026-02-21 181438" src="https://github.com/user-attachments/assets/be6d04c2-d196-4f02-940b-0e49381907f4" />

# Creating a Domain

1. Install Active Directory Domain Services
- Open Server Manager and click Add Roles and Features
- Proceed through the wizard and select AD DS
- Click Install and wait for completion

<img width="782" height="553" alt="Screenshot 2026-02-21 184314" src="https://github.com/user-attachments/assets/bd6a9eb4-7e1a-4fc3-b181-058df3d56944" />

2. Promote the Server to a Domain Controller
- Click the notification flag icon in Server Manager
- Select Promote this server to a domain controller
<img width="1719" height="903" alt="Screenshot 2026-02-24 164323" src="https://github.com/user-attachments/assets/a5046522-ed99-4474-a137-f90b61387cc9" />

3. Create a New Forest
- Select Add a new forest
- Enter your desired domain name and click Next to proceed
<img width="753" height="559" alt="Screenshot 2026-02-24 164451" src="https://github.com/user-attachments/assets/e257192c-8a2f-49a1-a3eb-6b5055fe4608" />

4. Set a Recovery Password
- Enter a Directory Services Restore Mode (DSRM) password
- Continue clicking Next through the wizard and click Install
- Server will restart automatically

<img width="751" height="554" alt="Screenshot 2026-02-24 165038" src="https://github.com/user-attachments/assets/64349462-bf82-4716-994c-c2d87b19c58f" />

5. Create an Admins Organisational Unit

- Log in with the new domain admin account after restart
- Open Active Directory Users and Computers
- Right-click on your domain > New > Organisational Unit
- Name it Admins
<img width="751" height="521" alt="image" src="https://github.com/user-attachments/assets/61f944c3-78c0-4081-8dc1-e69c01a8fc78" />

6. Create Your First Admin User

- Right-click Admins > New > User
- Fill in the user's first name, last name, and logon name (a-username is admin standard)
- Set a password and complete the wizard

<img width="435" height="370" alt="image" src="https://github.com/user-attachments/assets/a54df87c-2481-46db-86c6-cdc7c6dc1d40" />

<img width="434" height="372" alt="image" src="https://github.com/user-attachments/assets/c54f2f4f-b460-41c3-8ef1-fa6ef70eda9a" />

7. Assign Domain Admin Privileges

- Right-click the new user > Properties > Member Of > Add
- Add the user to the Domain Admins group and click Apply

<img width="412" height="537" alt="image" src="https://github.com/user-attachments/assets/59ec8b5a-cea7-42d3-bebb-297358e1e544" />

8. Verify Admin Login
- Sign out and log back in using the new admin account credentials
- Confirm successful access to the domain environment

<img width="1710" height="861" alt="image" src="https://github.com/user-attachments/assets/8f1bd26a-5e17-41ad-bd0d-e0c5e8570507" />Password1

# Remote access Server & NAT setup
Will allow windows client to connect to server via private virtual network

1. Install the Remote Access Role
- Open Server Manager > Add Roles and Features
- Proceed through the wizard, select the server, then choose Remote Access
- Enable Routing, click Next, and install

<img width="780" height="553" alt="image" src="https://github.com/user-attachments/assets/7746865b-2175-4773-98c2-cad66b0d4409" />

2. Open Routing and Remote Access
- Click Tools in the top-right of Server Manager
- Select Routing and Remote Access
<img width="621" height="446" alt="image" src="https://github.com/user-attachments/assets/2aa142d6-6016-4334-b847-f960207ce37d" />

3. Configure NAT
- Right-click on the Domain Controller entry > Configure and Enable Routing and Remote Access
- Select NAT when prompted
- Select the internet not the local network (internal) adapter and complete the wizard

<img width="494" height="427" alt="image" src="https://github.com/user-attachments/assets/81fa926c-e6e6-4c47-b664-1eea38ec937b" />

<img width="490" height="422" alt="image" src="https://github.com/user-attachments/assets/6211843c-31f1-461b-96ca-5e4574a558cf" />

4. Install DHCP

- In Server Manager > Add Roles and Features, select DHCP Server
- Complete the wizard and install

<img width="772" height="551" alt="image" src="https://github.com/user-attachments/assets/e941291e-a613-4564-96cc-0d879daecd91" />

5. Configure a DHCP Scope

- Open Tools > DHCP

<img width="1050" height="583" alt="image" src="https://github.com/user-attachments/assets/ab0063f7-12f2-4800-bf98-bf86df70885c" />

- right-click IPv4 > New Scope
- Enter a scope name, assign an IP address range, configure exclusions, and set the lease duration

<img width="507" height="419" alt="image" src="https://github.com/user-attachments/assets/daa65dd7-c145-4c47-b511-60d609029298" />
<img width="509" height="414" alt="image" src="https://github.com/user-attachments/assets/44af3d81-c585-40b6-84d6-e5d210882be6" />

<img width="508" height="419" alt="image" src="https://github.com/user-attachments/assets/e569cfb9-cf01-4c9b-abb1-e5de088b9819" />
<img width="514" height="426" alt="image" src="https://github.com/user-attachments/assets/4543ab72-6149-4d5f-9304-59ae3de1ecf9" />

<img width="512" height="423" alt="image" src="https://github.com/user-attachments/assets/c4e2a20a-a02d-4d73-b335-50e7345e8127" />

- Complete the wizard and choose Yes to activate the scope

- Right click DHCP authosrise and refefresh ipv4 
<img width="697" height="478" alt="image" src="https://github.com/user-attachments/assets/d151fd53-85b5-4fce-a1e1-8a91dc7d608b" />

6. Internet setup (optional)
- In server manager click Configure this local server
<img width="1863" height="806" alt="image" src="https://github.com/user-attachments/assets/f0a9831a-6d6d-4455-aab3-e874aa6d9a9c" />

- Disable IE Enhanced Security Configuration to allow easier browsing during setup
  
<img width="419" height="442" alt="image" src="https://github.com/user-attachments/assets/db7cf5bf-e359-4b6b-a0ea-0992c0022f85" />

7. Create Bulk Users via PowerShell

- Open PowerShell ISE as Administrator
- Run command Set-ExecutionPolicy Unrestricted
- Execute third-party user creation script to generate 1000+ domain users

<img width="1285" height="476" alt="image" src="https://github.com/user-attachments/assets/cc7bb923-b0a1-4ee5-8b83-4975e580e905" />

- Verify the new users appear in the Users folder in Active Directory Users and Computers
<img width="748" height="562" alt="image" src="https://github.com/user-attachments/assets/af0b4dab-0360-4eb3-8f94-e74930658f05" />


# Creating Windows 10 Client

1. Create the Client VM
- In VirtualBox, create a new VM and set the network adapter to Internal Network
- Mount the Windows 10 ISO file
- Select Windows 10 Pro during installation
<img width="562" height="260" alt="image" src="https://github.com/user-attachments/assets/39b08271-fa58-4f0f-a23c-fb8551b37496" />
<img width="618" height="269" alt="image" src="https://github.com/user-attachments/assets/9193cd8e-14a9-4dd5-bfd8-23039c6b43a9" />

2. Verify Network Connectivity
Once logged in, open Command Prompt and run ipconfig
Confirm the default gateway matches the Domain Controller's internal IP

<img width="553" height="101" alt="image" src="https://github.com/user-attachments/assets/40a6d6b9-cbd8-4f2e-a0a3-0766c74413b8" />

3. Join the Client to the Domain
- Go to Settings > System > About > Rename this PC (Advanced) > Change
- Enter a new PC name and enter your domain name 

<img width="726" height="470" alt="image" src="https://github.com/user-attachments/assets/84ed700e-7a22-4e2a-a774-77c6c54679aa" />

- When prompted, enter domain admin credentials and click OK

<img width="452" height="293" alt="image" src="https://github.com/user-attachments/assets/bbb2972d-07ca-423a-9251-11926620efa9" />

4. Restart and Verify
Restart the client VM after successfully joining the domain
At the login screen, sign in using any domain user account to confirm domain connectivity
<img width="1020" height="754" alt="image" src="https://github.com/user-attachments/assets/551084b7-9d79-4cc3-8410-5e486af37d63" />

#Domain Admin Testing

1. View DHCP Address Leases
- On the Domain Controller, open the DHCP tool
- Expand IPv4 > Scope > Address Leases
- Confirm the client machine has been assigned an IP address

<img width="840" height="330" alt="image" src="https://github.com/user-attachments/assets/2292643e-2ab7-4a11-bca1-c85f9896ed4f" />

2. View Active Computers in AD
- Open Active Directory Users and Computers
- Navigate to the Computers container
- Confirm the client machine appears as a domain-joined computer
<img width="744" height="295" alt="image" src="https://github.com/user-attachments/assets/2657d56a-7550-4922-bd28-243847c8d320" />

3. Reset a User Password
- In Active Directory Users and Computers, locate the target user
- Right-click the user > Reset Password
- Enter and confirm the new password and click OK
<img width="756" height="515" alt="image" src="https://github.com/user-attachments/assets/b6c17136-d071-4b02-b726-dea1cd92ab34" />


