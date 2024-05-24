# Fabric Real Time Data Demo

## Prerequisites
To complete this demo you will need to have completed following
1. [Enabled Fabric for your organization](https://learn.microsoft.com/en-us/fabric/admin/fabric-switch)
2. [Created a new workspace that is assigned to a Fabric capacity](https://learn.microsoft.com/en-us/fabric/admin/capacity-settings?tabs=power-bi-premium#manage-your-capacity)


## Demo Instructions

### 1. Create EventHouse and KQL Database

- Navigate to your newly created workspace

- Select "New" then select "More Options"

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/80a47219-ab9a-4a21-b6e1-4819288e2b5a)

- Scroll down and select "Eventhouse"

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/d4709a3a-1599-4fd3-896a-9ea20068f41a)

- Give your event house a name
- Once the EvenhHouse is created, you will be navigated to the event house overview page. Notice that a KQL database has been created for you. This will the the storage layer for our real time streaming data.

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/368972ec-3217-46e6-9a7a-d904378076a7)

- Now let's go get some real time data!



### 2. Create EventStream

- Navigate to your newly created workspace

- Select "New" then select "More Options"

- Scroll down and select "Eventstream"

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/7de254c1-e194-477c-b15b-9fc210d87c7d)

- Give your Eventsream a name
- You will then be automatically navigated to the Eventstream design studio
- Seelct "Add source" and then "Sample data"

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/8730d806-b86e-4393-8c24-2862212f7812)

- Define a "Source Name" and select "Yello Taxi (high data-rate)" under "Sample data"

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/caeff7b5-3214-4b0c-90e6-329a4073f494)

- Click "Add"
- (Optional) Add a filter by clicking "Transform events or add destination" and selecting a transformation
  - Click the pencil edit icon on the transformation you selected and configure as you please
 
- Hover over the rightmost activity in the event stream and click the green '+' sign that appears. Then select "KQL Database" under "Destinations"

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/441ade90-3665-420e-b024-f373a32a411b)

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/f312f4f4-feb0-4c9b-b428-4aaf320424c3)

- Fill in the details on the blade that appears on the right and point the destination to the KQL database that was created in the previous steps.
  - Use the "Create New" option under "Destination Table"
  - When you fill in all fields it should look something like this:
 
      ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/4ca4fe61-2be5-4e11-9d10-f957cd3c71c8)

  - Click "Save"
  - Click "Publish" at the top of the Eventstream editor


 
### 3. Explore data in KQL Database

- Navigate back to the workspace and select the KQL Database created in step 1.

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/c047b1a3-f475-4b32-85bb-8985cff299b3)

- You should now see a table created by the Eventstream created in step 2. Right click on this table and s

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/318ed443-2d9d-4320-9c92-c50a056cd452)

- Right click on the table name and select "Query Table" and "Get Table Schema"
  - This will open a query editor and automatically run the query. In the results pane you will see the table schema.
 
      ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/a6c26a9f-7150-4241-904f-ff3d59f9294a)

- Try out some of these other queries
```
// Count of ingested records in 5 minute increments
TaxiData
| summarize IngestionCount = count() by bin(ingestion_time(), 5m)

// Sum of total fares in 5 minute increments
TaxiData
| extend fare_amount_decimal = todecimal(fare_amount)
| summarize FareSum = sum(fare_amount_decimal) by bin(ingestion_time(), 5m )
```



### 4. Create Real Time Dashboard

- Navigate to your workspace

- Select "New" then select "More Options"

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/80a47219-ab9a-4a21-b6e1-4819288e2b5a)

- Scroll down and select "Realtime Dashboard" and give your new real time dashboard a name.

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/ec47efd5-63ac-4033-baf4-be4e8bf511ad)

- Select "+ Add Tile"

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/d8655223-749f-494c-8126-2c5b25149059)

- Select "+ Data Source" to add a data source to your realtime dashobard

- Select the KQL database created in Step 1 in the blade that appears on the right and click "Create"

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/1774bf7b-a98b-40c1-aa47-4846cf728824)

- Copy a query from the previous step into the query window and select "Run". Then observe the results in the bottom pane

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/60740050-3817-4933-9484-b126fa78468b)

- Select "Add Visual". Select "Column Chart" under "Visual type".
- Then click "Apply Changes"

    ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/f721381b-acef-495b-9c7d-ab047945f173)

- (Optional) Create a second visual with the second query from the previous step
    


### 5. Explore the Real Time Hub

- Click on "Real-TIme hub on the left blade
   - You may need to expand your window or close some fabric objects if you do not see this option

     ![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/6387b06a-aba6-45fd-bd13-41806706f9de)



### 6. Create an alert on the data
- Select the elipsis in the top right of one of the previously created visuals in the realtime dashboard and select "Create Alert"
  
![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/076f98b2-0d5b-4ed0-8f6e-ed635ed18895)

- You should now see a table in your KQL Database that is being populated by the Eventstream

- Configure alert as such
  
![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/c8254a94-a45e-47c5-bc26-61231e13ad83)

- Navigate back to your workspace and observe the newly created Reflex object. Click on the Reflex and review the settings

![image](https://github.com/raymondes/FabricRealTimeDataDemo/assets/78865370/1582f76a-7b2c-4115-8e25-fc15e6f5f9d6)



## Congratulations you have completed this demo!
