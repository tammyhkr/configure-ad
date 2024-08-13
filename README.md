<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
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

- Step 1: Setup Resources in Azure
- Step 2: Ensure Connectivity between the Client and Domain Controller
- Step 3: Install Active Directory
- Step 4: Create an Admin and Normal User Account in Active Directory
- Step 5: Join Clien-1 to your domain (mydomain.com)
- Step 6: Setup Remote Deskstop for non-administrative users on Client-1
- Step 7: Create a bunch of additional user to attempt to log into Client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

1. Setup Resources in Azure
   - Create virtual machines in Azure (Note: when you create the virtual machine, it will automatically create a resouce group and virtual network for you)
   - Create the Domain Controller VM and name it 'DC-1' 
       - Name your resource group 'AD-Lab' by selecting 'Create New' and select 'Ok'
       - Enter DC-1 in the 'Virtual machine name' field
       - Select US West 2 or 3 region (whichever works for you)
       - Ensure the 'Availability options' field is 'No infrastructure redundancy required'
       - Ensure the 'Security type' field is 'Standard'
       - Select Windows Server 2022 in the 'Image' field
       - Select at least 2 vcpus for the 'Size' field
       - Create your own user name and password
       - Select 'Review + create -> then Create
         
   - Create the Client-1 VM and name it 'Client-1'
       - Select 'AD-Lab' as the resource group
       - Enter Client-1 in the 'Virtual machine name' field
       - Select US West 2 or 3 region (whichever works for you)
       - Ensure the 'Availability options' field is 'No infrastructure redundancy required'
       - Ensure the 'Security type' field is 'Standard'
       - Select Windows 10 Pro in the 'Image' field
       - Select at least 2 vcpus for the 'Size' field
       - Create your own user name and password
       - Check the 'Licensing' box
       - Click 'Next' to get to the 'Networking tab'
       - Select the same Virtual network as the DC-1 VM (Note: if you do not do this step, you will not be able to connect to the Domain Controller)

          *Note: Give DC-1 at least 10 minutes to completely deploy in order to select same virtual network
       - Select 'Review + create -> then Create
           

