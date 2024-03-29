# AlwaysOnExercise
Practice SQL Server AlwaysOn Setup &amp; Configuration. From building virtual machines, Installing  OS & SQL Server, Setup Cluster to the actual Setup of AlwaysOn AG features.
<br/>
<br/>

**A. Introduction**
------------------------------------------------------------------------------------------------------------------------------------
Step by Step procedure to create SQL Server AlwaysOn Availability Group. Starting from building virtual machines virtual machines, Installing  OS & SQL Server, Setup Cluster to the actual Setup of AlwaysOn AG features.
<br/>
<br/>

**B. Prerequisites**
------------------------------------------------------------------------------------------------------------------------------------
**B1.** Hyper-V features installed or VMware Player or Workstation. I'll be using Hyper-V. To install Hyper-V features, go to [Install Hyper-V on Windows 10.](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)
<br/>

**B2.** SQL Server Installer. I'll be using SQL Server Developer Edition here. To download, go to ["SQL Server on-premises".](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
<br/>

**B3.** SQL Server Management Studio (SSMS). To download, go to, [Download SQL Server Management Studio (SSMS).](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)
<br/>
<br/>

**C. Stages**
------------------------------------------------------------------------------------------------------------------------------------
[**I. Create virtual machines using Hyper-V**](https://github.com/fortehub/AlwaysOnPractice/blob/c7a4065279e3cb53f55ff35e9316e5ee341e1d5a/I.%20Create%20virtual%20machines%20using%20Hyper-V.md)
<br/>

[**II. Install OS and OS features such as Active Directory Domain Services**](https://github.com/fortehub/AlwaysOnPractice/blob/5861528fd1fb75867866bb79b4f6ef80cce91921/II.%20Install%20OS%20and%20OS%20features%20such%20as%20Active%20Directory%20Domain%20Services,%20and%20so%20on.md)
<br/>

[**III. Join MS SQL Database Server to Domain & Create a Windows Failover Cluster**](https://github.com/fortehub/AlwaysOnPractice/blob/317c69b5cb15e205538b469f847784d8688564db/III.%20Join%20MS%20SQL%20Database%20Server%20to%20Domain%20&%20Create%20a%20Windows%20Failover%20Cluster.md)
<br/>

[**IV. Install & Configure SQL Server & SSMS on both SQL Server Nodes**](https://github.com/fortehub/AlwaysOnPractice/blob/e77e7461f693bedf89bc4c02019e6ef2189619e6/IV.%20Install%20&%20Configure%20SQL%20Server%20&%20SSMS%20on%20virtual%20Machines.md)
<br/>

[**V. Add a sample database and Setup AlwaysON AG features**](https://github.com/fortehub/AlwaysOnExercise/blob/dfa8dc8c2ae0d3155d51eb1bc90f67eed037e881/V.%20Add%20a%20sample%20database%20and%20Setup%20AlwaysON%20AG%20features.md)
<br/>

[**VI. Create or Restore databases & Add DB to Availability Group**](https://github.com/fortehub/AlwaysOnExercise/blob/ca220104a7c41e3cfaf84bd89a735272c4456acb/VI.%20Create%20or%20Restore%20databases%20&%20Add%20DB%20to%20Availability%20Group.md)
<br/>
<br/>

**D. Note**
------------------------------------------------------------------------------------------------------------------------------------
Whenever you go the stages, always switch to  "**main**" branch.
<br/>
![image](https://user-images.githubusercontent.com/95063830/187341133-1fec7d69-6afd-43cf-9019-6c717f8a728c.png)
<br/>
<br/>

**First Stage**
------------------------------------------------------------------------------------------------------------------------------------
[**I. Create virtual machines using Hyper-V**](https://github.com/fortehub/AlwaysOnPractice/blob/c7a4065279e3cb53f55ff35e9316e5ee341e1d5a/I.%20Create%20virtual%20machines%20using%20Hyper-V.md)


