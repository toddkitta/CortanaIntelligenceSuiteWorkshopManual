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

    ```json
    {
        "name": "AzureMLLinkedService",
        "properties": {
            "type": "AzureML",
            "description": "",
            "typeProperties": {
                "mlEndpoint": "<Specify the batch scoring URL>",
                "apiKey": "<Specify the published workspace model's API key>"
            }
        }
    }
    ```
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

    ```json
    {
        "name": "AzureBlobDataInPut",
        "properties": {
            "type": "AzureBlob",
            "external": true,
            "linkedServiceName": "OutputLinkedService-AzureBlobStorage",
            "typeProperties": {
                "fileName": "FlightsAndWeather.csv",
                "folderPath": "sparkcontainer/flights",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Minute",
                "interval": 60
            }
        }
    }
    ```
1. Click Deploy.
### Task 3: Create Azure ML Scored Dataset
1. Click on **…More**.
    ![Screenshot](images/create_azure_ml_scored_dataset_0.png)
1. Click on the **New dataset**.
    ![Screenshot](images/create_azure_ml_scored_dataset_1.png)
1. Select **Azure Blob storage** from the list.
2. In the new window, be sure change the JSON file to match the following or copy the below JSON text and paste into the browser window. You can also copy this from **Listing 3** in [http://aka.ms/adfcode](http://aka.ms/adfcode).

    ```json
    {
        "name": "AzureBlobScoredDataOutPut",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "OutputLinkedService-AzureBlobStorage",
            "typeProperties": {
                "fileName": "Scored_FlightsAndWeather.csv",
                "folderPath": "sparkcontainer/flights",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Minute",
                "interval": 60
            }
        }
    }
    ```
1. Click **Deploy**.
### Task 4: Create Azure ML Predictive Pipeline
1. Click on **…More**.
    ![Screenshot](images/create_azure_ml_predictive_pipeline_0.png)
1. Click on the **New pipeline**.
    ![Screenshot](images/create_azure_ml_predictive_pipeline_1.png)
1. In the new window, be sure change the JSON file to match the following or copy the below JSON text and paste into the browser window. You can also copy this from **Listing 4** in [http://aka.ms/adfcode](http://aka.ms/adfcode).

    ```json
    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "Use AzureML model",
            "activities": [
                {
                    "type": "AzureMLBatchExecution",
                    "typeProperties": {
                        "webServiceInput": "AzureBlobDataInPut",
                        "webServiceOutputs": {
                            "output1": "AzureBlobScoredDataOutPut"
                        },
                        "globalParameters": {}
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobDataInPut"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobScoredDataOutPut"
                        }
                    ],
                    "policy": {
                        "timeout": "02:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Minute",
                        "interval": 60
                    },
                    "name": "MLActivity",
                    "description": "prediction analysis on batch input",
                    "linkedServiceName": "AzureMLLinkedService"
                }
            ],
            "start": "2016-09-14T00:00:00Z",
            "end": "2016-09-15T00:00:00Z"
        }
    }
    ```
1. Make sure to change the **start** to today&#39;s date and **end** to today + 1 date.
2. Click **Deploy**.
### Task 5: Monitor Your Pipeline Activities
1. Close the current blade by clicking on the **X** from the top right corner of the blade.
2. Click on the **Monitor &amp; Manage** from the **Actions** section.
3. Maximize the new window and you can see the diagram view of the data flow.
    ![Screenshot](images/monitor_your_pipeline_activities_0.png)
1. You should start to see **Ready** status activity listed on the bottom of the new window.
2. Close the **Monitor &amp; Manage** browser tab.
