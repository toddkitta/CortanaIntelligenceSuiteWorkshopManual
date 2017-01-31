# Exercise 2: Setup Azure Data Factory

Duration: 20 mins

Synopsis: In this exercise, attendees will create a baseline environment for Azure Data Factory development for further operationalization of data movement and processing. They will create a Data Factory service and then install the Data Management Gateway which is the agent that facilitates data movement from on-premises to Microsoft Azure.

This exercise has 4 tasks:

* [Task 1: Connect to the Lab VM](#task-1-connect-to-the-lab-vm)
* [Task 2: Download and Stage Data to be Processed](#task-2-download-and-stage-data-to-be-processed)
* [Task 3: Install and Configure Data Management Gateway on the Lab VM](#task-3-install-and-configure-data-management-gateway-on-the-lab-vm)
* [Task 4: Log in to the Azure Portal](#task-4-log-in-to-the-azure-portal)
* [Task 5: Create new Azure Data Factory Service](#task-5-create-new-azure-data-factory-service)

## Get out of Jail Free

If, for whatever reason, you cannot complete this lab whether due to time contraints or if you are not able to troubleshoot an issue, we have created a "get out of jail free" exercise. If you wish to use this exercise at any time, please proceed to the [Appendix B](10 Appendix B - Alternative to Data Factory Exercises.md). Please note that using this exercise will let you surpass all of the Azure Data Factory exercises. After completing Appendix B, you can continue to [Exercise 5](05 Exercise 5 - Summarize Data Using HDInsight Spark.md).

## Task 1: Connect to the Lab VM

1. **NOTE:** If you are already connected to your Lab VM, skip to [Task 2](#task-2-download-and-stage-data-to-be-processed).
1. From the left side of the Azure portal, click on **All resources**.
2. In the **Filter items...** box, type in **lab**.
3. Select your lab VM. Keep in mind the name of the virtual machine will begin with the "app name" you provided when setting up this workshop environment (in the prerequisite deployment).
2. At the top of the blade for your VM, click on **Connect**.

    ![Screenshot](images/connect_to_the_lab_vm_1.png)

3. Download and open the RDP file.
1. When the **Remote Desktop Connection** screen appears, check the **Don't ask me again...** box and click on **Connect** button.

    ![Screenshot](images/ex02_connect_to_the_lab_vm_2.png)

1. Log in with the following credentials:
   - User name: **cortana**
   - Password: **Password.1!!**

## Task 2: Download and Stage Data to be Processed

1. Once you have logged in to the Lab VM, open a web browser. A shortcut for Chrome is on the Desktop and Internet Explorer can be accessed from the Start Screen.
2. Download the AdventureWorks sample CSV file. You can download it from [http://aka.ms/flightsandweather](http://aka.ms/flightsandweather) and save it to a new folder called **C:\Data**.

## Task 3: Install and Configure Data Management Gateway on the Lab VM

3. To download the latest version of Microsoft Data Management Gateway, go to [https://aka.ms/dmgdownload](https://aka.ms/dmgdownload).
4. Click on the **Download** button from the middle of the webpage.
5. Select the 64-bit version and click **Next**.

    ![Screenshot](images/ex02_install_and_configure_data_management_gateway_on_the_lab_vm_0.png)

1. Run the installer once downloaded.
2. When you see the following screen, click on the **Next** button.

    ![Screenshot](images/ex02_install_and_configure_data_management_gateway_on_the_lab_vm_1.png)

1. Check the box to accept the terms and click **Next** button.
2. Click **Next**.
3. Click **Install**.
4. Click **Finish**.
5. Once the installation is complete, the following screen will appear; keep it open for now. Next, you will provision the Data Factory service in Azure and obtain the gateway key so we can connect Data Factory to this &quot;on-premises&quot; server.

    ![Screenshot](images/ex02_install_and_configure_data_management_gateway_on_the_lab_vm_2.png)

## Task 4: Log in to the Azure Portal

1. Launch a new browser session and navigate to [https://portal.azure.com](https://portal.azure.com). Once prompted, log in with your Microsoft Azure credentials. If prompted, choose whether your account is an organization account or a Microsoft Account.  This will be based on which account was used to provision your Azure subscription that are using for these labs.
   - **Note** : You may need to launch an InPrivate/Incognito session in your browser if you have multiple Microsoft Accounts.

## Task 5: Create new Azure Data Factory Service

1. From left top corner of the Azure Portal, click on **+New**.

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_0.png)

1. Select **Intelligence + analytics** , click on **Data Factory**.

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_1.png)

1. Provide a name like [insert your initial here]-adf (example **jcho-adf** ).
2. Make sure to you have the right subscription selected.
3. For Resource Group, choose **Use Existing** and select the Resource Group you created when deploying the workshop prerequisites.
4. Select **East US** or **West US** for the region.
5. Check the box **Pin to dashboard** and click on the **Create** button.

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_2.png)

1. Deployment of the ADF will take couple of minutes.
2. Once it has completed, you will be taken to the Data Factory blade.
3. Click on **Author and deploy**.

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_3.png)

1. Click on **…More**.

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_4.png)

1. Click on **New data gateway**.

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_5.png)

1. Type in a **Data gateway name** like [insert your initial here]-dmg (example **jcho-dmg** ).
2. Click **OK** button.
3. Click on **Configure** and copy out the **NEW KEY**.

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_11.png)

4. _Don't close the current browser session._
5. Go back to the Remote Desktop session of the virtual machine.
6. Paste the **Key** into the box in the middle of the screen.

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_6.png)

1. Click on the **Register** button.
2. It will take couple of minutes to register. If it takes more than couple minutes and the screen does not respond or get an error message, then close the screen by click on the **Cancel** button.
3. You should see below screen with confirmation message.

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_7.png)

1. Confirmed that gateway is connected by going to the system tray of the bottom right corner and double click on the follow icon.

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_8.png)

1. You should see following screen:

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_9.png)

1. You can go back to the Azure Data Factory browser page within the Azure Portal, and Click **OK** twice to finish setting up the Data gateway.

    ![Screenshot](images/ex02_create_new_azure_data_factory_service_10.png)

Next Exercise: [Exercise 3 - Develop Data Factory Pipeline for Data Movement](03 Exercise 3 - Develop Data Factory Pipeline for Data Movement.md)