# AlwaysOnExercise

**IV. Install & Configure SQL Server & SSMS on the two Nodes
<br/>

**Steps**
------------------------------------------------------------------------------------------------------------------------------------
1. Get a copy of MS SQL Server Developer Edition and SSMS (SQL Server Management Studio). Visit these sites to download: <br/>
[SQL Server on-premises](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) <br/>
[Download SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16) <br/>

2. Create folders on both SQL Database Server for  Database Files, Log Files & Backup files. You can use the commands below to create a folder by using CMD or PowerShell.

```PowerShell
mkdir D:\mssql\datalogs,E:\mssql\backups
```
![image](https://user-images.githubusercontent.com/95063830/187057514-d7dbe9d6-5ded-4d35-80cb-606cb3e11b75.png)

Where the folder "**D:\mssql\datalogs**" will house the database and log files and "**E:\mssql\backups**" for database backups. Please be note, this is only a testing environment, that this folders setup is not a recommended setup. **Database, Transact Log Files, Temp files, and Backups** should be put on a separate disks to avoid disk contention and utilization problems.

3. Follow the images below for the installation of SQL Server Developer Edition.
 <br/>
 
*Run the setup.exe* <br/>
![image](https://user-images.githubusercontent.com/95063830/172394933-9071b8ed-8ffe-40da-9cc0-5a026dada8bf.png)
 <br/>
 <br/>
  
*Go to **Installation** section and select "New SQL Server stand-alone installation or add features to an existing installation"* <br/>
![image](https://user-images.githubusercontent.com/95063830/172395964-48a96a86-3783-4f67-a9f8-ea736be40439.png)
 <br/>
 <br/>
  
 *Select "Developer" edition and click Next*  <br/>
![image](https://user-images.githubusercontent.com/95063830/172396083-4cdbc1e0-28ef-4253-a47d-2524e8c76ac3.png)
 <br/>
 <br/>
  
  *Accept the License and click Next*  <br/>
![image](https://user-images.githubusercontent.com/95063830/172396194-9506e14e-e607-40f3-996c-4193917aaa5b.png)
 <br/>
 <br/>
  
  *Check the following:*  <br/>
  Database Engine Services  <br/>
  SQL Server Replication  <br/>
  Full-Text and Semantic Extractions for Search  <br/>
  Client Tools Connectivity  <br/>
  Client Tools Backward Compatibility  <br/>
  SQL Client Connectivity SDK  <br/>
  
  Leave the default locations of Root and Shared directories.  <br/>
  Then click Next.  <br/>
 ![image](https://user-images.githubusercontent.com/95063830/172397569-6ca8f9e6-6942-4786-8ee4-3e09831f7436.png)
 <br/>
 <br/>
  
  *We will gonna used the Default Instance Name/ID. Click Next*  <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172397974-5a22481e-cb90-498d-806f-9fe5e344d1d2.png)
 <br/>
 <br/>
  
  *Set the SQL Server Agent and SQL Server Database Engine Startup Type to "Automatic". Then click Next*. [Optional] if needed, you can check the[ Grant Perform Volume Maintenance Task](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-instant-file-initialization?redirectedfrom=MSDN&view=sql-server-ver16)  <br/>
  
  ![image](https://user-images.githubusercontent.com/95063830/172398434-b5b6b7c4-208a-4a28-a4f3-9c586434de80.png)
 <br/>
 <br/>
  
  *At the Server Configuration Section, select Mixed Mode, type in the (sa) password, and click the "Add the Current User" button.  <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172398836-97bbdaa0-d22b-4203-aefb-16b1960a4f2d.png)
 <br/>
 <br/>
  
  *At the Data Directories section, change the path of User Database & Log directories as well as the backup. We're gonna use the folders we've created earlier.* <br/>
 ![image](https://user-images.githubusercontent.com/95063830/187057669-49789a76-7f50-4cc0-8d72-7d4aad88a23b.png)
 <br/>
 <br/>
  
  *Leave everything to default configuration for the TempDB section*  <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172399670-a45db223-082f-4f30-b464-06fd50f90f36.png)
 <br/>
 <br/>
  
  *Also for MAXDOP*  <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172399892-27655df1-b854-4de3-bdd4-1a520e523caf.png)
 <br/>
 <br/>
  
  *Check the Recommended and the Click here to accept button*. My preferences for Max Memory configuration, is the 85% of total system Memory (4096 MB) <br/>
![image](https://user-images.githubusercontent.com/95063830/187057989-6a79e134-43e9-4ee2-aeff-4307cd8abedb.png)
 <br/>
 <br/>
  
  *Click Install and wait*  <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172400182-971c35ad-c12a-427b-8fca-71136b7dd8dc.png)
 <br/>
 <br/>
  
  *Installation Done. Repeat the steps above to the 2nd MS SQL Database Server*  <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172401531-f8e3fa56-20e2-44f9-9700-572f857c8672.png)
 <br/>
 <br/>
  
3. Open, SQL Server Configuration Manager

First, we must replace the logon account for SQL Server Services. We will going to use in this testing environment the Domain Administrator account. Note, this is not a recommended setup for production environment, create another Domain service account for SQL Server services. Open SQL Server Configuration Manager. 

Go to SQL Services section, and replace the default account with the Domain Administrator for **SQL Server Server & SQL Server Agent** services, then click Apply. Restart the SQL Server services after enabling, in order for the changes to take effect.

![image](https://user-images.githubusercontent.com/95063830/187067120-1af291db-50ad-4f43-b166-bb9da2376f43.png)

Repeat the steps on the 2nd database server (hjc-sqldr01).

Second, enable TCP/IP connections for SQL Server. Go to SQL Server Network Configuration\Protocols for MSSQLSERVER, Select and right-click the TCP/IP protocol name and select "Enable". Restart the SQL Server services after enabling, in order for the changes to take effect.

![image](https://user-images.githubusercontent.com/95063830/187067154-b1c387ca-7783-4e0d-8c06-7435e5556115.png)

Repeat the steps on the 2nd database server (hjc-sqldr01).

4. For SSMS Installation, follow this guide, [How To Install Microsoft SQL Server Management Studio (SSMS)?.](https://www.c-sharpcorner.com/article/how-to-install-microsoft-sql-server-management-studio-ssms/)

  
  **Next Stage**
------------------------------------------------------------------------------------------------------------------------------------

[V. Add a sample database and Setup AlwaysON AG features](https://github.com/fortehub/AlwaysOnPractice/blob/53ca40141376f0776d22c860de07eb40fe24b310/V.%20Add%20a%20sample%20database%20and%20Setup%20AlwaysON%20AG%20features.md)

 
  
  
  

  
  
  



