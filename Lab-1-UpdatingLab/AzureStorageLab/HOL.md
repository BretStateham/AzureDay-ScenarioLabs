<a name="Title"></a>
# Migrating a Website's Assets to AzureStorage #

---
<a name="Scenario"></a>
## Scenario ##

 You have a website running on premise, but to increase the availability of your site's assets, or to reduce the traffic against your own bandwidth, you want to move them into the cloud.

The are many beneifts you could gain by doing this, including:

 - Reducing the network congestion on connections coming in to your on premise networks
 - Reducing the amount storage consumed by these assets on your on premise storage systems.
 - Leveraging the Windows Azure Content Distribution Network (CDN) features

In this hands-on lab, you will explore the basic process for creating an Azure Storage account, uploading your website assets (images, videos, etc) to blob containers in the storage account, and modifying your on-premise site to reference the new cloud based assets. 

<a name="Objectives"></a>
### Objectives ###

In this hands-on lab, you will learn how to:

- Create a Windows Azure Storage account
- Use the Visual Studio tooling to manage Windows Azure Store
- Use the Visual Studio tooling to upload assets into Windows Azure Storage blob containers
- Reference public assets stored in Windows Azure Storage blob containers from anywhere in the world. 

<a name="Prerequisites"></a>
### Prerequisites ###

The following is required to complete this hands-on lab:

