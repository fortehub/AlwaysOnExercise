# AlwaysOnExercise

**II. Install OS and OS features such as Active Directory Domain Services**
<br/>
<br/>
<br/>

**A. Resources**
------------------------------------------------------------------------------------------------------------------------------------
**A1.** Windows Server 2016 or 2019 ISO. I use windows server 2016 here, since this is the OS I'm most familiar with. To download, go to [Windows Server 2016 download.](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2016)
<br/>
<br/>

**B. Operating System Installation**
------------------------------------------------------------------------------------------------------------------------------------
**B1.** Instructions to install an OS to virtual Machines. Visit this, [Installing OS On Windows Hyper-V Manager.](https://www.c-sharpcorner.com/UploadFile/746cd9/installing-os-on-windows-hyper-v-manager/)
<br/>
<br/>

**B2.** I'm going to use the Windows Server 2016 Standard (Desktop Experience).
<br/>
![image](https://user-images.githubusercontent.com/95063830/170939074-549ff061-3589-4ca8-8151-8a689129811f.png)
<br/>
<br/>

**B3.** We'll install the OS on the virtual Disk with a 32GB size. The 10 GB virtual disk is for the volume Drive D.
<br/>
Ongoing OS Installation:
<br/>
![image](https://user-images.githubusercontent.com/95063830/170940628-2c7042dc-f61c-475a-9823-cbf9dabc2bdb.png)
<br/>
<br/>

**B4.** OS installed succesfully!
<br/>
Just like a typical newly installed Windows Server, fill in the requirements such as the password, for the post OS installation procedures.
<br/>

![image](https://user-images.githubusercontent.com/95063830/170941210-91081907-2ffe-4ef4-9fef-b9d9ee0a6752.png)
<br/>
<br/>

**B5.** Name the exactly like the virtual machine name. Set the proper IP address, Date & time, configure both Drive C (SYSTEM) & DRIVE D (DATABACKUP), and so on.  <br/>
<br/>

**B6.** For the IPv4 Address, here's the configuration:  
<br/>
**hjc-adprod  = 192.168.10.125/24**  <br/>
**hjc-sqlprod = 192.168.10.200/24**  <br/>
**hjc-sqldr01 = 192.168.10.201/24**  <br/>
<br/>

**B7.** Repeat the above steps for the 2 remaining virtual machines for SQL Server Nodes, but additional Disk Drive **E**.
<br/>

**B8.** *(Optional)* After that, we will now proceed to the needed install Windows Server Roles and Features for this project. Before that, I will take a checkpoints of each virtual machine first. So even an incident happens, I can easily revert the virtual machines before the changes that I've made. To create a checkpoint, navigate to Hyper-Manager, right-click to Virtual Machine and click "Checkpoint". 
<br/>
![image](https://user-images.githubusercontent.com/95063830/171326297-1aa494a6-2735-4677-9a91-f054c4f9262c.png)
<br/>
A checkpoint has created for hjc-adprod virtual machines. Repeat the steps for other virtual machines.
<br/>
![image](https://user-images.githubusercontent.com/95063830/171326462-17cb363e-7dab-40f9-8340-d54f84b28fed.png)
<br/>
<br/>
<br/>

**C. Install OS Roles & Features**
------------------------------------------------------------------------------------------------------------------------------------
**C1.** Install .NET Frameworks on the 3 servers. Open PowerShell and run the following command.
```PowerShell
Add-WindowsFeature NET-Framework-Features -Source E:\sources\sxs
```
![image](https://user-images.githubusercontent.com/95063830/171327635-76bfcc94-89b3-4f65-b43d-3db87b5a0914.png)
<br/>
Succesfully installed!
<br/>
<br/>

**C2.** Now we are going to install ADDS (Active Directory Domain Services) on hjc-adprod server. Before that, reconfigure the DNS address of the server first. Set the preferred DNS same as with the server IPv4 Address as shown below:
<br/>
![image](https://user-images.githubusercontent.com/95063830/171328607-3cfec5da-e1d2-4057-ad41-50732fc1447d.png)

**C3.** Open PowerShell or CMD and run the following:
```PowerShell
ipconfig /flushdns
ipconfig /registerdns
```
![image](https://user-images.githubusercontent.com/95063830/171328779-d0234ebc-83bf-469c-b1e8-5dec48c2d0c4.png)
<br/>
<br/>

**C4.** Install ADDS and DNS Server Role using powerShell
```PowerShell
Add-WindowsFeature AD-Domain-Services,DNS -IncludeAllSubFeature -IncludeManagementTools
```
![image](https://user-images.githubusercontent.com/95063830/171332854-20bd8022-01ab-4144-bfcc-06c7a9b33e00.png)

Successfully Installed!
<br/>
<br/>

**C5.** Install ADDS Forest by using PowerShell. Run the following on PowerShell.
```PowerShell
Install-ADDSForest `
-CreateDnsDelegation:$false `
-DatabasePath "C:\Windows\NTDS" `
-DomainMode "Win2012R2" `
-DOmainName "type here the name of your domain" `
-DomainNetBiosName "type here the name of your domain in Capital Letters" `
-ForestMode "Win2012R2" `
-InstallDns:$true
-LogPath "C:\Windows\NTDS" `
-NoRebootOnCompletion:$false `
-SysvolPath "C:\Windows\SYSVOL" `
-Force:$true
```
You will be ask to type the password twice. After that, it will restart automatically. 
<br/>
![image](https://user-images.githubusercontent.com/95063830/187054093-80c3b3fa-9239-474e-aa1d-0f2263d9fea8.png)
![image](https://user-images.githubusercontent.com/95063830/171442420-fcc580be-efbe-4d93-8190-9d8cef2df2d1.png)
AD Forest (hjcdom.local) successfully promoted!
<br/>
<br/>

**C6.** If you happen to encounter this DNS issue (preferred DNS address became 127.0.0.1) after Domain promotion, change the preferred DNS address to the actual IPv4 address of the server. Then run ipconfig /flushdns & ipconfig /registerdns and restart DNS services.
<br/>
![image](https://user-images.githubusercontent.com/95063830/187054320-30579760-96aa-4fab-80c7-8481c7506524.png)
![image](https://user-images.githubusercontent.com/95063830/187054333-76475b02-bf0f-4cab-8900-302eeea7cf01.png)
![image](https://user-images.githubusercontent.com/95063830/187054379-ec19c199-6060-4c41-8df9-6183c9867095.png)
<br/>
<br/>

**C7.** To double check the promotion of AD Forest. Perform the following:
<br/>
Run Windows+R (run), browse \\localhost or \\servername. A succesful and good AD promotion should have 2 shared folders (NETLOGON & SYSVOL).
<br/>
![image](https://user-images.githubusercontent.com/95063830/187054469-64a73bda-1ec6-446a-86fb-69ef414dbb60.png)
![image](https://user-images.githubusercontent.com/95063830/187054476-c08a7468-91fe-466e-8dd0-f4852b4491d0.png)
<br/>
On the Event Viewer\Application and Services Logs\DFS Replication, look for **Event ID 4602**. This event log is one way to confirm if AD promotion is good and succesful.
<br/>
![image](https://user-images.githubusercontent.com/95063830/187054560-4ad58ef5-6a59-471d-bb0c-6705d04dbbd3.png)

Here's the Event ID 4602 log's content:
<br/>
![image](https://user-images.githubusercontent.com/95063830/187054597-29e80550-42ed-4430-95ea-8abda1850832.png)
<br/>
<br/>

**C8.** [Otional] For post installation procedure, install Windows Server Backup features. Use the PowerShell script below to install it:
```PowerShell
Install-WindowsFeature Windows-Server-Backup
```
![image](https://user-images.githubusercontent.com/95063830/171443979-a277cccc-bd79-4d47-8b97-d63983a5fe9e.png)
<br/>
OS Features installed!
<br/>
To create a scheduled windows server backup, follow this -> [How to use the backup feature to back up and restore data.](https://docs.microsoft.com/en-us/troubleshoot/windows-server/backup-and-storage/use-backup-feature-back-up-restore-data)
<br/>
<br/>

**C9.** Create Windows MMC (Microsoft Management Console) for Active Directory Tools, etc. Run Windows+R (run) again, and type "**mmc**". 
<br/>
![image](https://user-images.githubusercontent.com/95063830/187054724-79897a18-1d88-4758-8cd1-0468d1a2c0ee.png)

Go to **File** then click **Add & Remove Snap-in**. Select and the follow (listed below). Click OK after. <br/>
**a.** Active Directory Domains and Trusts <br/>
**b.** Active Directory Sites and Services <br/>
**c.** Active Directory Users and Computers <br/>
**d.** DNS <br/>
**e.** Group Policy Management <br/>
<br/>
![image](https://user-images.githubusercontent.com/95063830/187054845-3e86fe50-bc48-4a00-a060-14af8357bf71.png)
<br/>

Click **File** and click **Save As**, save this mmc to desktop, and name it on your own preferences.

![image](https://user-images.githubusercontent.com/95063830/187054892-9f84a156-b3eb-4f81-88de-2d4994bb561d.png)
![image](https://user-images.githubusercontent.com/95063830/187054904-57104bad-9359-4fc1-a04a-f1e6ee196fd3.png)
<br/>
<br/>

**C10.** Go to ADUC (Active Directory Users and Computers), righ-click on the domain (hjcdom.local), select **New\Organization Unit** and named it "Servers". Click OK after.

![image](https://user-images.githubusercontent.com/95063830/187054936-7b3a87eb-a810-4929-94b5-13acee6944e4.png)
![image](https://user-images.githubusercontent.com/95063830/187054949-bd5d9a17-957e-477d-83d3-f82025132c3a.png)
![image](https://user-images.githubusercontent.com/95063830/187054962-d03236a5-140c-4347-b45d-089c170113ae.png)
<br/>
<br/>

**C11.** On the **Servers** OU: Right click and select New\Computers.

![image](https://user-images.githubusercontent.com/95063830/187054986-9bacdddd-b5c3-4248-9465-6ab5295a92f1.png)

Create & Register the following computers: <br/>
**hjc-sqlpod** = network/server name of Primary SQL Server ReplicaInstance/Instance/Node 
<br/>
**hjc-sqldr01** = network/server name of Secondary SQL Server ReplicaInstance/Node      
<br/>
Click OK after.
<br/>
![image](https://user-images.githubusercontent.com/95063830/187055032-150fc415-5b9d-4002-90e5-2d10579e4878.png)
![image](https://user-images.githubusercontent.com/95063830/187118653-14f89a9d-d369-49f2-a838-be72feb9a2cb.png)
<br/>
<br/>

**C12.** Create service account for SQL server. To create a service account follow this procedures [Configure Managed Service Accounts for SQL Server Always On Availability Group](https://www.sqlshack.com/configure-managed-service-accounts-for-sql-server-always-on-availability-groups/). This is the recommended setup, but in this demo we will just use the AD Domain "Administrator" account instead though this is not recommended on production environment. 
<br/>

Add AD Domain Administrator account to "Logon as a Service". Navigate Group Policy Management\Forest, right-click and click Edit on "Default Domain Policy". Now, navigate to Computer configuration\Policies\Windows Settings\Security Settings\User Rights Assignment and right-click the "Logon as a Service" policy. On the "Logon as a Service" properties, check the "Define these policy settings", click the "Add User or Group" button and browse the "**Administrator**'' account. Click Apply and OK after.

![image](https://user-images.githubusercontent.com/95063830/187118826-bb119795-b0ad-4505-b01c-d11c7e54d27a.png)
<br/>
<br/>

**C13.** Open PowerShell, and run the following:
```PowerShell
gpupdate /force
```
![image](https://user-images.githubusercontent.com/95063830/187056491-fd153d32-ba72-474e-b8fa-59aaeed7d7fa.png)

<br/>
Save every changes that you've made.
<br/>

You can also, create a virtual machine checkpoint or create a windows server backup for disaster recovery purposes.
<br/>
<br/>
<br/>

**Next Stage**
------------------------------------------------------------------------------------------------------------------------------------
[**III. Join MS SQL Database Server to Domain & Create a Windows Failover Cluster**](https://github.com/fortehub/AlwaysOnPractice/blob/317c69b5cb15e205538b469f847784d8688564db/III.%20Join%20MS%20SQL%20Database%20Server%20to%20Domain%20&%20Create%20a%20Windows%20Failover%20Cluster.md)


