<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />
<hr>
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
<hr>
<h2>Set Up The Virtual Machines</h2>
<p>1). Create a Resource Group for your virtual machines. Search up "Resources Groups" on Azure -> click "Create" -> Fill in the name and create. 

<b>Make sure to keep the resource group, vm, and virtual network on the same server.</b></p>

![image](https://github.com/user-attachments/assets/9dcd57d5-56f1-452f-964f-ae44e2c065b3)

![image](https://github.com/user-attachments/assets/e7500298-aad0-4cde-9feb-aa84fb4acec6)
