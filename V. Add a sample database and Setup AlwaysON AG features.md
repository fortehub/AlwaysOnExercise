# AlwaysOnPractice

**V. Add a sample database and Setup AlwaysON AG features**
<br/>

**Resources**
------------------------------------------------------------------------------------------------------------------------------------
1. Download the AdventureWorks Sample databases here, [AdventureWorks sample databases.](https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms)
 
We are going the 2016 & 2019 version of AdventureWorks database.  <br/>

**Steps**
------------------------------------------------------------------------------------------------------------------------------------
**Database Restoration**

1. Restore the 2 databases on hjc-sqlprod server only. We can use the GUI to restore a database [Restore a Database](https://www.quackit.com/sql_server/sql_server_2016/tutorial/restore_a_database_in_sql_server_2016.cfm), or follow the T-SQL script below.

```SQL
Use master
GO

Restore Database [AdventureWorks2016] from disk = 'D:\mssql\backups\AdventureWorks2016.bak' With File = 1, 
Move N'D:\mssql\datalogs\AdventureWorks2016_Data.mdf', 
Move N'D:\mssql\datalogs\AdventureWorks2016_log.mdf'
GO
```

Database Restoration Complete!
![image](https://user-images.githubusercontent.com/95063830/172409617-a189dfbc-1ab4-4d73-a696-80ed833f455c.png)

Repeat the steps above for AdventureWorks2019 database!
<br/>
<br/>


**Setup AlwaysON Availability Group**
1. First, we must replace the logon account for SQL Server Services. We will going to use in this testing environment the Domain Administrator account. Note, this is not a recommended setup for production environment, create another Domain service account for SQL Server services. 

Open SQL Server Configuration Manager. Go to SQL Services section, and replace the default account with the Domain Administrator for SQL Server Server & SQL Server Agent servces, then click Apply.

![image](https://user-images.githubusercontent.com/95063830/172411363-327038ec-a6f8-451a-800b-56cb45bbed3d.png)

Repeat the steps on the 2nd database server (hjc-sqldr01).
<br/>

2. Enable TCP/IP connections for SQL Server. Go to SQL Server Network Configuration\Protocols for MSSQLSERVER, Select and right-click the TCP/IP protocol name and select "Enable". Restart the SQL Server services after enabling to take effect the changes.

![image](https://user-images.githubusercontent.com/95063830/172412797-52f994d7-6b80-46be-a9b1-dcc7039f267e.png)
<br/>

3. At the SQL Server Configuration\SQL Server Services, right-click SQL Server(MSSQLSERVER), go to "AlwaysOn Availability Groups" section and check the Enable button. Click apply and OK. Repeat this on the 2nd Database Server (hjc-sqldr01). Restart the SQL Server services to take effect.

![image](https://user-images.githubusercontent.com/95063830/172414076-b40dcf22-a0c4-4e59-87c9-6b53c68cd70f.png)
<br/>

4. Back to SSMS, right-click on the Always-On High Availability section and select "New Availability Group Wizard".

![image](https://user-images.githubusercontent.com/95063830/173292528-ad5f23e9-0de6-4f36-9013-40cf19f06da9.png)
<br/>

5. Provide an AG name, then click Next.
 
![image](https://user-images.githubusercontent.com/95063830/173292764-dd9be221-4d11-41c2-a094-8265d207b8fb.png)
<br/>

Note: Make sure all the database is in "Full Recovery" model. Full recovery model is required to setup Availability Group.

![image](https://user-images.githubusercontent.com/95063830/173293196-5514ce4e-6364-495b-8733-3bd556d3772d.png)
<br/>

6. Create a database backup first for each databases, to proceed on this step. In the image below, AdventureWorks2016 does not a have a database backup, so we are going to take a backup of this particular database.

![image](https://user-images.githubusercontent.com/95063830/173293775-0771d623-cba2-4d35-bffc-30735d36c0ab.png)

In the image above, AdventureWorks2016 does not a have a database backup, so we are going to take a backup of this particular database. You can [perform database backup thru the GUI](https://www.mssqltips.com/sqlservertutorial/7/sql-server-full-backups/) , but we can also perform using T-SQL. To perform backup, follow the T-SQL script below:

```T-SQL
BACKUP DATABASE [AdventureWorrks2016]
TO DISK = N'D:\mssql\backups\AdventureWorks2016.bak' WITH NOFORMAT,
NAME = N'AdventureWorks2016-FullDatabase Backup'
GO
```

![image](https://user-images.githubusercontent.com/95063830/173294496-5a6a4562-b156-46d1-920d-5d3041292413.png) <br/>
Backup also complete!
<br/>

Now click the "Refresh" button to update the Status. It should be ok now. Check the database you want to be part of (AG)Availability Group, then click Next.

![image](https://user-images.githubusercontent.com/95063830/173294869-29d67baf-6ea3-44f1-8697-1b8397c411b9.png)
<br/>

7. Follow the images below for the Replicas steps.

Check the Automatic Failover (Up to 5), Availability Mode is Synchronous commit, Readable Secondary is Yes. Click the Add Replica button and add the 2nd SQL Server Instance (hjc-sqldr01).

![image](https://user-images.githubusercontent.com/95063830/173295620-92fce8c6-762d-4f4d-ab65-eb37e7cf4f06.png)

Click the Add Replica button and add the 2nd SQL Server Instance (hjc-sqldr01). A "Connect to Server' mini windows will appear. type in the network names or IP of the 2nd SQL Server instance, and connect to it.

![image](https://user-images.githubusercontent.com/95063830/173295832-bd6a5c1f-f88e-4654-9cb4-c21939a5f94b.png)

Copy the configuration of the 1st SQL Server Instance.

![image](https://user-images.githubusercontent.com/95063830/173296037-4a9ea5ae-831e-49df-95b3-4ac92b5fd20f.png)

Leave the default configuration for "Backup Preferences" which is "Prefer Secondary". We can skip the "Listener" creation and do it later. Set aside Read-Only Routing, we don't need in this setup. Click Next to proceed.
<br/>

8. At the "Select Data Synchronization", you can choose base on your preferences. Here I will choose the default "Automatic seeding". Just make sure both the database, log and backups are in the same path/location for all SQL Server Instance to make this work. Click Next.

![image](https://user-images.githubusercontent.com/95063830/173297393-8fb39a17-a9a1-4cbc-9816-ce2214bb1f93.png)
<br/>

9. Click to Proceed. 

![image](https://user-images.githubusercontent.com/95063830/173297490-c8233fc5-e12a-4a5e-9665-15c29cdadcc7.png)
<br/>

10. Click Finish

![image](https://user-images.githubusercontent.com/95063830/173297559-99602737-e3ed-4cf8-8e8a-fdb0f39ad230.png)
<br/>

11. Done!

![image](https://user-images.githubusercontent.com/95063830/173297642-9b5e7657-265e-48ab-8377-e9f9918f297d.png)

![image](https://user-images.githubusercontent.com/95063830/173297853-62384730-3a20-47d8-aec3-33ba9923a9aa.png)
<br/>

12. Before we create the AG listener, Go to hjc-adprod, grant the computer object associated with the cluster group 'create computer objects' rights in Active Directory as per this link [Failover Cluster Step-by-Step Guide: Configuring Accounts in Active Director.](ttps://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002(v=ws.10)?redirectedfrom=MSDN#BKMK_steps_precreating)<br/>

Follow the images below:

Click Properties

![image](https://user-images.githubusercontent.com/95063830/173306358-61eb21b1-8e03-45a5-a83e-27f1d1c1ce65.png)

Go to Security, click Advanced button.

![image](https://user-images.githubusercontent.com/95063830/173306570-a56d79e6-d7ae-4b3a-8b11-6464059dcfd0.png)

Click Add button

![image](https://user-images.githubusercontent.com/95063830/173306663-895d9684-c072-4d56-b29d-10f159b5faf6.png)

Click "Select Principal"

![image](https://user-images.githubusercontent.com/95063830/173306757-c95ecfee-ddd7-41ee-af90-ea5d7cfbe709.png)

Click Object Types, and check Computers only.

![image](https://user-images.githubusercontent.com/95063830/173306887-0bcf57d0-e739-4a85-9bbb-8711ed861357.png)

Type in the Cluster Name object if you know the name, else click Advance >> Find Now buttons and search the Computer cluster account. After that, click OK.

![image](https://user-images.githubusercontent.com/95063830/173307196-80afb41e-ebc3-4a02-8fb4-70ce8efeb7d8.png)

In the Permissions list, find and select "Create Computer Objects" then click OK. Go to hjc-sqlprod instance and restart then validate the cluster

![image](https://user-images.githubusercontent.com/95063830/173308982-5f4666c6-4f1d-47fc-ba6f-8a0297145ee3.png)
<br/>

13.Create AG listener. Go to "Always On High Availability" section, expand the group that we have created earlier. Right-click on the "Availability Group Listeners" and select "Add Listener".

![image](https://user-images.githubusercontent.com/95063830/173298171-a6efc5ed-01c7-48fa-bda3-1be44f43e377.png)

Follow the image below. Provide Listener DNS Name, it should be unique. Port is 1433, Network Mode is Static IP. Click the Add button below to add a Unique Static IP Address. Afterward, click OK.

![image](https://user-images.githubusercontent.com/95063830/173298957-187a02b3-2e78-4bc6-8f04-29e28f2c00d2.png)

AG Listener created successfully!

![image](https://user-images.githubusercontent.com/95063830/173309295-9d2a7b54-5832-4abc-b026-3b131a2b7eac.png)

14. Try to ping the network name or ip address of the AG listner. Also try to connect to SQL Server using the network name of of AG Listener.

![image](https://user-images.githubusercontent.com/95063830/173309765-00b545ef-497f-4821-acba-385fee7ef515.png)

![image](https://user-images.githubusercontent.com/95063830/173309926-61162cea-8860-45d0-bf83-c32dd1bfa670.png)
<br/>

15. Show the Availability Dashboard to check the status of the AG we have created. Right click on the AG and select Show Dash board.

![image](https://user-images.githubusercontent.com/95063830/173310261-5f057537-326e-43c0-ae31-bdcb2fdca61b.png)

![image](https://user-images.githubusercontent.com/95063830/173310352-2ff48307-5616-4b8e-a6fa-44071cb893cb.png)
<br/>
<br/>

And that's it we have created our SQL Server AlwaysOn Availability Group! I might add something to this guide in the future.






