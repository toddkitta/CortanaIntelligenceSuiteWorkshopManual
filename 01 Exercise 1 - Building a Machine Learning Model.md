# Exercise 1: Building a Machine Learning Model

Duration: 90 mins

Synopsis: In this exercise, attendees will implement a classification experiment. They will load the training data from their local machine into a dataset. Then they will explore the data to identify the primary components they should use for prediction, and use two different algorithms for predicting the classification. They will evaluate the performance of both and algorithms choose the algorithm that performs best. The model selected will be exposed as a web service that is integrated with the sample web app.

This exercise has 8 tasks:

* [Task 1: Navigate to Machine Learning Studio](#task-1-navigate-to-machine-learning-studio)
* [Task 2: Upload the Sample Datasets](#task-2-upload-the-sample-datasets)
* [Task 3: Start a New Experiment](#task-3-start-a-new-experiment)
* [Task 4: Prepare the Weather Data](#task-4-prepare-the-weather-data)
* [Task 5: Join the Flight and Weather Datasets](#task-5-join-the-flight-and-weather-datasets)
* [Task 6: Train the Model](#task-6-train-the-model)
* [Task 7: Operationalize the Experiment](#task-7-operationalize-the-experiment)
* [Task 8: Note Web Service Integration Information](#task-8-note-web-service-integration-information)

## Task 1: Navigate to Machine Learning Studio

1. In your browser, go to [https://studio.azureml.net](https://studio.azureml.net) and log in using the same account you used in the Azure portal to deploy the prerequisites for this workshop.
2. Once you are signed in, ensure the workspace that was created as part of the prerequisites is selected from the top bar.

## Task 2: Upload the Sample Datasets

1. Before you begin creating a machine learning experiment, there are three datasets you need to load.
2. Download the three CSV sample datasets from here: http://aka.ms/awtdata and save AdventureWorksTravelDatasets.zip to your Desktop.
  - **Note:** You will need to unblock the zip file before extracting its files. Do this by right clicking on it, selecting **Properties**, and then unblocking the file in the resulting dialog.
3. Extract the ZIP and verify you have the following files:
  - FlightDelaysWithAirportCodes.csv
  - FlightWeatherWithAirportCodes.csv
  - AirportCodeLocationClean.csv
4. Click **+ NEW** at the bottom, point to **Dataset** , and select **From Local File**.

    ![Screenshot](images/upload_the_sample_datasets_0.png)

5. In the dialog that appears, click **Choose File** and browse to the **FlightDelaysWithAirportCodes.csv** file and click **OK**.
6. Change the name of the dataset to **FlightDelaysWithAirportCodes**.
7. Click on the check mark on the bottom right corner of the screen.

    ![Screenshot](images/upload_the_sample_datasets_1.png)

8. Repeat the previous step for the **FlightWeatherWithAirportCodes.csv** and **AirportCodeLocationLookupClean.csv** setting the name for the dataset in a similar fashion.

## Task 3: Start a New Experiment

1. Click **+ NEW** in the command bar.
2. In the options that appear, click **Blank Experiment**.

    ![Screenshot](images/start_a_new_experiment_0.png)

3. Give your new experiment a name, such as **AdventureWorks Travel** by editing the label near the top of the design surface.

    ![Screenshot](images/start_a_new_experiment_1.png)

4. In the toolbar on the left, in the Search experiment items box, type the name of the dataset you created with flight delay data (FlightDelaysWithAirportCodes). You should see a component for it listed under **Saved Datasets** -> **My Datasets**.

    ![Screenshot](images/start_a_new_experiment_2.png)

5. Click and drag on the **FlightDelaysWithAirportCodes** to add it to the design surface.

    ![Screenshot](images/start_a_new_experiment_3.png)

6. Next, you will explore each of the datasets to understand what kind of cleanup (aka data munging) will be necessary.
7. Hover over the output port of the **FlightDelaysWithAirportCodes** dataset.

    ![Screenshot](images/start_a_new_experiment_4.png)

8. Right click on the port and select **Visualize**.

    ![Screenshot](images/start_a_new_experiment_5.png)

9. A new dialog will appear showing a maximum of 100 rows by 100 columns sample of the dataset. You can see at the top that the dataset has a total of 2,719,418 rows (also referred to as examples in Machine Learning literature) and has 20 columns (also referred to as features).

    ![Screenshot](images/start_a_new_experiment_6.png)

10. Because all 20 columns are displayed, you can scroll the grid horizontally. Scroll until you see the **DepDel15** column and click it to view statistics about the column. The **DepDel15** column displays a 1 when the flight was delayed at least 15 minutes and 0 if there was no such delay. In the model you will construct, you will try to predict the value of this column for future data.

    ![Screenshot](images/start_a_new_experiment_7.png)

11. Notice in the Statistics panel that a value of 27444 appears for Missing Values. This means that 27,444 rows do not have a value in this column. Since this value is very important to our model, we will eliminate any rows that do not have a value for this column.
12. To eliminate these problem rows, close the dialog and go back to the design surface. From the toolbar, search for **Clean Missing Data**.

    ![Screenshot](images/start_a_new_experiment_8.png)

13. Drag this module on to the design surface beneath your **FlightDelaysWithAirportCodes** dataset. Click the small circle at the bottom of the **FlightDelaysWithAirportCodes** dataset, drag and release when your mouse is over the circle found in the top center of the Clean Missing Data module. These circles are referred to as ports, and by taking this action you have connected the output port of the dataset with the input port of the Clean Missing Data module, which means the data from the dataset will flow along this path.

    ![Screenshot](images/start_a_new_experiment_9.png)

14. Click **Save** on the command bar at the bottom to save your in-progress experiment.

    ![Screenshot](images/start_a_new_experiment_10.png)

15. Click **Run** in the command bar at the bottom to run the experiment.

    ![Screenshot](images/start_a_new_experiment_11.png)

16. When the experiment is finished running, you will see a finished message in the top right corner of the design surface, and green check marks over all modules that ran.

    ![Screenshot](images/start_a_new_experiment_12.png)

17. You should run your experiment whenever you need to update the metadata describing what data is flowing through the modules, so that newly added modules can be aware of the shape of your data (most module have dialogs that can suggest columns, but before they can make suggestions you need to have run your experiment).
18. Click the **Clean Missing Data** module to select it. The property panel on the right will display the settings appropriate to the selected module.
19. In this case, we want to remove rows that have no value for the DepDelay15 column. Begin by clicking **Launch Column Selector**.

    ![Screenshot](images/start_a_new_experiment_13.png)

20. Ensuring **With Rules** is selected on the left side of the dialog, under the **Begin With** section, select **No Columns**. In the row of controls that appears, change the second drop down to **Column Names**. Then in the text box that appears begin to type **DepDel15** and select that item from type-ahead list.

    ![Screenshot](images/start_a_new_experiment_14.png)

21. Click the checkmark to apply the settings. You have now indicated to the Clean Missing Data module that DepDel15 is the only column it should act on.

    ![Screenshot](images/start_a_new_experiment_15.png)

22. In the **Properties** panel for **Clean Missing Data** , click the **Cleaning mode** drop-down and select **Remove entire row**. Now your Clean Missing Data module is fully configured to remove any rows that are missing values for DepDel15.

    ![Screenshot](images/start_a_new_experiment_16.png)

23. To verify the result, run your experiment again. After it is finished, click the output port (1) of the Clean Missing Data module and select **Visualize**.
24. In the dialog that appears, scroll over to **DepDel15** and click the column. In the statistics you should see that Missing Values reads 0.

    ![Screenshot](images/start_a_new_experiment_17.png)

25. Our model will approximate departure times to the nearest hour, but departure time is captured as an integer. For example, 8:37 am is captured as 837. Therefore, we will need to process the CRSDepTime column and round it down to the nearest hour.
26. To perform this rounding will require two steps, first you will need to divide the value by 100 (so that 837 becomes 8.37). Second, you will round this value down to the nearest hour (so that 8.37 becomes 8).
27. Begin by adding an **Apply Math Operation** module beneath the Clean Missing Data module and connect the output port (1) of the Clean Missing Data module to the input port of the Apply Math Operation.

    ![Screenshot](images/start_a_new_experiment_18.png)

28. In the properties of the Apply Math Operation, set the Category to **Operations** , Basic operation to **Divide** , Operation argument type to **Constant** , Constant operation argument to **100** , Selected columns to **CRSDepTime** (see screenshot below), and Output mode to **Append**.

    ![Screenshot](images/start_a_new_experiment_19.png)

    ![Screenshot](images/start_a_new_experiment_20.png)

29. Run the experiment to update the metadata.
30. This module will add a new column to the dataset called Divide(CRSDeptTime\_$100), but we want to rename it to CRSDepHour. To do so, add an **Edit Metadata** module and connect its input port to the output port of Apply Math Operation.

    ![Screenshot](images/start_a_new_experiment_21.png)

31. For the properties of the **Edit Metadata** , set the Selected Columns to **Divide(CRSDeptTime\_$100)** and New column names to **CRSDepHour**.

    ![Screenshot](images/start_a_new_experiment_22.png)

    ![Screenshot](images/start_a_new_experiment_23.png)

32. Run the experiment to update the metadata.
33. Add another **Apply Math Operation** module to round the time down to the nearest hour. Set the Category to **Rounding** , Selected columns to **CRSDepHour** (see screenshot for how to select), and Output mode to **Inplace**.

    ![Screenshot](images/start_a_new_experiment_24.png)

    ![Screenshot](images/start_a_new_experiment_25.png)

34. Run the experiment to update the metadata.
35. We do not need all of the columns present in the FlightDelaysWithAirportCodes dataset. To pare down the columns we can use multiple options, but in this case we chose to use an R Script module that selects out only the columns of interest using R code.
36. Add an **Execute R Script** module beneath the last Apply Math Operation, and connect the output of the Apply Math Operation to the first input port (leftmost) of the Execute R Script.

    ![Screenshot](images/start_a_new_experiment_26.png)

37. In the **Properties** panel for Execute R Script, click the **Double Windows** icon to maximize the script editor.

    ![Screenshot](images/start_a_new_experiment_27.png)

38. Replace the default script with the following and click the checkmark to save it (press CTRL+A to select all of the text then CTRL+V to paste and then immediately click the checkmark. Don&#39;t worry if the formatting is off before hitting the checkmark.

    ``` R
    ds.flights <- maml.mapInputPort(1)
    # Trim the columns to only those we will use for the predictive model
    ds.flights = ds.flights[, c("OriginAirportCode","OriginLatitude", "OriginLongitude", "Month", "DayofMonth", "CRSDepHour", "DayOfWeek", "Carrier", "DestAirportCode", "DestLatitude", "DestLongitude", "DepDel15")]
    maml.mapOutputPort("ds.flights");
    ```

39. Run the experiment to update the metadata (this may take a minute or two to complete).
40. Right-click on the first output port of your Execute R Script module and select **Visualize**.
41. Verify that the dataset only contains the 12 columns referenced in the R script.

    ![Screenshot](images/start_a_new_experiment_28.png)

42. At this point the Flight Delay Data is prepared, and we turn to preparing the historical weather data.

## Task 4: Prepare the Weather Data

1. To the right of the FlightDelaysWithAirportCodes dataset, add the **FlightWeatherWithAirportCodes** dataset.

    ![Screenshot](images/prepare_the_weather_data_0.png)

2. Right-click the output port of the FlightWeatherWithAirportCodes dataset and select **Visualize**.

    ![Screenshot](images/prepare_the_weather_data_1.png)

3. Observe that this data set has 406,516 rows and 29 columns. For this model, we are going to focus on predicting delays using WindSpeed (in MPH), SeaLevelPressure (in inches of Hg), and HourlyPrecip (in inches). We will focus on preparing the data for those features.
4. In the dialog, click the **WindSpeed** column and review the statistics. Observe that the Feature Type was inferred as String and that there are 32 Missing Values. Below that, examine the histogram to see that, even though the type was inferred as string, the values are all actually numbers (e.g. the x-axis values are 0, 6, 5, 7, 3, 8, 9, 10, 11, 13). We will need to ensure that we remove any missing values and convert WindSpeed to its proper type as a numeric feature.

    ![Screenshot](images/prepare_the_weather_data_2.png)

5. Next, click the **SeaLevelPressure** column. Observe that the Feature Type was inferred as String and there are 0 Missing Values. Scroll down to the histogram, and observe that many of the features of a numeric value (e.g., 29.96, 30.01, etc.), but there are many features with the string value of M for Missing. We will need to replace this value of M&quot;with a suitable numeric value so that we can convert this feature to be a numeric feature.

    ![Screenshot](images/prepare_the_weather_data_3.png)

6. Finally, examine the **HourlyPrecip** feature. Observe that it too was inferred to have a Feature Type of String and is missing values for 374,503 rows. Looking at the histogram, observe that besides the numeric values, there is a value T for Trace amount of rain). We will need to replace the T with a suitable numeric value and covert this feature to a numeric feature.

    ![Screenshot](images/prepare_the_weather_data_4.png)

7. Let us begin by cleaning up the missing values for both WindSpeed and HourlyPrecip.
8. Below the FlightWeatherWithAirportCode dataset, drop a **Clean Missing Data** module and connect the output of the dataset to the input of module.

    ![Screenshot](images/prepare_the_weather_data_5.png)

9. Run the experiment to update the metadata available to the Clean Missing Data module.
10. In the **Properties** panel for Clean Missing Data, set the Selected columns to **HourlyPrecip** and **WindSpeed** (see screenshot for help selecting if needed), set the Cleaning mode to **Custom substitution value** and set the Replacement value to **0.0**.

    ![Screenshot](images/prepare_the_weather_data_6.png)

    ![Screenshot](images/prepare_the_weather_data_7.png)

11. Next, add an **Execute R Script** module below the Clean Missing Data module and connect the first output port of the former to the first input port of the latter.

    ![Screenshot](images/prepare_the_weather_data_8.png)

12. In the **Properties** panel for the Execute R Script, click the &quot;double windows&quot; icon to open the script editor.
13. Paste in the following script and click the checkmark (press CTRL+A to select all of the text then CTRL+V to paste and then click the checkmark. Don&#39;t worry if the formatting is off). This script replaces the HourlyPrecip values having T with 0.05, WindSpeed values of M with 0.0, and the SeaLevelPressure values of M with the global average pressure of 29.92. It also narrows the dataset to just the few feature columns we want to use with our model.

    ``` R
    ds.weather <- maml.mapInputPort(1)

    # Round weather time up to the next hour since
    # that's the hour for which we want to use flight data
    ds.weather$Hour = ceiling(ds.weather$Time / 100)

    # Replace any WindSpeed values of "M" with 0.005 and make the feature numeric
    speed.num = ds.weather$WindSpeed
    speed.num[speed.num == "M"] = 0.005
    speed.num = as.numeric(speed.num)
    ds.weather$WindSpeed = speed.num 

    # Replace any SeaLevelPressure values of "M" with 29.92 (the average pressure) and make the feature numeric
    pressure.num = ds.weather$SeaLevelPressure
    pressure.num[pressure.num == "M"] = 29.92
    pressure.num = as.numeric(pressure.num)
    ds.weather$SeaLevelPressure = pressure.num 

    # Adjust the HourlyPrecip variable (convert "T" (trace) to 0.005)
    rain = ds.weather$HourlyPrecip
    rain[rain %in% c("T")] = "0.005"
    ds.weather$HourlyPrecip = as.numeric(rain)

    # Pare down the variables in the Weather dataset
    ds.weather = ds.weather[, c("AirportCode", "Month", "Day", "Hour", "WindSpeed", "SeaLevelPressure", "HourlyPrecip")]

    maml.mapOutputPort("ds.weather");
    ```

14. Run the experiment. Currently it should appear as follows:

    ![Screenshot](images/prepare_the_weather_data_9.png)

15. Click the first output port of the Execute R Script module and select **Visualize**.
16. In the statistics, verify that WindSpeed, SeaLevelPressure, and HourlyPrecip are now all Numeric Feature types and that they have no missing values.

## Task 5: Join the Flight and Weather Datasets

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

## Task 6: Train the Model

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

## Task 7: Operationalize the Experiment

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

## Task 8: Note Web Service Integration Information

1. From the Web Service dashboard, copy the API key, open a copy of Notepad.exe on your local workstation, and paste the value in the editor.
2. On the Web Service dashboard, click the **REQUEST/RESPONSE** link.
3. Scroll down until you see the first Request URL. From this URL, you can get the Workspace ID and the Service ID. The URL will look something like the following:
    * https://ussouthcentral.services.azureml.net/workspaces/f8a24adcde7c45e1bb1c9ef85fe36f62/services/656c53c99cc848e1a476f5068ba39fb6/execute?api-version=2.0&amp;details=true
1. The first GUID after workspaces is your Workspace ID. The second GUID after services is your Service ID.
2. Copy each of these values into Notepad as well. Make sure you note which GUID is which because you will need these in a later step.
