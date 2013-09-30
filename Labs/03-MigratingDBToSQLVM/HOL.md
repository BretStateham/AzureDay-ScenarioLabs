<a name="Title"></a>
# Migrating an On-Premises SQL Server Database to a Windows Azure Virtual Machine with SQL Server #

---
<a name="Overview"></a>
## Overview ##

This hands-on lab walks you through the process of configuring a Windows Azure Virtual Machine with SQL Server 2012 pre-installed.  You will then create a Windows Azure Storage account, and export a .bacpac file from an on-premises SQL Database into Azure Storage.  

For this lab, we will treat the "Development Virtual Machine" you created as part of the **"01-SetupVS2013SQLVM"** lab as our "on-premises" server.  Yes, it really is running in Azure, but for this lab pretend it is just another Virtual Machine on your own local network.  

Next, we'll configure the security (Firewall and SQL Logins) on the Windows Azure Virtual Machine with SQL Server to ensure that we can connect to the database remotely.

Finally, we will deploy the .bacpac exported from our "on-premises" server into the SQL Server instance running the Windows Azure Virtual Machine, and verify that we can connect to it remotely.  

![Exporting to Windows Azure Storage](Images/01-highlevel.png?raw=true)

_Exporting to Windows Azure Storage_

<a name="Objectives"></a>
### Objectives ###

In this hands-on lab, you will learn how to:

- Provision a Windows Azure Virtual Machine with SQL Server 2012 pre-installed
- Create a Windows Azure Storage Account to store the **.bacpac** file
- Use SQL Server Management Studio to Export a Data-tier Application (**.bacpac**) 
- Configure security on the new Virtual Machine
- Deploy a .bacpac file into a Windows Azure Virtual Machine with SQL Server.  

<a name="Prerequisites"></a>
### Prerequisites ###

The following is required to complete this hands-on lab:

