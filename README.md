<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)
<p>
  Part 2: Delopying Active Directory
</p></h1>
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

- Step 1: Install Active Directory
- Step 2: Create a Domain Admin user within the domain
- Step 3: Join Client-1 to your domain (mydomain.com)

<h2>Deployment and Configuration Steps</h2>

<h3>Installing Active Directory </h3>
<p>
Continuing from Part 1, we will be installing active directory domain services on the DC-1 VM.  
</p>
<p>
<img width="1903" alt="1" src="https://github.com/user-attachments/assets/913b05b6-27cf-4bcc-9220-9985acc71100" />
</p>
<p>
To do this, log into DC-1 and go to > Windows > Open Server Manager > Add Roles and Features. 
</p>
<br />

<p>
<img width="779" alt="2" src="https://github.com/user-attachments/assets/4fdae979-440d-46f6-b6b1-7a04e20bfb7c" />

</p>
<p>
When you have opened the Server Roles menu in the Add Roles and Features Wizard, check the box next to Active Directory Domain Services and click next until it lets you click install. 
</p>
<br />
<p>
  Now, we will promote DC-1 as a working Domain Controller by setting up a new forest as mydomain.com. 
</p>
<p>
<img width="1903" alt="3" src="https://github.com/user-attachments/assets/3f9614cf-bc20-4296-a66f-bca974a54f87" />

</p>
<p>
To do this, go to Server Manager > Click on the notification button (top right flag icon) > Click on ‘Promote this server to a domain controller.’ 
</p>
<br />

<p>
<img width="759" alt="4" src="https://github.com/user-attachments/assets/80f7238b-16c8-4e8a-b67c-4f7560f5199e" />
</p>
<p>
On this pop-up window, click ‘Add a new forest’ > Type in the Root domain name (mydomain.com) > Click Next until you can Install like the next screenshot. 
</p>
<p>
  <img width="758" alt="5" src="https://github.com/user-attachments/assets/c017ed32-2bcf-4595-8f99-f50f2e4ba59f" />=
</p>
<br />

<p>
  DC-1 will restart automatically when installed
</p>
<p>
<img width="1167" alt="6" src="https://github.com/user-attachments/assets/af56aa02-86fd-4f1b-8093-c51a0f1117bb" />
</p>
<p>
 When you log back in, you now have to use 'mydomain.com\<USERNAME>' as the username in Windows App. 
  </p>
  <p>
    This is because DC-1 is a domain controller now which means that you need to specify the context of the domain and the user. 
  </p>
<br />

<h3>Creating a Domain Admin user within the domain </h3>

<p>
Now, we will create a Domain Admin user within the domain in DC-1.   
</p>

<p>
<img width="1029" alt="7" src="https://github.com/user-attachments/assets/d19be2a3-7c14-4274-8db8-6c4f8b44e6f2" />

</p>
<p>
To do this, log into DC-1 and open Active Directory Users and Computers. 
</p>
<br />

<p>
<img width="1026" alt="8" src="https://github.com/user-attachments/assets/6ec3900a-17b4-4315-90b8-b300fc89eb9a" />
</p>
<p>
Right click mydomain.com > New > Organizational Unit.
</p>
<br />

<p>
<img width="2560" alt="9" src="https://github.com/user-attachments/assets/bf0ce709-9cdb-4850-b9ab-a61f55ae6764" />

</p>
<p>
Name it “_EMPLOYEES” 
</p>
<br />

<p>
  <img width="1024" alt="10" src="https://github.com/user-attachments/assets/90862761-4c66-4d92-9416-8e2bc7057165" />
<img width="1003" alt="10-2" src="https://github.com/user-attachments/assets/a5add157-dffb-4175-8c5d-6b2183231b30" />
</p>
<p>
Follow the same steps and make two more organizational units named “_ADMINS” and “_CLIENTS” 
</p>
<br />

<p>
  Now, we will create a new User named Jane Doe in the _ADMINS folder.
</p>
<p>
  <img width="1026" alt="11" src="https://github.com/user-attachments/assets/53c516e7-86e8-4342-954c-254b7727105c" />
</p>
<p>
Go to the _ADMINS folder > right click > New > User 
</p>
<br />

<p>
  <img width="1027" alt="12" src="https://github.com/user-attachments/assets/5c50704e-08b4-4ced-80c9-73a4de062672" />
</p>

<p>
Type Jane’s username and password (mine were jane_admin and Password1). Then create the User.  
</p>

<p>
Putting Jane in the _ADMINS folder doesn’t make her an actual admin. 
</p>
<p>
  So, we are going to make Jane an admin by adding her to the “Domain Admins” Security Group. 
</p>
<p>
<img width="1024" alt="13" src="https://github.com/user-attachments/assets/59936c53-f379-4dd7-9202-3a8e6ce6a3a8" />
</p>

<p>
To do this right click Jane Doe > Properties > Member of > Add > Type Domain Admins > OK. 
</p>
<br />

<p>
<img width="1166" alt="Screenshot 2025-01-20 at 6 47 54 PM" src="https://github.com/user-attachments/assets/659e3f9c-5137-4553-921e-affd9453da37" />
</p>
<p>
Now that Jane Doe is a Domain Admin, we are going to log out of DC-1 and log back into DC-1 as Jane to see if we can log in as Jane. 
</p>
<p>
  Make sure to put in mydomain.com\jane_admin as the username and the correct password. 
</p>
<br />

<h3>Joinning Client-1 to your domain (mydomain.com) </h3>
<p>
  Now, from the Client-1 VM (windows 10), we will join the domain. 
</p>

<p>
<img width="1195" alt="14" src="https://github.com/user-attachments/assets/25efcf09-9b31-4605-893e-d8c611392eb8" />
</p>
<p>
Log into Client-1 > Right click the Windows button > System > Rename this PC (on the right). 
</p>
<br />

<p>
<img width="763" alt="15" src="https://github.com/user-attachments/assets/d9ed4c06-6dfe-4ff5-b364-c12033be6c94" />
</p>
<p>
Computer name > Change... > Domain > Type mydomain.com. We are able to join the domain that we created because Client-1 is in DC-1's DNS server. 
</p>
<br />

<p>
<img width="454" alt="16" src="https://github.com/user-attachments/assets/10260561-1e55-4231-8553-15c948f710dd" />

</p>
<p>
When you see this pop-up screen, put Jane Doe’s credentials and it will authorize it because we set Jane as the domain admin. 
</p>
<p>
  This will make you restart the computer.  
</p>
<br />

<p>
<img width="1003" alt="17" src="https://github.com/user-attachments/assets/263afaf5-5b4f-4842-95cc-9a9b880972d6" />

</p>
<p>
Now when you log into DC-1 > Open Active Directory Users and Computers > Click mydomain.com > Computers, you will see that Client-1 is now added to the domain.  
</p>
<br />

<p>
<img width="1003" alt="18" src="https://github.com/user-attachments/assets/a148146c-839b-499e-b305-6237a84bd129" />
</p>
<p>
Drag Client-1 from Computers to the _CLIENTS folder. 
</p>
<br />

We have succesfully set everything up for the Domain Controller and the client's computer. On Part 3 of this lab, we will explore what you can do with Active Dicrectory.
