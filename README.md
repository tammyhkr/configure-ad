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
       - Select the same Virtual network as the DC-1 VM (Note: if you do not so this step, you will not be able to connect to the Domain Controller)
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



 
 