![image](https://github.com/user-attachments/assets/4ad4ac77-9908-4d4d-902c-8bcda09d2663)
![image](https://github.com/user-attachments/assets/3c137316-d58f-4e4e-bbfb-881103594cdf)
![image](https://github.com/user-attachments/assets/40fc0ec1-8948-4448-ad4e-4b6434690897)
![image](https://github.com/user-attachments/assets/8db260ce-c546-402c-8f47-b5b2eb76afda)
![image](https://github.com/user-attachments/assets/e0f11683-ce57-4f86-9340-6606ef33241b)
![image](https://github.com/user-attachments/assets/5bbb604d-6be8-4d67-b9ea-a2c41b89e63f)
![image](https://github.com/user-attachments/assets/aa8fef89-5098-4ddd-a24a-291b148cfebc)

  - Set Domain Controller’s NIC Private IP address to be static
    - Go to Virtual Machines
    - Select DC-1
    - Select 'Networking' -> 'Network Settings'
    - Click on the 'Network Interface' (NIC) hyperlink
    - Click 'IP Configurations'
    - Click the 'ipconfig1' hyperlink to change the Private IP Address from 'Dynamic' to 'Static'
    - Select 'Static' and Save
    


![image](https://github.com/user-attachments/assets/c9b1ec18-62a8-4b24-bc7f-6f108f504615)
![image](https://github.com/user-attachments/assets/ce4f74fd-aae8-4b2d-88fc-40b9f89eeafe)
![image](https://github.com/user-attachments/assets/adee90c2-9deb-44ea-97eb-abc0ee829f1e)
![image](https://github.com/user-attachments/assets/6f4c99d3-8f5e-49b8-97e5-9ed5dfced1ed)


2. Ensure Connectivity between the Client and Domain Controller

 - Log into Client-1 and ping DC-1's IP address with 'ping -t' (perpetual ping)
    - Connect and log in with credentials created in Azure
    - Copy the Domain Controllers Private IP Address -> Azure -> Virtual Machines -> DC-1 -> scroll until you see 'Networking' -> copy the Private IP Address
    - Go to 'Start' and open the 'Command Prompt' app
    - Ping the DC's Private IP address
    - You'll notice the request will time out due to DC-1's Windows Firewall blocking ICMPv4 traffic
 - Minimize Client-1 VM and log into the Domain Controller (DC-1) via Remote Desktop Connection and enable ICMPv4 on the local Windows Firewall
    - Click start and open up 'Server Manager' if it has not opened automatically after logging in
    - Click start to open up the Windows Firewall
        - Click Inbound rules (maximize the Firewall)
        - Click 'Protocol'
        - Enable ICMPv4 (select line, right click -> enable)
        - Minimize DC-1
    - Check back at Client-1 to see ping succeed a.k. 'Reply'
        -Stop the ping by hitting 'ctrl +c' on your keyboard

 
 ![image](https://github.com/user-attachments/assets/aa6548d5-4c88-4779-b5b1-9c9a770cea4a)
 ![image](https://github.com/user-attachments/assets/e3272cd4-510b-468f-9783-10377d1fba48)
 ![image](https://github.com/user-attachments/assets/76741db9-27d9-49a0-9e8e-9157b8f4d093)
 ![image](https://github.com/user-attachments/assets/e3cf7311-02aa-400e-91b3-00a32df7270a)
 
 ![image](https://github.com/user-attachments/assets/bdce7dfe-1c44-4660-8b70-8e2c87f0eb88)
 ![image](https://github.com/user-attachments/assets/a0183cbe-33e2-4a65-9cef-805205da3b41)
 ![image](https://github.com/user-attachments/assets/ab103da5-5de0-40c0-9f80-4866899caed3)
 ![image](https://github.com/user-attachments/assets/749efea2-73f3-48e4-adfe-0bfcdb3b9209)
 ![image](https://github.com/user-attachments/assets/cc2140be-e4ca-445a-b8d9-293dc659aff8)



 
3. Install Active Directory

   - Go to DC-1 VM
   - Open Sever Manager (if it is not already open, click the start menu and select 'Sever Manager')
   - Once Sever Manager is open, click 'Add roles and features
   - Click 'Next' until you reach 'Sever Roles'-> select 'Active Directory Domain Services' -> you'll get a pop up to 'Add Features' (click that)
   - Click 'Next' through all of the dependencides and select Install -> Click 'Close' once the install finishes
   - After the intall, go back to Server Manager and click on the yellow caution icon -> click on 'Promote this sever to a doman controller'
   - Once 'Deployment Configuration' opens click on 'Add new forest' -> name the domain in the 'Root domain name' field (create any name) -> click 'Next' -> create your own password -> click 'Next' until you reach 'Install'
   - After the installation you'll automactically be kicked out of DC-1 VM
   - Log back into DC-1 VM using your original credentials + Root Domain Name created when installing AD (Reason being is, we have now turned this VM into a Domain Controller)

![image](https://github.com/user-attachments/assets/73c8bc44-0fc6-4773-8c09-06b450e94d7d)
![image](https://github.com/user-attachments/assets/03eb547d-49da-4d0c-a4e0-4f83b8409905)
![image](https://github.com/user-attachments/assets/aeda361e-3409-436c-8276-a639c17365f9)
![image](https://github.com/user-attachments/assets/358150f1-ae45-4eb7-8283-79f40456749d)
![image](https://github.com/user-attachments/assets/da5c9cb3-b0f8-405d-9680-0c30ba99ef9d)
![image](https://github.com/user-attachments/assets/c3e2e542-7de1-4796-b703-11414f0b5223)
![image](https://github.com/user-attachments/assets/39f7edbb-cc7c-4945-9886-1a6f37ac69f7)
![image](https://github.com/user-attachments/assets/2928a253-19c0-419a-9ffe-53962ea788c4)


4. Create an Admin and Normal User Account in AD

   - Once logged back into DC-1 VM, open Active Directory
       - Go to tools within 'Server Manager' -> click Active Directory Users and Computers
       - Create a couple of Organizational Units (OU) [think of them like folders]
          - Right click 'mydomain.com' -> go to 'New' -> click 'Organizational Unit'
          - In the 'Name' field, enter _EMPLOYEES -> click 'Ok'
          - Repeat steps and name another Organizational Unit _ADMINS
   - Right click 'mydomain.com' and click refresh to view the OU's created
   - Create an administrative account
       - Go to '_ADMINS' and right click -> go to 'New' -> select 'User'
       - Create your own credentials -> click 'Next' -> create your own password -> uncheck the box that says 'User must change password at next logon' -> click 'Next' and then 'Finish'
       -  After the user is created, right click the user's name -> go to 'Properties' -> click the 'Member of' tab -> click 'Add' -> enter 'domain' in the emtpy field -> click 'Check Names' -> join the 'Domain Admins' group -> click 'Ok' and apply
          
         
   - Log off of DC-1 VM and log back in using the admin credentials we just created
     - Go to start and open the 'Command Promt' app -> type 'logoff'
     - Log back in using admin credentials
     - Click start and open the 'Command Promt' app -> type 'whoami' and you'll see you're now logged in as jane_admin in DC-1 VM

         
  
 ![image](https://github.com/user-attachments/assets/7baca008-fd5e-47ce-9213-352f188c855e)
 ![image](https://github.com/user-attachments/assets/f8df5232-5b1a-4c27-995b-01b956b6affd)
 ![image](https://github.com/user-attachments/assets/47f1c87c-7f90-4e8c-a63c-a526df21f7cf)
 
 ![image](https://github.com/user-attachments/assets/7dc5f06d-d52f-4651-bb56-2b003b3bfbfc)
![image](https://github.com/user-attachments/assets/239ce7ca-7456-4da9-9aca-bc7599ae1fbb) ![image](https://github.com/user-attachments/assets/d68ae2ef-17db-4303-830d-6091ac45e06c)
![image](https://github.com/user-attachments/assets/aa9f8994-07df-4eb7-b4cf-223f7ca050de)
![image](https://github.com/user-attachments/assets/2d99d9c7-2749-489e-9747-2e4a1474ac1c)
![image](https://github.com/user-attachments/assets/0dd753c6-7d8d-40a3-94f9-3bebb6ea4d99)
![image](https://github.com/user-attachments/assets/ac24c07b-dd2d-40de-80b6-45ece8f33ff3)
![image](https://github.com/user-attachments/assets/700d6622-408f-4974-acd1-5bac6bfb1b26)
![image](https://github.com/user-attachments/assets/6d8cf668-eff8-489a-9082-ddbfd0df4b7f)
![image](https://github.com/user-attachments/assets/1c35cf48-702e-4e8b-9856-1ab326bdeb16)
![image](https://github.com/user-attachments/assets/9f4f328c-8978-4858-ab5e-ac028552c4a2)



5. Join Clien-1 to your domain (mydomain.com)
   
  - From the Azure Portal, set Client-1's VM DNS settings to the DC's Private IP address
     - Get DC-1 Private IP Address in Azure
     - Go to Azure -> Virtual Machines -> Client-1 -> click on Network -> click on Network settings -> click on the blue hyperlink under NIC that says 'client-1852' -> go to 'DNS Servers' -> select 'custom' -> enter DC-1's Private IP Address in the 'DNS Server' field -> click 'Save'
  - Restart Client-1 VM in Azure
     - Go to Virtual Machines -> Client-1 -> click 'Restart'
  - Log into Client-1 as the original local admin and join it to the domain (computer will restart)
     - Once logged in, right click the start menu -> go to System -> click 'Rename this PC (advancded) on the right -> click 'Change' -> select 'Domain' -> enter your Domain Name -> click 'Ok' -> enter the admins credentials 'mydomain.com\jane_admin' -> click 'Ok' -> a window will pop up asking to restart the computer, click 'Restart now' (if not, go to the start menu and click 'Restart' to force it, log back into Client-1 VM w/ the admin credentials)
  - You have now joined Client-1 to the Domain successfully

![image](https://github.com/user-attachments/assets/ceee869e-671e-413f-a06e-b802fe3db40c)
![image](https://github.com/user-attachments/assets/ae7226ca-f1a4-440b-81f0-1315a8be6033)
![image](https://github.com/user-attachments/assets/90e8def0-8f34-4ef9-b47d-0030aec8c1b9)
![image](https://github.com/user-attachments/assets/d1ddcd20-74f8-47b3-ac7f-c3c83a4de51a)
![image](https://github.com/user-attachments/assets/55b05aad-5f9d-4eb7-8434-9c38d486dc60)
![image](https://github.com/user-attachments/assets/91081427-7576-4bc3-8267-abbfe7a4186a)
![image](https://github.com/user-attachments/assets/2def5aaf-6a38-49e1-9f07-26ad0221faf9)
![image](https://github.com/user-attachments/assets/7f767e2d-42fa-4479-a52c-f7ee51ecd5b0)




6. Setup Remote Desktop for non-administrative users on Cleint-1

  - Log into CLient-1 as 'mydomain.com\jane_admin' -> right click the start menu -> click 'System' -> select 'Remote Desktop' on the right -> scroll down and click 'Select users that can remotely access this PC' -> click 'Add' -> enter 'domain user' in the empty box field -> click 'Check Names' -> click 'Ok'
  - Minimize Client-1
  - Open up DC-1 VM -> open up Active Directory -> go to 'mydomain.com' -> click the drop down -> click 'Users' -> you should see 'Domain Users' added -> double click 'Domain User's -> select the 'Members' tab to view the users of this security group that are allowed to remotely log into Client-1



![image](https://github.com/user-attachments/assets/664d7adb-f374-4cad-8793-244d97f82f36)
![image](https://github.com/user-attachments/assets/e8b8614b-c509-4487-bb36-494896615545)
![image](https://github.com/user-attachments/assets/e72de4d4-0bd5-40e5-a896-98a6504ecaf5)
![image](https://github.com/user-attachments/assets/a59f7f3a-89cb-4af3-8d73-16ff9719fc41)
![image](https://github.com/user-attachments/assets/5ef1ea1f-88f3-4b32-9e03-40794454416f)



7. Create a bunch of additional users and attempt to log into Client-1 with on of the users

  - Open up DC-1 and log in as 'mydomain.com\jane_admin'
  - Click the start menu and open 'Windows Powershell ISE' (right click to 'Run as administrator') -. click 'Yes' for the pop up
  - Click on the blank paper icon to create a new file
  - Paste your script under the 'Untitled' tab
  - Run the script by click the green play icon at the top
  - You'll start to see random user names being created
  - Minimize Powershell
  - Go back into Active Directory -> click on 'mydomain.com' -> click '_EMPLOYEES' -> you should see all the users that) are being created from PowerShell ISE in this folder (if you do not see them, right click '_EMPLOYEES' and click 'Refresh')
  - Double click on a random user name from the '_EMPLOYEES' folder (pick an easy username)
  - Copy the 'Display name' aka the 'Account Name'
  - Minimize DC-1
  - Log out of Client-1 and log back in with the users credentials we copied (in this case: 'mydomain.com\ban.jot)
      *Note: The password for all these users is 'Password1' per the script we pasted into PowerShell ISE
  - Open the command line and type 'whoami' to view user name -> type 'hostname' to view the host you're logged into
  


![image](https://github.com/user-attachments/assets/642a1ebf-6109-4967-9d83-4fa9173fb817)
![image](https://github.com/user-attachments/assets/cfd71347-6fe1-4680-8ad5-3a273209ad8b)
![image](https://github.com/user-attachments/assets/3c0ff4c7-51a0-4038-8e7e-ac44941ec74e)
![image](https://github.com/user-attachments/assets/37b5457e-f5e2-4f4d-aafb-02c55651ad11)
![image](https://github.com/user-attachments/assets/2c38216f-a70e-4ab8-aee3-fa6c343da2c9)
![image](https://github.com/user-attachments/assets/3728d79a-8eaa-4195-a67e-ed7b8665668d)
![image](https://github.com/user-attachments/assets/1f05aba7-141b-406d-9dcc-2b6494df4789)

You have successfully configured Active Directory!



*Trouble Shooting task for when a user gets locked out of their account after too many log in attempts

 - Go to DC-1
 - Pick another random user from Active Directory (pick an easy one)
 - Copy the 'Display name'
 - Log into Client-1 with that user name (in this case: mydomain.com\bej.hid)
 - Purposley attempt to log in at least 10 times with the wrong password
 - Go back into DC-1
 - Right click the user we attempted to log into 10 times
 - Go to 'Properties'
 - Click on the 'Accounts' tab
 - Click the check box next to 'Unlock account'
 - Click 'Ok'
 - If the user were to forget their password, right click the user's name -> select 'Reset Password'



![image](https://github.com/user-attachments/assets/d3a01dc8-2ac8-4b1b-942d-350f1da0c3c6)
![image](https://github.com/user-attachments/assets/a28a5a6f-dc29-42c2-bf89-cb215c5f57e5)
![image](https://github.com/user-attachments/assets/ed61b4b5-7580-4d64-8856-470e31db6778)
![image](https://github.com/user-attachments/assets/faa0ace0-1d4a-44c8-ba45-ad361552fabc)


Congrats, you've successfully resolved the users issue! That concludes this project.







