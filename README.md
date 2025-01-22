<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />
<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up the virtual machines
- Ensure Connectivity between the Client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>
<h2>Set Up The Virtual Machines</h2>
<p>1). Create a Resource Group for your virtual machines. Search up "Resources Groups" on Azure -> click "Create" -> Fill in the name and create. 

<b>Make sure to keep the resource group, vm, and virtual network on the same server.</b></p>

![image](https://github.com/user-attachments/assets/9dcd57d5-56f1-452f-964f-ae44e2c065b3)

![image](https://github.com/user-attachments/assets/e7500298-aad0-4cde-9feb-aa84fb4acec6)

<p>2). Create the Virtual Network for your virtual machines. Search up "Virtual Network" on Azure -> click "Create" -> Fill in the name and create.</p>

![image](https://github.com/user-attachments/assets/73b863f7-db20-4f75-8706-1b93bfb9c70f)

![image](https://github.com/user-attachments/assets/cc4e0aca-0fe9-4b0e-993a-ea750240f24e)

<p>3). Create the Domain Controller virtual machine named "DC-1"</p>

<p>Search up "Virtual Machines" on Azure -> click "Create" -> Fill in the name, Set the resource group, set the image to Windows Server 2022, set the size to 2 VCPUs, and set the admin login.</p>

![image](https://github.com/user-attachments/assets/eb8e8336-9df1-4bcc-bb84-a91c657ac4e0)

![image](https://github.com/user-attachments/assets/289e6f66-ee2e-4f27-9baa-4166f8dcb4e9)

![image](https://github.com/user-attachments/assets/6a051fee-0cca-4d48-b922-06ef7007b82c)

<p>Head to Networking section of the virtual machines configuration and make sure the vm is in the virtual network we made earlier.</p>

![image](https://github.com/user-attachments/assets/1e46241f-d2a5-48a7-9f1f-a59c36a0393f)

<p>Press Review + Create and deploy your virtual machine.</p>