<!-- TODO: UPDATE THE Pre-Reqs to match the actual setup lab name  -->
- A Windows Azure subscription -  [sign up for a free trial](http://aka.ms/WATK-FreeTrial) 
- **Either** Completing the "**Setup**" lab to create a Windows Azure Virtual Machine for use as your development environment.  
- **OR** A development workstation with **Visual Studio 2013 Ultimate RC** and **SQL Server 2012 Express** installed. 
- Download of the [Lab Files]( https://github.com/BretStateham/AzureDay-ScenarioLabs/archive/master.zip)



---
<a name="Exercises"></a>
## Exercises ##

This hands-on lab includes the following exercises:

- [Exercise 1: Opening and running the MVC4Sample Website](#Exercise1)
- [Exercise 2: Creating a Windows Azure Storage Account](#Exercise2)
- [Exercise 3: Uploading Web Site Assets to Azure Storage](#Exercise3)

<a name="Exercise1"></a>
### Exercise 1: Opening and running the MVC4Sample Website ###

In this section, you will open an existing MVC Website in Visual Studio.  This website simulates an existing on-premise website.  Later, we will create an Azure Storage Account and migrate the websites assets into that storage account. 

>Note: The website created in this account is the same website that is created in the **Windows Azure Training Kit** [Building and Publishing ASP.NET Applications with Windows Azure Web Sites and Visual Studio 2012](https://github.com/WindowsAzure-TrainingKit/HOL-ASPNETAzureWebSitesVS2012) lab, but with one exception.  The database will be stored in a full SQL Server 2012 Express instance rather than LocalDB. 

<a name="Exercise1Task1"></a>
#### Task 1 – Opening the MVC Sample Website in Visual Studio 2013 ####

<!-- TODO: FIX DOWNLOAD LINK -->
1. Download the lab files [Lab Files]( https://github.com/BretStateham/AzureDay-ScenarioLabs/archive/master.zip) from https://github.com/BretStateham/AzureDay-ScenarioLabs/archive/master.zip and save the file to the desktop in your development workstation VM.

2. Right click on the downloaded .zip file and select "Properties".

3. In the properites window, click the "**Unblock**" button and click "**OK**".

	![Download Properties](images/download-properties.png?raw=true)

	_Download Properties_

4. Right click on the downloaded .zip file again, and select "Extract All..."

5. In the "Extract Compressed (Zipped) Folders" dialog, under "**Files will be extracted to this folder:**" ***type*** "**C:\Labs**" as the target folder.  If folder doesn't currently exist, but will be created as part of the extraction.   

	![Extract Labs](images/extract-labs.png?raw=true)

	_Extract labs to **C:\Labs**_

6. Open **Microsoft Visual Studio Ultimate 2013 RC** and click the **Open Project** link in the start page, or from the menu bar select **File** | **New** | **Project/Solution...**. Browse to "C:\Labs\AzureDay-ScenarioLabs-master\Lab-1-UpdatingLab\AzureStorageLab\Assets\MVC4SampleWeb" and open the MVC4SampleWeb.sln solution file.  

	![Open the Starter Web Project](images/open-the-starter-web-project.png?raw=true)

	_Open the starter web project_	

7. Debug the application in Internet Explorer by clicking on the debug button on the toolbar:

	![Debug the application in the browser](images/debug-the-application-in-the-browser.png?raw=true)

	_Debug the sample web site in Internet Explorer_


8. In the address bar, go to the **/Customer** view for the site (***http://localhost:&lt;port&gt;/Customer***), and click the "**Create New**" link.  

	![Customer View](images/customer-view.png?raw=true)

	_Customer View_

	>Note: The sample application includes an MVC Customer model, controller and view.  The model saves it's data in a SQL Express database named MVC4Sample.  If the MVC4Sample database or Customer table don't already exist, they will be created by the model on the fly. 

9. Complete the "**Create**" form to create a new customer with sample data, and click the "**Create**" button.

	![Create Customer](images/create-customer.png?raw=true)
	
	_Create a new customer_

10. Verify that the new customer data was saved correctly.  
	
	![Saved Customer](images/saved-customer.png?raw=true)

	_Verify the saved customer_

11. Close the browser window to stop the debug session.

>Note: At this point, we have verified that the site is working correctly, and that SQL Server Express is successfully installed.

---

<a name="Exercise2"></a>
### Exercise 2: Creating a Windows Azure Storage Account ###

In this exercise, you will create a new Windows Azure Storage account in the Windows Azure Management Portal.  We will then create a container in the Blob storage as a repository for the website's images.  


<a name="Exercise2Task1"></a>
#### Task 1 – Creating the Azure Storage Account ####

1. Ensure that you are connected via remote desktop to your development virtual machine.  

1. Open the [Windows Azure Management Portal](https://manage.windowsazure.com) (https://manage.windowsazure.com) in the browser, and login with your credentials.

1. Click on "**STORAGE**" along the left had side

	![Storage Icon](images/storage-icon.png?raw=true)

	_Storage_

1. Click the "+ NEW" button in the lower left corner

	![New Icon](images/new-icon.png?raw=true)

	_Click New_

1. Click "**DATA SERVICES**" | "**STORAGE**" | "**QUICK CREATE**".  
	- Give your storage account a unique name.  Ensure that the green circle with a checkmark appears.
	- Choose an appropriate "**LOCATION/AFFINITY GROUP**" for your storage account
	- If you have more than one subscription, choose the "**SUBSCRIPTION**" the storage account should be created in
	- Leave the "**Enable Geo-Replication**" box CHECKED
	- Click the "**CREATE STORAGE ACCOUNT**"

	![Storage Account Creation](images/storage-account-creation.png?raw=true)

	_Storage Account Creation_

	>Note: Your storage account name must be globally unique. Enter a valid host name (no spaces or special characters) that isn't used anywhere else.  The storage account name creates the hostname portion of the fully qualified _**&lt;accountname&gt;.*.core.windows.net**_ URLs for the storage services endpoints.  If you have entered a valid name, a green circle with a white checkmark will appear.  

1. Once the storage account has been successfully created (it should only take a couple of minutes at most), you should see a notification along the bottom of the browser window. Once you have viewed the notification, click "**OK**" to continue.

	![Storage Account Creation Notification](images/storage-account-creation-notification.png?raw=true)

	_Storage Account Created Successfully_

<a name="Exercise2Task2"></a>
#### Task 2 – Create a Blob Storage Container for Public Access ####

1. Back on the "STORAGE" page in the Windows Azure Management Portal, click the name of your newly created storage account

	![Click New Storage Account](images/click-new-storage-account.png?raw=true)

	_Open the new storage account_

1. Click on the "**CONTAINERS**" link along the top to view the current containers in the account's blob storage, the click "**CREATE A CONTAINER**"...

	![Blob Containers](images/blob-containers.png?raw=true)

	_Blob Storage Containers_

1. For the new container set the:
	- "**NAME**" to "**images**"
	- "**ACCESS**" to "**Public Blob**"
	- If you hover over the ![Question Mark](images/question-mark.png?raw=true "Question Mark") next to "**ACCESS**" you can read the differences between the "Private","Public Blob", and "Public Container" access levels.
	- Click the ![Check Mark OK](images/check-mark-ok.png?raw=true) button to create the new container.

	![Create Images Container](images/create-images-container.png?raw=true)

	_Create the "images" container_

1. Verify that the new container was created properly, and note the fully qualified URL for it.  

	![Image Container Details](images/image-container-details.png?raw=true)

	_Image Container Details_

	>Note: At this point, you have successfully created a new storage account in your Windows Azure Subscription, and provisioned a blob storage container that allows public access to the blobs in it.  In the next exercise, we will upload the images for our website into the blob storage "images" container, and modify the website's CSS to reference the images in blob storage. 

<a name="Exercise2Task3"></a>
#### Task 3 – Configure Visual Studio for Storage Account Management ####

1. With the [Windows Azure Management Portal](https://manage.windowsazure.com/) still open, change to the "**DASHBOARD**" page for the new storage account we just created, and click the "**MANAGE ACCESS KEYS**" link along the bottom. 

	![Storage Account Dashboard](images/storage-account-dashboard.png?raw=true)

	_Storage Account Dashboard_

1. In the "**Manage Access Keys**" window, click the ![Copy Icon](images/copy-icon.png?raw=true) icon to the right of the "**PRIMARY ACCESS KEY**" value.

	![Copy Primary Access Key](images/copy-primary-access-key.png?raw=true)

	_Copy the Primary Access Key to the Clipboard_

1. If you are prompted to allow access to the clipboard, click "**Allow access**"

	![Allow Clipboard Access](images/allow-clipboard-access.png?raw=true)

	_Allow Clipboard Access_


1. Return to Visual Studio Ultimate 2013 RC.  From the menu bar, select "**View**" | "**Server Explorer**" (or use the keybaord combination: **Ctrl+W,L**).  Expand "**Windows Azure**" | "**Storage**".

	![Azure Storage In Server Explorer](images/azure-storage-in-server-explorer.png?raw=true)

	_Azure Storage In Server Explorer_

1. Right click the "**Storage**" node, and select "**Attach External Storage...**" 

	![Attach External Storage](images/attach-external-storage.png?raw=true)

	_Attach External Storage_

1. In the "Add New Storage Account" window, enter the "**Account name**" of your storage account (created previously), and for the "**Account key**" paste in the primary access key we just copied from the portal.  Leave all other settings at their default values, and click "OK.

	![Add New Storage Account](images/add-new-storage-account.png?raw=true)

	_Add Your Storage Account_

	For example: 

	![Add Storage Account Example](images/add-storage-account-example.png?raw=true)

	_Example Account_

1. Back in the Visual Studio "Server Explorer" window, you can now expand the node for the storage account you just added, then "**Blobs**" | "**images**".  Double click on the "**images**" container to open it in Visual Studio.  The container should be empty (for now). 

	![Empty Image Container](images/empty-image-container.png?raw=true)

	_Empty "images" Container_

1. On the "**images [Container]**" tab's toolbar, click the ![Upload Icon](images/upload-icon.png?raw=true) button to upload images into the container. 

	![Upload Icon In Container](images/upload-icon-in-container.png?raw=true)

	_Upload Button_

1. Browse to the "**Assets\Alternate Images**" folder we viewed previously, select **ALL** of the files, and click "**OK**".



---

<a name="Exercise3"></a>
### Exercise 3: Uploading Web Site Assets to Azure Storage ###

In this exercise, you will create a new Windows Azure Storage account in the Windows Azure Management Portal.  We will then create a container in the Blob storage as a repository for the website's images.  


<a name="Exercise3Task1"></a>
#### Task 1 – View the existing images ####

1. If needed, open the sample website in Visual Studio Ultimate 2013 RC.

1. In the "Solution Explorer" window, expand the "**MVC4SampleWeb**" project, right-click the "**Images**" folder, and select "**Open Folder in File Explorer**"

	![Open Images In Explorer](images/open-images-in-explorer.png?raw=true)

	_Open the Images folder in the Windows File Explorer_

1. In the Windows Explorer window, click the "**View**" menu, and change the view to "**Large icons**" to view the images in the folder:

	![Default Site Images](images/default-site-images.png?raw=true)

	_Default web site images_

	>Note: The images in this folder are the default images used by the sample website.  

1. In the Windows Explorer Window, change to the "**text here**Assets/AlternateImages**" folder for the lab to view a set of images that are similar to the originals, but have different colors, soon we will upload these images to blob storage, and modify the website to point to them instead of the default images.  The change in color will help to verify that the images in blob storage are being used:

	![Alternate Web Site Images](images/alternate-web-site-images.png?raw=true)

	_Alternate Web Site Images_

<a name="Exercise3Task2"></a>
#### Task 1 – Upload the Alternate Images to Blob Storage####


	
---
<a name="Summary"></a>
## Summary ##
In this hands-on lab, you have created a new MVC web site using MVC 4 Scaffolding and published it to Windows Azure Web Sites. Web site publication and deployment has never been easier in Windows Azure. Using familiar tools such as Web Deploy or Git, and virtually no changes to the development workflow, Windows Azure Web Sites is the next step in the Microsoft Azure platform for web developers. 
