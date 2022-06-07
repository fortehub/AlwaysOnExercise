# AlwaysOnPractice

**IV. Install & Configure SQL Server & SSMS on virtual Machines**
<br/>

**Steps**
------------------------------------------------------------------------------------------------------------------------------------
1. Get a copy of MS SQL Server Developer Edition and SSMS (SQL Server Management Studio). Visit these sites to download: <br/>
[SQL Server on-premises](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) <br/>
[Download SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16) <br/>

2. Create folders on both SQL Database Server for  Database Files, Log Files & Backup files. You can use the commands below to create a folder by using CMD or PowerShell.

```PowerShell
mkdir d:\mssql\datalogs,d:\mssql\backups
```
![image](https://user-images.githubusercontent.com/95063830/172392986-4dc3245b-8810-4bb2-b286-21dd26c70fef.png)

Where the folder "d:\mssql\datalogs" will house the database and log files and "d:\mssql\backups" for database backups. Please be note, this is only a testing environment, that this folders setup is not a recommended setup. Database & Log Files, and backups should be put on a separate disks to avoid disk contention and utilization problems.

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
  
  *Set the SQL Server Agent and SQL Server Database Engine Startup Type to "Automatic". Then click Next*  <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172398434-b5b6b7c4-208a-4a28-a4f3-9c586434de80.png)
 <br/>
 <br/>
  
  *At the Server Configuration Section, select Mixed Mode, type in the (sa) password, and click the "Add the Current User" button.  <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172398836-97bbdaa0-d22b-4203-aefb-16b1960a4f2d.png)
 <br/>
 <br/>
  
  *At the Data Directories section, change the path of User Database & Log directories as well as the backup. We're gonna use the folders we've created a while ago.* <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172399284-060bbb33-51b2-4e99-af98-8b85fb08c404.png)
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
  
  *Check the Recommended and the Click here to accept button*  <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172400075-1c1b16fd-1227-4e2c-816b-75e29ea72237.png)
 <br/>
 <br/>
  
  *Click Install and wait*  <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172400182-971c35ad-c12a-427b-8fca-71136b7dd8dc.png)
 <br/>
 <br/>
  
  *Installation Done. Repeat the steps above to the 2nd MS SQL Database Server*  <br/>
  ![image](https://user-images.githubusercontent.com/95063830/172401531-f8e3fa56-20e2-44f9-9700-572f857c8672.png)

  
  
 
  
  
  

  
  
  



