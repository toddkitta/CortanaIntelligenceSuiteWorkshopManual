# Workshop Overview

AdventureWorks Travel (AWT) provides concierge services for business travelers. In an increasingly crowded market, they are always looking for ways to differentiate themselves and provide added value to their corporate customers.

They are looking to pilot a web-app that their internal customer service agents can use to provide additional information useful to the traveler during the flight booking process. They want to enable their agents to enter in the flight information and produce a prediction as to if the departing flight will encounter a 15 minute or longer delay, taking into account the weather forecasted for the departure hour.

In this lab, attendees will build an end-to-end solution to predict flight delays taking into account the weather forecast.

# Requirements

- Microsoft Azure Subscription should be pay-as-you-go, MSDN, or EA.
  - Recommendation is to have each user have their own Azure subscription; this will allow every attendee to have their own sandbox.
- Setup is required before performing the steps in these exercises. Please see [http://aka.ms/cortanasetup](http://aka.ms/cortanasetup) before going any further.
- Please keep in mind that HDInsight cluster and VM you provision as setup for this workshop will incur charges, so provision these resources closest to the workshop date as possible.  Preferably the afternoon/night before the workshop.

# Cortana Intelligence Lab Exercises

## Exercise 1: Building a Machine Learning Model

Duration: 90 mins

Synopsis: In this exercise, attendees will implement a classification experiment. They will load the training data from their local machine into a dataset. Then they will explore the data to identify the primary components they should use for prediction, and use two different algorithms for predicting the classification. They will evaluate the performance of both and algorithms choose the algorithm that performs best. The model selected will be exposed as a web service that is integrated with the sample web app.

### Task 1: Navigate to Machine Learning Studio

1. In your browser, go to [https://studio.azureml.net](https://studio.azureml.net) and log in using the same account you used in the Azure portal to deploy the prerequisites for this workshop.
2. Once you are signed in, ensure the workspace that was created as part of the prerequisites is selected from the top bar.

### Task 2: Upload the Sample Datasets

1. Before you begin creating a machine learning experiment, there are three datasets you need to load.
2. Download the three CSV sample datasets from here: http://aka.ms/awtdata and save AdventureWorksTravelDatasets.zip to your desktop for now.
  1. Note that you will want to unblock the zip file before extracting files from it. Do this by right clicking on it, selecting **Properties** , and then perform the unblock from the resulting dialog.
3. Extract the ZIP and verify you have the following files:

1.
  - FlightDelaysWithAirportCodes.csv
  - FlightWeatherWithAirportCodes.csv
  - AirportCodeLocationClean.csv

1. Click **+ NEW** at the bottom, point to **Dataset** , and select **From Local File**.

![Screenshot](images/upload_the_sample_datasets_0.png)

1. In the dialog that appears, click **Choose File** and browse to the FlightDelaysWithAirportCodes file and click **OK**.
2. Change the name of the dataset to **FlightDelaysWithAirportCodes**.
3. Click on the check mark on the bottom right corner of the screen.

![Screenshot](images/upload_the_sample_datasets_1.png)

1. Repeat the previous step for the FlightWeatherWithAirportCodes and AirportCodeLocationsClean setting the name for the dataset in a similar fashion.

### Task 3: Start a New Experiment

1. Click **+ NEW** in the command bar.
2. In the options that appear, click **Blank Experiment**.

![Screenshot](images/start_a_new_experiment_0.png)

1. Give your new experiment a name, such as **AdventureWorks Travel** by editing the label near the top of the design surface.

![Screenshot](images/start_a_new_experiment_1.png)

1. In the toolbar on the left, in the Search experiment items box, type the name of the dataset you created with flight delay data (FlightDelaysWithAirportCodes). You should see a component for it listed under Saved Datasets, My Datasets.

![Screenshot](images/start_a_new_experiment_2.png)

1. Click and drag on the **FlightDelaysWithAirportCodes** to add it to the design surface.

![Screenshot](images/start_a_new_experiment_3.png)

1. Next, you will explore each of the datasets to understand what kind of cleanup (aka data munging) will be necessary.
2. Hover over the output port of the **FlightDelaysWithAirportCodes** dataset.

![Screenshot](images/start_a_new_experiment_4.png)

1. Right-click on the port and select **Visualize**.

![Screenshot](images/start_a_new_experiment_5.png)

1. A new dialog will appear showing a maximum of 100 rows by 100 columns sample of the dataset. You can see at the top that the dataset has a total of 2,719,418 rows (also referred to as examples in Machine Learning literature) and has 20 columns (also referred to as features).

![Screenshot](images/start_a_new_experiment_6.png)

1. Because all 20 columns are displayed, you can scroll the grid horizontally. Scroll until you see the DepDelay15 column and click it to view statistics about the column. The DepDelay15 column displays a 1 when the flight was delayed at least 15 minutes and 0 if there was no such delay. In the model you will construct, you will try to predict the value of this column for future data.

![Screenshot](images/start_a_new_experiment_7.png)

1. Notice in the Statistics panel that a value of 27444 appears for Missing Values. This means that 27,444 rows do not have a value in this column. Since this value is very important to our model, we will eliminate any rows that do not have a value for this column.
2. To eliminate these problem rows, close the dialog and go back to the design surface. From the toolbar, search for **Clean Missing Data**.

![Screenshot](images/start_a_new_experiment_8.png)

1. Drag this module on to the design surface beneath your **FlightDelaysWithAirportCodes** dataset. Click the small circle at the bottom of the **FlightDelaysWithAirportCodes** dataset, drag and release when your mouse is over the circle found in the top center of the Clean Missing Data module. These circles are referred to as ports, and by taking this action you have connected the output port of the dataset with the input port of the Clean Missing Data module, which means the data from the dataset will flow along this path.

![Screenshot](images/start_a_new_experiment_9.png)

1. Click **Save** on the command bar at the bottom to save your in-progress experiment.

![Screenshot](images/start_a_new_experiment_10.png)

1. Click **Run** in the command bar at the bottom to run the experiment.

![Screenshot](images/start_a_new_experiment_11.png)

1. When the experiment is finished running, you will see a finished message in the top right corner of the design surface, and green check marks over all modules that ran.

![Screenshot](images/start_a_new_experiment_12.png)

1. You should run your experiment whenever you need to update the metadata describing what data is flowing through the modules, so that newly added modules can be aware of the shape of your data (most module have dialogs that can suggest columns, but before they can make suggestions you need to have run your experiment).
2. Click the **Clean Missing Data** module to select it. The property panel on the right will display the settings appropriate to the selected module.
3. In this case, we want to remove rows that have no value for the DepDelay15 column. Begin by clicking **Launch Column Selector**.

![Screenshot](images/start_a_new_experiment_13.png)

1. Ensuring **With Rules** is selected on the left side of the dialog, under the **Begin With** section, select **No Columns**. In the row of controls that appears, change the second drop down to **Column Names**. Then in the text box that appears begin to type **DepDel15** and select that item from type-ahead list.

![Screenshot](images/start_a_new_experiment_14.png)

1. Click the checkmark to apply the settings. You have now indicated to the Clean Missing Data module that DepDel15 is the only column it should act on.

![Screenshot](images/start_a_new_experiment_15.png)

1. In the **Properties** panel for **Clean Missing Data** , click the **Cleaning mode** drop-down and select **Remove entire row**. Now your Clean Missing Data module is fully configured to remove any rows that are missing values for DepDel15.

![Screenshot](images/start_a_new_experiment_16.png)

1. To verify the result, run your experiment again. After it is finished, click the output port (1) of the Clean Missing Data module and select **Visualize**.
2. In the dialog that appears, scroll over to **DepDel15** and click the column. In the statistics you should see that Missing Values reads 0.

![Screenshot](images/start_a_new_experiment_17.png)

1. Our model will approximate departure times to the nearest hour, but departure time is captured as an integer. For example, 8:37 am is captured as 837. Therefore, we will need to process the CRSDepTime column and round it down to the nearest hour.
2. To perform this rounding will require two steps, first you will need to divide the value by 100 (so that 837 becomes 8.37). Second, you will round this value down to the nearest hour (so that 8.37 becomes 8).
3. Begin by adding an **Apply Math Operation** module beneath the Clean Missing Data module and connect the output port (1) of the Clean Missing Data module to the input port of the Apply Math Operation.

![Screenshot](images/start_a_new_experiment_18.png)

1. In the properties of the Apply Math Operation, set the Category to **Operations** , Basic operation to **Divide** , Operation argument type to **Constant** , Constant operation argument to **100** , Selected columns to **CRSDepTime** (see screenshot below), and Output mode to **Append**.

![Screenshot](images/start_a_new_experiment_19.png)

![Screenshot](images/start_a_new_experiment_20.png)

1. Run the experiment to update the metadata.
2. This module will add a new column to the dataset called Divide(CRSDeptTime\_$100), but we want to rename it to CRSDepHour. To do so, add an **Edit Metadata** module and connect its input port to the output port of Apply Math Operation.

![Screenshot](images/start_a_new_experiment_21.png)

1. For the properties of the **Edit Metadata** , set the Selected Columns to **Divide(CRSDeptTime\_$100)** and New column names to **CRSDepHour**.

![Screenshot](images/start_a_new_experiment_22.png)

![Screenshot](images/start_a_new_experiment_23.png)

1. Run the experiment to update the metadata.
2. Add another **Apply Math Operation** module to round the time down to the nearest hour. Set the Category to **Rounding** , Selected columns to **CRSDepHour** (see screenshot for how to select), and Output mode to **Inplace**.

![Screenshot](images/start_a_new_experiment_24.png)

![Screenshot](images/start_a_new_experiment_25.png)

1. Run the experiment to update the metadata.
2. We do not need all of the columns present in the FlightDelaysWithAirportCodes dataset. To pare down the columns we can use multiple options, but in this case we chose to use an R Script module that selects out only the columns of interest using R code.
3. Add an **Execute R Script** module beneath the last Apply Math Operation, and connect the output of the Apply Math Operation to the first input port (leftmost) of the Execute R Script.

![Screenshot](images/start_a_new_experiment_26.png)

1. In the **Properties** panel for Execute R Script, click the **Double Windows** icon to maximize the script editor.

![Screenshot](images/start_a_new_experiment_27.png)

1. Replace the default script with the following and click the checkmark to save it (press CTRL+A to select all of the text then CTRL+V to paste and then immediately click the checkmark. Don&#39;t worry if the formatting is off before hitting the checkmark.

ds.flights &lt;- maml.mapInputPort(1)

# Trim the columns to only those we will use for the predictive model

ds.flights = ds.flights[, c(&quot;OriginAirportCode&quot;,&quot;OriginLatitude&quot;, &quot;OriginLongitude&quot;, &quot;Month&quot;, &quot;DayofMonth&quot;, &quot;CRSDepHour&quot;, &quot;DayOfWeek&quot;, &quot;Carrier&quot;, &quot;DestAirportCode&quot;, &quot;DestLatitude&quot;, &quot;DestLongitude&quot;, &quot;DepDel15&quot;)]

maml.mapOutputPort(&quot;ds.flights&quot;);

1. Run the experiment to update the metadata (this may take a minute or two to complete).
2. Right-click on the first output port of your Execute R Script module and select **Visualize**.
3. Verify that the dataset only contains the 12 columns referenced in the R script.

![Screenshot](images/start_a_new_experiment_28.png)

1. At this point the Flight Delay Data is prepared, and we turn to preparing the historical weather data.

### Task 4: Prepare the Weather Data

1. To the right of the FlightDelaysWithAirportCodes dataset, add the **FlightWeatherWithAirportCodes** dataset.

![Screenshot](images/prepare_the_weather_data_0.png)

1. Right-click the output port of the FlightWeatherWithAirportCodes dataset and select **Visualize**.

![Screenshot](images/prepare_the_weather_data_1.png)

1. Observe that this data set has 406,516 rows and 29 columns. For this model, we are going to focus on predicting delays using WindSpeed (in MPH), SeaLevelPressure (in inches of Hg), and HourlyPrecip (in inches). We will focus on preparing the data for those features.
2. In the dialog, click the **WindSpeed** column and review the statistics. Observe that the Feature Type was inferred as String and that there are 32 Missing Values. Below that, examine the histogram to see that, even though the type was inferred as string, the values are all actually numbers (e.g. the x-axis values are 0, 6, 5, 7, 3, 8, 9, 10, 11, 13). We will need to ensure that we remove any missing values and convert WindSpeed to its proper type as a numeric feature.

![Screenshot](images/prepare_the_weather_data_2.png)

1. Next, click the **SeaLevelPressure** column. Observe that the Feature Type was inferred as String and there are 0 Missing Values. Scroll down to the histogram, and observe that many of the features of a numeric value (e.g., 29.96, 30.01, etc.), but there are many features with the string value of M for Missing. We will need to replace this value of M&quot;with a suitable numeric value so that we can convert this feature to be a numeric feature.

![Screenshot](images/prepare_the_weather_data_3.png)

1. Finally, examine the **HourlyPrecip** feature. Observe that it too was inferred to have a Feature Type of String and is missing values for 374,503 rows. Looking at the histogram, observe that besides the numeric values, there is a value T for Trace amount of rain). We will need to replace the T with a suitable numeric value and covert this feature to a numeric feature.

![Screenshot](images/prepare_the_weather_data_4.png)

1. Let us begin by cleaning up the missing values for both WindSpeed and HourlyPrecip.
2. Below the FlightWeatherWithAirportCode dataset, drop a **Clean Missing Data** module and connect the output of the dataset to the input of module.

![Screenshot](images/prepare_the_weather_data_5.png)

1. Run the experiment to update the metadata available to the Clean Missing Data module.
2. In the **Properties** panel for Clean Missing Data, set the Selected columns to **HourlyPrecip** and **WindSpeed** (see screenshot for help selecting if needed), set the Cleaning mode to **Custom substitution value** and set the Replacement value to **0.0**.

![Screenshot](images/prepare_the_weather_data_6.png)

![Screenshot](images/prepare_the_weather_data_7.png)

1. Next, add an **Execute R Script** module below the Clean Missing Data module and connect the first output port of the former to the first input port of the latter.

![Screenshot](images/prepare_the_weather_data_8.png)

1. In the **Properties** panel for the Execute R Script, click the &quot;double windows&quot; icon to open the script editor.
2. Paste in the following script and click the checkmark (press CTRL+A to select all of the text then CTRL+V to paste and then click the checkmark. Don&#39;t worry if the formatting is off). This script replaces the HourlyPrecip values having T with 0.05, WindSpeed values of M with 0.0, and the SeaLevelPressure values of M with the global average pressure of 29.92. It also narrows the dataset to just the few feature columns we want to use with our model.

ds.weather &lt;- maml.mapInputPort(1)

# Round weather time up to the next hour since

# that&#39;s the hour for which we want to use flight data

ds.weather$Hour = ceiling(ds.weather$Time / 100)

# Replace any WindSpeed values of &quot;M&quot; with 0.005 and make the feature numeric

speed.num = ds.weather$WindSpeed

speed.num[speed.num == &quot;M&quot;] = 0.005

speed.num = as.numeric(speed.num)

ds.weather$WindSpeed = speed.num

# Replace any SeaLevelPressure values of &quot;M&quot; with 29.92 (the average pressure) and make the feature numeric

pressure.num = ds.weather$SeaLevelPressure

pressure.num[pressure.num == &quot;M&quot;] = 29.92

pressure.num = as.numeric(pressure.num)

ds.weather$SeaLevelPressure = pressure.num

# Adjust the HourlyPrecip variable (convert &quot;T&quot; (trace) to 0.005)

rain = ds.weather$HourlyPrecip

rain[rain %in% c(&quot;T&quot;)] = &quot;0.005&quot;

ds.weather$HourlyPrecip = as.numeric(rain)

# Pare down the variables in the Weather dataset

ds.weather = ds.weather[, c(&quot;AirportCode&quot;, &quot;Month&quot;, &quot;Day&quot;, &quot;Hour&quot;, &quot;WindSpeed&quot;, &quot;SeaLevelPressure&quot;, &quot;HourlyPrecip&quot;)]

maml.mapOutputPort(&quot;ds.weather&quot;);

1. Run the experiment. Currently it should appear as follows:

![Screenshot](images/prepare_the_weather_data_9.png)

1. Click the first output port of the Execute R Script module and select **Visualize**.
2. In the statistics, verify that WindSpeed, SeaLevelPressure, and HourlyPrecip are now all Numeric Feature types and that they have no missing values.

### Task 5: Join the Flight and Weather Datasets

1. With both datasets ready, we want to join them together so that we can associate historical flight delay with the weather data at departure time.
2. Drag the **Join Data** module on to the design surface, beneath and centered between both Execute R Script modules. Connect the output port (1) of the left Execute R module to input port (1) of the Join Data module, and the output port (1) of the right Execute R module to the input port (2) of the Join Data module.

![Screenshot](images/join_the_flight_and_weather_datasets_0.png)

1. In the **Properties** panel, relate the rows of data between the two sets L (the flight delays) and R (the weather). Set the Join key columns for L to include **OriginAirportCode** , **Month** , **DayofMonth** , and **CRSDepHour**.

![Screenshot](images/join_the_flight_and_weather_datasets_1.png)

1. Set the join key columns for R to include **AirportCode** , **Month** , **Day** , and **Hour**.

![Screenshot](images/join_the_flight_and_weather_datasets_2.png)

1. Leave the Join Type at inner join and uncheck **Keep right key columns in joined table** (so that we do not include the redundant values of AirportCode, Month, Day, and Hour).

![Screenshot](images/join_the_flight_and_weather_datasets_3.png)

1. Next, add an **Edit Metadata** module and connect its input port to the output port of the Join Data module. We will use this module to convert the fields that were unbounded String feature types, to the enumeration like Categorical feature. On the **Properties** panel, set the Selected columns to **DayOfWeek** , **Carrier** , **DestAirportCode** , and **OriginAirportCode**. Set the Categorical drop down to **Make categorical**.

![Screenshot](images/join_the_flight_and_weather_datasets_4.png)

![Screenshot](images/join_the_flight_and_weather_datasets_5.png)

1. Run the experiment to update the metadata.
2. Add a **Select Columns in Dataset** module, connect the output of the previous **Edit Metadata** to the input of the **Select Columns in Dataset** module, then set the selected columns to exclude (see the screenshot for how to do this): **OriginLatitude** , **OriginLongitude** , **DestLatitude** , and **DestLongitude**.

![Screenshot](images/join_the_flight_and_weather_datasets_6.png)

1. Save your experiment.

### Task 6: Train the Model

AdventureWorks Travel wants to build a model to predict if a departing flight will have a 15-minute or greater delay. In the historical data they have provided, the indicator for such a delay is found within the DepDelay15 (where a value of 1 means delay, 0 means no delay). To create model that predicts such a binary outcome, we can choose from the various Two-Class modules that Azure ML offers. For our purposes, we begin with a Two-Class Logistic Regression. This type of classification module needs to be first trained on sample data that includes the features important to making a prediction and must also include the actual historical outcome for those features.

The typical pattern is split the historical data so a portion is shown to the model for training purposes, and another portion is reserved to test just how well the trained model performs against examples it has not seen before.

1. Drag a **Split Data** module beneath **Select Columns in Dataset** and connect them.

![Screenshot](images/train_the_model_0.png)

1. On the **Properties** panel for the Split Data module, set the Fraction of rows in the first output dataset to **0.7** (so 70% of the historical data will flow to output port 1). Set the Random seed to **7634**.

![Screenshot](images/train_the_model_1.png)

1. Next, add a **Train Model** module and connect it to output 1 of the Split Data module.

![Screenshot](images/train_the_model_2.png)

1. On the **Properties** panel for the Train Model module, set the Selected columns to **DepDelay15**.

![Screenshot](images/train_the_model_3.png)

1. Drag a **Two-Class Logistic Regression** module above and to the left of the Train Model module and connect the output to the leftmost input of the Train Model module.

![Screenshot](images/train_the_model_4.png)

1. Below the Train Model drop a **Score Model** module. Connect the output of the Train Model module to the leftmost input port of the Score Model and connect the rightmost output of the Split Data module to the rightmost input of the Score Model.

![Screenshot](images/train_the_model_5.png)

1. Run the experiment.
2. When the experiment is finished running (which takes a few minutes), right-click on the output port of the Score Model module and select **Visualize** to see the results of its predictions. You should have a total of 13 columns.

![Screenshot](images/train_the_model_6.png)

1. If you scroll to the right so that you can see the last two columns, observe there is a Scored Labels column and a Scored Probabilities column. The former is the prediction (1 for predicting delay, 0 for predicting no delay) and the latter is the probability of the prediction. In the following screenshot, for example, the last row shows a delay predication with a 53.1% probability.

![Screenshot](images/train_the_model_7.png)

1. While this view enables you to see the prediction results for the first 100 rows, if you want to get more detailed statistics across the prediction results to evaluate your models performance you can use the Evaluate Model module.
2. Drag an **Evaluate Model** module on to the design surface beneath the Score Model module. Connect the output of the Score Model module to the leftmost input of the Evaluate Model module.

![Screenshot](images/train_the_model_8.png)

1. Run the experiment.
2. When the experiment is finished running, right-click the output of the Evaluate Model module and select **Visualize**. In this dialog box, you are presented with various ways to understand how your model is performing in the aggregate. While we will not cover how to interpret these results in detail, we can examine the ROC chart that tells us that at least our model (the blue curve) is performing better than random (the light gray straight line going from 0,0 to 1,1) — which is a good start for our first model!

![Screenshot](images/train_the_model_9.png)

### Task 7: Operationalize the Experiment

1. Now that we have a functioning model, let&#39;s package it up into a predictive experiment that can be called as web service.
2. In the command bar at the bottom, click **Set Up Web Service** and then select **Predictive Web Service**. If you see that the **Set Up Web Service** option is grayed out, then you may need to run the experiment again by click on the **RUN** button.

![Screenshot](images/operationalize_the_experiment_0.png)

1. A copy of your training experiment is created that contains the trained model wrapped between web service input (e.g. the web service action you invoke with parameters) and web service output modules (e.g., how the result of scoring the parameters are returned).
2. We will make some adjustments to the web service input and output modules to control the parameters we require and the results we return.
3. When packaging the Predictive Web Service, Azure ML added two Apply Transformation modules which are not needed. Delete both of the Apply Transformation Modules.

![Screenshot](images/operationalize_the_experiment_1.png)

1. The Apply Transformation Modules were added to support the Clean Missing Data modules. We will not be using these steps in our flow, so delete both Clean Missing Data Modules.

![Screenshot](images/operationalize_the_experiment_2.png)

1. Reconnect the FlightDelaysWithAirportCodes to the input to Apply Math Operation module that is directly beneath it.

![Screenshot](images/operationalize_the_experiment_3.png)

1. Reconnect the FlightWeatherWithAirportCodes module to the leftmost input port of the Execute R Script module beneath it.

![Screenshot](images/operationalize_the_experiment_4.png)

1. Now move the web service input down so it is to the right of the Join Data module. Connect the output of the Web service input to input of the **Edit Metadata** module.

![Screenshot](images/operationalize_the_experiment_5.png)

1. Right-click the line connecting the **Join Data** module and the **Edit Metadata** module and select **Delete**.

![Screenshot](images/operationalize_the_experiment_6.png)

1. In between the **Join Data** and the **Edit Metadata** modules, drop a **Select Columns in Dataset** module and connect **Join Data** to this new **Select Columns in Dataset** module. In the **Properties** panel for the **Select Columns in Dataset** module, set the Select columns to all columns, exclude (again, notice this is an exclude operation) columns **DepDel15** , **OriginLatitude** , **OriginLongitude** , **DestLatitude** , and **DestLongitude**. This configuration will update the web service metadata so that these columns do not appear as required input parameters for the web service.

![Screenshot](images/operationalize_the_experiment_7.png)

1. Connect the **Select Columns in Dataset** output to **Edit Metadata**.

![Screenshot](images/operationalize_the_experiment_8.png)

1. Select the **Select Columns in Dataset** module that comes after the **Edit Metadata** module and delete it.
2. Connect the output of the **Edit Metadata** module directly to the right input of the **Score Model** module.

![Screenshot](images/operationalize_the_experiment_9.png)

1. As we removed the latitude and longitude columns from the dataset to remove them as input to the web service, we have to add them back in before we return the result so that the results can be easily visualized on a map.
2. To add these fields back, begin by deleting the line between the Score Model and Web service output.
3. Drag the **AirportCodeLocationLookupClean** dataset on to the design surface, positioning it below the Score Model module.

![Screenshot](images/operationalize_the_experiment_10.png)

1. Add a **Join Data** module. Connect the output of the **Score Model** module to the leftmost input of the **Join Data** module and the output of the dataset to the rightmost input of the **Join Data** module.

![Screenshot](images/operationalize_the_experiment_11.png)

1. In the **Properties** panel for the Join Data module, for the Join key columns for L set the selected columns to **OriginAirportCode**. For the Join key columns for R, set the Selected columns to **AIRPORT**. Uncheck Keep right key columns in joined table.

![Screenshot](images/operationalize_the_experiment_12.png)

1. Add a **Select Columns in Dataset** module beneath the **Join Data** module.  Connect the **Join Data** output to the input of the **Select Columns in Dataset** module.

![Screenshot](images/operationalize_the_experiment_13.png)

1. In the **Properties** panel, set the Selected columns to **exclude** (_not include_) the columns: **AIRPORT\_ID** and **DISPLAY\_AIRPORT\_NAME**.

![Screenshot](images/operationalize_the_experiment_14.png)

![Screenshot](images/operationalize_the_experiment_15.png)

1. Add a **Edit Metadata** module.  Connect the output of the **Select Columns in Dataset** module to the input of the **Edit Metadata** module.

![Screenshot](images/operationalize_the_experiment_16.png)

1. In the **Properties** panel for the **Edit Metadata** module, set the Selected columns to **LATITUDE** and **LONGITUDE**. In the New column names enter: **OriginLatitude** , **OriginLongitude**.

![Screenshot](images/operationalize_the_experiment_17.png)

1. Connect the output of the **Edit Metadata** module to the input of the web service output module.

![Screenshot](images/operationalize_the_experiment_18.png)

1. Run the experiment. This should take 5-7 minutes.
2. When the experiment is finished running, click **Deploy Web Service**. When the deployment is complete, you will be taken to the Web Service dashboard.

![Screenshot](images/operationalize_the_experiment_19.png)

1. Leave this page open for the next task.

### Task 8: Note Web Service Integration Information

1. From the Web Service dashboard, copy the API key, open a copy of Notepad.exe on your local workstation, and paste the value in the editor.
2. On the Web Service dashboard, click the **REQUEST/RESPONSE** link.
3. Scroll down until you see the first Request URL. From this URL, you can get the Workspace ID and the Service ID.

https://ussouthcentral.services.azureml.net/workspaces/ **f8a24adcde7c45e1bb1c9ef85fe36f62** /services/ **656c53c99cc848e1a476f5068ba39fb6** /execute?api-version=2.0&amp;details=true

1. The first GUID after workspaces is your Workspace ID. The second GUID after services is your Service ID.
2. Copy each of these values into Notepad as well. Make sure you note which GUID is which because you will need these in a later step.

## Exercise 2: Setup Azure Data Factory

Duration: 20 mins

Synopsis: In this exercise, attendees will create a baseline environment for Azure Data Factory development for further operationalization of data movement and processing. They will create a Data Factory service and then install the Data Management Gateway which is the agent that facilitates data movement from on-premises to Microsoft Azure.

### Task 1: Connect to the Lab VM

### Task 2: Install and Configure Data Management Gateway on the Lab VM

### Task 3: Log in to the Azure Portal

### Task 4: Create new Azure Data Factory Service

## Exercise 3: Develop Data Factory Pipeline for Data Movement

Duration: 20 mins

Synopsis: In this exercise, attendees will implement Azure Data Factory pipeline to copy data (.csv file) from on-premises server (lab virtual machine) to Azure Blob Storage. The goal of the exercise is to demonstrate data movement from an on-premises location to Azure Storage (via the Data Management Gateway). The attendee will see how these assets are created, deployed, executed, and monitored.

### Task 1: Create Copy Pipeline Using the Copy Data Wizard

1. Go the Azure Portal and select Azure Data Factory (ADF) Service you create in previous exercise.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_0.png)

1. From **Actions** section, click on **Copy data**.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_1.png)

1. New browser window will open.  Under Properties tab, type in **CopyPipeline–OnPrem2Azure**.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_2.png)

