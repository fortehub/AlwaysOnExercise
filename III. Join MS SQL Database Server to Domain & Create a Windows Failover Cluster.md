# AlwaysOnExercise

**III. Join MS SQL Database Server to Domain & Create a Windows Failover Cluster**
<br/>
<br/>
<br/>

**A. Join the 2 SQL Server Nodes to AD Domain**
------------------------------------------------------------------------------------------------------------------------------------
**A1.** Make sure both servers can communicate with each other. Do a ping test. Disable the firewall of both servers (hjc-adprod, hjc-sqlprod, & hjc-sqldr01). This is not a recommended setup for a Production Environment. We are in testing environment in this demo so we can disable it here. 
<br/>

To disable, perform the PowerShell script below:
```PowerShell
Set-NetFirewallProfile -Profile Public,Private,Domain -Enabled False
```
![image](https://user-images.githubusercontent.com/95063830/172056165-32b69b43-f1fd-416e-862f-8e7091b941be.png)

Follow this guide to configure firewall in a production environment for SQL Server, [Configure the Windows Firewall to Allow SQL Server Access.](https://docs.microsoft.com/en-us/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access?view=sql-server-ver16) 

**Note!** Perform this on the 3 server (hjc-adprod, hjc-sqlprod, hjc-sqldr01).
<br/>
<br/>

**A2.** Change the Preferred DNS addresses of the MS SQL Server Replica Nodes, match with the IPv4 Address of the AD Domain Controller (hjc-adprod). 
<br/>
![image](https://user-images.githubusercontent.com/95063830/172056382-1406bdb8-7f1e-4d1a-bd51-596b61bb4257.png)

After changing, open CMD or PowerShell and run the commands below to flush and register DNS records in DNS Cache.
```PowerShell
ipconfig /flushdns
ipconfig /registerdns
```
![image](https://user-images.githubusercontent.com/95063830/172056509-06c95782-a0d4-46f1-ba38-432cda8111be.png)
<br/>
<br/>

**A3.** Join the MS SQL Database Nodes to AD Domain. To join, perform the following CMDlets below:
```PowerShell
Add-Computer -DomainName <the domain name> -Credential HJCDOM\Administrator  -Restart
```
Type in the Domain credential/Password:
<br/>
![image](https://user-images.githubusercontent.com/95063830/187055932-e6e3bc39-bb31-4ec4-a5d9-4a11dab709b1.png)

The server will restart automatically after joining successfully.
<br/>
<br/>

**A4.** Repeat these steps for the 2nd SQL Server Node.
<br/>
<br/>
<br/>

**B. Create a Windows Failover Cluster without Network Shared Storage**
------------------------------------------------------------------------------------------------------------------------------------
**B1.** Open PowerShell and run the following CMDlet below on the **2 SQL Server Nodes**, to install WFC (Windows Failover Clustering) roles.
```PowerShell
Install-WindowsFeature Failover-Clustering -IncludeManagementTools
```
![image](https://user-images.githubusercontent.com/95063830/172080500-0a3653c2-a109-49fc-9f06-37d1ed2aacd0.png)
<br/>
<br/>

**B2.** To create a cluster using GUI, follow this guide, [How To Create a Windows Server Failover Cluster Without Shared Storage.](https://redmondmag.com/articles/2014/07/14/windows-server-failover-cluster.aspx)

For PowerShell follow the succeeding steps.
<br/>
<br/>

**B3.** Validate the Cluster first. Run the following CMDlet below.
```PowerShell
Test-Cluster -Node <Name of nodes>
```
![image](https://user-images.githubusercontent.com/95063830/172083619-44671259-f307-4ade-8db0-0478b0f05c14.png)
<br/>
<br/>

**B4.** Create the Cluster.
<br/>
Run the following command to create the cluster:
```PowerShell
New-Cluster -Name <cluster network name> -Node <name of nodes,nodes> -NoStorage -StaticAddress <cluster IPv4 Address>
```
Where -Name is your preferred cluster network name. Here, I named it "**hjc-wfcluster**''.
<br/>
![image](https://user-images.githubusercontent.com/95063830/172085190-3b17e090-0e86-4c5e-a48c-25aa4760b05d.png)

Clustered created but there are some minor error based on the screenshot above. Upon checking of the logs, the error is about cluster witness, which in our case, we don't need it, so we can disregard the error.
<br/>
![image](https://user-images.githubusercontent.com/95063830/172085131-6b63e9f5-9e36-402b-8bd9-be2b51a7bf84.png)

Verifying thru the Failover Cluster Manager.
<br/>
![image](https://user-images.githubusercontent.com/95063830/172085248-52970aaf-df75-4617-8631-48868f5f0e32.png)

Clustered creation verified!!
<br/>

For more info, visit this guide, [Create a Two-Node Windows Cluster.](https://argonsys.com/microsoft-cloud/articles/create-two-node-windows-cluster/)
<br/>

After the creation of Windows Failover Cluster, a new computer object of Cluster will be created on ADUC\hjcdom.local\Servers OU.
![image](https://user-images.githubusercontent.com/95063830/187057143-1e682d29-484d-4853-bca8-a3866b623bde.png)
<br/>
<br/>

**B5.** Grant "Create Computer Object" permission. This permission is a prerequisites to setup SQL Server AlwaysOn AG. On the mmc we have created before, navigate to ADUC, click **View**, select **Advanced Features**. 
<br/>
![image](https://user-images.githubusercontent.com/95063830/187055317-f10824da-6c98-4c3f-9b7a-6fe095be037b.png)
<br/>
<br/>

**B6.** Right-click on **Servers** OU, navigate to Properties\Security\Advanced. 
<br/>
![image](https://user-images.githubusercontent.com/95063830/187055349-10b70ba9-3a87-40c9-81e3-6aab57368c05.png)
<br/>
<br/>

**B7.** On the **Advanced Security Settings for Servers** click the **Add** button. Navigate to Principal\Object Types\ and uncheck all selection but check the *Computer* object types only. Click OK after. And in the "Enter the object name to select"", type in the network name of the Windows failover cluster (refer to step #4).
<br/>

Click OK after.
<br/>
![image](https://user-images.githubusercontent.com/95063830/187055518-edf71656-2be2-4afe-9f52-c0fc440f94c6.png)
![image](https://user-images.githubusercontent.com/95063830/187127768-86efb9d6-e464-41bf-a1e4-90c06ec55c0e.png)
<br/>
<br/>

**B8.** Click the **Advanced** button to go to "Advanced Security Settings for Servers". Click the **Add** button again to go "Permission Entry for Servers", click "Principal" then set "Object Types" to "Computer" only again, then find the CNO (the virtual network name of the cluster)
<br/>
![image](https://user-images.githubusercontent.com/95063830/187128163-78df953e-e1e4-4898-8dfe-2d2a7f215d6e.png)
<br/>
<br/>

**B9.** In the Permissions list, find and select "Create Computer Objects" then click OK.
<br/>
![image](https://user-images.githubusercontent.com/95063830/187055560-c15cc45a-06a1-4339-b515-5bea6fae5fcc.png)

**B10.** Save every changes that you've made.
<br/>
<br/>
<br/>

**Next Stage**
------------------------------------------------------------------------------------------------------------------------------------
[IV. Install & Configure SQL Server & SSMS on both SQL Server Nodes](https://github.com/fortehub/AlwaysOnPractice/blob/e77e7461f693bedf89bc4c02019e6ef2189619e6/IV.%20Install%20&%20Configure%20SQL%20Server%20&%20SSMS%20on%20virtual%20Machines.md)






