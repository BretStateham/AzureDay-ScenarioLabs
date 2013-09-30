<a name="Title"></a>
# DevTest on Windows Azure #

---
<a name="Overview"></a>
## Overview ##

<a name="Objectives"></a>
### Objectives ###

In this hands-on lab, you will learn how to:

- Create a Team Foundation Service Account
- Create a Team Foundation Service Project
- Add a Visual Studio 2013 RC project to Team Foundation Service
- Use Team Foundation Service for Continuous Deployment to Windows Azure
- Create and run unit tests using Team Foundation Service
- Create and run load tests using Team Foundation Service

<a name="Prerequisites"></a>
### Prerequisities ###

The following is required to complete this hands-on lab:
- A Windows Azure subscription - [sign up for a free trial](http://aka.ms/thecloud)
- A Windows Azure virtual machine with Visual Studio 2013 RC - [instructions](https://github.com/agrocholski/VS2013OnAzure-HOL)

---
<a name="Exercises"></a>
## Exercises ##

This hands-on lab includes the following exercises:
- [Exercise 1: Setting up Team Foundation Service](#Exercise1)
- [Exercise 2: Continuous Deployment](#Exercise2)
- [Exercies 3: Unit Testing](#Exercise3)
- [Exercise 4: Load Testing](#Exercise4)

<a name="Exercise1"></a>
### Exercise 1: Setting up Team Foundation Service ###

In this exercise you will create a Team Foundation Service account and set up your first Team Foundation Service project.

<a name="Ex1Task1"></a>
#### Task 1: Creating a Team Foundation Service Account ####

1. Open Internet Explorer and go to the [Team Foundation Service site](https://tfs.visualstudio.com).

1. Click the **Sign up for free** button

	![Signing up for a Team Foundation Service account](Images/Ex1Task1-01-tfs-sign-up.png?raw=true "Signing up for a Team Foundation Service account")

	_Signing up for a Team Foundation Service account_

1. Enter the **Account URL** you would like to use for your Team Foundation Service account.

	![Creating a Team Foundation Service account](Images/Ex1Task1-02-tfs-account-creation.png?raw=true "Creating a Team Foundation Service account")

	_Creating a Team Foundation Service account_

1. After your account is created you will be redirected to a sign in page. Sign in with your Microsoft account.

	![Sign in with your Microsoft account](Images/Ex1Task1-03-tfs-sign-in.png?raw=true "Sign in with your Microsoft account")

	_Sign in with your Microsoft account_

1. Upon successful sign in you will be redirected to your the dashboard for your Team Foundation Service account.

	![Team Foundation Service dashboard](Images/Ex1Task1-04-tfs-dashboard.png?raw=true "Team Foundation Service dashboard")

	_Team Foundation Service dashboard_

<a name="Ex1Task2"></a>
#### Task 2: Creating a Team Foundation Service Project ####

If the dashboard for your Team Foundation Service account is still open from Task 1. You can skip ahead to step 3.

1. Open Internet Explorer and go to the [Team Foundation Service site](https://tfs.visualstudio.com).

1. Click the **Sign in** button.

	![Team Foundation Service sign in](Images/Ex1Task2-01-tfs-sign-in.png?raw=true "Team Foundation Service sign in")

	_Team Foundation Service sign in_

1. You will be redirected to a sign in page. Sign in with your Microsoft account.

	![Sign in with your Microsoft account](Images/Ex1Task2-02-msft-sign-in.png?raw=true "Sign in with your Microsoft account")

	_Sign in with your Microsoft account_

1. Upon successful sign in you will be redirected to a page that lists the Team Foundation Service accounts associated with your Microsoft account.

	![Team Foundation Service accounts](Images/Ex1Task2-03-tfs-accounts.png?raw=true "Team Foundation Service accounts")

	_Team Foundation Service accounts_

1. Click the link of the Team Foundation Service account you created earlier. This will redirect you to the dashbaord for teh account.

	![Team Foundation Service dashboard](Images/Ex1Task2-04-tfs-dashboard.png?raw=true "Team Foundation Service dashboard")

	_Team Foundation Service dashboard_

1. Click the **New team project** button

	![New team project](Images/Ex1Task2-05-new-team-project.png?raw=true "New team project")

	_New team project_

1. Enter a **Project name** and a **Description** for your new team project. Use the defaults for **Process template** and **Version control**. Click the **Create project** button.

	![Create new team project](Images/Ex1Task2-06-create-new-team-project.png?raw=true "Create new team project")

	_Create new team project_

1. After the project has been successfully created, click the **Navigate to project** button.

	![Project created](Images/Ex1Task2-07-project-created.png?raw=true "Project created")

	_Project created_

1. You will be redirected to the dashboard for your Team Foundation Service project.

	![Project dashboard](Images/Ex1Task2-08-project-dashboard.png?raw=true "Project dashboard")

	_Project dashboard_

<a name="Exercise2"></a>
### Exercise 2: Continuous Deployment ###

In this exercise you will create a new Windows Azure Web Site, use Visual Studio to update the Web Site, and automatically deploy changes using Team Foundation Service.

<a name="Ex2Task1"></a>
#### Task 1: Creating a Windows Azure Web Site ####

1. Open Internet Explorer and go to the [Windows Azure management portal](https://manage.windowsazure.com). Sign in using the Microsoft credentials associated with your subscription.

	![Log on to the Windows Azure management portal](Images/Ex2Task1-01-azure-sign-in.png?raw=true "Log on to the Windows Azure management portal")

	_Log on to the Windows Azure management portal_

1. Click **New** on the command bar

	![Creating a new Web Site](Images/Ex2Task1-02-new-web-site.png?raw=true "Creating a new Web Site")

	_Creating a new Web Site_

1. Click **Compute**, **Web Site** and then **Quick Create**. Provide an available URL for the new web site and click **Create Web Site**.

	![Creating a new Web Site using Quick Create](Images/Ex2Task1-03-create-web-site.png?raw=true "Creating a new Web Site using Quick Create")

	_Creating a new Web Site using Quick Create_

1. Wait until the new **Web Site** is created.

<a name="Ex2Task2"></a>
#### Task 2: Set Up Deployment from Team Foundation Service ####

1. Once the Web Site is created, click the **arrow** next to the name of the Web Site in the list of web sites.

	![Web Site list](Images/Ex2Task2-01-web-site-list.png?raw=true "Web Site list")

	_Web Site list_

1. This will redirect you to the **Quick Start** page for your site. Click the **DASHBOARD** link.

	![Web Site dashboard](Images/Ex2Task2-02-dashboard.png?raw=true "Web Site dashboard")

	_Web Site dashboard_

1. In the **Dashboard** page, under the **quick glance** section, click the **Set up deployment from source control** link.

	![Quick Glance](Images/Ex2Task2-03-quick-glance.png?raw=true "Quick Glance")

	_Quick Glance_

1. In the **Set up Deployment** wizard, select **Team Foundation Service** as the location for your source code, then click the **Next** button.

	![Source Code Location](Images/Ex2Task2-04-source-code-location.png?raw=true "Source Code Location")

	_Source Code Location_

1. Enter the **URL** to your Team Foundation Service account and click the **Authorize Now** button.

	![Authorize TFS Connection](Images/Ex2Task2-05-authorize-tfs.png?raw=true "Authorize TFS Connection")

	_Authorize TFS Connection_

1. Accept the connection request by clicking the **Accept** button.

	![Accept Connection Request](Images/Ex2Task2-06-accept.png?raw=true "Accept Connection Request")

	_Accept Connection Request_

1. After authorization succeeds, select the **Repository** that you want to use to manage source code for your Web Site. This should correspond to the Team Foundation Service project you create earlier. Finish by clicking the **Checkmark** button.

	![Choose Repository](Images/Ex2Task2-07-choose-repo.png?raw=true "Choose Repository")

	_Choose Repository_

1. Wait for the Team Foundation Service project to be linked to your Web Site.

	![Project Linked](Images/Ex2Task2-08-project-linked.png?raw=true "Project Linked")

	_Project Linked_

<a name="Ex2Task3"></a>
#### Task 3: Create a new ASP.NET MVC Project Connected to Team Foundation Service ####

1. Open Internet Explorer and go to the [Windows Azure management portal](https://manage.windowsazure.com). Sign in using the Microsoft credentials associated with your subscription.

	![Log on to the Windows Azure management portal](Images/Ex2Task3-01-azure-sign-in.png?raw=true "Log on to the Windows Azure management portal")

	_Log on to the Windows Azure management portal_

1. Go to the **Virtual Machine** portion of the portal and click on your virtual machine that is running Visual Studio 2013 Ultimate RC.

	![Select the Virtual Machine](Images/Ex2Task3-02-select-vm.png?raw=true "Select the Virtual Machine")

	_Select the Virtual Machine_

1. Click **Connect** on the command bar and log in to your virtual machine using the credentials you provided when creating the machine.

	![Connect to the Virtual Machine](Images/Ex2Task3-03-connect.png?raw=true "Connect to the Virtual Machine")

	_Connect to the Virtual Machine_

1. Once connected to the virtual machine, launch **Microsoft Visual Studio 2013 Ultimate RC**.

1. Select **File**, **New**, **Project**.

1. In the **New Project** dialog, expand the **Visual C#** node, select *Web*, select **ASP.NET Web Application**, enter a name for the project, specify a location on your machine, and make sure **Add to source control** is checked, and click **OK**.

	![Project Creation](Images/Ex2Task3-04-project-creation.png?raw=true "Project Creation")

	_Project Creation_

1. In the **New ASP.NET Project** dialog, select the **MVC** template, and check the **Add unit tests** checkbox. Click the **Create Project** button.

	![New ASP.NET Project](Images/Ex2Task3-05-aspnet-project.png?raw=true "New ASP.NET Project")

	_New ASP.NET Project_

1. In the **Connect to Team Foundation Server** dialog, click the **Servers** button.

	![Connect to Team Foundation Server](Images/Ex2Task3-06-connect-to-tfs.png?raw=true "New Connect to Team Foundation Server")

	_Connect to Team Foundation Server_

1. Click the **Add** button to add a new Team Foundation Server.

	![Add Server](Images/Ex2Task3-07-add-server.png?raw=true "Add Server")

	_Add Server_

1. Enter the **URL** of your Team Foundation Service account

	![Enter Team Foundation Service URL](Images/Ex2Task3-08-add-account.png?raw=true "Enter Team Foundation Service URL")

	_Enter Team Foundation Service URL_

1. Sign in using the Microsoft credentials associated with your Team Foundation Service account.

	![Sign in](Images/Ex2Task3-09-sign-in.png?raw=true "Sign in")

	_Sign in_

1. Once authentication is successful and the server has been added to the list of servers, click **Close**.

	![Server Added](Images/Ex2Task3-10-server-added.png?raw=true "Server Added")

	_Server Added_

1. Select your **Team Project** and click **Connect**.

	![Connect](Images/Ex2Task3-11-connect.png?raw=true "Connect")

	_Connect_

1. Keep the defaults when prompted to add the solution to source control and click **OK**.

	![Add to Source Control](Images/Ex2Task3-12-add-to-source-control.png?raw=true "Add to Source Control")

	_Add to Source Control_

1. Press **F5** to run the project.

	![Run the Project](Images/Ex2Task3-13-run-project.png?raw=true "Run the Project")

	_Run the Project_

<a name="Ex2Task4"></a>
#### Task 4: Deploy Changes using Team Foundation Service ####

1. In Internet Explorer, browse to your Team Foundation Service account (**https://[your account name].visualstudio.com**) and click on your project.

	![Select Project](Images/Ex2Task4-01-open-project.png?raw=true "Select Project")

	_Select Project_

1. On your project's dashboard, click the **Build** link.

	![Build](Images/Ex2Task4-02-build.png?raw=true "Build")

	_Build_

1. Open a second tab in Internet Explorer and navigate to the [Windows Azure management portal](https://manage.windowsazure.com). Sign in using the Microsoft credentials associated with your subscription.

1. Go to the **Web Sites** portion of the portal and click on your Web Site.

	![Web Site](Images/Ex2Task4-03-web-site.png?raw=true "Web Site")

	_Web Site_

1. Click on the **Dashboard** link, then clikc on the **Site URL** link in the **Quick Glance** portion of the dashboard.

	![Launch Web Site](Images/Ex2Task4-04-site-url.png?raw=true "Launch Web Site")

	_Launch Web Site_

1. Your Web Site should now be displayed in a separate tab in Internet Explorer.

	![Default Page](Images/Ex2Task4-05-default.png?raw=true "Default Page")

	_Default Page_

1. Go back to the tab in Interent Explorer containing the Windows Azure management portal and click the **Deployments** link.

	![Deployments](Images/Ex2Task4-06-deployments.png?raw=true "Deployments")

	_Deployments_

1. In the virtual machine running Visual Studio 2013 Ultimate RC, click **Pending Changes** in **Team Explorer**

	![Pending Changes](Images/Ex2Task4-07-pending-changes.png?raw=true "Pending Changes")

	_Pending Changes_

1. Enter a **Comment** and click the **Check In** button.

	![Check In](Images/Ex2Task4-08-check-in.png?raw=true "Check In")

	_Check In_

1. If prompted, confirm the check-in.

	![Confirm Check In](Images/Ex2Task4-09-confirm.png?raw=true "Confirm Check In")

	_Confirm Check In_

1. Once the check in completes, go to the tab in Internet Explorer that contains the **Build** dashboard for your Team Foundation Service project. Click **Queued**. A build for your project should display.

	![Queued Build](Images/Ex2Task4-10-queued-build.png?raw=true "Queued Build")

	_Queued Build_

1. After several minutes the build should complete. Click on the **Completed** link to see the results.

	![Completed Build](Images/Ex2Task4-11-completed-build.png?raw=true "Completed Build")

	_Completed Build_

1. Once the build completes it will automatically deploy to your Windows Azure Web Site. Click the **Deployed** link to see the deployment results.

	![Deployed Build](Images/Ex2Task4-12-deployed-build.png?raw=true "Deployed Build")

	_Deployed Build_

1. Go to the tab in Internet Explorer that contains the **Deployments** page for your Windows Azure Web Site. The Team Foundation Service deployment should display.

	![Deployed Build in Windows Azure](Images/Ex2Task4-13-deployed-build-azure.png?raw=true "Deployed Build in Windows Azure")

	_Deployed Build in Windows Azure_

1. Go to the tab in Internet Explorer that contains your Windows Azure Web Site and press **F5** to refresh the page. Your ASP.NET MVC application should display.

	![ASP.NET MVC Application Running in Windows Azure](Images/Ex2Task4-14-app-in-azure.png?raw=true "ASP.NET MVC Application Running in Windows Azure")

	_ASP.NET MVC Application Running in Windows Azure_

<a name="Exercise3"></a>
### Exercise 3: Unit Testing ###

In this exercise you will modify unit tests to leverage the continuous integration and deployment features of Team Foundation Service and Windows Azure.

<a name="Ex3Task1"></a>
#### Task 1: Creating a Failing Unit Test ####

1. In the virtual machine running Visual Studio Ultimate 2013 RC, open the **HomeControllTest.cs** file from **Solution Explorer**.

	![HomeControllerTest.cs](Images/Ex3Task1-01-home-controller.png?raw=true "HomeControllerTest.cs")

	_HomeControllerTest.cs_

1. Change the **Index()** method to the following:

	<!--mark:1-4-->
	````C#
	public void Index()
	{
		Assert.AreEqual(1, 0);
	}

1. In **Solution Explorer** righ-click your solution and click **Check In...**

	![Check In](Images/Ex3Task1-02-check-in.png?raw=true "Check In")

	_Check In_

1. Enter a **Comment** in Team Explorer and click the **Check In** button.

	![Check In from Team Explorer](Images/Ex3Task1-03-check-in.png?raw=true "Check In from Explorer")

	_Check In from Team Explorer_

1. If prompted, confirm the check-in.

1. Go back to the Internet Explorer tab that contains the **Build** dashboard for your Team Foundation Service project. Your build should appear as **Queued**.

	![Queued Build](Images/Ex3Task1-04-build-queued.png?raw=true "Queued Build")

	_Queued Build_

1. After a few minutes, the build should complete. Click the **Completed** link. The build should be listed as **Failed**.

	![Failed Build](Images/Ex3Task1-05-build-failed.png?raw=true "Failed Build")

	_Failed Build_

1. Double-click on the failed build and you can drill into the details. In this case you will see that the text for **Index** failed.

	![Failed Build Details](Images/Ex3Task1-06-build-failed-details.png?raw=true "Failed Build Details")

	_Failed Build Details_

1. Go back to the **Build** dashboard and click the **Deployed** link. The deployment failed as well.

	![Failed Deployment](Images/Ex3Task1-07-deployment-failed.png?raw=true "Failed Deployment")

	_Failed Deployment_

1. Go to the tab in Internet Explorer that contains the **Deployments** page for your Windows Azure Web Site. You should see the failed deployment here as well.

	![Failed Deployment](Images/Ex3Task1-08-deployment-failed-azure.png?raw=true "Failed Deployment")

	_Failed Deployment_

<a name="Ex3Task2"></a>
#### Task 2: Fix the Failing Unit Test ####

1. In the virtual machine running Visual Studio Ultimate 2013 RC, open the **HomeControllTest.cs** file from **Solution Explorer**.

	![HomeController.cs](Images/Ex3Task2-01-home-controller.png?raw=true "HomeController.cs")

	_HomeController.cs_

1. Change the **Index()** method to the following:

	<!--mark:1-4-->
	````C#
	public void Index()
	{
		Assert.AreEqual(1, 1);
	}

1. In **Solution Explorer** righ-click your solution and click **Check In...**

	![Check In](Images/Ex3Task2-02-check-in.png?raw=true "Check In")

	_Check In_

1. Enter a **Comment** in Team Explorer and click the **Check In** button.

	![Check In](Images/Ex3Task2-03-check-in.png?raw=true "Check In")

	_Check In_

1. If prompted, confirm the check-in.

1. Go back to the Internet Explorer tab that contains the **Build** dashboard for your Team Foundation Service project. Your build should appear as **Queued**.

	![Queued Build](Images/Ex3Task2-04-queued-build.png?raw=true "Queued Build")

	_Queued Build_

1. After a few minutes, the build should complete. Click the **Completed** link. The build should be listed as **Succeeded**.

	![Build Succeeded](Images/Ex3Task2-05-build-succeeded.png?raw=true "Build Succeeded")

	_Build Succeeded_

1. Click the **Deployed** link. The deployment succeeded as well.

	![Deployment Suceeded](Images/Ex3Task2-06-deployment-succeeded.png?raw=true "Deployment Suceeded")

	_Deployment Succeeded_

1. Go to the tab in Internet Explorer that contains the **Deployments** page for your Windows Azure Web Site. You should see the successful deployment here as well.

	![Deployment Succeeded](Images/Ex3Task2-07-deployment-succeeded-azure.png?raw=true "Deployment Succeeded")

	_Deployment Succeeded_

<a name="Exercise4"></a>
### Exercise 4: Load Test ###

In this exercise you will learn how to create and record a web performance test, create a load test, and run a load test using Team Foundation Service.

<a name="Ex4Task1"></a>
#### Task 1: Disable Enhanced Protected Mode in Internet Explorer ####

In order to record web performance tests in Internet Explorer, you need to disable Enhanced Protected Mode. The following steps will walk you through how to disable this feature. It is recommended at the end of this excercise that enable Enhanced Protected Mode.

1. Start **Internet Explorer** for the desktop.

1. Tap or click **Tools**, and then tap or click **Internet options**.

1. On the **Advanced** tab, clear teh **Enabled Enhanced Protected Mode** check box under **Security**.

4. Tap or click **OK**.

	![Disable Enhanced Protected Mode](Images/Ex4Task1-01-enhanced-protected-mode.png?raw=true "Disable Enhanced Protected Mode")

	_Disable Enhanced Protected Mode_

**Note** You must restart your browser for this setting to take effect.

<a name="Ex4Task2"></a>
#### Task 2: Create and Record a Web Performance Test ####

1. In the Interet Explorer navigate to the [Windows Azure management portal](https://manage.windowsazure.com), log in, go to the **Dashboard** for your Windows Azure Web Site. Copy your the **Site URL** from the **Quick Glance** portion of the dashboard.

	![Copy Site URL](Images/Ex4Task2-01-site-url.png?raw=true "Copy Site URL")

	_Copy Site URL_

1. In the virtual machine running Visual Studio 2013 Ultimate RC, right-click the solution in **Solution Explorer**, select **Add**, and select **New Project...**

	![Add New Project](Images/Ex4Task2-02-add-project.png?raw=true "Add New Project")

	_Add New Project_

1. In the **Add New Project** dialog, under **Visual C##** select **Test**, then select **Web Performance and Load Test Project**. Give the project a **Name** and click **OK**.

	![Add Web Performance and Load Test Project](Images/Ex4Task2-03-perf-load.png?raw=true "Add Web Performance and Load Test Project")

	_Add Web Performance and Load Test Project_

1. In Solution Explorer, right-click the new web performance and load test project, select **Add**, and select **Web Performance Test...**. Your web browser will open.

	![Add Web Performance Test](Images/Ex4Task2-04-add-web-perf-test.png?raw=true "Add Web Performance Test")

	_Add Web Performance Test_

1. You may be prompted to enable add-ons. Click **Choose add-ons**. Enable **Microsoft Web Test Recorder 12.0 Helper** and **Web Test Recorder 12.0** and click **Done**. The **Web Test Recorder** should now display in the browser.

	![Web Test Recorder](Images/Ex4Task2-05-web-test-recorder.png?raw=true "Web Test Recorder")

	_Web Test Recorder_

1. Enter the URL for your Windows Azure Web Site.

	![Enter Site URL](Images/Ex4Task2-06-site-url.png?raw=true "Enter Site URL")

	_Enter Site URL_

1. Start clicking through your Web Site to record HTTP requests and responses. Once you have a list of 15-20 items in the Web Test Record click **Stop**.

	![Stop Recording](Images/Ex4Task2-07-stop.png?raw=true "Stop Recording")

	_Stop Recording_

1. Visual Studio will look for dynamic parameters for teh HTTP responses to each of your HTTP requests. A progress bar is displayed while this happens. If dynamic properties are found, a table displays allowing you to assign constant values to each of the dynamic parameters.

1. Right-click on a test and select **Properties**. Change the **Response Time Goal (seconds)** property value to 1.

	![Web Test Properties](Images/Ex4Task2-08-properties.png?raw=true "Web Test Properties")

	_Web Test Properties_

1. Modify the properties of other tests in your recording and **Save**.

<a name="Ex4Task3"></a>
#### Task 3: Createa a Load Test ####

1. In **Solution Explorer**, right-click the **Web Peformance and Load Test** project, click **Add*, and click **Load Test...**

	![Add Load Test](Images/Ex4Task3-01-add-load-test.png?raw=true "Add Load Test")

	_Add Load Test_

1. When the **New Load Test Wizard** appears, choose the **Load Pattern** steop. Change the load patter to step load which gradually adds users over time.

	![Load Pattern](Images/Ex4Task3-02-load-pattern.png?raw=true "Load Pattern")

	_Load Pattern_

1. Choose the **Test Mix** step, click **Add**, move the test you recorded to the list of test runs. Click **OK** to dismiss the **Add Tests** dialog.

	![Test Mix](Images/Ex4Task3-03-test-mix.png?raw=true "Test Mix")

	_Test Mix_

1. Click **Finish** to complete the **New Load Test Wizard**. The web performance test is added to teh load test and appears in the load test editor.

	![Load Test Editor](Images/Ex4Task3-04-load-test-editor.png?raw=true "Load Test Editor")

	_Load Test Editor_

<a name="Ex4Task4"></a>
#### Task 4: Run a Load Test Using Team Foundation Service ####

1. In **Solution Explorer** expand the **Solution Items** folder and double-click the **Local.testsettings** file.

	![Local.testsettings](Images/Ex4Task4-01-test-settings.png?raw=true "Local.testsettings")

	_Local.testsettings_

1. In the **Test Settings** dialog, select **Run tests using Visual Studio Team Foundation Service**, click **Apply**, and click **Close**.

	![Use Team Foundation Service](Images/Ex4Task4-02-tfs.png?raw=true "Use Team Foundation Service")

	_Use Team Foundation Service_

1. In the **Load Test Editor**, click the **Run Load Test** button to run the load test using Team Foundation Service.

	![Run Load Test](Images/Ex4Task4-03-run.png?raw=true "Run Load Test")

	_Run Load Test_

1. You can see when your test is in the queue for the service. When the test is ready to be run, the status changes to **Acquiring Resources**.

	![Queued Run](Images/Ex4Task4-04-queued-run.png?raw=true "Queued Run")

	_Queued Run_

1. When the test is running, you can see the performance.

	![Test Performance](Images/Ex4Task4-05-performance.png?raw=true "Test Performance")

	_Test Performance_

1. You can also view the throughput.

	![Test Throughput](Images/Ex4Task4-06-throughput.png?raw=true "Test Throughput")

	_Test Throughput_

1. After the load test is finished, you can download the report.

	![Download Report](Images/Ex4Task4-07-download-report.png?raw=true "Download Report")

	_Download Report_

1. You can view the report once it has downloaded.

	![View Report](Images/Ex4Task4-08-view-report.png?raw=true "View Report")

	_View Report_

1. The results for the completed test includes performance counter data, threshold violations, and error information. Choose the **Detail** view. by analyzing the step load pattern for users, you can identify the user count where your performance fails to meet your requirements.

	![Report Detail View](Images/Ex4Task4-09-report-detail-view.png?raw=true "Report Detail View")

	_Report Detail View_