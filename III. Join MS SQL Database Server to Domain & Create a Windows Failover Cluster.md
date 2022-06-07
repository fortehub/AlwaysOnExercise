# AlwaysOnPractice

**III. Join MS SQL Database Server to Domain & Create a Windows Failover Cluster**
<br/>

**Steps**
------------------------------------------------------------------------------------------------------------------------------------
**Joining to Domain**

1. Make sure both servers can communicate with each other. Do a ping test. Disable the firewall of both servers (hjc-adprod, hjc-sqlprod, & hjc-sqldr01). This is not a recommended setup for a Production Environment. We are in testing environment in this demo so we can disable it here. <br/>

To disable, perform the PowerShell script below: <br/>
```PowerShell
Set-NetFirewallProfile -Profile Public,Private,Domain -Enabled False
```
![image](https://user-images.githubusercontent.com/95063830/172056165-32b69b43-f1fd-416e-862f-8e7091b941be.png)

Perform this on the 3 server.

2. Change the Preferred DNS addresses of the MS SQL Database Servers, match with the IPv4 Address of the Domain Controller (hjc-adprod). 

![image](https://user-images.githubusercontent.com/95063830/172056382-1406bdb8-7f1e-4d1a-bd51-596b61bb4257.png)

After changing, open CMD or PowerShell and run the commands below to flush and register DNS records in DNS Cache.

```PowerShell
ipconfig /flushdns
ipconfig /registerdns
```
![image](https://user-images.githubusercontent.com/95063830/172056509-06c95782-a0d4-46f1-ba38-432cda8111be.png)


3. [Optional] On the Domain Controller side (hjc-adprod), add new OU (Organization Unit) for the SQL Server on the ADUC (Active Directory Users and Computers).

![image](https://user-images.githubusercontent.com/95063830/172057337-d5fa251f-a205-4596-9d8a-5044997b7137.png)

Get the Distinguised name of the OU we have just created. We need this for the next step. To get the Distinguised name, follow this guide, [How to find the distinguishedName of an OU.](https://support.xink.io/support/solutions/articles/1000246165-how-to-find-the-distinguishedname-of-an-ou-)


4. Join the MS SQL Database Servers to join. To join, perform the following CMDlets below:

```PowerShell
Add-Computer -DomainName "the domain name" -Credential HJCDOM\Administrator -OUPath "OU=SQL Server,OU=SQL Server,DC=hjcdom,DC=local" -Restart
```
Where -OUPath is the Distinguished Name of the OU we've created a while ago.

Type in the Domain credential/Password:

![image](https://user-images.githubusercontent.com/95063830/172058816-41999c2e-5358-4605-b6dc-7fe34a243311.png)

The server will restart automatically after joining successfully.

5. Repeat the steps for the 2nd MSSQL Database Server.<br/>
<br/>
<br/>



**Creating a Windows Failover Cluster**

1. Open PowerShell and run the following CMDlet below on the 2 Database Server, to install WFC (Windows Failover Clustering) roles.

```PowerShell
Install-WindowsFeature Failover-Clustering -IncludeManagementTools
```
![image](https://user-images.githubusercontent.com/95063830/172080500-0a3653c2-a109-49fc-9f06-37d1ed2aacd0.png)

2. To create a cluster using GUI, follow this guide, [How To Create a Windows Server Failover Cluster Without Shared Storage.](https://redmondmag.com/articles/2014/07/14/windows-server-failover-cluster.aspx)

For PowerShell follow the succeeding steps.

3. Validate the Cluster first. Run the following CMDlet below.

```PowerShell
Test-Cluster -Node <Name of nodes>
```
![image](https://user-images.githubusercontent.com/95063830/172083619-44671259-f307-4ade-8db0-0478b0f05c14.png)

4. Create the Cluster. <br/>
Run the following command to create the cluste:

```PowerShell
New-Cluster -Name "your cluster name" -Node <name of nodes,nodes> -NoStorage -StaticAddress 192.168.10.130
```
![image](https://user-images.githubusercontent.com/95063830/172085190-3b17e090-0e86-4c5e-a48c-25aa4760b05d.png)

Clustered created but there are some minor error based on the screenshot above. Upon checking of the logs, the error is about cluster witness, which in our case, we don't need it, so we can disregard the error.

![image](https://user-images.githubusercontent.com/95063830/172085131-6b63e9f5-9e36-402b-8bd9-be2b51a7bf84.png)

Verifying thru the Failover Cluster Manager.

![image](https://user-images.githubusercontent.com/95063830/172085248-52970aaf-df75-4617-8631-48868f5f0e32.png)

Clustered creation verified!!

For more info, visit this guide, [Create a Two-Node Windows Cluster.](https://argonsys.com/microsoft-cloud/articles/create-two-node-windows-cluster/)

**Next Stage**
------------------------------------------------------------------------------------------------------------------------------------

[IV. Install & Configure SQL Server & SSMS on virtual Machines](https://github.com/fortehub/AlwaysOnPractice/blob/e77e7461f693bedf89bc4c02019e6ef2189619e6/IV.%20Install%20&%20Configure%20SQL%20Server%20&%20SSMS%20on%20virtual%20Machines.md)






