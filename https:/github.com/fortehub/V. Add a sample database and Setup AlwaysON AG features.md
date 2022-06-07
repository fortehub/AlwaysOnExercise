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
1. First, we must replace the logon account for SQL Server Services. We will going to use in this testing environment the Domain Administrator account. Note, this is not a recommended setup for production environment, create another Domain account for SQL Server services. 

Open SQL Server Configuration Manager. Go to SQL Services section, and replace the default account with the Domain Administrator for SQL Server Server & SQL Server Agent servces, then click Apply.

![image](https://user-images.githubusercontent.com/95063830/172411363-327038ec-a6f8-451a-800b-56cb45bbed3d.png)

Repeat the steps on the 2nd database server (hjc-sqldr01).
<br/>

2. Enable TCP/IP connections for SQL Server. Go to SQL Server Network Configuration\Protocols for MSSQLSERVER, Select and right-click the TCP/IP protocol name and select "Enable". Restart the SQL Server services after enabling to take effect the changes.
![image](https://user-images.githubusercontent.com/95063830/172412797-52f994d7-6b80-46be-a9b1-dcc7039f267e.png)
<br/>

3. At the SQL Server Configuration\SQL Server Services, right-click SQL Server(MSSQLSERVER), go to "AlwaysOn Availability Groups" section and check the Enable button. Click apply and OK. Repeat this on the 2nd Database Server (hjc-sqldr01). Restart the SQL Server services to take effect.

![image](https://user-images.githubusercontent.com/95063830/172414076-b40dcf22-a0c4-4e59-87c9-6b53c68cd70f.png)


