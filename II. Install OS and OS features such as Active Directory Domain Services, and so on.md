# AlwaysOnExercise

**II. Install OS and OS features such as Active Directory Domain Services, and so on**
<br/>

**Resources**
------------------------------------------------------------------------------------------------------------------------------------
Windows Server 2016 or 2019 ISO. I use windows server 2016 here, since this is the OS I'm most familiar with. To download, go to [Windows Server 2016 download.](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2016) <br/>

**Steps**
------------------------------------------------------------------------------------------------------------------------------------
1. Instructions to install an OS to virtual Machines. Visit this, [Installing OS On Windows Hyper-V Manager.](https://www.c-sharpcorner.com/UploadFile/746cd9/installing-os-on-windows-hyper-v-manager/) <br/>
2. I'm going to use the Windows Server 2016 Standard (Desktop Experience).

![image](https://user-images.githubusercontent.com/95063830/170939074-549ff061-3589-4ca8-8151-8a689129811f.png)

3. We'll install the OS on the virtual Disk with a 32GB size. The 10 GB virtual disk is for the volume Drive D.

4. Ongoing OS Installation:

![image](https://user-images.githubusercontent.com/95063830/170940628-2c7042dc-f61c-475a-9823-cbf9dabc2bdb.png)

5. OS installed succesfully.

![image](https://user-images.githubusercontent.com/95063830/170941210-91081907-2ffe-4ef4-9fef-b9d9ee0a6752.png)

6. Just like a typical newly installed Windows Server, fill in the requirements such as the password, for the post OS installation procedures.  <br/>
7. Name the exactly like the virtual machine name. Set the proper IP address, Date & time, configure both Drive C (SYSTEM) & DRIVE D (DATABACKUP), and so on.  <br/>
8. For the IPv4 Address, here's the configuration:  <br/>
**hjc-adprod  = 192.168.10.125/24**  <br/>
**hjc-sqlprod = 192.168.10.200/24**  <br/>
**hjc-sqldr01 = 192.168.10.201/24**  <br/>

9. Repeat the above steps for the 2 remaining virtual machines. <br/>
10. (Optional) After that, we will now proceed to the needed install Windows Server Roles and Features for this project. Before that, I will take a checkpoints of each virtual machine first. So even an incident happens, I can easily revert the virtual machines before the changes that I've made. To create a checkpoint, navigate to Hyper-Manager, right-click to Virtual Machine and click "Checkpoint". 

![image](https://user-images.githubusercontent.com/95063830/171326297-1aa494a6-2735-4677-9a91-f054c4f9262c.png)

A checkpoint has created for hjc-adprod virtual machines. Repeat the steps for other virtual machines.

![image](https://user-images.githubusercontent.com/95063830/171326462-17cb363e-7dab-40f9-8340-d54f84b28fed.png)

11. Install .NET Frameworks on the 3 servers. Open PowerShell and run the following command.

```PowerShell
Add-WindowsFeature NET-Framework-Features -Source E:\sources\sxs
```

Succesfully installed!

![image](https://user-images.githubusercontent.com/95063830/171327635-76bfcc94-89b3-4f65-b43d-3db87b5a0914.png)

12. Now we are going to install ADDS (Active Directory Domain Services) on hjc-adprod server. Before that, reconfigure the DNS address of the server first. Set the preferred DNS same as with the server IPv4 Address as shown below:

![image](https://user-images.githubusercontent.com/95063830/171328607-3cfec5da-e1d2-4057-ad41-50732fc1447d.png)

13. Open PowerShell or CMD and run the following:

```PowerShell
ipconfig /flushdns
ipconfig /registerdns
```

![image](https://user-images.githubusercontent.com/95063830/171328779-d0234ebc-83bf-469c-b1e8-5dec48c2d0c4.png)

14. Install ADDS and DNS Server Role using powerShell

```PowerShell
Add-WindowsFeature AD-Domain-Services,DNS -IncludeAllSubFeature -IncludeManagementTools
```

Successfully Installed!

![image](https://user-images.githubusercontent.com/95063830/171332854-20bd8022-01ab-4144-bfcc-06c7a9b33e00.png)

15. Install ADDS Forest by using PowerShell. Run the following on PowerShell.

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

You will be ask to type the password twice. After that, it will restart automatically. <br/>

![image](https://user-images.githubusercontent.com/95063830/187054093-80c3b3fa-9239-474e-aa1d-0f2263d9fea8.png)


AD Forest (hjcdom.local) successfully promoted!

![image](https://user-images.githubusercontent.com/95063830/171442420-fcc580be-efbe-4d93-8190-9d8cef2df2d1.png)

16. If you happen to encounter this DNS issue (preferred DNS address became 127.0.0.1) after Domain promotion, change the preferred DNS address to the actual IPv4 address of the server. Then run ipconfig /flushdns & ipconfig /registerdns and restart DNS services.

![image](https://user-images.githubusercontent.com/95063830/187054320-30579760-96aa-4fab-80c7-8481c7506524.png)
![image](https://user-images.githubusercontent.com/95063830/187054333-76475b02-bf0f-4cab-8900-302eeea7cf01.png)
![image](https://user-images.githubusercontent.com/95063830/187054379-ec19c199-6060-4c41-8df9-6183c9867095.png)


17. To double check the promotion of AD Forest. Perform the following:

Run Windows+R (run), browse \\localhost or \\servername. A succesful and good AD promotion should have 2 shared folders (NETLOGON & SYSVOL).

![image](https://user-images.githubusercontent.com/95063830/187054469-64a73bda-1ec6-446a-86fb-69ef414dbb60.png)

![image](https://user-images.githubusercontent.com/95063830/187054476-c08a7468-91fe-466e-8dd0-f4852b4491d0.png)


On the Event Viewer\Application and Services Logs\DFS Replication, look for **Event ID 4602**. This event log is one way to confirm if AD promotion is good and succesful.

![image](https://user-images.githubusercontent.com/95063830/187054560-4ad58ef5-6a59-471d-bb0c-6705d04dbbd3.png)

Here's the Event ID 4602 log's content:

![image](https://user-images.githubusercontent.com/95063830/187054597-29e80550-42ed-4430-95ea-8abda1850832.png)


18. [Otional] For post installation procedure, install Windows Server Backup features. Use the PowerShell script below to install it:

```PowerShell
Install-WindowsFeature Windows-Server-Backup
```

OS Features installed!

![image](https://user-images.githubusercontent.com/95063830/171443979-a277cccc-bd79-4d47-8b97-d63983a5fe9e.png)


To create a scheduled windows server backup, follow this -> [How to use the backup feature to back up and restore data.](https://docs.microsoft.com/en-us/troubleshoot/windows-server/backup-and-storage/use-backup-feature-back-up-restore-data)

19. Create Windows MMC (Microsoft Management Console) for Active Directory Tools, etc. Run Windows+R (run) again, and type "**mmc**". 

![image](https://user-images.githubusercontent.com/95063830/187054724-79897a18-1d88-4758-8cd1-0468d1a2c0ee.png)

Go to **File** then click **Add & Remove Snap-in**. Select and the follow (listed below). Click OK after. <br/>
a. Active Directory Domains and Trusts <br/>
b. Active Directory Sites and Services <br/>
c. Active Directory Users and Computers <br/>
d. DNS <br/>
e. Group Policy Management <br/>

![image](https://user-images.githubusercontent.com/95063830/187054845-3e86fe50-bc48-4a00-a060-14af8357bf71.png)

Click **File** and click **Save As**, save this mmc to desktop, and name it on your own preferences.

![image](https://user-images.githubusercontent.com/95063830/187054892-9f84a156-b3eb-4f81-88de-2d4994bb561d.png)
![image](https://user-images.githubusercontent.com/95063830/187054904-57104bad-9359-4fc1-a04a-f1e6ee196fd3.png)

20. Go to ADUC (Active Directory Users and Computers), righ-click on the domain (hjcdom.local), select **New\Organization Unit** and named it "Servers". Click OK after.

![image](https://user-images.githubusercontent.com/95063830/187054936-7b3a87eb-a810-4929-94b5-13acee6944e4.png)

![image](https://user-images.githubusercontent.com/95063830/187054949-bd5d9a17-957e-477d-83d3-f82025132c3a.png)

![image](https://user-images.githubusercontent.com/95063830/187054962-d03236a5-140c-4347-b45d-089c170113ae.png)


21. On the **Servers** OU: Right click and select New\Computers.

![image](https://user-images.githubusercontent.com/95063830/187054986-9bacdddd-b5c3-4248-9465-6ab5295a92f1.png)

Create & Register the following computers: <br/>
**hjc-sqlpod** = network/server name of Primary SQL Server ReplicaInstance/Instance/Node <br/>
**hjc-sqldr01** = network/server name of Secondary SQL Server ReplicaInstance/Node       <br/>
**hjc-wfcluster** = network name of windows failover cluster   <br/>
**hjc-aglsnr** =  network name of SQL Server AlwaysOn AG (Availability Group)  <br/>

Click OK after.

![image](https://user-images.githubusercontent.com/95063830/187055032-150fc415-5b9d-4002-90e5-2d10579e4878.png)

![image](https://user-images.githubusercontent.com/95063830/187055142-03cd080f-07ac-4613-81c8-38d2fbaedbe7.png)

22. Grant "Create Computer Object" permission to **Servers** OU. This permission is a prerequisites to setup SQL Server AlwaysOn AG. On the mmc we have created before, navigate to ADUC, click **View**, select **Advanced Features**. 
![image](https://user-images.githubusercontent.com/95063830/187055317-f10824da-6c98-4c3f-9b7a-6fe095be037b.png)

Right-click on **Servers** OU, navigate to Properties\Security\Advanced. 

![image](https://user-images.githubusercontent.com/95063830/187055349-10b70ba9-3a87-40c9-81e3-6aab57368c05.png)

On the **Advanced Security Settings for Servers** click the**Add** button. Navigate to Principal\Object Types\ and uncheck all selection but check the *Computer* object types only. Click OK after. And in the "Enter the object name to select"", type in the network name of the Windows failover cluster (refer to step #21).
Click OK after.

![image](https://user-images.githubusercontent.com/95063830/187055518-edf71656-2be2-4afe-9f52-c0fc440f94c6.png)

In the Permissions list, find and select "Create Computer Objects" then click OK.

![image](https://user-images.githubusercontent.com/95063830/187055560-c15cc45a-06a1-4339-b515-5bea6fae5fcc.png)

Save every changes that you've made.


**Next Stage**
------------------------------------------------------------------------------------------------------------------------------------

[**III. Join MS SQL Database Server to Domain & Create a Windows Failover Cluster**](https://github.com/fortehub/AlwaysOnPractice/blob/317c69b5cb15e205538b469f847784d8688564db/III.%20Join%20MS%20SQL%20Database%20Server%20to%20Domain%20&%20Create%20a%20Windows%20Failover%20Cluster.md)


