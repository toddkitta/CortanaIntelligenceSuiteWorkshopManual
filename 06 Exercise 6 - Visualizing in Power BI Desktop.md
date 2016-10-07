## Exercise 6: Visualizing in Power BI Desktop

Duration: 20 mins

Synopsis: In this exercise, attendees will construct a report in Power BI Desktop Client that uses the map visualization to illustrate the predicted delays, using the data originally scored using Machine Learning, but summarized in a table using Spark on HDInsight.

### Task 1: Connect to the Lab VM

1. You may already be logged in to the Lab VM from a previous exercise. If so, then go the Remote Desktop session of the Lab VM and skip to Task 2. If not, then to follow steps 2-9 below to log in to the Lab VM again.
2. From the left side of the Azure portal, click on **Browse &gt;**.
3. In the **filter** box, type in **virtual**.
4. Click on **Virtual Machines**.

![Screenshot](images/connect_to_the_lab_vm_0.png)

1. Keep in mind the name of the virtual machine will be the unique app name you provided when setting up this workshop environment.
2. Click on the **&lt;YOUR\_APP\_NAME&gt;lab** virtual machine from the list and click on **Connect**.

![Screenshot](images/connect_to_the_lab_vm_1.png)

1. Download and open the RDP file.
2. When Remote Desktop Connection screen appear, check the box **Don&#39;t ask…** and click on **Connect** button.

![Screenshot](images/connect_to_the_lab_vm_2.png)

1. Log in with the credentials you specified for the Lab VM during the deployment of the workshop prerequisites.

### Task 2: Install Spark ODBC Driver

1. Go to [https://aka.ms/sparkdriver](https://aka.ms/sparkdriver).
2. Scroll down and click on **Download** button.
3. Check the box for **SparkODBC64.msi** and click on **Next** button to begin the download.
4. Once downloaded, launch the installer.
5. Click on **Next** button.

![Screenshot](images/install_spark_odbc_driver_0.png)

1. Check the box to accept the terms and click the **Next** button.
2. Click on the **Next** button again.

![Screenshot](images/install_spark_odbc_driver_1.png)

1. Click on the **Install** button.
2. Click on **Finish** button to close the window.

### Task 3: Connect to the HDInsight Spark Using Power BI Desktop

1. Launch Power BI Desktop using the shortcut on the Desktop of the Lab VM.
2. Click on **Get Data** from the left side of the welcome window.

![Screenshot](images/connect_to_the_hdinsight_spark_using_power_bi_desktop_0.png)

1. Click on the **Azure** from the left and select **Azure HDInsight Spark (Beta)** from the new **Get Data** window. Click on the **Connect** button on the bottom right corner.

![Screenshot](images/connect_to_the_hdinsight_spark_using_power_bi_desktop_1.png)

1. Click **Continue** on the bottom right corner of the new window.
2. Type or copy/paste the **URL** of your Azure HDInsight Spark cluster, which can be found from the address bar when connected to the Azure HDInsight Spark cluster in previous Exercise 5. It should look something like this example: [**https://jcholab199spark.azurehdinsight.net**](https://jcholab199spark.azurehdinsight.net).
3. Click the **OK** button.
4. Enter the cluster user name and password.

![Screenshot](images/connect_to_the_hdinsight_spark_using_power_bi_desktop_2.png)

1. Give the system a few moments to connect to Spark and load the metadata.
2. When the dialog box disappears, look for the **flightdelaysummary** from the list.

![Screenshot](images/connect_to_the_hdinsight_spark_using_power_bi_desktop_3.png)

1. Select **flightdelaysummary** by checking box next to it.
2. Click on the **Load** button from the bottom right corner of the window. It will take couple of minutes to load the data into the Power BI Desktop client.

### Task 4: Create Power BI Report

1. Once the data load is completed, you will find the **flightdelaysummary** to the right side of the screen under the **Fields** area.

![Screenshot](images/create_power_bi_report_0.png)

1. From the **Visualizations** area, which is left to the **Fields** area, click the **Globe** icon to add a Map visualization to the report design surface.

![Screenshot](images/create_power_bi_report_1.png)

1. With the Map visualization still selected, in the **Fields** area at right, expand the tabled called **flightdelayssummary**.

![Screenshot](images/create_power_bi_report_2.png)

1. Click and drag the field labeled **OriginLatLong** and drop it into the **Location** field located just below visualizations.

![Screenshot](images/create_power_bi_report_3.png)

1. Next, drag the field labeled **NumDelays** and drop it into the **Size** field.

![Screenshot](images/create_power_bi_report_4.png)

1. Your map should look something like the following:

![Screenshot](images/create_power_bi_report_5.png)

1. Unselect the Map visual by clicking on the white space on the report page.
2. From the **Visualizations** area, which is left to the **Fields** area, click the **Stacked Column Chart** icon to add a bar chart visualization to the report design surface.

![Screenshot](images/create_power_bi_report_6.png)

1. With the **Stacked Column Chart** visualization still selected, in the Fields area at right, expand the tabled called **flightdelayssummary**.

![Screenshot](images/create_power_bi_report_7.png)

1. Click and drag the field labeled **Day** and drop it into the **Axis** field located just below visualizations.

![Screenshot](images/create_power_bi_report_8.png)

1. Next, drag the field labeled **AvgDelayProbability** and drop it into the **Value** field.

![Screenshot](images/create_power_bi_report_9.png)

1. Grab the corner of the new **Stacked Column Chart** Visual and drag it out by making wide as the bottom of your report design surface.
2. Your report should look something like the following:

![Screenshot](images/create_power_bi_report_10.png)

1. Unselect the Stacked Column Chart visual by clicking on the white space on the report page.
2. From the **Visualizations** area, which is left to the **Fields** area, click the **Treemap** icon to add this visualization to the report design surface.

![Screenshot](images/create_power_bi_report_11.png)

1. With the **Treemap** visualization still selected, in the Fields area at right, expand the tabled called **flightdelayssummary**.

![Screenshot](images/create_power_bi_report_12.png)

1. Click and drag the field labeled **OriginAirportCode** and drop it into the **Group** field located just below visualizations.

![Screenshot](images/create_power_bi_report_13.png)

1. Next, drag the field labeled **NumDelays** and drop it into the **Value** field.

![Screenshot](images/create_power_bi_report_14.png)

1. Grab the corner of the new **Treemap** Visual and drag it out by making wide as the top of your report design surface. Your report should look similar to the following:

![Screenshot](images/create_power_bi_report_15.png)

1. You can cross filter the visualizations on the report by click on the one of the other visuals within the report as shown below.

![Screenshot](images/create_power_bi_report_16.png)

1. You can save this Power BI report by click on Save icon from the top left corner of the screen.