1. Optional: Type in a description like: **This pipeline copies CSV file from on-premises virtual machine C:\Data to Azure Blob Storage as a one-time job**.
2. Select the **Run once now** option.
3. Click on the **Next** button from the bottom of the screen.
4. From the Source Data Store screen tab, select **File**.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_3.png)

1. Click on **Next** button from the bottom of the screen.
2. From the Specify File Server share connection tab, type **InputLinkedService-OnPremServer** into the **Linked service name**.
3. For the **Path** , type in **C:\Data**.
4. For the **User name** , type in the user name that was used to log into the lab virtual machine.
5. For the **Password** , type in the password that was used to log into the lab virtual machine.
6. For the Gateway, it should already be filled in with the gateway connection you created in the previous exercise.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_4.png)

1. Click on the **Next** button from the bottom of the screen.
2. From the **Choose the input file or folder** tab, click on the **FlightsAndWeather.csv**.
3. Click on the **Choose** button from the bottom right corner of the screen.
4. Click on the **Next** button from the bottom of the screen.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_5.png)

1. From the File format settings tab, leave everything as defaulted except check the box **Column name in the first data row**.  You can see the preview of the file from the bottom of the screen.
2. Click **Next** button from the bottom of the screen.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_6.png)

1. From the **Destination data store** tab, click on the **Azure Blob Storage**.
2. Click on the **Next** button from the bottom of the screen.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_7.png)