<!-- TODO: UPDATE THE Pre-Reqs to match the actual setup lab name  -->
<!-- TODO: FIX DOWNLOAD LINK -->
- A Windows Azure subscription -  [sign up for a free trial](http://aka.ms/WATK-FreeTrial) 
- **Either** Completing the **"01-SetupVS2013SQLVM"** lab to create a Windows Azure Virtual Machine for use as your development environment, **OR** A development workstation with **Visual Studio 2013 Ultimate RC** and **SQL Server 2012 Express** installed. 
- Successful completion of the **"02-AzureStorageLab"**.  The steps in that lab create the **"MVC4Sample"** database we will export in this lab. 
- Download of the [Lab Files](http://aka.ms/BLabs)

<a name="Setup"></a>
### Setup ###

The instructions in the lab assume you are remoted into the development Virtual Machine created during the **"01-SetupVS2013SQLVM"** lab.  If you prefer can follow the exact same steps on a Windows machine with **Visual Studio 2013 Ultimate RC** and **SQL Server 2012 Express** installed, however the instructions assume the environment created in the setup lab.

1. Open a **Remote Desktop Connection** to the development Virtual Machine you created in the **"01-SetupVS2013SQLVM"** lab.


---
<a name="Exercises"></a>
## Exercises ##

This hands-on lab includes the following exercises:

- [Exercise 1: Create a Windows Azure Virtual Machine with SQL Server](#Exercise1)
- [Exercise 2: Create a Windows Azure Storage Account to Store Bacpac Files](#Exercise2)
- [Exercise 3: Use SQL Server Management Studio to Export a SQL bacpac file to Windows Azure Storage](#Exercise3)
- [Exercise 4: Configure Security on the Windows Azure Virtual Machine with SQL Server](#Exercise4)
- [Exercise 5: Deploy the .bacpac to SQL Server running in a Windows Azure Virtual Machine with SQL Server](#Exercise5)

<!--
========================================
Exercise 1
========================================
-->

---
<a name="Exercise1"></a>
### Exercise 1: Create a Windows Azure Virtual Machine with SQL Server 2012 ###

In this exercise, you will provision a new **Windows Azure Virtual Machine** with **SQL Server 2012** already installed.  We will later deploy the **.bacpac** we created above into the SQL Server instance in that Virtual Machine.    

<a name="Exercise1Task1"></a>
#### Task 1 – Create a Windows Azure Virtual Machine with SQL Server 2012 ####

1. In the management portal, select "VIRTUAL MACHINES" along the left hand side, then click the "+ NEW" button in the bottom left hand corner:

	![15-New-Virtual-Machine](images/15-new-virtual-machine.png?raw=true "New Virtual Machine")

	_Create a new Virtual Machine_

1. In the **"NEW"** panel, select **"COMPUTE"** | **"VIRTUAL MACHINE"** | **"FROM GALLERY"**.

	![16-From-Gallery](images/16-from-gallery.png?raw=true "From Gallery")

	_Create a new Virtual Machine from the Gallery_

1. Pick the **"SQL Server 2012 SP1 Enterprise on WS 2012"** image.  As it's name implies, this will be a **Windows Server 2012 Virtual Machine** with **SQL Server 2012 SP1 Enterprise** pre-installed.  Click the **right arrow** to continue:

	![17-WS2012-SQL2012](images/17-ws2012-sql2012.png?raw=true "Windows Server 2012 with SQL Server 2012")

	_Windows Server 2012 with SQL Server 2012_

1. Give the new Virtual Machine a **name**, **size** and **user name** and password.  You may want to use the same user name and password that you used for the development Virtual Machine to help reduce confusion.  Click the **right arrow** to continue:

	![18-VM-Config](images/18-vm-config.png?raw=true "Virtual Machine Configuration")

	_Virtual Machine Name, Size, User Name and Password_

1. **Create a new cloud service** for the Virtual Machine.  Make sure the cloud service has a **unique DNS name**, then assign an appropriate **subscription**, **Region**, **Storage Account** and **Availability Set**.  Clic the **right arrow** to continue:

	![19-VM-Cloud-Service](images/19-vm-cloud-service.png?raw=true "Cloud Service")

	_Virtual Machine Cloud Service Settings_

1. We will want to connect to this Virtual Machine from other servers (for example, a Windows Azure Web Site).  Since we know that already we can provision the Cloud Service with an Endpoint that maps **MSSQL traffic (port 1433)** to the Virtual Machine's port 1433.  To do this, click the drop-down arrow next to **"ENTER OR SELECT A VALUE"** and select **"MSSQL"** from the list.  Then click the checkmark button in the lower right corner to finish the wizard:

	![20-Pick-MSSQL](images/20-pick-mssql.png?raw=true "Pick MSSQL From the List")

	_Pick MSSQL From the list of values_

	![21-MSSQL-Endpoint](images/21-mssql-endpoint.png?raw=true "MSSQL Endpoint")

	_MSSQL Endpoint_

1. Provisioning the Virtual Machine will take anywhere from 5-15 minutes.  While the provisioning is being done go ahead and continue with Exercises 2 and 3.  


<!--
========================================
Exercise 2
========================================
-->

---
<a name="Exercise2"></a>
### Exercise 1: Create a Windows Azure Storage Account to Store Bacpac Files ###

In this exercise you will use create a **Windows Azure Storage** account and to store the **.bacpac** files we will export using **SSMS** in the next exercise.  

<a name="Exercise2Task1"></a>
#### Task 1 – Create a Windows Azure Storage Account ####

1. On the remote development Virtual Machine, open the **Windows Azure Management Portal**, and select the **"Storage"** icon along the left.  Click the **"+ NEW"** button in the lower left corner. 

	![01-Storage-Portal](images/01-storage-portal.png?raw=true "Storage Accounts in the Portal")

	_Storage accounts in the portal_

1. In the **"NEW"** Panel, select **"DATA SERVICES"** | **"STORAGE"** | **"QUICK CREATE"**.  Give your new storage account a unique name, a location, a subscription (if needed) and click **"CREATE STORAGE ACCOUNT"**.

	![02-New-Storage-Account](images/02-new-storage-account.png?raw=true "New Storage Account")

	_Create a new storage account_

1. Once the storage accont is created, select it by clicking to the right of it's name in the Management Portal.  Click **"MANAGE ACCESS KEYS"** along the bottom to view the access keys for the account.  

	![03-Select-New-Account](images/03-select-new-account.png?raw=true "Select the New Storage Account")

	_Select the New Storage Account_

1. In the **"Manage Access Keys"** window, click the copy button (![04-Copy-Icon](images/04-copy-icon.png?raw=true "Copy Icon")) next to the **"PRIMARY ACCESS KEY"** to copy the key to the clipboard.  If prompted to allow access to the clipboard, click **"Allow"**:

	![05-Copy-Primary-Access-Key](images/05-copy-primary-access-key.png?raw=true "Copy Primary Access Key")

	_Copy the Storage Account's Primary Access Key to the Clipboard_

1. Make note of the storage account name and primary access key. You will need these both in a later exercise.

<!--
========================================
Exercise 3
========================================
-->

---
<a name="Exercise3"></a>
### Exercise 3: Use SQL Server Management Studio to Export a SQL bacpac file to Windows Azure Storage ###

In this section you will use **SQL Server Management Studio Express (SSMS)** to export a database to **".bacpac"** file.  **".Bacpac"** files include all the information needed to recreate a database, including it's data, where as **".dacpac"** files include just the database schema.

**SSMS** has the ability to create both **.bacpac** and **.dacpac** files. You can do this in both the **Express** version of **SQL Server Management Studio (SSMS)**, or the full version installed with SQL Server itself.   

<a name="Exercise3Task1"></a>
#### Task 1 – Exporting the on-premises database ####

This exercise is about creating a **bacpac** file and putting it in **Windows Azure Storage**. As you recall, in [Exercise 2](#Exercise2) we created a storage account for this very purpose.  

For this lab, assume the development Virtual Machine is your **"on-premises"** server.  We will export the **MVC4Sample** database on the on-premises SQL Server to a **.bacpac** file in **Windows Azure Storage**.  We will then later deploy that **.bacpac** file to the **SQL Server** instance running the **Windows Azure Virtual Machine** we created in [Exercise 1](#Exercise1).

1. In the development workstation VM, open **SQL Server Management Studio**, and connect to the "**(local)\SQLEXPRESS"** instance.  

1. In the **SSMS Object Explorer**, expand **"(local)\SQLEXPRESS...)"** | **"Databases"**.  **Right-click** on **MVC4Sample**. Choose **Tasks** | **Export Data-tier Application**. 

	> **Note:** There are actually a number of ways to move a database into Windows Azure.  From this menu you can see options like **"Generate Scripts..."**, **"Deploy Database to SQL Azure"**, **"Extract Data-tier Application..."** and **"Export Data-tier Application..."**.  In this lab we will use the **"Export Data-tier Application"** option so that we create an independent bacpac file we can use later.

	![02-Export-Data-Tier-Application](images/06-export-data-tier-application.png?raw=true "Export Data Tier Application")

	_Export Data Tier Application_

1. In the **"Export Data-tier Application 'MVC4Sample'"** wizard click **"Next"** to continue:

	![07-Export-Data-Tier-Application-Wizard](images/07-export-data-tier-application-wizard.png?raw=true "Export Data-tier Application Wizard")

	_Export Data-tier Application Wizard_

1. On the **"Export Settings"** page, select **"Save to Windows Azure"** and click **"Connect..."** to connect to the Storage Account we created above:

	![08-Save-To-Azure](images/08-save-to-azure.png?raw=true "Save to Windows Azure")

	_Save to Windows Azure_

1. In the "Connect to Windows Azure Storage" window, In the **"Storage account:"** field enter the name of the storage account you created above.  In the **"Account key:"** paste in the **Primary Access Key** for the storage account that we copied to the clipboard above.  Click **"Connect"** to connect.  

	![09-Connect-To-Storage](images/09-connect-to-storage.png?raw=true "Connect to Storage Account")

	_Connect to Storage Account_

1. Back on the **"Export Settings"** page enter the name of the blob storage container you want the .bacpac to be stored in.  If the container doesn't exist, it will be created during the export and click **"Next"**:

	![10-Select-Container](images/10-select-container.png?raw=true "Enter the Container Name")

	_Enter the Container Name_

1. On the **"Summary"** page, click **"Finish"** to complete the **"Export Data-tier Application 'MVC4Sample'"** Wizard:

	![11-Finish](images/11-finish.png?raw=true "Finish")

	_Finish the Wizard_

1. On the **"Results"** page, verify that the operation completed successfully and click **"Close"**:

	![12-Export-Results](images/12-export-results.png?raw=true "Verify Export Results")

	_Verify the Export Results_

1. Return to the **Azure Management Portal**, and open the **Storage Account** we just created.  Select the **"Containers"** page, and then click on the container name you created during the Export wizard above.  

	![13-Open-New-Container](images/13-open-new-container.png?raw=true "Open the New Storage Container")

	_Open the new Storage Container_

1. Verify that the **.bacpac** file exists in the container:

	![14-Verify-BacPac](images/14-verify-bacpac.png?raw=true "Verify the BacPac File in the Container")

	_Verify that the **.bacpac** file exists in the container_


<!--
========================================
Exercise 4
========================================
-->

---
<a name="Exercise4"></a>
### Exercise 4: Configure Security on the Windows Azure Virtual Machine with SQL Server ###

In this exercise you will first complete the configuraiton of the Windows Azure Virtual Machine with SQL Server 2012 that we provisioned in [Exercise 1](#Exercise1).  

There are two basic tasks here.  First is to open the Windows Firewall to allow traffic on port 1433 to make it into the SQL Server Instance.  The second task is to configure SQL Server to allow both SQL Server and Windows Authentication, as well as to provision a SQL Server Authenticated login that can be used for remote connections.

<a name="Exercise4Task1"></a>
#### Task 1 – Open the Windows Firewall to Allow Port SQL Server Traffic ####

Even though we created an Endpoint for **"MSSQL" (port 1433)** when we provisioned the Virtual Machine, that was for the external firewall on our Cloud Service.  We need to also open 1433 on the Windows Firewall inside the Virtual Machine if we want the traffic to be able to make it all the way in to the SQL Server instance on the Virtual Machine.  We'll do that here.

1. On the development Virtual Machine (remember it is playing the role of our "on-premises" server for this lab), in the Windows Azure Managment Portal, select the new **Windows Azure Virtual Machine with SQL Server** we created in [Exercise1](#Exercise1), click **"CONNECT"** along the bottom:

	![22-Connect-To-SQLVM](images/22-connect-to-sqlvm.png?raw=true "Connect to the SQL Server Virtual Machine")
	
1. Open the .RDP file and follow the prompts to connect to the SQL Server VM with Remote Desktop:

	_Connect to the SQL Server Virtual Machine_

	![23-Open-RDP](images/23-open-rdp.png?raw=true "Open the .RDP File")

	_Open the .RDP file_

1. In the **SQL Server VM** Remote Desktop Connection, move your mouse to the lower left corner, then click "Start"

	![24-Go-To-Start](images/24-go-to-start.png?raw=true "Go to the Start Screen")

	_Go to the Start Screen_

1. From the Start Screen, pick **"Administrative Tools"**:

	![25-Select-Admin-Tools](images/25-select-admin-tools.png?raw=true "Administrative Tools")
	
	_Administrative Tools_
	
1. Double click on "Windows Firewall with Advanced Security":

	![26-Windows-Firewall](images/26-windows-firewall.png?raw=true "Windows Firewall with Advanced Security")

	_Windows Firewall with Advanced Security_

1. In the **"Windows Firewall with Advanced Security"** window, right click **"Inbound Rules"** and select **"New Rule..."** from the pop-up menu:

	![27-New-Rule](images/27-new-rule.png?raw=true "New Rule")

	_New Rule_

1. In the **"New Inbound Rule Wizard"**, on the **"Rule Type"** page, select **"Port"** and click **"Next"**:

	![28-Port](images/28-port.png?raw=true "Port")

	_Select Port_

1. On the **"Protocol and Ports"** page, select **"TCP"**.  Then select **"Specific local ports:"** and enter **"1433"**.  SQL Server traffic goes to port 1433 by default.  Click "Next" to continue:

	![29-Enable-Port-1433](images/29-enable-port-1433.png?raw=true "Enable Port 1433")

	_Enable port 1433_

1. On the **"Action"** page, select **"Allow the connection"** and click **"Next"** to continue:

	![30-Allow-Connection](images/30-allow-connection.png?raw=true "Allow the Connection")

	_Allow the Connection_

1. On the **"Profile"** page, enable all the profiles and click "Next" to continue:

	![31-All-Profiles](images/31-all-profiles.png?raw=true "All Profiles")

	_Enable All Profiles_

1. On the **"Name"** page, enter a **Name** and **Description**, then click **"Finish"**:

	![32-Rule-Name](images/32-rule-name.png?raw=true "Rule Name")

	_Rule Name and Description_

1. You can close the **"Windows File with Advanced Security"** window.

<a name="Exercise4Task2"></a>
#### Task 2 – Configure SQL Server Security and Create a SQL Server Login ####

The SQL Server 2012 Instance installed via the gallery image is setup for Windows Authentication Only.  Because the computers in this lab don't belong to the same Active Directory Domain, security will be easier in general if we used SQL Server Authentication rather than Windows Authentication.  It should be said that in the long-run Windows Authentication would be preferred, but would require a little more configuration that we have time for in this lab.  

With that in mind, in this task, we will configured the SQL Server instance to support both SQL Server and Windows Authentication, then create a SQL Server Authenticated login that we can use to connect.

1. In the "Windows Azure VM with SQL Server" Remote Desktop Connection, open SQL Server Management Studio.  To do so you can go to the **Start** screen and start typing **SSMS**, then click the "SQL Server Management Studio" shortcut:

	![33-Find-SSMS](images/33-find-ssms.png?raw=true "Lanch SSMS")

	_Start SQL Server Management Studio_

1. When SSMS Opens, connect to the SQL Instance on the Virtual Machine with Windows Authentication:

	![34-Connect-SSMS](images/34-connect-ssms.png?raw=true "Connect SSMS")

	_Connect to the SQL Server Instance in the Virtual Machine_

1. In the SSMS **"Object Explorer"**, right click on the server node and select "Properties"

	![35-Server-Properties](images/35-server-properties.png?raw=true "Server Properties")

	_Go to the Server Properties_

1. In the "Server Properties" window, go to the "Security" and select "SQL Server and Windows Authentication mode".  Click "OK" to close the window:

	![36-Mixed-Mode-Security](images/36-mixed-mode-security.png?raw=true "Mixed Mode Security")

	_Enable SQL Server and Windows Authentication_

1. A dialog appear informing you to restart the SQL Server instance for the new settings to take effect.  Click "OK":

	![37-Restart-Warning](images/37-restart-warning.png?raw=true "Restart Warning")

	_Restart Warning_

1. In the SSMS **"Object Explorer"** right click the server node again and select **"Restart"**.  Click "Yes" in the window that appears to confirm the restart:

	![38-Restart-SQL](images/38-restart-sql.png?raw=true "Restart SQL")

	_Restart SQL Server_

1. Once the SQL Server Instance has restarted, in the **"Object Explorer"** window, expand **"Security"** | **"Logins"**.  Right click on **"Logins"** and select **"New Login..."**:

	![39-New-Login](images/39-new-login.png?raw=true "New Login")

	_Create a new Login_

1. In the **"Login - New"** window, on the **"General"** page, select **"SQL Server authentication"** and enter a SQL **Login name** and **Password**.  ***Again, you might want to use the same credentials as you have been using for the Virtual Machine login credentials just to reduce confusion.***  Ensure that the **"Enforce password policy"** check box is **CLEARED**:

	![40-Login-General](images/40-login-general.png?raw=true "Login General Settings")

	_General Settings_

1. Switch to the **"Server Roles"** page and select **"sysadmin"**.  The click **"OK"** to create the new login:

	> **Note:** Making this new login a sysadmin is overkill, but for the lab we don't want to have any security related issues.  You should consider a more restricted security configuration for production use.  

	![41-Make-Sysadmin](images/41-make-sysadmin.png?raw=true "Make Sysadmin")

	_Make the new Login a Sysadmin_

<!--
========================================
Exercise 5
========================================
-->

---
<a name="Exercise5"></a>
### Exercise 5: Deploy the .bacpac to SQL Server running in a Windows Azure Virtual Machine with SQL Server ###

In this final exercise, we will deploy the .bacpac file created back in Exercises [2](#Exercise2) and [3](#Exercise3) into the Windows Azure Virtual Machine with SQL Server.

First, we'll deploy the .bacpac to the Windows Azure Virtual Machine with SQL Server.  Then, we'll return to our development VM to verify that we can connect to the database remotely. 

<a name="Exercise5Task1"></a>
#### Task 1 – Deploy the .bacpac ####

1. In the Remote Desktop Connection to the Windows Azure Virtual Machine with SQL Server, in SQL Server Management Studio, right click on the "Databases" node and select "Import Data-tier Application":

	![32-Import-Bacpac](images/32-import-bacpac.png?raw=true "Import Data-tier Application")

	_Import Data-tier Application_

1. In the "Import Data-tier Application" wizard, on the "Introduction" page, click "Next" to continue:

	![43-Import-Intro](images/43-import-intro.png?raw=true "Import Introduction")

	_Introduction_

1. For the next step, you will need to recall the **Windows Azure Storage Account Name** and **Primary Access Key**.  We saw these back in [Exercise 2](#Exercise2).  If you need to return to the Management Portal to copy those values again, do so now.  Having the Primary Access Key copied to the clipboard will help with the next step.

1. On the "Import Settings" page, select "Import from Windows Azure" and click "Connect":

	![44-Import-Settings-Connect](images/44-import-settings-connect.png?raw=true "Import Settings")

	_Import Settings_

1. In the **"Connect to Windows Azure Storage"** window, enter your **"Storage account:"** name, and paste your **Primary Access Key** in for the **"Account key:"**.  Click **"Connect"** to close the window:

	![45-Connect-To-Storage](images/45-connect-to-storage.png?raw=true "Connect to Windows Azure Storage")

	_Connect to Windows Azure Storage_

1. Back on the "Import Settings" page, select the appropriate **"Container:"** and **"File name:"** values, then click **"Next"** to continue:

	![46-Pick-Blob](images/46-pick-blob.png?raw=true "Pick Blob")

	_Select the Blob Container and File Name_

1. On the **"Database Settings"** page, make sure the database name is "MVC4Sample" and click "Next".  

	> **Note:** If you change the database name you will need to remember that for all future steps.  The instructions assume a databas name of MVC4Sample.

	![47-Database-Name](images/47-database-name.png?raw=true "Database Name")

	_Target Database Name_

1. On the "Summary" page, click "Finish":

	![48-Finish](images/48-finish.png?raw=true "Finish")

	_Finish_

1. On the "Results" page, verify that the operation completed successfully and click "Close":

	![49-Close-Wizard](images/49-close-wizard.png?raw=true "Close Wizard")

	_Verify Successful Completion_


<a name="Exercise5Task1"></a>
#### Task 2 – Connect to the new database ####

1. Return to the **development workstation VM**, and open **SQL Server Management Studio**.  In the connect dialog, enter in the DNS name of the Cloud Service that hosts the Windows Azure Virtual Machine with SQL Server we created in [Exercise 1](#Exercise1).  Change the Authentication to "SQL Server Authentication" and enter the credentials for the SQL Server Login you created in (Exercise 4)[#Exercise4], then click "Connect":

	![50-SQL-Connection](images/50-sql-connection.png?raw=true "SQL Connection")

	_Connect remotely to the Windows Azure Virtual Machine with SQL Server_

1. In the SSMS **"Object Explorer"**, expand **"Databases"** | **"MVC4Sample"** | **"Tables"**. Right click on the **"dbo.Customers"** table and select **"Select Top 1000 Rows"** and in the query window that appears, verify that your records appear.  

	![51-Verify-Connection](images/51-verify-connection.png?raw=true "Verify Connection")

	_Verify Connection_


<!--
========================================
Summary
========================================
-->

---
<a name="Summary"></a>
## Summary ##
Wow, that was a long lab, but you got a lot accomplished.  

