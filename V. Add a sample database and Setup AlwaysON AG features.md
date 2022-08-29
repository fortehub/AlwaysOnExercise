# AlwaysOnExercise

**V. Add a sample database & Setup AlwaysON AG features**
<br/>

**Resources**
------------------------------------------------------------------------------------------------------------------------------------
1. Download the AdventureWorks Sample databases here, [AdventureWorks sample databases.](https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms)
 
We are going the use the 2016 & 2019 version of AdventureWorks database.  <br/>

**Steps**
------------------------------------------------------------------------------------------------------------------------------------
**Database Restoration**

1. Restore the 2 databases on hjc-sqlprod server only. We can use the GUI to restore a database [Restore a Database](https://www.quackit.com/sql_server/sql_server_2016/tutorial/restore_a_database_in_sql_server_2016.cfm), or follow the T-SQL script below.

First, get the database logical names from the backup:

```T-SQL
Use master
Go

RESTORE FILELISTONLY from disk = N'C:\Users\administrator.HJCDOM\Downloads\AdventureWorks2016.bak';
RESTORE FILELISTONLY from disk = N'C:\Users\administrator.HJCDOM\Downloads\AdventureWorks2019.bak';
Go
```

![image](https://user-images.githubusercontent.com/95063830/187068848-53eb8dfc-0dc2-45d4-b416-26e08a160470.png)

Second, Perform the following T-SQL Script to restore the databases.

```T-SQL
Use master
Go

Restore Database [AdventureWorks2016] from disk = 'C:\Users\administrator.HJCDOM\Downloads\AdventureWorks2016.bak' With File = 1,
Move N'AdventureWorks2016_Data' To N'D:\mssql\datalogs\AdventureWorks2016_Data.mdf',
Move N'AdventureWorks2016_Log' To N'D:\mssql\datalogs\AdventureWorks2016_log.mdf'
Go

Restore Database [AdventureWorks2019] from disk = 'C:\Users\administrator.HJCDOM\Downloads\AdventureWorks2019.bak' With File = 1,
Move N'AdventureWorks2017' To N'D:\mssql\datalogs\AdventureWorks2019_Data.mdf',
Move N'AdventureWorks2017_log' To N'D:\mssql\datalogs\AdventureWorks2019_log.mdf'
Go
```
Database Restoration Complete!

![image](https://user-images.githubusercontent.com/95063830/187068889-133a4528-7ce9-4eb2-a016-381c4415faa3.png)

Third, change the "Recovery Model" from Simple to **Full** of the 2 database that we have restored. You can choose via T-SQL or GUI for this. For GUI [How to Change MS SQL Database Recovery Model?](https://manage.accuwebhosting.com/knowledgebase/2425/How-to-Change-MS-SQL-Database-Recovery-Model.html)

For T-SQL, follow the script below.
```T-SQL
USE master
GO

ALTER DATABASE [AdventureWorks2016] SET RECOVERY FULL;
ALTER DATABASE [AdventureWorks2019] SET RECOVERY FULL;
GO
```

Fourth, create a Full Database backup (This is needed for Always On AG Setup). Follow the T-SQL script below to perform a Full Database backup. Backup using GUI, follow this [SQL Server Full Backup.](https://www.sqlservertutorial.net/sql-server-administration/sql-server-full-backup/)

```T-SQL
USE MASTER;
GO

BACKUP DATABASE [AdventureWorks2016]
TO DISK = N'E:\mssql\backups\AdventureWorks2016.bak' WITH NOFORMAT,
NAME = N'AdventureWorks2016-08282022-FullDB';
GO

BACKUP DATABASE [AdventureWorks2019]
TO DISK = N'E:\mssql\backups\AdventureWorks2019.bak' WITH NOFORMAT,
NAME = N'AdventureWorks2019-08282022-FullDB';
GO
```
![image](https://user-images.githubusercontent.com/95063830/187069344-55624048-6777-4e0f-8c9e-a660eaf1b26b.png)


<br/>
<br/>

**Setup AlwaysON Availability Group**

1. At the SQL Server Configuration\SQL Server Services, right-click SQL Server(MSSQLSERVER), go to "AlwaysOn Availability Groups" section and check the Enable button. Click apply and OK. Repeat this on the 2nd Database Server (hjc-sqldr01). Restart the SQL Server services to take effect.

![image](https://user-images.githubusercontent.com/95063830/172414076-b40dcf22-a0c4-4e59-87c9-6b53c68cd70f.png)
<br/>

2. Open SSMS, make sure all the databases (system & user databases, except TempDB) are in "Full Recovery" model. Full recovery model is required to setup Availability Group. Right on each databases, click Properties\Options, and change the "Recovery Model" to "**Full**".

![image](https://user-images.githubusercontent.com/95063830/187068039-e872f017-6181-4327-9015-601c0fb96c7f.png)

![image](https://user-images.githubusercontent.com/95063830/173293196-5514ce4e-6364-495b-8733-3bd556d3772d.png)

3. Back to SSMS, right-click on the Always-On High Availability section and select "New Availability Group Wizard".

![image](https://user-images.githubusercontent.com/95063830/173292528-ad5f23e9-0de6-4f36-9013-40cf19f06da9.png)
<br/>

4. Provide an AG name, then click Next.
5. 
![image](https://user-images.githubusercontent.com/95063830/187069684-898e2dcc-9251-4d64-a082-b698239b12f2.png)

Note: Make sure all the database is in "Full Recovery" model. Full recovery model is required to setup Availability Group.


<br/>

6. Create a database backup first for each databases, to proceed on this step. In the image below, AdventureWorks2016 does not a have a database backup, so we are going to take a backup of this particular database.

![image](https://user-images.githubusercontent.com/95063830/173293775-0771d623-cba2-4d35-bffc-30735d36c0ab.png)

In the image above, AdventureWorks2016 does not a have a database backup, so we are going to take a backup of this particular database. You can [perform database backup thru the GUI](https://www.mssqltips.com/sqlservertutorial/7/sql-server-full-backups/) , but we can also perform using T-SQL. To perform backup, follow the T-SQL script below:



![image](https://user-images.githubusercontent.com/95063830/173294496-5a6a4562-b156-46d1-920d-5d3041292413.png) <br/>
Backup also complete!
<br/>

Now click the "Refresh" button to update the Status. It should be ok now. Check the database you want to be part of (AG)Availability Group, then click Next.

![image](https://user-images.githubusercontent.com/95063830/173294869-29d67baf-6ea3-44f1-8697-1b8397c411b9.png)
<br/>

7. Follow the images below for the Replicas steps.

Check the Automatic Failover (Up to 5) but this is optional, Availability Mode is Synchronous commit for Primary Replica while Asynchronous for Secondary, Readable Secondary is Yes. Click the Add Replica button and add the 2nd SQL Server Instance (hjc-sqldr01).

![image](https://user-images.githubusercontent.com/95063830/173295620-92fce8c6-762d-4f4d-ab65-eb37e7cf4f06.png)

Click the Add Replica button and add the 2nd SQL Server Instance (hjc-sqldr01). A "Connect to Server' mini windows will appear. type in the network names or IP of the 2nd SQL Server instance, and connect to it.

![image](https://user-images.githubusercontent.com/95063830/173295832-bd6a5c1f-f88e-4654-9cb4-c21939a5f94b.png)

![image](https://user-images.githubusercontent.com/95063830/187128860-edaded1e-cd61-4cd8-8731-e0c867dffb4e.png)

Leave the default configuration for "Backup Preferences" which is "Prefer Secondary". We can skip the "Listener" creation and do it later. Set aside Read-Only Routing, we don't need in this setup. Click Next to proceed.
<br/>

8. At the "Select Data Synchronization", you can choose base on your preferences. Here I will choose the "Automatic seeding" or "Full Database and log backup". Just make sure both the database, log and backups are in the same path/location/folders for all SQL Server Instance to make this work. If you want to use the 2nd option, make the file share is accessible to/by all replicas. Click Next.

![image](https://user-images.githubusercontent.com/95063830/187070101-8d7ed679-360a-4e7c-be84-6eec50e5a0f5.png)

![image](https://user-images.githubusercontent.com/95063830/173297393-8fb39a17-a9a1-4cbc-9816-ce2214bb1f93.png)
<br/>

9. Click to Proceed. 

![image](https://user-images.githubusercontent.com/95063830/187070307-d73f1176-cbc3-4920-9145-d8a15d2fd76f.png)

10. Click Finish

Using Automatic Seeding
![image](https://user-images.githubusercontent.com/95063830/187128961-7e9bf7a3-a21f-4ad7-8a2c-dd6070574666.png)

Using Full Database and log backup
![image](https://user-images.githubusercontent.com/95063830/187070330-41e40feb-2520-4702-b297-8b86ad3e305d.png)

11. Done!

![image](https://user-images.githubusercontent.com/95063830/187070399-db1ce453-1e6a-4d4a-8238-9ef06fb13ac4.png)

![image](https://user-images.githubusercontent.com/95063830/187129541-3b0e5da4-3c7a-4b3f-98c6-68ce8fc2757b.png)

![image](https://user-images.githubusercontent.com/95063830/173297853-62384730-3a20-47d8-aec3-33ba9923a9aa.png)
<br/>

12.Create AG listener. Go to "Always On High Availability" section, expand the group that we have created earlier. Right-click on the "Availability Group Listeners" and select "Add Listener".

![image](https://user-images.githubusercontent.com/95063830/173298171-a6efc5ed-01c7-48fa-bda3-1be44f43e377.png)

Follow the image below. Provide Listener DNS Name, it should be unique. Port is 1433, Network Mode is Static IP. Click the Add button below to add a Unique Static IP Address. Afterward, click OK.

![image](https://user-images.githubusercontent.com/95063830/173298957-187a02b3-2e78-4bc6-8f04-29e28f2c00d2.png)

Or you can use the T-SQL below to create the AlwaysON AG Listener

```T-SQL
USE [master]
GO
ALTER AVAILABILITY GROUP [hjc-alwaysgrp]
ADD LISTENER N'hjc-aglsnr' (
WITH IP
((N'192.168.10.150', N'255.255.255.0')
)
, PORT=1433);
GO
```
![image](https://user-images.githubusercontent.com/95063830/187129788-8a51dd59-bd98-4837-adab-4ac5bba48ec1.png)

AG Listener created successfully!

![image](https://user-images.githubusercontent.com/95063830/187126753-90c0695a-10ef-449a-8ad1-5836300258d6.png)

13. Try to ping the network name or ip address of the AG listner. Also try to connect to SQL Server using the network name of of AG Listener.

![image](https://user-images.githubusercontent.com/95063830/173309765-00b545ef-497f-4821-acba-385fee7ef515.png)

![image](https://user-images.githubusercontent.com/95063830/173309926-61162cea-8860-45d0-bf83-c32dd1bfa670.png)
<br/>

15. Show the Availability Dashboard to check the status of the AG we have created. Right click on the AG and select Show Dash board.

![image](https://user-images.githubusercontent.com/95063830/187127053-aab29c4a-9be1-48e7-9a21-9e6ae8e75833.png)

![image](https://user-images.githubusercontent.com/95063830/187127025-2b85a0dc-495c-4f65-b5eb-ff2347fdb8e4.png)
<br/>
<br/>

And that's it we have created our SQL Server AlwaysOn Availability Group! I might add something to this guide in the future.






