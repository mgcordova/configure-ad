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
- Script to Generate Names and Users https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1

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

<p>Now, you have set up the client-1 virtual machine.</p>

<hr>
<h2>Ensuring Connectivity between the Client and Domain Controller</h2>

<p>Log into client-1 with Remote Desktop, ping dc-1's private ip address with a ping.</p>

![image](https://github.com/user-attachments/assets/83f2351d-356c-4426-933f-b2c8a910548d)

<p>Open powershell by searching it through the windows search bar.</p>

![image](https://github.com/user-attachments/assets/c43ef2c7-5195-4dac-bbbd-f39674ca543e)

<p>Type ping [private ip of dc-1] in Powershell. If it looks like the image below that means your ping succeeded.</p>

![image](https://github.com/user-attachments/assets/80582f88-9f25-4c55-98c9-cf44a555acc5)

<p>Next, check the DNS server of the client-1 vm and it should show DC-1's private IP address.

Type ipconfig /all in Powershell.</p>

![image](https://github.com/user-attachments/assets/aa4d32a3-45b3-48b9-90a4-3b0f8da4e96c)

<hr>
<h2>Installing Active Directory on The Domain Controller</h2>

<p>Log into dc-1 through Remote Desktop.</p>

![image](https://github.com/user-attachments/assets/83f2351d-356c-4426-933f-b2c8a910548d)

<p>Open the Server Manager and select "Add Roles and Features". Continue through the set up installation </p>

![image](https://github.com/user-attachments/assets/91cce698-f287-40e4-9df8-265277c2a241)

<p>Continue by pressing next and make sure the server selected is dc-1.</p>

![image](https://github.com/user-attachments/assets/6ed55f4d-41a7-46f2-8aac-ffffc3a51a20)

![image](https://github.com/user-attachments/assets/78b9369e-8267-4f15-b285-1e3a5a1f6fcd)

<p>In the Server Roles section make sure you install Active Directory Domain Services</p>

![image](https://github.com/user-attachments/assets/0a98d97d-4e12-40f5-8995-5c694e3ced44)

<p>Continue with the installation</p>

![image](https://github.com/user-attachments/assets/fcf6ca67-524c-4ac2-8f77-e173c2649284)

<p>Next we want to promote the vm to a domain controller. Click the flag on the top right and select "Promote this server to a domain controller."</p>

![image](https://github.com/user-attachments/assets/6e0960e0-a4b6-489d-9f31-a4419d3d402d)

<p>Now, we want to set up a new forest and for this example we will set it as mydomain.com

After filling out the domain name continue with the installation process.</p>

![image](https://github.com/user-attachments/assets/9e89ad75-e023-4005-931d-b5bfa4c3faee)

<p>Set the password to "Password1" for this lab.</p>

![image](https://github.com/user-attachments/assets/d48a48f3-0d78-4215-949e-3009faea0d62)

<p>Make sure that "Create DNS Delegation" is unchecked.</p>

![image](https://github.com/user-attachments/assets/e3ff62c5-5c6f-4674-aae0-18fec78e4d85)

<p>Continue pressing next as there is nothing left to configure then press install. 

Your VM should restart on its own once it is done installing. </p>

![image](https://github.com/user-attachments/assets/3ece1a53-2b65-437d-8d80-4c8e954e0e1d)

<p>Now, we want to log in back into the dc-1, but now we have to use the domain whilst logging in. So it recognizes us as a domain user.</p>

![image](https://github.com/user-attachments/assets/ddb23e09-7e05-4eae-a845-cacd554c1566)

<hr>
<h2>Creating an Admin and Normal User Account in Active Directory</h2>

<p>6). Go on the Windows search bar and search "Active Directory Users and Computers"</p>

![image](https://github.com/user-attachments/assets/00f9889a-f753-4219-a844-f84599699a59)

<p>Next, right click "mydomain.com" and create an Organizational Unit (OU) called “_EMPLOYEES”</p>

<p>Create another OU called "_ADMINS" as well.</p>

![image](https://github.com/user-attachments/assets/fb031d5b-77fb-4cef-aaaf-746132b3a407)

![image](https://github.com/user-attachments/assets/e584fdde-4209-43b6-8527-66052332e153)

<p>in the "_ADMINS" OU create a user called "Jane Doe" with the username of jane_admin and passsword Cyberlab123!</p>

![image](https://github.com/user-attachments/assets/aefc4884-3378-4151-a874-a01c0a0ee318)

![image](https://github.com/user-attachments/assets/63e6c752-8b5d-47d9-b30d-529de6ee89c2)

![image](https://github.com/user-attachments/assets/a32d737c-7694-42b4-a529-8f0e385067fa)

<p>Right click Jane Doe and select Properties </p>

![image](https://github.com/user-attachments/assets/a34ad367-5437-4af6-b5e5-3e898d9b62ac)

<p>Click the Member Of tab and click Add.</p>

![image](https://github.com/user-attachments/assets/44fd49f9-6009-40ad-b228-b7f973d0ed2e)

<p>Type Domain Admins in the objects, click check names, click ok, and apply.</p>

![image](https://github.com/user-attachments/assets/c661874c-cd02-4394-9d79-67c24a919739)

![image](https://github.com/user-attachments/assets/0d1e7861-7ca1-4798-aa92-6aa9b4ed5207)

<p>Now we want to close the remote desktop connection and log into dc-1 as Jane Doe. We will be using this admin account from now on.

The login details should be mydomain.com\jane_admin and password Cyberlab123!</p>

![{6A73ED15-633A-43E3-863B-C2C9AB187C44}](https://github.com/user-attachments/assets/0aef8ef9-01c4-429c-a6cb-2c84b9afe5bc)

<hr>
<h2>Joining Client-1 to your domain (mydomain)</h2>

<p>Log into client-1 through Remote Desktop and search in the Windows search bar "About your PC"</p>

![image](https://github.com/user-attachments/assets/8bc6d72d-c7d1-49f2-a138-3917f5d3ad22)

<p>Click Advanced System Settings on the right, then the Computer Name tab, click change Domain </p>

![image](https://github.com/user-attachments/assets/eb96ca03-3e48-4a29-832a-72c5639e2260)

![image](https://github.com/user-attachments/assets/762c06ab-29f4-4ff3-86bd-588b45935ed5)

<p>Click ok and it'll ask for a user that is apart of the domain to log in. We'll use Jane's admin account. After successfully logging and closing everything the VM will restart on its own.</p>

![image](https://github.com/user-attachments/assets/4c2d65db-9ea2-4a17-98d3-8a4d53a7a2c5)

![image](https://github.com/user-attachments/assets/f6965825-4ba8-4fc1-9d84-97771efd0abc)

<p>Log into the Domain Controller through Remote Desktop and verify Client-1 shows up in the Active Directory Users and Computers (ADUC) inside the "Computers" OU of the domain.</p>

<p>Create a new OU named "_CLIENTS" and drag Client-1 into the newly created OU.</p>

![image](https://github.com/user-attachments/assets/b4f8d378-eda2-4404-b12a-17f80ddbd1b6)

![image](https://github.com/user-attachments/assets/bedcdf98-66d1-4dac-86d0-49d2bd5e7011)

<hr>
<h2>Setting up Remote Desktop for non-administrative users on Client-1</h2>

<p>Log into Client-1 as mydomain.com\jane_admin and open system by searching for it in the Windows search bar.

Click “Remote Desktop”.</p>

![image](https://github.com/user-attachments/assets/c53a79af-4e8b-4a40-809f-9a62bfefc1f7)

<p>Click "Select users that can remotely access this PC" at the bottom and add Domain Users.</p>

![image](https://github.com/user-attachments/assets/0924a45b-17db-4d8d-b23c-96205f032df5)

![image](https://github.com/user-attachments/assets/edfea038-03c4-46c5-b175-9e8135736d88)

<p>You can now log into Client-1 as a normal, non-administrative user now.

Normally you’d want to do this with Group Policy that allows you to change MANY systems at once</p>

<hr>
<h2>Create Additional Users With A Script and Attempt To Log Into Client-1 With One of The Users</h2>

<p>Log in to dc-1 as jane_admin and run Powershell ISE as Administrator.</p>

![image](https://github.com/user-attachments/assets/28115754-230b-4af7-a60c-3bcedbd1c913)

<p>Create a file in Powershell and paste the contents of the script into it. Then save it as a file on your desktop named "script".</p>

![image](https://github.com/user-attachments/assets/31b9d277-5de9-4614-ab54-35f12513a38d)

![image](https://github.com/user-attachments/assets/5238ee75-9dcb-4b1c-a693-c14a9cd1210e)

<p>We can now run the script (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) and observe it create users into the AD.</p>

![image](https://github.com/user-attachments/assets/bd4f8b02-6d98-4998-9e1d-c62aa0e2368b)

![image](https://github.com/user-attachments/assets/0151bda1-2e27-4270-90c4-352145eb99d1)

<p>After a few minutes you can stop the script and note down a random account of your choosing name and the password should be Password1</p>

![image](https://github.com/user-attachments/assets/b50c322f-cf5f-4ba8-85cf-a8c2c95522bb)

<p>Now, attempt to log into client-1 with one of the random accounts.</p>

![{8734E86E-3975-4A0F-9C83-562A5CEECB0D}](https://github.com/user-attachments/assets/4744e5f4-6a9d-41c3-8572-9b6e54abb929)

![{353496B2-4448-400D-80AB-2743A570E3A2}](https://github.com/user-attachments/assets/72160010-5d4e-4163-9975-e133c61d62af)

<hr>

<p>I hope this tutorial helped you gain a better understanding of active directory. This can be done easily on both PC and Mac, with the only extra step on Mac being the download of the Remote Desktop App.

Now that we're finished, make sure to CLEAN UP YOUR AZURE ENVIRONMENT to avoid any unnecessary charges.

Remember to close your Remote Desktop connection, delete the Resource Group(s) you created at the start of the tutorial, and verify that the Resource Group has been successfully deleted.</p>

<hr>
