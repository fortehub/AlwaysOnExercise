# AlwaysOnExercise

**VI. Create or Restore databases & Add to Availability Group**
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

**D3.** Create Full Database backup. Esxecute the following T-SQL to perform a full DB backup.
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

**D5.**