1. From the **Specify the Azure Blob storage account** tab, Type in **OutputLinkedService-AzureBlobStorage** into the **Linked service name**.
2. From the **Storage account name** dropdown list, select **&lt;YOUR\_APP\_NAME&gt;sparkstorage**.
3. Click on the **Next** button from the bottom of the screen.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_8.png)

1. From the Choose the output file or folder tab, type in **sparkcontainer/flights** into the **folder path** textbox.
2. Click on the **Next** button from the bottom of the screen.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_9.png)

1. From the File format settings tab, leave everything as defaulted except check the box **Add header to file**. You can see the preview of the file from the bottom of the screen.
2. Click **Next** button from the bottom of the screen.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_10.png)

1. Review the summary tab and click **Finish** button from the bottom.
2. You should see a **Deploying** status. This process will take few minutes and see the **Deployment Complete** status on top of the screen.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_11.png)

1. Click on the hyperlink **Click here to monitor copy pipeline**.
2. From the **Resource Explorer** , you should see from the **Activity Windows** (bottom of the screen) the pipeline activity status **Ready**. This indicates the CSV file was successfully copied from your VM to your Azure Blob Storage location.

![Screenshot](images/create_copy_pipeline_using_the_copy_data_wizard_12.png)

## Exercise 4: Operationalize ML Scoring with Azure ML and Data Factory

