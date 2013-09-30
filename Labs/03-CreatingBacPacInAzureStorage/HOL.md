<a name="Title"></a>
# Exporting an On-Premises Database to Windows Azure Storage #

---
<a name="Overview"></a>
## Overview ##

This hands-on lab walks you through the process to export an on-premise SQL database as a **.bacpac** file and storing the **.bacpac** file in Windows Azure Storage as a blob.  There are a number of ways that an on-premise database can be deployed to Windows Azure.  In this lab we will use **"SQL Server Management Studio"**'s (SSMS) **"Export Data-tier Application"** feature to create a **.bacpac** and store it in a **Windows Azure Storage Account**.

![Exporting to Windows Azure Storage](Images/01-highlevel.png?raw=true)

_Exporting to Windows Azure Storage_

Once you create the **bacpac** file, you can import into another SQL database such as an **Azure SQL Database** or a database on SQL Server instance running in a **Windows Azure Virtual Machine**.

<a name="Objectives"></a>
### Objectives ###

In this hands-on lab, you will learn how to:

- Create a Windows Azure Storage Account to store the **.bacpac** file
- Use SQL Server Management Studio to Export a Data-tier Application (**.bacpac**) 

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

- [Exercise 1: Create a Windows Azure Storage Account to Store Bacpac Files](#Exercise1)
- [Exercise 2: Use SQL Server Management Studio to Export a SQL bacpac file to Windows Azure Storage](#Exercise2)

<!--
========================================
Exercise 1
========================================
-->

---
<a name="Exercise1"></a>
### Exercise 1: Create a Windows Azure Storage Account to Store Bacpac Files ###

In this exercise you will use create a **Windows Azure Storage** account and to store the **.bacpac** files we will export using **SSMS** in the next exercise.  

<a name="Exercise1Task1"></a>
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
Exercise 2
========================================
-->

---
<a name="Exercise2"></a>
### Exercise 2: Use SQL Server Management Studio to Export a SQL bacpac file to Windows Azure Storage ###

In this section you will use **SQL Server Management Studio Express (SSMS)** to export a database to **".bacpac"** file.  **".Bacpac"** files include all the information needed to recreate a database, including it's data, where as **".Dacpac"** files include just the database schema.

**SSMS** has the ability to create both **.bacpac** and **.dacpac** files. You can do this in both the **Express** version of SQL Server Management Studio, or the full version installed with SQL Server itself.   

<a name="Exercise2Task1"></a>
#### Task 1 – Exporting the on-premises database ####

This exercise is about creating a **bacpac** file and putting it in **Windows Azure Storage**. As you recall, in [Exercise 1](#Exercise1) we created a storage account for this very purpose.  

1. In the development workstation VM, open SQL Server Management Studio from the "start" screen, and connect to the "(local)\SQLEXPRESS" instance.  

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

1. Return to the A**zure Management Portal**, and open the **Storage Account** we just created.  Select the **"Containers"** page, and then click on the container name you created during the Export wizard above.  

	![13-Open-New-Container](images/13-open-new-container.png?raw=true "Open the New Storage Container")

	_Open the new Storage Container_

1. Verify that the **.bacpac** file exists in the container:

	![14-Verify-BacPac](images/14-verify-bacpac.png?raw=true "Verify the BacPac File in the Container")

	_Verify that the **.bacpac** file exists in the container_

<a name="Summary"></a>

## Summary ##
You have now successfully exported the **bacpac** file to **Windows Azure Storage**. You are now able to leverage this **blob** or **bacpac** file and import into an Azure SQL Database or a Windows Azure SQL VM.

