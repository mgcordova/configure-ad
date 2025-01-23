<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />
<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up the Domain Controller and Client
- Ensure Connectivity between the Client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users

<h2>Setup Domain Controller in Azure</h2>
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

<p>Go to virtual machines, click dc-1, click network settings on the left, click the network interface configuration.</p>

![image](https://github.com/user-attachments/assets/b62a016b-642e-4d05-b54f-d0967d4458e8)

![image](https://github.com/user-attachments/assets/4d069397-3ce5-466b-803d-36fe0a821435)

<p>Click on the ipconfig1 file and set the private ip address to static and save.</p>

![image](https://github.com/user-attachments/assets/cac97095-b5a0-47d7-ba2f-2b5804a8aab1)

![image](https://github.com/user-attachments/assets/1a88ce0f-39ea-4ac7-8f71-659be797c71f)

<p>Now we need to allow traffic on port 53 (DNS) so Client-1 can resolve the domain through dc-1 by seting security rules.</p>

<p>Go to the top of the webpage and search "Network Security Groups. This should appear and make sure it is not the classic version. Then you want to click dc-1-nsg</p>

![image](https://github.com/user-attachments/assets/0d5fd48d-0e32-43a6-bab4-155e1c891f05)

<p>Click settings then Inbound Security Rules and click "Add"</p>

![image](https://github.com/user-attachments/assets/0b1fc4a2-95d5-4ee7-9f29-ef70b4670fb0)

<p>Set Destination Port Ranges to 53 and set Priority to 290.</p>

![image](https://github.com/user-attachments/assets/76100c8c-41f7-4560-ac90-809b6a0ae91f)

![image](https://github.com/user-attachments/assets/38fbb728-e4a1-4cf7-9121-a8424d4d5f55)

<p>Click add and add the same rule to Outbound Security Rules.

Set the Destionation Port Ranges to 53 and set Priority to 290.</p>

![image](https://github.com/user-attachments/assets/3bb616f1-5900-4c54-80c3-7591b79eaca8)

![image](https://github.com/user-attachments/assets/d56cd5a6-ac65-428f-aca8-c660f147e369)

<p>Finally, restart dc-1</p>

![image](https://github.com/user-attachments/assets/001ce2dc-cff7-4432-9895-df462095ec47)

<p>Next, we want to log into dc-1 using the public ip address and disable Windows Firewall for testing connectivity.</p>

![image](https://github.com/user-attachments/assets/a679a5b4-bddd-44e6-a46c-ffb250f9f5a2)

<p>When you log into the virtual machine, the program server manager should automatically start up. This means you set up the dc-1 viritual machine correctly. </p>

<p>Go the windows search bar, right click the windows logo and click run or press Windows+R at the same time. You will want to search up "wf.msc" which opens up Windows Firewall.</p>

![image](https://github.com/user-attachments/assets/a42968a0-7383-40a6-82fe-a60b8bfc0d5e)

<p>Click on "Windows Defender Firewall Properties"</p>

![image](https://github.com/user-attachments/assets/22756266-b784-439c-8e57-a6f3d2fb7c97)

<p>Set the Firewall State to off and click apply./p>

![image](https://github.com/user-attachments/assets/8ea89452-edfb-4fa0-895d-29a9fba1a16c)

<p>You will want to do this for the Private profile and Public profile.</p>

![image](https://github.com/user-attachments/assets/4a1cdb33-e7ae-4507-960e-780bbb6c5b01)

![image](https://github.com/user-attachments/assets/879696f6-a235-4782-a645-fb61230dba14)

<p>You have now set up the Domain Controller for this lab.</p>

<hr>
<h2>Setup Client-1 in Azure</h2>

<p>4). Search up "Virtual Machines" on Azure -> click "Create" -> Fill in the name, Set the resource group, set the image to Windows 10, set the size to 2 VCPUs, and set the admin login.

<b>Make sure that this machine is in same region and virtual network as the domain controller</b></p>

![image](https://github.com/user-attachments/assets/d423d1d4-9cb1-45f2-853f-40a43c498c61)

![image](https://github.com/user-attachments/assets/860ef986-7e0f-41e3-bd02-1f7db99e5ee7)

![image](https://github.com/user-attachments/assets/9351f51c-4d72-4e8f-aef1-c78ea268ee68)

<p>Head to Networking section of the virtual machines configuration and make sure the vm is in the virtual network we made earlier.</p>

![image](https://github.com/user-attachments/assets/77e579f1-5c77-4bd7-a44e-e9a3deb7164e)

<p>Press Review + Create and deploy your virtual machine.</p>

<p>Now we need to allow traffic on port 53 (DNS) so Client-1 can resolve the domain through dc-1 by seting security rules.</p>

<p>Go to the top of the webpage and search "Network Security Groups. This should appear and make sure it is not the classic version. Then you want to click client-1-nsg</p>

![image](https://github.com/user-attachments/assets/8b40ef82-d011-4409-9c11-2a83c351cba3)

<p>Click settings then Inbound Security Rules and click "Add"</p>

![image](https://github.com/user-attachments/assets/a16d0868-c08b-45bf-b296-931b1ce8adb0)

<p>Set Destination Port Ranges to 53 and set Priority to 290.</p>

![image](https://github.com/user-attachments/assets/bb6660d9-b6a7-4373-8d8b-ed7a38568d69)

![image](https://github.com/user-attachments/assets/090a3ea1-24d5-4045-97aa-4b222aa14e6c)

<p>Click add and add the same rule to Outbound Security Rules.

Set the Destionation Port Ranges to 53 and set Priority to 290.</p>

![image](https://github.com/user-attachments/assets/c351dd21-3f6d-4477-8123-129238ab5d64)

![image](https://github.com/user-attachments/assets/bb6660d9-b6a7-4373-8d8b-ed7a38568d69)

![image](https://github.com/user-attachments/assets/090a3ea1-24d5-4045-97aa-4b222aa14e6c)

<p>Finally, restart client-1</p>

![image](https://github.com/user-attachments/assets/e7e306a3-e687-42a8-a6ef-1c2c066625ed)

<hr>
<h2>Set Client-1’s DNS settings to DC-1’s Private IP address</h2>

<p>5). Go to Virtual Machines and select client-1</p>

![image](https://github.com/user-attachments/assets/4b6fb63b-bccf-4e98-b62c-4a8ee9c47ed1)

<p>Click Networking then click Network settings and then click Network Interface Configuration.</p>

![image](https://github.com/user-attachments/assets/20834834-e5e4-4283-b9d1-229d89dda3c2)

<p>Click Settings then click DNS servers and set the server to the private ip of dc-1. [The private ip can be found by clicking dc-1 and reading the system information of dc-1. Save your changes.</p>

![image](https://github.com/user-attachments/assets/2c574e98-8659-4933-a1f0-aa9cb99ba00f)

<p>Finally, restart client-1</p>

![image](https://github.com/user-attachments/assets/e7e306a3-e687-42a8-a6ef-1c2c066625ed)

<hr>
