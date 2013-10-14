<a name="Title"></a>
# Migrating an On-Premises a Web Site to Windows Azure Web Site #

---
<a name="Overview"></a>
## Overview ##

This hands-on lab walks you through the process of migrating an on-premise Web Site to a Windows Azure Web Site.  In the previous lab the database was either exported to a Windows Azure SQL Database or to a database on SQL Server running in a Windows Azure Virtual Machine.  In this lab you will migrate the Web Site into Azure as well and connect it to the Database in Azure.  

<a name="Objectives"></a>
### Objectives ###

In this hands-on lab, you will learn how to:

- Migrate an On-Premise Web Site into Windows Azure
- Connect an Azure Web Site to a Database in Azure.

<a name="Prerequisites"></a>
### Prerequisites ###

The following is required to complete this hands-on lab:

<!-- TODO: UPDATE THE Pre-Reqs to match the actual setup lab name  -->
<!-- TODO: FIX DOWNLOAD LINK -->
- A Windows Azure subscription -  [sign up for a free trial](http://aka.ms/WATK-FreeTrial) 
- **Either** Completing the **"01-SetupVS2013SQLVM"** lab to create a Windows Azure Virtual Machine for use as your development environment, **OR** A development workstation with **Visual Studio 2013 Ultimate RC** and **SQL Server 2012 Express** installed. 
- Successful completion of the **"02-AzureStorageLab"**.  The steps in that lab create the **"MVC4Sample"** database we will export in this lab. 
- Successful completion of either the **"03-MigratingDBToSQLVM"** or the **"03-MigratingDBToSQLDatabase"** labs.

<a name="Setup"></a>
### Setup ###

The instructions in the lab assume you are remoted into the development Virtual Machine created during the **"01-SetupVS2013SQLVM"** lab.  If you prefer can follow the exact same steps on a Windows machine with **Visual Studio 2013 Ultimate RC** and **SQL Server 2012 Express** installed, however the instructions assume the environment created in the setup lab.

1. Open a **Remote Desktop Connection** to the development Virtual Machine you created in the **"01-SetupVS2013SQLVM"** lab.


---
<a name="Exercises"></a>
## Exercises ##

This hands-on lab includes the following exercises:

- [Exercise 1: Migrate a Web Site to Windows Azure](#Exercise1)


<!--
========================================
Exercise 1
========================================
-->

---
<a name="Exercise1"></a>
### Exercise 1: Migrate a Web Site to Windows Azure ###

In this exercise, you will create a new **Windows Azure Web Site**.  Then you will open the **MVC4Sample.Web** application we used previously and **Publish** it to the new **Windows Azure Web Site**.  

<a name="Exercise1Task1"></a>
#### Task 1 – Create a Windows Azure Web Site ####

1. On the remote development Virtual Machine, open the **Windows Azure Management Portal**, and select the **"Web Sites"** icon along the left.  Then, click the **"+ NEW"** button in the lower left corner. 

	![01-New-Web-Site](images/01-new-web-site.png?raw=true "New Web Site")

	_New Web Site_

1. In the "NEW" panel, select **"COMPUTE"** | **"WEB SITE"** | **"QUICK CREATE"**.  Give the new Web Site a unique name, a region, and if needed a subscription, then click **"CREATE WEB SITE"**.

	![02-Create-Web-Site](images/02-create-web-site.png?raw=true "Create Web Site")

	_Create a new Web Site_

1. Verify that the new Web Site was created successfully:

	![03-Verify-New-Site](images/03-verify-new-site.png?raw=true "Verify the New Web Site")

	_Verify the new web site was created successfully_

<a name="Exercise1Task2"></a>
#### Task 2 – Publish the on-premise Web Site to the new Windows Azure Web Site ####

1. On the development Virtual Machine, open **Visual Studio 2013** and open the **"MVC4Sample.Web"** project we worked with previously. 

	![04-Open-Web-Project](images/04-open-web-project.png?raw=true "Open the Web Site Project")

	_Open the Web Site Project_

1. In the Visual Studio **"Solution Explorer"** window, select the **"MVC4Sample.Web"** project.  Then from the menu bar select **"BUILD"** | **"Publish MVC4Sample.Web"**.

	![05-Publish-Menu-Option](images/05-publish-menu-option.png?raw=true "Publish Menu Option")

	_Publish Web Site Menu Option_

1. In the **"Publish Web"** window, on the **"Profile"** page, click the **"Import..."** button to import publisher settings from Windows Azure:

	![06-Publish-Import](images/06-publish-import.png?raw=true "Import Publisher")

	_Import Publisher Settings_

1. In the "Import Publish Profile" window, select "Import from a Windows Azure web site" then click the "Add Windows Azure subscription" link:

	![07-Add-Azure-Subscription](images/07-add-azure-subscription.png?raw=true "Add a Windows Azure Subscription")
	
	_Add a Windows Azure Subscription_

1. In the "Import Windows Azure Subscriptions" window, click the "Download subscription file" link:

	![08-Download-Subscription-File](images/08-download-subscription-file.png?raw=true "Download Subscription File")

	_Download Subscription File_

1. The browser window will open, if you are prompted to login to the Windows Azure Management Portal, enter your credentials.  Once connected, you will be prompted to download the **".publishsettings"** file for your subscription.  Click the "Save" button to save the file to the default **"Downloads"** folder on the development Virtual Machine.


	![09-Save-PublishSettings](images/09-save-publishsettings.png?raw=true "Save Publisher Settings")

	_Save the **".publishsettings"** file to the **"Downloads"** folder_

1. Close the browser and return to Visual Studio's **"Import Windows Azure Subscriptions"** window.  Click the **"Browse..."** button and navigate to the "Downloads" folder and select the ".publishsettings" file you just downloaded.

	![10-Select-PublishSettings](images/10-select-publishsettings.png?raw=true "Select Publish Settings")

	_Import the downloaded **".publishsettings"** file_

1. Click the "Import" button to import the publisher settings:

	![11-Import-Publisher-Settings](images/11-import-publisher-settings.png?raw=true "Import Publisher Settings")

	_Import Publisher Settings_

1. Back in the "Import Publish Profile" window, select the web site we created above from the drop-down list, and click "OK" to configure Visual Studio to publish to the Web Site:

	![12-Select-New-Web-Site](images/12-select-new-web-site.png?raw=true "Select New Web Site")

	_Select the New Web Site_

1. On the **"Publish Web"** window's **"Connection"** page, you don't need to change any settings, but you can click a1oaiinx6l.database.windows.net and verify that the connection succeeds (by the green circle and checkmark appearing).  Click **"Next"** to continue. 

	![13-Validate-Connection](images/13-validate-connection.png?raw=true "Validate Connection")

	_Validate the Connection_

1. On the "Settings" page, click the ellipsis ("...") button to the right of the "CustomerContext" connection string.  We will change this connection to point to either the Azure SQL Database, or to the SQL Database running in a Windows Azure Virtual Machine, depending on whether you completed the **"04-MigratingDBToSQLDatabase"** lab or the **"04-MigratingDBToSQLVM"** lab.

	![14-CustomerContext](images/14-customercontext.png?raw=true "Customer Context Connection")

	_Modify the CustomerContext Connection_

1. In the **"Destination Connection String"** window, for the server name enter the fully qualified DNS name for either your **Azure SQL Database Server** (**"_&lt;AzureSQLDatabaseServer&gt;_.database.windows.net"**), or for the **Cloud Service** that houses your **Windows Azure Virtual Machine** with **SQL Server** installed (**"_&lt;CloudServiceName&gt;_.cloudapp.net"**).  Then enter the appropriate **user name** and **password** for the SQL Database (you created these credential earlier when you migrated the database to Windows Azure), and finally, select the **"MVC4Sample"** database you deployed earlier. Click **"OK"** to close the dialog.

	![15-Destination-Connection-Settings](images/15-destination-connection-settings.png?raw=true "Destination Connection Settings")

	Destination Connection Settings_

1. Back on the "Settings" page, click "Next" to continue:

	![16-Settings-Next](images/16-settings-next.png?raw=true "Settings then Next")

	_Settings_

1. On the "Preview" page, simply click "Publish":

	![18-Publish](images/18-publish.png?raw=true "Publish")

	_Publish_

1. You can monitor the progress of the Publish operation in the "Web Publish Activity" window.  When the publish succeeds, click on the link to the web site to open it in the browser:

	![19-Web-Publish-Activity-Window](images/19-web-publish-activity-window.png?raw=true "Web Publish Activity Window")

	_Web Publish Activity Window_

1. Confirm that the web site opens properly in the browser, and that the URL points to the Web Site in Windows Azure:

	![20-Web-Site-Running-In-Azure](images/20-web-site-running-in-azure.png?raw=true "Web Site Running In Azure")

	_Web Site Running in Windows Azure_

1. Open the **http://_&lt;sitename&gt;_.azurewebsites.net/Customer/** page to verify that the customer data is being properly served by the SQL Database in Azure:

	![21-Data-Coming-From-SQL-In-Azure](images/21-data-coming-from-sql-in-azure.png?raw=true "Data Coming from SQL in Azure")

	_Customer data coming from SQL in Azure_


<!--
========================================
Summary
========================================
-->

---
<a name="Summary"></a>
## Summary ##
You have now successfully exported the **bacpac** file to **Windows Azure Storage**. You are now able to leverage this **blob** or **bacpac** file and import into an Azure SQL Database or a Windows Azure SQL VM.

