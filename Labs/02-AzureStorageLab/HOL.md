<a name="Title"></a>
# Migrating a Website's Assets to AzureStorage #

---
<a name="Scenario"></a>
## Scenario ##

In this scenario, assume you already have a website running on premise, but to increase the availability of your site's assets, or to reduce the traffic against your own bandwidth, you want to move them into the cloud.

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
<!-- TODO: FIX DOWNLOAD LINK -->
- A Windows Azure subscription -  [sign up for a free trial](http://aka.ms/WATK-FreeTrial) 
- **Either** Completing the **"01-SetupVS2013SQLVM"** lab to create a Windows Azure Virtual Machine for use as your development environment.  
- **OR** A development workstation with **Visual Studio 2013 Ultimate RC** and **SQL Server 2012 Express** installed. 
- Download of the [Lab Files](http://aka.ms/BLabs)

<a name="Setup"></a>
### Setup ###

The instructions in the lab assume you are remoted into the development Virtual Machine created during the **"01-SetupVS2013SQLVM"** lab.  If you prefer can follow the exact same steps on a Windows machine with **Visual Studio 2013 Ultimate RC** and **SQL Server 2012 Express** installed, however the instructions assume the environment created in the setup lab.

1. Open a **Remote Desktop Connection** to the development Virtual Machine you created in the **"01-SetupVS2013SQLVM"** lab. 

<!--
========================================
Exercise 1
========================================
-->

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

>**Note:** The website created in this account is the same website that is created in the **Windows Azure Training Kit** [Building and Publishing ASP.NET Applications with Windows Azure Web Sites and Visual Studio 2012](https://github.com/WindowsAzure-TrainingKit/HOL-ASPNETAzureWebSitesVS2012) lab, but with one exception.  The database will be stored in a full SQL Server 2012 Express instance rather than LocalDB. 

<a name="Exercise1Task1"></a>
#### Task 1 – Opening the MVC Sample Website in Visual Studio 2013 ####

<!-- TODO: FIX DOWNLOAD LINK -->
1. Open a Remote Desktop Connection to the development Virtual Machine. 

1. Open Internet Explorer and download the lab files **[Lab Files](http://aka.ms/BLabs)** from **http://aka.ms/BLabs** and save the file to a folder in the development workstation VM.

2. Locate the downloaded .zip file, **right click** on it, and select **"Properties"**.

3. In the properites window, click the "**Unblock**" button and click "**OK**".

	![Download Properties](images/01-download-properties.png?raw=true)

	_Unblock the downloaded .zip file_

	> **Note:** Unblocking the .zip file before you extract will keep Visual Studio from showing a security warning each time you open a project in the .zip file. 

4. **Right click** on the downloaded .zip file again, and select **"Extract All..."**

5. In the **"Extract Compressed (Zipped) Folders"** dialog, under **"Files will be extracted to this folder:"** ***type*** "**C:\Labs**" as the target folder.  The folder doesn't currently exist, but will be created as part of the extraction.   

	![Extract Labs](images/02-extract-labs.png?raw=true)

	_Extract labs to **C:\Labs**_

6. Open **Microsoft Visual Studio Ultimate 2013 RC** and click the **Open Project** link in the start page, or from the menu bar select **File** | **Open** | **Project/Solution...**. Browse to **"C:\Labs\AzureDay-ScenarioLabs-master\Labs\02-AzureStorageLab\Source\MVC4Sample.Web"** and open the **MVC4Sample.Web.sln** solution file.  

	![Open the Starter Web Project](images/03-open-the-starter-web-project.png?raw=true)

	_Open the starter web project_	

7. **Debug the application in Internet Explorer** by clicking on the debug button on the toolbar:

	![Debug the application in the browser](images/04-debug-the-application-in-the-browser.png?raw=true)

	_Debug the sample web site in Internet Explorer_


8. In the address bar, go to the **/Customer** view for the site (***http://localhost:&lt;port&gt;/Customer***), and click the "**Create New**" link.  

	![Customer View](images/05-customer-view.png?raw=true)

	_Customer View_

	>Note: The sample application includes an MVC Customer model, controller and view.  The model saves it's data in a SQL Express database named MVC4Sample.  If the MVC4Sample database or Customer table don't already exist, they will be created by the model on the fly. 

9. Complete the "**Create**" form to create a new customer with sample data, and click the "**Create**" button.

	![Create Customer](images/06-create-customer.png?raw=true)
	
	_Create a new customer_

10. Verify that the new customer data was saved correctly.  
	
	![Saved Customer](images/07-saved-customer.png?raw=true)

	_Verify the saved customer_

11. Close the browser window to stop the debug session.

>Note: At this point, we have verified that the site is working correctly, and that SQL Server Express is successfully installed.


<!--
========================================
Exercise 2
========================================
-->

---
<a name="Exercise2"></a>
### Exercise 2: Creating a Windows Azure Storage Account ###

In this exercise, you will create a new Windows Azure Storage account in the Windows Azure Management Portal.  We will then create a container in the Blob storage as a repository for the website's images.  


<a name="Exercise2Task1"></a>
#### Task 1 – Creating the Azure Storage Account ####

1. Ensure that you are connected via remote desktop to your development virtual machine.  

1. Open the [Windows Azure Management Portal](https://manage.windowsazure.com) (https://manage.windowsazure.com) in the browser, and login with your credentials.

1. Click on "**STORAGE**" along the left had side

	![Storage Icon](images/08-storage-icon.png?raw=true)

	_Storage_

1. Click the "+ NEW" button in the lower left corner

	![New Icon](images/09-new-icon.png?raw=true)

	_Click New_

1. Click "**DATA SERVICES**" | "**STORAGE**" | "**QUICK CREATE**".  
	- Give your storage account a unique name.  Ensure that the green circle with a checkmark appears.
	- Choose an appropriate "**LOCATION/AFFINITY GROUP**" for your storage account
	- If you have more than one subscription, choose the "**SUBSCRIPTION**" the storage account should be created in
	- Leave the "**Enable Geo-Replication**" box CHECKED
	- Click the "**CREATE STORAGE ACCOUNT**"

	![Storage Account Creation](images/10-storage-account-creation.png?raw=true)

	_Storage Account Creation_

	>Note: Your storage account name must be globally unique. Enter a valid host name (no spaces or special characters) that isn't used anywhere else.  The storage account name creates the hostname portion of the fully qualified _**&lt;accountname&gt;.*.core.windows.net**_ URLs for the storage services endpoints.  If you have entered a valid name, a green circle with a white checkmark will appear.  

1. Once the storage account has been successfully created (it should only take a couple of minutes at most), you should see a notification along the bottom of the browser window. Once you have viewed the notification, click "**OK**" to continue.

	![Storage Account Creation Notification](images/11-storage-account-creation-notification.png?raw=true)

	_Storage Account Created Successfully_

<a name="Exercise2Task2"></a>
#### Task 2 – Create a Blob Storage Container for Public Access ####

1. Back on the "STORAGE" page in the Windows Azure Management Portal, click the name of your newly created storage account

	![Click New Storage Account](images/12-click-new-storage-account.png?raw=true)

	_Open the new storage account_

1. Click on the "**CONTAINERS**" link along the top to view the current containers in the account's blob storage, the click "**CREATE A CONTAINER**"...

	![Blob Containers](images/13-blob-containers.png?raw=true)

	_Blob Storage Containers_

1. For the new container set the:
	- "**NAME**" to "**images**"
	- "**ACCESS**" to "**Public Blob**"
	- If you hover over the ![Question Mark](images/14-question-mark.png?raw=true "Question Mark") next to "**ACCESS**" you can read the differences between the "Private","Public Blob", and "Public Container" access levels.
	- Click the ![Check Mark OK](images/15-check-mark-ok.png?raw=true) button to create the new container.

	![Create Images Container](images/16-create-images-container.png?raw=true)

	_Create the "images" container_

1. Verify that the new container was created properly, and note the fully qualified URL for it.  

	![Image Container Details](images/17-image-container-details.png?raw=true)

	_Image Container Details_

	>Note: At this point, you have successfully created a new storage account in your Windows Azure Subscription, and provisioned a blob storage container that allows public access to the blobs in it.  In the next exercise, we will upload the images for our website into the blob storage "images" container, and modify the website's CSS to reference the images in blob storage. 

<a name="Exercise2Task3"></a>
#### Task 3 – Configure Visual Studio for Storage Account Management ####

1. With the [Windows Azure Management Portal](https://manage.windowsazure.com/) still open, change to the "**DASHBOARD**" page for the new storage account we just created, and click the "**MANAGE ACCESS KEYS**" link along the bottom. 

	![Storage Account Dashboard](images/18-storage-account-dashboard.png?raw=true)

	_Storage Account Dashboard_

1. In the "**Manage Access Keys**" window, click the ![Copy Icon](images/19-copy-icon.png?raw=true) icon to the right of the "**PRIMARY ACCESS KEY**" value.

	![Copy Primary Access Key](images/20-copy-primary-access-key.png?raw=true)

	_Copy the Primary Access Key to the Clipboard_

1. If you are prompted to allow access to the clipboard, click "**Allow access**"

	![Allow Clipboard Access](images/21-allow-clipboard-access.png?raw=true)

	_Allow Clipboard Access_


1. Return to Visual Studio Ultimate 2013 RC.  From the menu bar, select "**View**" | "**Server Explorer**" (or use the keybaord combination: **Ctrl+W,L**).  Expand "**Windows Azure**" | "**Storage**".

	![Azure Storage In Server Explorer](images/22-azure-storage-in-server-explorer.png?raw=true)

	_Azure Storage In Server Explorer_

1. Right click the "**Storage**" node, and select "**Attach External Storage...**" 

	![Attach External Storage](images/23-attach-external-storage.png?raw=true)

	_Attach External Storage_

1. In the "Add New Storage Account" window, enter the "**Account name**" of your storage account (created previously), and for the "**Account key**" paste in the primary access key we just copied from the portal.  Leave all other settings at their default values, and click "OK.

	![Add New Storage Account](images/24-add-new-storage-account.png?raw=true)

	_Add Your Storage Account_

	For example: 

	![Add Storage Account Example](images/25-add-storage-account-example.png?raw=true)

	_Example Account_

1. Back in the Visual Studio "Server Explorer" window, you can now expand the node for the storage account you just added, then "**Blobs**" | "**images**".  Double click on the "**images**" container to open it in Visual Studio.  The container should be empty (for now). 

	![Empty Image Container](images/26-empty-image-container.png?raw=true)

	_Empty "images" Container_


>Note: We now have a Windows Azure Storage account, and a blob container that can store the images for our site.  In the next exercise, we'll upload images to the container and modify the website to reference the images in the cloud rather than locally. 

<!--
========================================
Exercise 3
========================================
-->

---
<a name="Exercise3"></a>
### Exercise 3: Uploading Web Site Assets to Azure Storage ###

In this exercise, you will create a new Windows Azure Storage account in the Windows Azure Management Portal.  We will then create a container in the Blob storage as a repository for the website's images.  


<a name="Exercise3Task1"></a>
#### Task 1 – View the existing images ####

1. If needed, open the sample website in Visual Studio Ultimate 2013 RC.

1. In the "Solution Explorer" window, expand the "**MVC4SampleWeb**" project, right-click the "**Images**" folder, and select "**Open Folder in File Explorer**"

	![Open Images In Explorer](images/27-open-images-in-explorer.png?raw=true)

	_Open the Images folder in the Windows File Explorer_

1. In the Windows Explorer window, click the "**View**" menu, and change the view to "**Large icons**" to view the images in the folder:

	![Default Site Images](images/28-default-site-images.png?raw=true)

	_Default web site images_

	>Note: The images in this folder are the default images used by the sample website.  

1. In the Windows Explorer Window, change to the "**text here**Assets/AlternateImages**" folder for the lab to view a set of images that are similar to the originals, but have different colors, soon we will upload these images to blob storage, and modify the website to point to them instead of the default images.  The change in color will help to verify that the images in blob storage are being used:

	![Alternate Web Site Images](images/29-alternate-web-site-images.png?raw=true)

	_Alternate Web Site Images_

<a name="Exercise3Task2"></a>
#### Task 2 – Upload the Alternate Images to Blob Storage####

1. Return to Visaul Studio, and in the Server Explorer Window, esure you are in the "Windows Azure" | "On the "**images [Container]**" tab's toolbar, click the ![Upload Icon](images/30-upload-icon.png?raw=true) button to upload images into the container. 

	![Upload Icon In Container](images/31-upload-icon-in-container.png?raw=true)

	_Upload Button_

1. Browse to the "**Assets\Alternate Images**" folder we viewed previously, select **ALL** of the files, and click "**Open**".

	![Upload Alternate Images](images/32-upload-alternate-images.png?raw=true)

	_Upload Alternate Images to Blob Storage_

1. While the images are being uploaded, you can view the progress in Visual Studio's "**Windows Azure Activity Log**"

	![Windows Azure Activity Log During Upload](images/33-windows-azure-activity-log-during-upload.png?raw=true)

	_Window Azure Activity Log Showing Upload Progress_

1. Once the upload has completed, you can see the blobs now in the image container.  If you expand the "URL column", you can view the entire URL to the image. Right click on any of the images, (for example orderedList0.png) and select "**Copy URL**"

	![Copy URL for Image](images/34-copy-url-for-image.png?raw=true)

	_Copy the URL for an Image_

1. Because we marked the container with "Public Blob" permissions, anybody, anywhere in the world, can access the blobs in the container.  To test that out, open your browser, and paste the URL we just copied above, into the address bar.  Verify that the image appears in the browser.  

	![Image From Plub Blob Container](images/35-image-from-plub-blob-container.png?raw=true)

	_Image Loaded From Public Blob Conatiner_

	>Note: Now that we have images in blob storage in the cloud, and we know we can access them publicly, we can now modify the website to reference those images rather than the local images

1. In the Visual Studio "Solution Explorer" window, expand the "**MVC4Sample.Web**" | "**Content**" folder, and double click the "**Site.css**" file to open it in the editor, and scroll to **Line 108** to see the "**background**" property of the "**.main-content**" style

	![Main Content Style](images/36-main-content-style.png?raw=true)

	_Main Content Style_

	>Note: To reference our new images in the cloud, we want to modify all "**../Images/**..." urls in the **Site.css** file to point to our "**image**" container in blob storage, "**http://*your_account_name*.blob.core.windows.net/images/**...".  The quickest way to do that is to let Visual Studio do the work for us.  

1. From the Visual Studio menu bar, select "**EDIT**" | "**Find and Replace**" | "**Quick Replace**", (or just press **Ctrl+H**). Enter "**../Images/**" into the top box of the quick replace window, and enter your image container url (the URL we copied above, just without the image file name, keep the trailing "/" though).

	![Quick Replace Window](images/37-quick-replace-window.png?raw=true)

	_Quick Replace Window_

	For example:

	![Quick Replace Example](images/38-quick-replace-example.png?raw=true)

	_Quick Replace Example_

	>Note: If you make a mistake, just close the Site.css file and don't save the changes.  You can then open it back up and try again.  Of couse "**EDIT**" | "**Undo**" (or **Ctrl+Z**) is an option as well. 

1. Click the "Replace All" (![Replace All Button](images/39-replace-all-button.png?raw=true)) button to replace all the "**../Images/**" occurrances with your container url.  Click "**OK**" to confirm the "**13 occurrence(s) replaced.**"

	![Replace URLs](images/40-replace-urls.png?raw=true)

	_"**../Images/**" URLs Replaced_

1. From the Visual Studio toolbar, click the "**Debug in Internet Explorer**" button to open the site in the browser

	![Debug in IE](images/41-debug-in-ie.png?raw=true)

	_Debug in Internet Explorer_

1. Verify that the images are being retrieved from blob storage in the cloud rather than locally.  You can tell easily by the green color in the images.  If you still see the old black images, not the green ones, try hitting the refresh button in the browser:

	![Alternate Images Being Loaded From Cloud](images/42-alternate-images-being-loaded-from-cloud.png?raw=true)

	_Website images are being retrieved from the cloud_


	
---
<a name="Summary"></a>
## Summary ##
In this hands-on lab, you first verified the functionality of an existing MVC 4 website, **MVC4SampleWeb**. We then created a Windows Azure Storage account in the cloud, and provisioned a public container to store the website images in, and finally uploaded our website images into the container and modified the website to retrieve its images from the cloud.  

This lab helps to serve as an example then of how you can leverage a portion of the Windows Azure platform (Windows Azure Storage in this case) while keeping the remainder of a legacy site untouched.  

This concept could be extended to: 

- Leveraging Windows Azure SQL Database or Table Storage to host your web site's data in the cloud, 
- Using Windows Azure Active Directory and Access Control to authenticate your website users
- Useing Windows Azure Media Services to stream video
- And much more!  

