<a name="Title"></a>
# Migrating Database Workloads To The Cloud	 #

---
<a name="Overview"></a>
## Overview ##

One of the biggest trends in IT today is moving database workloads into the cloud. The main factors driving this trend include cost, compatibility, flexibility, maintenance, compliance and scale - to name a few.

### High Level Options ###
Figure 1 below highlights some of the options available to you. There are at least four separate ways you could leverage SQL Server.

Notice that there is the concept of cloud-based solutions versus on premises solutions. Note that the top half of figure 1 describes the public cloud. If you drill further into the public cloud, you will notice that it consists of two main pillars: (1) Infrastructure as a Service - IaaS; (2) Platform as a Service – PaaS. The bottom half of the figure 1 illustrates the options with respect to on premises SQL Server. Notice that one of those options is hosting your own private cloud. The second option to running SQL Server on premises is the traditional method, which is running SQL Server on raw, physical hardware, free of any virtualization technology. 

![High Level Diagram](Images/highlevel.png?raw=true "High Level Diagram")

_Figure 1: SQL Server Migration Options_

<a name="Objectives"></a>
### Objectives ###

In this hands-on lab, you will learn how to:

- Migrate an on-premises SQL Server database to **Windows Azure SQL Virtual Machines**
- Migrate an on-premises SQL Server database to **Windows Azure SQL Database**

<a name="Prerequisites"></a>
### Prerequisites ###

The following is required to complete this hands-on lab:

- A Windows Azure subscription
- Either a virtual machine or physical machine that has SQL Server 2012 Express database 
- SQL Server 2012 Express Management Studio

> **Note:** You can also complete the lab **Setting up an Azure Virtual Machine For Developers with Visual Studio 2013 Ultimate and SQL Server 2012 Express** to create the environment.
<a name="Setup"></a>

### Setup ###

In order to execute the exercises in this hands-on lab you need to set up your environment.

1. Start by logging into the **Windows Azure Portal** (http://manage.windowsazure.com).


---
<a name="Exercises"></a>
## Exercises ##

This hands-on lab includes the following exercises:

- [Getting Started: Creating an Azure Virtual Machine using the Windows Azure Virtual Image Gallery](#GettingStarted)
- [Exercise 1: Downloading and installing SQL Server 2012 Express](#Exercise1)

<a name="GettingStarted"></a>
### Getting Started: Creating an MVC 4 Application using Entity Framework Code First ###

In this section, you will log into the Windows Azure Portal and create an Azure Virtual Machine using the Windows Azure Gallery. 

<a name="GettingStartedTask1"></a>
#### Task 1 – Creating an Azure Virtual Machine using the Windows Azure Virtual Image Gallery ####

1. x 

	![This lab plays an important role for the remaining 3 labs. This lab is all about establishing a storage account. This storage account will be used to store a bacpac file. The bacpac file will be a compressed database file. We will create this file using an on premises SQL Server database.](Images/x?raw=true)

	_This lab plays an important role for the remaining 3 labs. This lab is all about establishing a storage account. This storage account will be used to store a bacpac file. The bacpac file will be a compressed database file. We will create this file using an on premises SQL Server database._

1. Login into the **Windows Azure Portal**. On the left menu pane, select **Storage**. In the lower left corner select **+New** to create a new storage account. 

	![Creating a storage account](Images/Image001.png?raw=true)

	_Creating a storage account_

1. From the screen below, select **Storage | Quick Create**. 

	![Using Quick Create](Images/Image002.png?raw=true)

	_Using Quick Create_

1. Storage accounts are publicly exposed REST endpoints. This means that content can be accessed with ordinary http access. Note that in the screen below we need to provide the first segment of the **URL**. Notice that we choose **bacpackincloud**. You will need to choose your own **URL**. Naturally, files can also be made private, accessible with only a security key. Each storage account can contain one or more containers. Inside of each container is where files can be placed. These files are also known as **Blobs**. In the next exercise we will export the on-premises database as a **bacpac file** as a **blob,** inside a **container,** inside a **storage account**. In summary, you will provide 2 things here: (1) **URL**; (2) Data Center **Location**.  Finally, hit the **Create Storage Account** button on the lower right. 

	![Providing Storage Account Details](Images/Image003.png?raw=true)

	_Providing Storage Account Details_

1. It may take a few moments for the storage account to finish completion. 

	![Waiting for the Storage Account to complete](Images/Image004.png?raw=true)

	_Waiting for the Storage Account to complete_

1. At the bottom of the screen you will see a button called, **Manage Access Keys**. You will then be provided a popup dialog box where you can choose to copy the **management key** to the clipboard. You will need the **management key** for a future step, so you may want to copy it to a text file somewhere for future reference. After you've copied it, click the **checkmark** to dismiss the dialog box. 

	![Getting the Management Access Keys](Images/Image005.png?raw=true)

	_Getting the Management Access Keys_

1. Now that we've create a storage account, we have 2 vital pieces of information - the Account Storage Name and the Primary Access Key. You will need these pieces of data for future labs. 

	![Summary](Images/x?raw=true)

	_Summary_

