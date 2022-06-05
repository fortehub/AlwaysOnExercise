# AlwaysOnPractice
<br/>
**III. Join MS SQL Database Server to Domain & Create a Windows Failover Cluster**
<br/>

**Steps**
------------------------------------------------------------------------------------------------------------------------------------
**Joining tO Domain**

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
Where -OUPath is the Distinguished Name of the OUPath we created a while ago.

Type in the Domain credential/Password:

![image](https://user-images.githubusercontent.com/95063830/172058816-41999c2e-5358-4605-b6dc-7fe34a243311.png)

The server will restart automatically after joining successfully.

5. Repeat the steps for the 2nd MSSQL Database Server.<br/>


**Creating a Windows Failover Cluster**




