# AlwaysOnExercise

**VI. Create or Restore databases, Add DB to Availability Group, & Failover Availability Group**
<br/>
<br/>
<br/>

**A. Resources**
------------------------------------------------------------------------------------------------------------------------------------
**A1.** Download the database resources here: <br/>
	[BikeStores Database](https://www.sqlservertutorial.net/sql-server-sample-database/) <br/>
	[Wide World Importers Database](https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0) 
<br/>
<br/>
<br/>

 **B. Create BikeStores Database**
 ------------------------------------------------------------------------------------------------------------------------------------
**B1.** Go to SSMS, open New Query editor, and run the following T-SQL to create the database.
```T-SQL
USE master
GO

CREATE DATABASE [BikeStores];
GO
```
![image](https://user-images.githubusercontent.com/95063830/187194529-e50fff2e-8c10-4734-8336-755e40494854.png) 
<br/>
<br/>

**B2.** Or use the GUI, to create the database. Follow the images below:
<br/>
![image](https://user-images.githubusercontent.com/95063830/187194884-84b935f8-7308-4882-a4bc-030fbb879566.png)

![image](https://user-images.githubusercontent.com/95063830/187194290-d43c8029-1cb4-4d2e-8297-23c412a6034f.png)

If everything is fine, you will see the database BikeStores appears under Databases node as shown in the screenshot below: 
<br/>
![image](https://user-images.githubusercontent.com/95063830/187195132-50aa6667-01f1-4690-bc06-c5a3a1a1cf45.png)
<br/>
<br/>

**B3.** From the File menu, choose Open > File… menu item to open a script file.
<br/>
![image](https://user-images.githubusercontent.com/95063830/187195279-b5c96c6a-3f53-4577-ba72-25aca7b9dbcf.png)
<br/>
<br/>

**B4.** Select the BikeStores Sample Database – create objects.sql file and click the Open button
<br/>
![image](https://user-images.githubusercontent.com/95063830/187195654-99223e61-00b6-442e-b71e-c8e6cee6a30d.png)
<br/>
<br/>

**B5.** Click the Execute button to execute the SQL script. Make sure you have selected the right database (square-highlighted in the image below).
<br/>
![image](https://user-images.githubusercontent.com/95063830/187196966-6d9328c2-8c8c-456b-85f6-1b4265b04e08.png)

You should see the following result indicated that the query executed successfully.
<br/>
![image](https://user-images.githubusercontent.com/95063830/187197285-8915e077-cc70-4a08-88b2-c298812de6fa.png)

If you expand the BikeStores > Tables, you will see the schemas and their tables are created as shown below:

![image](https://user-images.githubusercontent.com/95063830/187198172-a15ab824-8c6b-40a6-97a4-f47f57523605.png)
<br/>
<br/>

**B6.** Open the file for loading data into the tables.<br/>
![image](https://user-images.githubusercontent.com/95063830/187198328-0c435f09-7521-48e2-9c3c-fe09d67488a6.png)
<br/>
<br/>

**B7.** Choose the BikeStores Sample Database – load data.sql file and click the Open button.
<br/>
![image](https://user-images.githubusercontent.com/95063830/187198547-571275d8-746c-4b57-ab73-d219af6c2d37.png)
<br/>
<br/>

**B8.** Click the Execute button to load data into the tables. You should see the following message indicating that all the statements in the script were executed successfully.
<br/>
![image](https://user-images.githubusercontent.com/95063830/187198934-ddc20fd2-f085-41c7-975d-debf377e929c.png)
<br/>
<br/>
<br/>

**C. Restore Wide World Importers Database**
------------------------------------------------------------------------------------------------------------------------------------
**C1.** Use T-SQL to restore the database from .bak file. <br/>
Use RESTORE FILELISTONLY with the .bak file to identify the logical names of the database &transact log files. Upon checking, there 4 logical name. Use this names for the restoration operation. <br/>
```T-SQL
USE master

RESTORE FILELISTONLY 
FROM DISK =N'C:\Users\administrator.HJCDOM\Downloads\WideWorldImporters-Full.bak';
GO
```
![image](https://user-images.githubusercontent.com/95063830/187206231-50be4967-7717-4533-af94-9c40e7528bf8.png)
<br/>
<br/>

**C2.** Run the following T-SQL for the database restoration process.
```T-SQL
RESTORE DATABASE [WideWorldImporters]
FROM  DISK = N'C:\Users\administrator.HJCDOM\Downloads\WideWorldImporters-Full.bak' WITH  FILE = 1,  
MOVE N'WWI_Primary' TO N'D:\mssql\datalogs\WideWorldImporters.mdf',  
MOVE N'WWI_UserData' TO N'D:\mssql\datalogs\WideWorldImporters_UserData.ndf',  
MOVE N'WWI_Log' TO N'D:\mssql\datalogs\WideWorldImporters.ldf',  
MOVE N'WWI_InMemory_Data_1' TO N'D:\mssql\datalogs\WideWorldImporters_InMemory_Data_1'
GO
```
![image](https://user-images.githubusercontent.com/95063830/187207015-0e324100-2e39-4662-8975-411d3066a6ef.png)
<br/>
<br/>

**C3.** OR Restore via GUI. Right-click in the "Databases" section and select "Restore Database". <br/>
![image](https://user-images.githubusercontent.com/95063830/187202451-9a546ca5-2453-4c65-aff3-41ecbe587121.png)
<br/>
<br/>

**C4.** At the "Restore Database" window, **[1]** Click Device; **[2]** Click 3-dot button; **[3]** Click Add button; **[4]** Navigate to the .bak file of Wide World Importers Database; **[5]** Click the OK button twice. <br/>
![image](https://user-images.githubusercontent.com/95063830/187203206-fb3f847f-9fb4-40b9-a7a6-14e16aeb3e38.png)
<br/>
<br/>

**C5.** Click to proceed with the database restoration! <br/>
![image](https://user-images.githubusercontent.com/95063830/187204691-94489fe1-20ef-43f0-a5eb-b6e0b5c9a0eb.png)

Wait for the restoration. <br/>
![image](https://user-images.githubusercontent.com/95063830/187204797-529c1b1b-99e2-430d-8cf9-2d70d930121f.png)

Restore successfully! <br/>
![image](https://user-images.githubusercontent.com/95063830/187204950-04353ecf-308b-49df-9136-ebfdd1667441.png)

Database Added!  <br/>
![image](https://user-images.githubusercontent.com/95063830/187207830-d8b20b1b-753a-46bb-91ec-d2af5c7701ff.png)
<br/>
<br/>

**C6.** Query anything on the two database. Use SQL SELECT statements below:
```SQL
USE BikeStores

SELECT * FROM production.categories;
GO

USE WideWorldImporters

SELECT * from Application.People;
GO
```
![image](https://user-images.githubusercontent.com/95063830/187208469-6b01c97b-0265-44d9-aa38-f388f20e5359.png)
<br/>
<br/>
<br/>

 **D. Add the database to Availability Group**
 ------------------------------------------------------------------------------------------------------------------------------------
**D1.** On the new 2 database, set the "Recovery Model" from Simple to "**FULL**". Run the following T-SQL:
```
USE master

ALTER DATABASE [BikeStores] SET RECOVERY FULL;
ALTER DATABASE [WideWorldImporters] SET RECOVERY FULL;
GO
```
![image](https://user-images.githubusercontent.com/95063830/187209743-12a8a91d-2d8b-416f-9dae-752b6dedbc89.png)
<br/>
<br/>

**D2.** Double-check by running the following T-SQL:
```T-SQL
SELECT 
name AS [Database Name],
recovery_model_desc AS [Recovery Model] 
FROM sys.databases
GO
```
![image](https://user-images.githubusercontent.com/95063830/187210501-ed258d27-8479-4134-a6c6-fc1b704bafc7.png)

Or use the GUI. Right-click on each databases, click Properties\Options, and change the "Recovery Model" to "Full". <br/>
![image](https://user-images.githubusercontent.com/95063830/187210891-7e5eb1a8-814f-472a-a5a2-adf70fe98b1f.png)
![image](https://user-images.githubusercontent.com/95063830/187210967-b9403558-f7ed-4341-89c1-154bd08ce8b7.png)
<br/>
<br/>

**D3.** Create Full Database backup. Execute the following T-SQL to perform a full DB backup.
```
USE master

BACKUP DATABASE [WideWorldImporters]
TO  DISK = N'E:\mssql\backups\WideWorldImporters-FullDB.bak' WITH NOFORMAT, NOINIT, 
NAME = N'WideWorldImporters-FULLDB-08292022';
GO

BACKUP DATABASE [BikeStores]
TO  DISK = N'E:\mssql\backups\BikeStores-FullDB.bak' WITH NOFORMAT, NOINIT, 
NAME = N'BikeStores-FULLDB-08292022';
GO
```
![image](https://user-images.githubusercontent.com/95063830/187212298-d21d5056-6962-4ef3-a450-faa87e1048a8.png)
<br/>
<br/>

**D4.** For backing up using the GUI, follow this, [How to create a simple database backup using SSMS?](https://www.mssqltips.com/sqlservertip/2968/how-to-create-a-simple-database-backup-using-sql-server-management-studio-ssms/)
<br/>
<br/>

**D5.** Create Transact Log Backup. Esxecute the following T-SQL to perform a Transact Log Backup.
```T-SQL
USE master

BACKUP LOG [BikeStores] 
TO DISK = N'E:\mssql\backups\BikeStores-TransactLogBackup.trn' WITH INIT,
NAME = N'BikeStores-TransactLog Backup';
GO

BACKUP LOG [WideWorldImporters]
TO DISK = N'E:\mssql\backups\WideWorldImporters-TransactLogBackup.trn' WITH INIT,
NAME = N'WideWorldImporters-TransactLog Backup';
GO
```
![image](https://user-images.githubusercontent.com/95063830/187324102-58b44c63-e020-46a2-b9fc-f5a07a835fab.png)
<br/>
<br/>

**D6.** For backing up using the GUI, follow this, [SQL Server Transaction Log Backups.](https://www.mssqltips.com/sqlservertutorial/8/sql-server-transaction-log-backups/)
<br/>

**D7.** Copy the above backup files to the secondary replica to restore them on that server to prepare the secondary database. <br/>
 <br/>
Use the following PowerShell cmdlet to copy the needed files. Navigate first to the backup location and perform **cp** (Copy-Item) cmdlet.
```PowerShell
cd cd E:\mssql\backups
ls
E:\mssql\backups> cp BikeStores-FullDB.bak,BikeStores-TransactLogBackup.trn,WideWorldImporters-FullDB.bak,WideWorldImporters-TransactLogBackup.trn `
-Destination \\HJC-SQLDR01\e$\mssql\backups
```
![image](https://user-images.githubusercontent.com/95063830/187325921-3fc66ac0-781b-4449-8680-4ac02324d453.png)

Traditional copy-pasting to secondary replica (hjc-sqldr01). <br/>
![image](https://user-images.githubusercontent.com/95063830/187324603-2937bc6c-8f9f-4e4f-b7fc-bd3c91945917.png)
 
Run the below commands to restore the "**Secondary Instance (HJC-SQLDR01)**" with the help of the above copied backup files.<br/>

First, use RESTORE FILELISTONLY:
```T-SQL
USE master

RESTORE FILELISTONLY FROM  DISK = N'E:\mssql\backups\BikeStores-FullDB.bak';
RESTORE FILELISTONLY FROM  DISK = N'E:\mssql\backups\WideWorldImporters-FullDB.bak';
GO
```
![image](https://user-images.githubusercontent.com/95063830/187327179-aed26985-8f9c-415e-bb0b-013d4f91f83c.png)

```T-SQL
--RESTORE BikeStores DB Backup
RESTORE DATABASE [BikeStores]
FROM DISK = N'E:\mssql\backups\BikeStores-FullDB.bak'
WITH NORECOVERY,
MOVE 'BikeStores' TO 'D:\mssql\datalogs\BikeStores.mdf',
MOVE 'BikeStores_log' TO 'D:\mssql\datalogs\BikeStores_log.ldf'
GO

--RESTORE BikeStores TransactLog Backup
RESTORE DATABASE [BikeStores]
FROM DISK = N'E:\mssql\backups\BikeStores-FullDB.bak'
WITH NORECOVERY,
MOVE 'BikeStores' TO 'D:\mssql\datalogs\BikeStores.mdf',
MOVE 'BikeStores_log' TO 'D:\mssql\datalogs\BikeStores_log.ldf'
GO

--RESTORE WideWorldImporters DB Backup
RESTORE DATABASE [WideWorldImporters]
FROM DISK = 'E:\mssql\backups\WideWorldImporters-FullDB.bak'
WITH NORECOVERY,
MOVE 'WWI_Primary' TO 'D:\mssql\datalogs\WideWorldImporters.mdf',
MOVE 'WWI_UserData' TO 'D:\mssql\datalogs\WideWorldImporters_UserData.ndf',
MOVE 'WWI_Log' TO 'D:\mssql\datalogs\WideWorldImporters.ldf',
MOVE 'WWI_InMemory_Data_1' TO 'D:\mssql\datalogs\WideWorldImporters_InMemory_Data_1'
GO

--RESTORE BikeStores TransactLog Backup
RESTORE DATABASE [BikeStores]
FROM DISK = N'E:\mssql\backups\BikeStores-TransactLogBackup.trn'
WITH NORECOVERY;
GO
 
--RESTORE WideWorldImporters TransactLog Backup
RESTORE DATABASE [WideWorldImporters]
FROM DISK = N'E:\mssql\backups\WideWorldImporters-TransactLogBackup.trn'
WITH NORECOVERY;
GO
```
![image](https://user-images.githubusercontent.com/95063830/187329282-17227c33-a208-42cb-93fd-96c85fb5aaf9.png)
<br/>
<br/>

**D8.** Once you prepared the secondary database, our next step is to add this database to the AOAG (AlwaysOn Availability Group) configuration. Connect to the primary replica and run the below T-SQL commands to add this newly created database to the Always On Availability Group.
```T-SQL
-- Connect to the server instance that hosts the primary replica (HJC-SQLPROD) to add a database to the availability group.  
ALTER AVAILABILITY GROUP [hjc-alwaysgrp] ADD DATABASE [BikeStores];  
GO

ALTER AVAILABILITY GROUP [hjc-alwaysgrp] ADD DATABASE [WideWorldImporters];  
GO
```
![image](https://user-images.githubusercontent.com/95063830/187329916-55847756-a61e-4c6f-bc0f-38d15249e906.png)
<br/>
<br/>

**D9.** Next, we will check and validate this change. We can run the dashboard report or we can also check in SQL Server Management Studio by expanding the respective folders. I checked both ways and you can see the 2 database has been added to this AOAG configuration as shown in the below image.  The AOAG configuration is healthy after adding this database. <br/>
![image](https://user-images.githubusercontent.com/95063830/187330223-753d122e-507d-467b-9bc2-832a2a928396.png)

For more details, go to [Adding a Database to an existing SQL Server Always ON Configuration](https://www.mssqltips.com/sqlservertip/5437/adding-a-database-to-an-existing-sql-server-always-on-configuration/)
<br/>
<br/>

**E. Failover**
------------------------------------------------------------------------------------------------------------------------------------
**E1.** Follow this guide for Failover descriptions, & descriptions:
<br/>
**a.** [Explore failover types in SQL Server Always On Availability Groups](https://www.sqlshack.com/explore-failover-types-in-sql-server-always-on-availability-groups/#:~:text=Failover%20options%20in%20the%20SQL,database%20online%20to%20accept%20connections.)
<br/>
**b.** [Perform a planned manual failover of an Always On availability group (SQL Server](https://github.com/MicrosoftDocs/sql-docs/blob/live/docs/database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)
<br/>	
**c.** [Manual SQL Server Availability Group Failover](https://www.mssqltips.com/sqlservertip/3437/manual-sql-server-availability-group-failover/)

