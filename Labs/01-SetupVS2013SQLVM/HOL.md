<a name="Title"></a>
# Setting up an Azure Virtual Machine For Developers with Visual Studio 2013 Ultimate and SQL Server 2012 Express #

---
<a name="Overview"></a>
## Overview ##

This hands-on lab walks you step by step through the process of creating a **Windows Azure Virtual Machine** that includes **Visual Studio 2013 RC**, **SQL Server 2012 Express**, and **SQL Server 2012 Management Studio Express**. You will use the virtual machine you create in this lab as the development environment for other labs. 

<a name="Objectives"></a>
### Objectives ###

In this hands-on lab, you will learn how to:

- Create a Virtual Machine with Visual Studio 2013 RC from the Windows Azure Management Portal
- Remotely manage Azure Virtual Machine instances
- Install additional software into Virtual Machines

<a name="Prerequisites"></a>
### Prerequisites ###

The following is required to complete this hands-on lab:

- A Windows Azure subscription - [sign up for a free trial](http://aka.ms/WATK-FreeTrial)  

<a name="Setup"></a>
### Setup ###

In order to execute the exercises in this hands-on lab you need to set up your environment.

1. Start by logging into the **Windows Azure Portal** (https://manage.windowsazure.com) to ensure that you have a valid Windows Azure subscription that you wish to create the Virtual Machine in.  


---
<a name="Exercises"></a>
## Exercises ##

This hands-on lab includes the following exercises:

- [Exercise 1: Creating an Azure Virtual Machine using the Windows Azure Virtual Image Gallery](#Exercise1)
- [Exercise 2: Installing SQL Server 2012 Express](#Exercise2)
- [Exercise 3: Installing SQL Server Management Studio Express](#Exercise3)

<!--  
========================================
Exercise 1 
========================================
-->

<a name="Exercise1"></a>
### Exercise 1: Creating an Azure Virtual Machine using the Windows Azure Virtual Image Gallery ###

In this section, you will log into the Windows Azure Portal and create an Azure Virtual Machine using the Windows Azure Gallery. 

<a name="Exercise1Task1"></a>
#### Task 1 – Creating an Azure Virtual Machine using the Windows Azure Virtual Image Gallery ####

1. Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription. On the left menu, select **Virtual Machines**, then click the "**+ NEW**" button in the lower left corner.

	![01AzurePortalNewButton](images/01azureportalnewbutton.png?raw=true "Azure Portal "+ NEW" Button")

	_Log into the Windows Azure Management Portal and click **"+ NEW"**_

1. In the menu located at the bottom, select **New | Compute | Virtual Machine | From Gallery** to start creating a new virtual machine.

	![02CreateAzureVM](images/02createazurevm.png?raw=true "Create a new Virtual Machine")	

	_Creating a new Virtual Machine from the Gallery_
 
1. In the **Virtual Machine image Selection** page, click **Platform Images** on the left menu and select the **Visual Studio Ultimate 2013 RC** from the list. Click the **arrow in the lower right corner** to continue.	

	![03VMImageSelection](images/03vmimageselection.png?raw=true "Virtual Machine Iamge Selection")

	_Selecting Visual Studio 2013 Ultimate 2013 RC_


1. The wizard will start by having you specify a **virtual machine name**. You will also choose the **machine size**. Finally, you need to supply an administrative Windows **User Name** and **Password**.  The password must conform to basic password complexity requirements. You will need to know your **user name** and **password** for later, when you log into the machine with **Remote Desktop**.  Click the next arrow when done. 

	> **Note:** **Be sure to pick an appropriate machine size!**  The [price per hour](http://www.windowsazure.com/en-us/pricing/details/virtual-machines/ "Virtual Machine Pricing") increases with each step up in Virtual Machine size.  The time used by this virtual machine will be billed against the subscription you select in the next screen according to the subscription's benefits.  This Virtual Machine's cores will also count against your subscriptions total core count.  Ensure that you choose a machine size with those details in mind. A "**Medium (2 cores, 3.5 GB memory)**" image should perform reasonably well throughout the lab.  You can also always increase it or decrease it later as needed.  

	![04VMNameSizeUser](images/04vmnamesizeuser.png?raw=true "Virtual Machine Name, Size and User Info")

	_Configuring the VM's Name, Size and User Info_

1. Next, configure the **"Cloud Service"** that the virtual machine will run within.  Cloud services provide administrative and security boundaries around a collection of virtual machines and network endpoints.  Each cloud service needs::

	- **Cloud Service**: Create a new cloud service. 
	- **Cloud Service DNS Name**: When creating a new cloud service you need to supply a unique cloud service name (see note)
   - **Subscripton**: The Windows Azure subscription to use for the VM (only appears if you have more than one subscription)
	- **Region**: The data center to you want the virtual machine to run in
	- **Storage Account**: The Windows Azure Storage account where you want the Virtual Hard Drive (vhd) blob to be stored.  You can choose an existing storage account, or let the wizard create one on the fly for you. 
	- **Availability Set**: We won't use Availability Sets in this lab, so leave it at "(None)".  You can learn more about Availability Sets [here](http://www.windowsazure.com/en-us/manage/windows/common-tasks/manage-vm-availability/ "Manage the Availability of Virtual Machines").

	![05VMCloudServiceRegionStorage](images/05vmcloudserviceregionstorage.png?raw=true "Virtual Machine Cloud Service, Region and Storage")

	_Configuring the VM's Cloud Service, Region and Storage_

	> **Note:** The URL used for the virtual machine corresponds to a DNS name and is subject to standard DNS naming rules. Moreover, the name is publicly visible and must therefore be unique. The portal ensures that the name is valid by verifying that the name complies with the naming rules and is currently available. A validation error will be shown if you enter a name that does not satisfy the rules.

1. Finally, configure the public **endpoints** to allow connections from outside the Windows Azure data center into the Cloud Service and it's VMs. For this lab, **you don't need to change anything**.  Just review them and **click the checkmark in the lower right corner** to continue.

	![06CloudServiceEndpoints](images/06cloudserviceendpoints.png?raw=true "Cloud Service Endpoints")

	_Cloud Service Endpoint Configuration_

1. It will take somewhere between **5 to 15 minutes** to fully provision the virtual machine. You can monitor the process in the management portal.  Eventually, the Virtual Machine **"STATUS"** should read **"Running"**:

	![07VMRunning](images/07vmrunning.png?raw=true "Virtual Machine Running")	

	_The virtual machine running_

<a name="Exercise1Task2"></a>
#### Task 2 – Connecting to the Virtual Machine using Remote Desktop ####

Now that the Virtual Machine has been provisioned, it's just like any  other Virtual Machine, or remote Windows Server you have worked with in the past.  You can connect to the Virtual Machine using standard management tools, provided that the appropriate endpoints and ports have been opened.  In the previous task, we configured our cloud service with an endpoint for **Remote Desktop (port 3389)** that we can use to remotely manage our Virtual Machine.  In addition we can connect with **PowerShell (port 5986)** to perform automated management tasks.   In this next task, you will connect to the new Virtual Machine using remote desktop, and disable the **Internet Explorer Enhanced Security Configuration (ESC)**. We need to disable it so we can download and install **SQL Server Management Studio Express** in a later exercise.  

1. In the **Windows Azure Management Portal**, on the **"VIRTUAL MACHINES"** page, select the Virtual Machine we just created.  and click the **"CONNECT"** button along the bottom:

	![08ConnectToVM](images/08connecttovm.png?raw=true "Connect to the Virtual Machine")

	_Connect to the new Virtual Machine using Remote Desktop_

1. The portal will display a notification about the .rdp file being downloaded.  You can **click "OK" to clear the notification**:

	![09PortalRdpNotification](images/09portalrdpnotification.png?raw=true "Portal RDP File Download Notification")

	_Portal RDP File Download Notification_

1. Internet Explorer will prompt you to open or save the .RDP file.  For this lab you can just **click "Open"**. However, saving it would allow you to configure the .RDP file settings (like display size, available resources, etc) for repeated use.  

	![10OpenRDPFile](images/10openrdpfile.png?raw=true "Open the RDP File")

	_Open the RDP File_

1. You may see a **"Remote Desktop Connection"** publisher confirmation dialog.  If so, click "**Connect**" to continue:

	![11RDPPublisherConfirmation](images/11rdppublisherconfirmation.png?raw=true "Remote Desktop Publisher Confirmation")

	_Remote Desktop Publisher Confirmation_

1. When prompted, enter the Virtual Machine administrative credentials you created in the previous task.  

	![12EnterVMCredentials](images/12entervmcredentials.png?raw=true "Enter Virtual Machine Credentials")

	_Enter the credentials for the virtual machine_

1. You will likely be prompted to confirm the certificate for the **Remote Desktop Connection**.  If so, click "**Yes**" to continue:

	![13RemoteDesktopCertificationConfirmation](images/13remotedesktopcertificationconfirmation.png?raw=true "Remote Desktop Certificate Confirmation")

	_Remote Desktop Certificate Confirmation_

1. Once you have successfully connected to the Virtual Machine in Remote Desktop, you will see the desktop of the remote machine, and the Windows "**Server Manager**" will run automatically (***it may take some time for it to appear when you first log in***).  Once it appears, click the "**Configure this local server**" link:

	> **Note:** If "Server Manager" doesn't run automatically, you can launch it from the icon on the remote machine's task bar. 


	![14ConfigureLocalServer](images/14configurelocalserver.png?raw=true "Configure Local Server in Server Manager")

	_Configure the Local Server in Server Manager_


1. On the "**Properties**" page for the Local Server, next to "**IE Enhanced Security Configuration**", click the "**Off**" link:

	![15IeEscLink](images/15ieesclink.png?raw=true "IE Enhanced Security Configuration Settings Link")

	_IE Enhanced Security Configuration Settings Link_

1. In the **"Internet Explorer Enhanced Security Configuration"**, select **"Off"** for both **Administrators** and **Users**.  Click **"OK"** to close the window:

	![16IeEscTurnedOff](images/16ieescturnedoff.png?raw=true "IE Enhanced Security Configuration Turned Off")

	_IE Enhanced Security Configuration Turned Off_

	> **Note:** For a normal server you would want to leave this on.  However because will be using the Virtual Machine more like a workstation, and we need to download and install other software later, we will turn it off.  

1. Close the **"Server Manager"** window.  At this point you have successfully created a Windows Azure Virtual Machine using an image from the Gallery.  You have also connected to that Virtual Machine using Remote Desktop and configured it!  Congratulations!   Keep the remote desktop connection open for the next exercise. 


<!--  
========================================
Exercise 2 
========================================
-->

<a name="Exercise2"></a>
### Exercise 2: Installing SQL Server 2012 Express ###

In this exercise, we will install **SQL Server 2012 Express** on the Virtual Machine we just created.  You could of course install a full version of SQL Server, but the "Visual Studio Ultimate 2013 RC" virtual machine image we used to create the VM already has the installation files for SQL Server 2012 Express, so we will just use that for convenience.   

<a name="Exercise2Task1"></a>
#### Task 1 – Install SQL Server 2012 Express ####

1. In the **Remote Desktop Connection** window for the remote Virtual Machine, ensure that you are on the Desktop.  There is a shortcut on the desktop to a folder named **"Configure Developer Desktop"**.  Double Click that shortcut to open the folder. 

	![17ConfigureDevelopDesktopShortcut](images/17configuredevelopdesktopshortcut.png?raw=true "Configure Developer Desktop Shortcut")

	_Open the "Configure Developer Desktop" folder shortcut_

1. In the **"ConfigureDeveloperDesktop"** folder, double click on the **"Scripts"** folder to open it:

	![18ScriptsFolder](images/18scriptsfolder.png?raw=true "Open the Scripts Folder")

	_Open the Scripts Folder_

1. Right click on the **"ConfigureSQLExpress"** icon, and select **"Run with PowerShell"**:

	![19ConfigureSQLExpressInPowerShell](images/19configuresqlexpressinpowershell.png?raw=true "Run ConfigureSQLExpress in PowerShell")
	
	_Run "ConfigureSQLExpress" in PowerShell_

1. You will be prompted to confirm the **PowerShell "Execution Policy Change"**.  Type **"Y"** at the prompt and press **"Enter"** to confirm it.  The script will start the SQL Server Express installation:

	![20PowerShellExecutionPolicyChangeConfirmation](images/20powershellexecutionpolicychangeconfirmation.png?raw=true "PowerShell Execution Policy Change Confirmation")

	_PowerShell Execution Policy Change Confirmation_

1. A second command prompt window will appear to run the actual SQL Server Express Setup.  **The setup process will run for 5-15 minutes.**  You won't see any changes until the script is complete.  Please be patient and wait for it to finish:

	![21SQLServerExpressSetup](images/21sqlserverexpresssetup.png?raw=true "SQL Server Express Setup")

	_SQL Server Express setup could take as long as 15 minutes_

<a name="Exercise2Task2"></a>
#### Task 2 – Connect to SQL Express from Visual Studio Ultimate 2013 RC ####

Now that **SQL Server 2012 Express** is installed, we will verify it by connecting to it from V**isual Studio Ultimate 2013 RC**.  This will confirm the installation, but also give us a chance to prepare Visual Studio for use. 

1. In the **Remote Desktop Connection** window for the virtual machine, return to the desktop and double click the **"Visual Studio 2013 RC"** shortcut.  

	![22VS2013Shortcut](images/22vs2013shortcut.png?raw=true "Visual Studio 2013 RC Shortcut")

	_Open the **"Visual Studio 2013 RC"** Shortcut_

1. When prompted, click **"Sign In"** button to sign in with your **Microsoft Account**:

	![23SignIntoVS2013](images/23signintovs2013.png?raw=true "Sign In")

	_Sign In_

	> **Note:** You are required to sign in with a valid Microsoft Account in order to activate the Visual Studio Ultimate 2013 RC license.

1. Enter your Microsoft Account credentials and click **"Sign in"**:

	![24MicrosoftAccountCredentials](images/24microsoftaccountcredentials.png?raw=true "Enter your Microsoft Account Credentials (aka Live ID)")

	_Enter your Microsoft Account (previously known as your "Live ID") credentials_

1. If you didn't sign in, or you don't have a Visual Studio profile stored for your Microsoft Account, you will be prompted to select the Visual Studio **"Development Settings"** and **"theme"**.  Select what you desire and click **"Start Visual Studio"**:

	![25VSEnvironmentSettings](images/25vsenvironmentsettings.png?raw=true "Visual Studio Environment Settings")

	_If prompted, select your Visual Studio environment settings_

1. Once Visual Studio is running, from the menu bar select **"VIEW"** | **"SQL Server Object Explorer"**:

	![26ViewSQLServerObjectExplorer](images/26viewsqlserverobjectexplorer.png?raw=true "View SQL Server Object Explorer")

	_Open the SQL Server Object Explorer_

1. In the **"SQL Server Object Explorer"** window, right click **"SQL Server"** and select **"Add SQL Server..."**:

	![27AddSQLServer](images/27addsqlserver.png?raw=true "Add a SQL Server Connection")

	_Add a SQL Server Connection_

1. In the **"Connect to Server"** window, enter **"(local)\SQLEXPRESS"** as the server name.  Keep the Authentication as **"Windows Authentication"** and click **"Connect"**:

	![28ConnectLocaSQLExpress](images/28connectlocasqlexpress.png?raw=true "Connect to (local)\SQLEXPRESS")

	_Connect to **"(local)\SQLEXPRESS"** using Windows Authentication_

1. Once connected, expand the **"(local)\SQLXPRESS ..."** node, then expand **"Databases"**.  You should see that **there are no user databases** on this brand new instance of SQL Server Express.  **Databases will be created in other labs.**  

	![29NoUserDatabases](images/29nouserdatabases.png?raw=true "No User Databases")

	_No User Databases_

1. Close **Visual Studio Ultimate 2013 RC** and return to the desktop of the remote machine.  Keep the remote connection window open for the next exercise.


<!--  
========================================
Exercise 3 
========================================
-->

<a name="Exercise3"></a>
### Exercise 3 – Installing SQL Server Management Studio Express ###

In the previous exercise we installed **SQL Server 2012 Express**, and verified that we could connect to it from Visual Studio.  Visual Studio does have basic SQL Server management capabilities, but it can't do everything we will need for future labs.  To solve that problem, we will install **SQL Server 2012 Management Studio Express**.  This is a free version of SQL Server Management Studio that can be used with SQL Server Express installations, and has all the functionality we need for the other labs in this series.  

<a name="Exercise3Task1"></a>
#### Task 1 - Install SQL Server Management Studio Express ###

1. In the **Remote Desktop Connection** window for your development virtual machine, open **Internet Explorer**, and navigate to **http://aka.ms/sqlexp12** .  This link should take you to the **"Microsoft SQL Server 2012 Express"** download page.  On that page click the **"Download"** button:

	> **Note:** The first time you launch Internet Explorer you may be prompted to use the recommended settings.  If so select the **"Use recommended security and compatability settings"** option and click **"OK"** to continue.

	![30ClickDownload](images/30clickdownload.png?raw=true "Click the Download Button")

	_Click the "Download" button_

1. In the "Choose the download you want" list, select ***ONLY*** **"ENU\x64\SQLManagementStudio_x64_ENU.exe"** and click **"Next"**:

	![31ChooseSSMS](images/31choosessms.png?raw=true "Choose the SQL Management Studio Download")

	_Choose **ONLY** **"ENU\x64\SQLManagementStudio_x64_ENU.exe"**_

1. If the pop-up is blocked by IE click **"Allow Once"** to continue:

	![32AllowPopUp](images/32allowpopup.png?raw=true "Allow Pop-ups ")

	_Allow Pop-ups_

1. When prompted to save the download, click **"Save"** to save it to the default **"Downloads"** folder on the remote machine:

	![33SaveToDownloads](images/33savetodownloads.png?raw=true "Save to Downloads")

	_Save to default Downloads folder_

1. The download may take a couple of minutes to complete (it is a 600MB file).  Remember that you are downloading into the Virtual Machine running in Azure, not to your local computer.  Once the download has completed, click the **"Run"** button to install SQL Server Management Studio Express (SSMS) into the remote Virtual Machine:

	![34RunSSMSInstall](images/34runssmsinstall.png?raw=true "Run the SSMS Installation")

	_Run the downloaded SSMS Installation_

1. In the **"SQL Server Installation Center"** window, on the **"Installation"** page, click **"New SQL Server stand-alone installation or add features to an existing installation"**:

	![35AddFeatures](images/35addfeatures.png?raw=true "Add Features to an Existing Installation")

	_Add Features to an Existing Installation_

1. If prompted to install any updates, do so.  Otherwise, click **"Next"** to continue:

	![36ConfirmUpdates](images/36confirmupdates.png?raw=true "Confirm Updates")

	_Confirm Updates_

1. On the **"Installation Type"** page, select **"Add features to an existing instance of SQL Server 2012"**, and select **"SQLEXPRESS"** from the dropdown list:

	![37AddToSQLExpress](images/37addtosqlexpress.png?raw=true "Add features to SQLEXPRESS")

	_Add features to the **"SQLEXPRESS"** instance_

1. On the **"Feature Selection"** page, turn ***ON*** the checkbox next to **"Management Tools - Basic"** and click **"Next"** to continue:

	![38SelectManagementToolsBasic](images/38selectmanagementtoolsbasic.png?raw=true "Select Management Tools - Basic")
	
	_Select "Management Tools - Basic"_

1. Accept the defaults on the **"Error Reporting"** page and click **"Next"** to continue:

	![39ErrorReportingDefaults](images/39errorreportingdefaults.png?raw=true "Accept the Error Reporting Defaults")

	_Accept the Error Reporting defaults_

1. **The installation may take 5-10 minutes to complete**.  When it does, the **"Complete"** page will appear.  Click **"Close"** to exit:

	![40InstallationComplete](images/40installationcomplete.png?raw=true "Installation Complete")
	
	_Installation Complete_

1. You can close the **"SQL Server Installation Center"** and **"Internet Explorer"** windows if they are still open.  

1. To get to the start screen on the remote virtual machine, first **left click** with your mouse inside the **"Remote Desktop Connection"** window for the virtual machine.  Then **press the "Windows" key** on the keyboard.  You can also move your mouse into the lower left hand corner of the remote desktop, or in either the upper or lower right corner to get to the Charms bar, and click "Start" from there. Regardless, you should now see a **"SQL Server Management Studio"** tile on the start screen.  **Click that tile to launch SSMS**.  

	![41SSMSTile](images/41ssmstile.png?raw=true "SSMS Tile")

	_Click the SQL Server Management Studio Tile_

1. In the **"Connect to Server"** window, enter **"(local)\SQLEXPRESS"** for the server name, leave the authentication as **"Windows Authentication"** and click **"Connect"**:

	![42SSMSConnectWindows](images/42ssmsconnectwindows.png?raw=true "SSMS Connection Window")

	_SSMS Connection Window_

1. You should be successfully connected, and the server should appear in the **SSMS** **"Object Explorer"** window.  

	![43SSMSObjectExplorer](images/43ssmsobjectexplorer.png?raw=true "SSMS Object Explorer")

	_**"(local)\SQLEXPRESS"** Connection in SSMS Object Explorer_


***Congratulations!  You have successfully added SQL Server 2012 Express & SQL Server Management Studio Express to a Virtual Machine running in Windows Azure!***


