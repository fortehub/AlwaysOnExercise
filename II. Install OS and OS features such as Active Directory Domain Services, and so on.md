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

![image](https://user-images.githubusercontent.com/95063830/187054079-687ac2dc-684b-478b-9d17-8ff285f9ac39.png)


16. AD Forest (HJCDOM) successfully installed!

![image](https://user-images.githubusercontent.com/95063830/171442420-fcc580be-efbe-4d93-8190-9d8cef2df2d1.png)

17. For post installation procedure, install Windows Server Backup features. Use the PowerShell script below to install it:

```PowerShell
Install-WindowsFeature Windows-Server-Backup
```

OS Features installed!

![image](https://user-images.githubusercontent.com/95063830/171443979-a277cccc-bd79-4d47-8b97-d63983a5fe9e.png)


To create a scheduled windows server backup, follow this -> [How to use the backup feature to back up and restore data.](https://docs.microsoft.com/en-us/troubleshoot/windows-server/backup-and-storage/use-backup-feature-back-up-restore-data)

**Next Stage**
------------------------------------------------------------------------------------------------------------------------------------

[**III. Join MS SQL Database Server to Domain & Create a Windows Failover Cluster**](https://github.com/fortehub/AlwaysOnPractice/blob/317c69b5cb15e205538b469f847784d8688564db/III.%20Join%20MS%20SQL%20Database%20Server%20to%20Domain%20&%20Create%20a%20Windows%20Failover%20Cluster.md)