Duration: 20 mins

Synopsis: In this exercise, attendees will extend the Data Factory service to operationalize the scoring of data using the previously created ML model.

### Task 1: Create Azure ML Linked Service

1. Go back to the **Azure Data Factory** service blade.
2. Click on the **Author and deploy** from **Actions** section.

![Screenshot](images/create_azure_ml_linked_service_0.png)

1. Click on **…More**.

![Screenshot](images/create_azure_ml_linked_service_1.png)

1. Click on **New Compute**.

![Screenshot](images/create_azure_ml_linked_service_2.png)

1. Select **Azure ML** from the list.
2. In the new window, be sure change the JSON file to match the following:
  1. In the new window, be sure change the JSON file to match the following or copy the below JSON text and paste into the browser window. You can also copy this from **Listing 1** in [http://aka.ms/adfcode](http://aka.ms/adfcode).
  2. Be sure to follow steps 7 to 13 to obtain **mlEndPoint** URL and **apiKey** value.

{

   &quot;name&quot;: &quot;AzureMLLinkedService&quot;,

   &quot;properties&quot;: {

      &quot;type&quot;: &quot;AzureML&quot;,

      &quot;description&quot;: &quot;&quot;,

      &quot;typeProperties&quot;: {

         &quot;mlEndpoint&quot;: &quot;&lt;Specify the batch scoring URL&gt;&quot;,

         &quot;apiKey&quot;: &quot;&lt;Specify the published workspace model&#39;s API key&gt;&quot;

      }

   }

}

1. Open a browser and navigate to your ML Studio and click **Web Services**. You can always get there by going to [https://studio.azureml.net/](https://studio.azureml.net/).
2. Click on your web service.
3. On this page, take note of the API key.  Copy the **API key** value and paste it in for the **apiKey** above in the JSON.
4. Below the Default Endpoint section, click the **Batch Execution** link.

![Screenshot](images/create_azure_ml_linked_service_3.png)

1. On this API Documentation page, scroll down to the first request (for Submit job) and observe the URL displayed.  Copy the URL starting at **https:** though (and including) **jobs** and paste it in for the **mlEndpoint** in red.
  1. Example URL: [https://ussouthcentral.services.azureml.net/workspaces/64f94487f2bc476dae5e02b37b8d8b17/services/ad88ee0b5f54649963116bc98fa03ab/jobs](https://ussouthcentral.services.azureml.net/workspaces/64f94487f2bc476dae5e02b37b8d8b17/services/ad88ee0b5f54649963116bc98fa03ab/jobs)

![Screenshot](images/create_azure_ml_linked_service_4.png)

1. Click **Deploy**.

### Task 2: Create Azure ML Input Dataset

1. Click on **…More**.

![Screenshot](images/create_azure_ml_input_dataset_0.png)

1. To create new dataset that will be copied into Azure Blob storage, click on the **New dataset** from the top.

![Screenshot](images/create_azure_ml_input_dataset_1.png)

1. Select **Azure Blob storage** from the list.
2. In the new window, be sure change the JSON file to match the following or copy the below JSON text and paste into the browser window. You can also copy this from **Listing 2** in [http://aka.ms/adfcode](http://aka.ms/adfcode).

{

   &quot;name&quot;: &quot;AzureBlobDataInPut&quot;,

   &quot;properties&quot;: {

      &quot;type&quot;: &quot;AzureBlob&quot;,

      &quot;external&quot;: true,

      &quot;linkedServiceName&quot;: &quot;OutputLinkedService-AzureBlobStorage&quot;,

      &quot;typeProperties&quot;: {

         &quot;fileName&quot;: &quot;FlightsAndWeather.csv&quot;,

         &quot;folderPath&quot;: &quot;sparkcontainer/flights&quot;,

         &quot;format&quot;: {

            &quot;type&quot;: &quot;TextFormat&quot;

         }

      },

      &quot;availability&quot;: {

         &quot;frequency&quot;: &quot;Minute&quot;,

         &quot;interval&quot;: 60

      }

   }

}

1. Click Deploy.

### Task 3: Create Azure ML Scored Dataset

1. Click on **…More**.

![Screenshot](images/create_azure_ml_scored_dataset_0.png)

1. Click on the **New dataset**.

![Screenshot](images/create_azure_ml_scored_dataset_1.png)

1. Select **Azure Blob storage** from the list.
2. In the new window, be sure change the JSON file to match the following or copy the below JSON text and paste into the browser window. You can also copy this from **Listing 3** in [http://aka.ms/adfcode](http://aka.ms/adfcode).

{

   &quot;name&quot;: &quot;AzureBlobScoredDataOutPut&quot;,

   &quot;properties&quot;: {

      &quot;type&quot;: &quot;AzureBlob&quot;,

      &quot;linkedServiceName&quot;: &quot;OutputLinkedService-AzureBlobStorage&quot;,

      &quot;typeProperties&quot;: {

         &quot;fileName&quot;: &quot;Scored\_FlightsAndWeather.csv&quot;,

         &quot;folderPath&quot;: &quot;sparkcontainer/flights&quot;,

         &quot;format&quot;: {

            &quot;type&quot;: &quot;TextFormat&quot;

         }

      },

      &quot;availability&quot;: {

         &quot;frequency&quot;: &quot;Minute&quot;,

         &quot;interval&quot;: 60

      }

   }

}

1. Click **Deploy**.

### Task 4: Create Azure ML Predictive Pipeline

1. Click on **…More**.

![Screenshot](images/create_azure_ml_predictive_pipeline_0.png)

1. Click on the **New pipeline**.

![Screenshot](images/create_azure_ml_predictive_pipeline_1.png)

1. In the new window, be sure change the JSON file to match the following or copy the below JSON text and paste into the browser window. You can also copy this from **Listing 4** in [http://aka.ms/adfcode](http://aka.ms/adfcode).

{

    &quot;name&quot;: &quot;PredictivePipeline&quot;,

    &quot;properties&quot;: {

        &quot;description&quot;: &quot;Use AzureML model&quot;,

        &quot;activities&quot;: [

            {

                &quot;type&quot;: &quot;AzureMLBatchExecution&quot;,

                &quot;typeProperties&quot;: {

                    &quot;webServiceInput&quot;: &quot;AzureBlobDataInPut&quot;,

                    &quot;webServiceOutputs&quot;: {

                        &quot;output1&quot;: &quot;AzureBlobScoredDataOutPut&quot;

                    },

                    &quot;globalParameters&quot;: {}

                },

                &quot;inputs&quot;: [

                    {

                        &quot;name&quot;: &quot;AzureBlobDataInPut&quot;

                    }

                ],

                &quot;outputs&quot;: [

                    {

                        &quot;name&quot;: &quot;AzureBlobScoredDataOutPut&quot;

                    }

                ],

                &quot;policy&quot;: {

                    &quot;timeout&quot;: &quot;02:00:00&quot;,

                    &quot;concurrency&quot;: 1,

                    &quot;executionPriorityOrder&quot;: &quot;NewestFirst&quot;,

                    &quot;retry&quot;: 1

                },

                &quot;scheduler&quot;: {

                    &quot;frequency&quot;: &quot;Minute&quot;,

                    &quot;interval&quot;: 60

                },

                &quot;name&quot;: &quot;MLActivity&quot;,

                &quot;description&quot;: &quot;prediction analysis on batch input&quot;,

                &quot;linkedServiceName&quot;: &quot;AzureMLLinkedService&quot;

            }

        ],

        &quot;start&quot;: &quot;2016-09-14T00:00:00Z&quot;,

        &quot;end&quot;: &quot;2016-09-15T00:00:00Z&quot;

    }

}

1. Make sure to change the **start** to today&#39;s date and **end** to today + 1 date.
2. Click **Deploy**.

### Task 5: Monitor Your Pipeline Activities

1. Close the current blade by clicking on the **X** from the top right corner of the blade.
2. Click on the **Monitor &amp; Manage** from the **Actions** section.
3. Maximize the new window and you can see the diagram view of the data flow.

![Screenshot](images/monitor_your_pipeline_activities_0.png)

1. You should start to see **Ready** status activity listed on the bottom of the new window.
2. Close the **Monitor &amp; Manage** browser tab.

## Exercise 5: Summarize Data Using HDInsight Spark

Duration: 60 mins

Synopsis: In this exercise, attendees will prepare a summary of flight delay data in HDFS using Spark SQL. Note: This exercise has the attendee copying and pasting some Spark code. The code is available in an easy to access text file at [http://aka.ms/sparkcode](http://aka.ms/sparkcode). Copying from the text file (rather than from this manual) can cut down on formatting problems.

### Task 1: Summarize Delays by Airport

1. Navigate to the blade for your Spark Cluster in the Azure Portal.

![Screenshot](images/summarize_delays_by_airport_0.png)

1. In the Quick Links section, click **Cluster Dashboards**.

![Screenshot](images/summarize_delays_by_airport_1.png)

1. From the **Cluster Dashboards** , click **Jupyter Notebooks**. You will have to login with the cluster username/password at this step.

![Screenshot](images/summarize_delays_by_airport_2.png)

1. On the **Jupyter notebooks** screen, click on the **New** dropdown list from top right corner and click **Spark**.

![Screenshot](images/summarize_delays_by_airport_3.png)

1. Copy the below text and paste it into the Jupyter notebook. Alternatively, copy Listing 1 from [http://aka.ms/sparkcode](http://aka.ms/sparkcode).

![Screenshot](images/summarize_delays_by_airport_4.png)

import sqlContext.implicits.\_

val flightDelayTextLines = sc.textFile(&quot;wasb:///flights/Scored\_FlightsAndWeather.csv&quot;)

case class AirportFlightDelays(OriginAirportCode:String,OriginLatLong:String,Month:Integer,Day:Integer,Hour:Integer,Carrier:String,DelayPredicted:Integer,DelayProbability:Double)

val flightDelayRowsWithoutHeader = flightDelayTextLines.map(s =&gt; s.split(&quot;,&quot;)).filter(line =&gt; line(0) != &quot;OriginAirportCode&quot;)

val resultDataFrame = flightDelayRowsWithoutHeader.map(

    s =&gt; AirportFlightDelays(

        s(0), //Airport code

        s(13) + &quot;,&quot; + s(14), //Lat,Long

        s(1).toInt, //Month

        s(2).toInt, //Day

        s(3).toInt, //Hour

        s(5), //Carrier

        s(11).toInt, //DelayPredicted

        s(12).toDouble //DelayProbability

        )

).toDF()

resultDataFrame.saveAsTable(&quot;FlightDelays&quot;)

1. Click the **Play** icon in the top of the screen to execute this code and create the FlightDelays table.

![Screenshot](images/summarize_delays_by_airport_5.png)

1. Click in the empty paragraph below the paragraph in which you entered your Scala script. In this paragraph, you are going to author a SQL query to view the results of the table you just created. In order to switch from running Scala code, to running SQL, your first line in the paragraph must start with %. This code can be copied from Listing 2 from [http://aka.ms/sparkcode](http://aka.ms/sparkcode).

%%sql

SELECT \* FROM FlightDelays

1. Click on the **Table** button.
2. You should see the results appear in a table form similar to the following:

![Screenshot](images/summarize_delays_by_airport_6.png)

1. Next, you will create a table that summarizes the flight delays data. Instead of containing one row per flight, this new summary table will contain one row per origin airport at a given hour along with a count of the quantity of anticipated delays.
2. In a new paragraph below, try running the following query. This code can be copied from Listing 3 from [http://aka.ms/sparkcode](http://aka.ms/sparkcode).

%%sql

SELECT  OriginAirportCode, OriginLatLong, Month, Day, Hour, Sum(DelayPredicted) NumDelays, Avg(DelayProbability) AvgDelayProbability

FROM FlightDelays

WHERE Month = 4

GROUP BY OriginAirportCode, OriginLatLong, Month, Day, Hour

Having Sum(DelayPredicted) &gt; 1

1. Click the **Play** icon in the top of the screen to execute this code.

![Screenshot](images/summarize_delays_by_airport_7.png)

1. This query should return a table that appears similar to the following:

![Screenshot](images/summarize_delays_by_airport_8.png)

1. Since the summary data looks good, the final step is save this summary calculation as a table that we can later query using Power BI.
2. To accomplish creating the table, enter a new paragraph and add the following Scala code and run it. This code can be copied from Listing 4 from [http://aka.ms/sparkcode](http://aka.ms/sparkcode).

val summary = sqlContext.sql(&quot;SELECT  OriginAirportCode, OriginLatLong, Month, Day, Hour, Sum(DelayPredicted) NumDelays, Avg(DelayProbability) AvgDelayProbability FROM FlightDelays WHERE Month = 4 GROUP BY OriginAirportCode, OriginLatLong, Month, Day, Hour Having Sum(DelayPredicted) &gt; 1&quot;)

summary.saveAsTable(&quot;FlightDelaysSummary&quot;)

1. Click the **Play** icon in the top of the screen to execute this code.

![Screenshot](images/summarize_delays_by_airport_9.png)

1. To verify your table was successfully created, go to another new paragraph and enter and run the following query. This code can be copied from Listing 5 from [http://aka.ms/sparkcode](http://aka.ms/sparkcode).

%%sql

SELECT \* FROM FlightDelaysSummary

1. Click the **Play** icon in the top of the screen to execute this code.

![Screenshot](images/summarize_delays_by_airport_10.png)

1. You should see results similar to the following:

![Screenshot](images/summarize_delays_by_airport_11.png)

1. You can also click on the other buttons like **Pie** , **Scatter** , **Line** , **Area** , and **Bar** to view these visualizations.

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

## Exercise 7: Deploy Intelligent Web App

Duration: 20 mins

Synopsis: In this exercise, attendees will deploy an intelligent web application to Azure from GitHub. This application leverages the operationalized machine learning model that was deployed in Exercise 1 to bring action-oriented insight to an already existing business process.

### Task 1: Deploy Web App from GitHub

1. Navigate to [http://aka.ms/cortanawebdeploy](http://aka.ms/cortanawebdeploy) in your browser of choice, but where you are already authenticated to the Azure Portal.
2. Read through the README information on the GitHub page.
3. Click the **Deploy to Azure** button.

![Screenshot](images/deploy_web_app_from_github_0.png)

1. On the following page, ensure the correct **Directory** and **Subscription** are selected.
2. Select the **Resource Group** that you have been using throughout the workshop.
3. Either keep the default **Site Name** or provide one that is globally unique and then choose a **Site Location**.
4. Finally, enter the ML API and Weather API information. Recall that you recorded the ML API information back in Task 8 of Exercise 1. Also, recall that you obtained the Weather API key back in the prerequisite steps for the workshop ( [http://aka.ms/cortanasetup](http://aka.ms/cortanasetup)).
5. Click **Next** and on the following screen, click **Deploy**.
6. The page should begin deploying your application while showing you a status of what is currently happening.

![Screenshot](images/deploy_web_app_from_github_1.png)

1. After a short time, deployment will complete and you will be presented with a link to your newly deployed web application. CTRL+Click to open it in a new tab.
2. Try a few different combinations of origin, destination, date, and time. The information you are shown are the result of both the ML API you published as well as information retrieved from the Weather Underground API.

![Screenshot](images/deploy_web_app_from_github_2.png)

1. **Congratulations!** You have built and deployed an intelligent system to Azure!

## Exercise 8: Cleanup of Azure resources

Duration: 10 mins

Synopsis: In this exercise, attendees will de-provision any Azure resources that were created in support of the lab.

### Task 1: Delete Resource Group

1. Using the Azure Portal, navigate to the Resource Group that you used throughout the lab by clicking on **Resource Groups** in the left nav.

![Screenshot](images/delete_resource_group_0.png)

1. You will see the various assets created throughout the workshop, delete each one by clicking on it and then clicking the **Delete** button for each.
  1. For the App Service plan, you will first need to delete the Web App within the plan before deleting the plan itself.

![Screenshot](images/delete_resource_group_1.png)

![Screenshot](images/delete_resource_group_2.png)

## Appendix A: Alternative to Azure ML Exercise

Duration: 20 mins

Synopsis: The purpose of this exercise is the same as Exercise 1 above. However, it is included as an alternative in the case attendees fall behind or are otherwise unable to complete the exercise. The attendee will import a complete predictive experiment into a workspace that he or she already created. This exercise assumes the attendee has already completed the first task of Exercise 1.

### Task 1: Import the Completed Predictive Experiment Into Your Workspace

1. Go to [http://aka.ms/cortanaml](http://aka.ms/cortanaml). This will open a completed version of the ML predictive experiment in the Cortana Intelligence Gallery.
2. Click the **Open in Studio** button on the right side of the screen.

![Screenshot](images/import_the_completed_predictive_experiment_into_your_workspace_0.png)

1. When prompted on the next screen, select the **South Central US** region and your workspace.

![Screenshot](images/import_the_completed_predictive_experiment_into_your_workspace_1.png)

1. After the copy operation finishes, the completed ML experiment will appear in your workspace.

![Screenshot](images/import_the_completed_predictive_experiment_into_your_workspace_2.png)

1. Run the experiment. This will take 5-7 minutes.
2. When the experiment is finished running, click **Deploy Web Service**. When the deployment is complete, you will be taken to the Web Service dashboard.

![Screenshot](images/import_the_completed_predictive_experiment_into_your_workspace_3.png)

1. At this point, you are now ready to return to and complete Exercise 2. The only task remaining is Task 8.
