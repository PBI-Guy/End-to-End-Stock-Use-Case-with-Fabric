## Let's get started
Once all prerequisites are met, you can log in to GitHub and access following [GitHub Repo.](https://github.com/PBI-Guy/End-to-End-Stock-Use-Case-with-Fabric) From there, press the button to create a fork.

<img src="./PNG/05%20Create%20a%20GitHub%20Fork.png" width="500">

Choose the owner if you have multiple and give a new repo name if you wish. Per default, the upstream repo (Fabric-Event) is used. Once done, hit the **Create fork** button and wait until it's synced. 

<img src="./PNG/06%20Create%20Fork.png" width="500">

### Create a personal access token in GitHub
As a next step, we need to create a personal access token for our GitHub which will be used afterwards to sync our Workspace with the GitHub repo. To create one, click on the **top right user icon** and select **Settings**

<img src="./PNG/07%20GitHub%20User%20Settings.png" width="500">

Scroll down and select **Developer settings**. From there, choose **Personal access tokens** and select **Fine-grained tokens**. Lastly, click the green **Generate new token** button.

<img src="./PNG/08%20Create%20Personal%20Access%20Token%20GitHub.png" width="500">

Next, you need to provide a Token name, e.g. _Fabric Event_, and give access to the repository. Make sure to only give access to the Fabric-Event repo we just forked. 

<img src="./PNG/09%20Token%20repo%20access.png" width="500">

Make ksure to provide Read and Write permissions on Contents.

<img src="./PNG/9.1%20Token%20permission.png" width="500">

Once everything configured, press the **Generate token** button at the bottom of the page. A message will pop up summarizing the configuration. Confirm by pressing the **Generate token** button.


<img src="./PNG/10%20Token%20generation.png" width="500">

Make sure to copy and store the access token securely as you will not be able to see it again once you leave this page. 


<img src="./PNG/11%20Copy%20Access%20Token.png" width="500">

### Sync GitHub repo with a Fabric workspace

As a final step, we need to sync now the GitHub repo with our Workspace. Open Microsoft Fabric, navigate to your workspace and press the **Workspace settings** at the top right. Now, select Git integration, choose GitHub and press the **Add account** button.

<img src="./PNG/12%20Workspace%20Git%20Integration%20Config.png" width="500">

In the pop-up window, give a Display Name and past your GitHub Personal Access Token which we created before. Also, provide the GitHub repo URL which has following structure: https://github.com/_YourGitHubUser_/End-to-End-Stock-Use-Case-with-Fabric, e.g. https://github.com/PBI-Guy/End-to-End-Stock-Use-Case-with-Fabric Hit **Add**.

<img src="./PNG/13%20Add%20GitHub%20account.png" width="500">

After the account has been successfully added, you can select **Connect**. Now, you have to select the main branch, and add **Workspace Items to the Git folder**. Lasty, click **Connect and sync**.

| Branch | Git folder |
|--------------|----------|
| main | Workspace Items |

<img src="./PNG/14%20Configure%20Repo.png" width="500">

Now, all Fabric items from the GitHub repo will be synced into the workspace. This can take a few minutes. You can follow the process at the top right corner.

Once all items have successfully synced and your workspace looks like on the picture below, we need to configure a few of them and create two Pipelines to orchestrate and ingest data. 

<img src="./PNG/14 1 Successfull GitHub Sync.png" width="500">


## Configure all items

### Spark Environment

Let's start with our Spark Environment config by selecting the **ENV_Spark_Stock_Use_Case** item. 

<img src="./PNG/19%20Spark%20Environment.png" width="500">

In the navigation pane on the left hand side, select **Public Library** and hit **Add from PyPI**. Search for **yahoo-fin** and make sure the version is at least **0.8.9.1**. Add a new library by selecting the **+ Add from PyPI** button at the top and search for yfinance. Make sure the version is at least **0.2.55**. 

<img src="./PNG/20%20Add%20Public%20Libraries.png" width="500">

Now, hit the **Publish** button at the top right and confirm by selecting **Publish all**. In the confirmation screen hit again **Publish**. This will take a few minutes to process.

<img src="./PNG/21%20Publish%20Spark%20changes.png" width="500">

### Notebooks and Eventhouse

In our next step, we're going to configure our Notebooks. Return to the Workspace overview by selecting the name in the left navigation pane. Select **02 Get Company Details** to open it and select Lakehouses in the Explorer. 

<img src="./PNG/15%20Select%20Lakehouse.png" width="500">

You'll see a "Missing Lakehouse" followed by a number. Select the two arrows next to it and choose **Remove all lakehouses**. In the pop up window just confirm by hitting the **Continue** button.

<img src="./PNG/16%20Remove%20Missing%20Lakehouse.png" width="500">

Next, let's add our existing Lakehouse to the Notebook by selecting **+ Lakehouses** or the green **Add** button in the explorer. In the pop up window, select **Existing lakehouse(s) without Schema** and continue by pressing the **Add** button.

<img src="./PNG/17%20Add%20Lakehouse%20to%20Notebook.png" width="500">

In the next pop up window, select the LH_Stocks Lakehouse and hit **Add**. Now, let's run the Notebook manually to test if it works. Just hit the Run all button in the ribbon and wait until suceeded. After the script run successfully, you can refresh the Tables view in the Lakehouse Explorer and see if the **bronze_company** data has been created.

<img src="./PNG/22%20Refresh%20Tables%20in%20Lakehouse.png" width="500">

Once finished, let's configure the other Notebook **01 Get Stock Details** by following the same steps to remove all existing Lakehouses connected to the Notebook and add the same Lakehouse to it. For this Notebook, we need the Eventhouse Cluster URL. For that, we can head back to our Workspace overview and select the **EH_Stocks** KQL Database.

<img src="./PNG/23%20KQL%20Database.png" width="500">

In the next screen, we need to copy the **Query URI**.

<img src="./PNG/24%20Copy%20Query%20URL.png" width="500">

Before we head back to our Notebook to add the Ingestion URI, we want to automatically copy our Tables & Data from our Eventhouse to OneLake. To do so, make sure to select the Kust Database Name and enable the OneLake Availability toggle.

<img src="./PNG/25%20Enable%20OneLake.png" width="500">

Lastly, we also want to create a shortcut in our Eventhouse to our **bronze_company** table sitting in our Lakehouse. To do so, we hit the **+ New** button in the ribbon and select **OneLake shortcut**. 

<img src="./PNG/26%20Eventhouse%20Shortcut%20to%20Lakehouse.png" width="500">

In the next screen, we select **Microsoft OneLake** and choose our **LH_Stocks** Lakehouse. By hitting **Next** we proceed to the next window in which we expand the Tables section and select the **bronze_company** table. By selecting **Next** we get to the overview page in which we **disable accelerated shortcuts** and confirm by selecting **Create**. After a few seconds a confirmation screen will appear.

<img src="./PNG/27%20Select%20Bronze%20Company%20Shortcut.png" width="500">


Now we go back to the **01 Get Stock Details** Notebook, go to the third cell, and replace the kustoCluster parameter.

<img src="./PNG/28%20Replace%20Kusto%20URL.png" width="500">

Let's test if our Notebook runs successfully by hitting the **Run all** button in the ribbon. After a few seconds, the job will finish and we can check if some data has arrived in our Eventhouse by selecting the Bronze_Stock table.

<img src="./PNG/29%20Check%20Eventhouse%20Data.png" width="500">

### Power BI Report and Semantic Model

Our next step is now to configure the Semantic Model to make sure the Power BI Report will run. For that, we head back to our workspace overview and select the three dots next to the Semantic Model and select **Settings**.

<img src="./PNG/30%20Semantic%20Model%20Settings.png" width="500">

In the **Settings** overview, we expand the Parameters section and replace the **Kusto Cluster URI** with our KQL Database Query URI which we copied a few steps before. We save the change by selecting the **Apply** button.

<img src="./PNG/31%20Replace%20Kusto%20Cluster%20URI%20Parameter.png" width="500">

Next, we edit the data source credentials by expanding the **Data source credentials** menu and select **Edit credentials**. 

<img src="./PNG/32%20Edit%20Sem.%20Model%20Credentials.png" width="500">

Select **Organizational** as Privacy level setting and proceed with **Sign in**.

<img src="./PNG/33%20Set%20Privacy%20level.png" width="500">

Our report is now ready to use. As we haven't configured our pipeline yet to ingest and transform the data as needed, therefore the report is empty. 

### Database

Before configuring our Pipeline, we need to make sure our meta information is stored in a SQL Database. We will create one in Microsoft Fabric to leverage the unified paltform but if you don't see an option to create a SQL DB in Fabric (not all regions are supported at the time of writing this guide), you can also create an Azure DQL Database outside of Fabric and connect to it as workaround. 

If we go back to our workspace overview and select **+ New Item** at the top left, we can search for SQL Database and select it. 

<img src="./PNG/34%20Create%20Fabric%20SQL%20DB.png" width="500">

We're going to call it **SQLDB_StockMetaData** and proceed with the **Create** button.

<img src="./PNG/35%20SQL%20DB%20Naming.png" width="500">

Select **New Query** in the Ribbon and paste [this SQL script](./SQL%20Script/Create%20objects%20in%20SQL%20DB.sql) to create all necessary schemas, tables, and stored procedures. 

<img src="./PNG/36%20Execute%20SQL%20Script%20to%20create%20objects.png" width="500">

Lastly, we need to load some initial data. For that, we're going to execute our LoadTransferObject stored procedure and check with a select statement if we see the values in the table. Create a new query, paste the below code and execute it.

```
EXECUTE [Helper].[LoadTransferObject] 
GO

SELECT [SourceObjectName]
      ,[Batch]
      ,[LoadMode]
      ,[Api]
      ,[Stockmarket]
      ,[LoadStatus]
      ,[WatermarkEH]
      ,[Enabled]

FROM [Core].[TransferObject]
; 
```

We should see now some data like in the picture below.

<img src="./PNG/37%20Load%20initial%20data.png" width="500">

This is our core table which defines which tickers we're interested in, what the current Watermark is and if it's enabled for loading. If new stock data should be loaded, we have to insert it into this table and everything else will happen automatically once the pipeline runs. So, we're now ready to configure our meta-data driven pipeline.


### Pipelines

We will start with the **Get Company Details** pipeline by going back to our workspace overview and clicking on the name of the pipeline. In here, we select the **Lookup** activity, go to **Settings**, select the **Connection** drop down, and hit **More** to create a new connection to our SQL DB.

<img src="./PNG/38%20Configure%20GetMetaDataObjects%20Connection.png" width="500">

In the wizard, we select the **SQLDB_StockMetaData** from the OneLake catalog and a connection will be created.

<img src="./PNG/39%20Select%20SQL%20DB%20from%20OneLake%20Catalog.png" width="500">

Once the connection is established, click on **Stored procedure** in the Use query option and select the **[Core].[GetGetMetadataObjects]** stored procedure.

<img src="./PNG/40%20Lookup%20GetMetaDataObjects%20settings.png" width="500">

We have now successfully configured our Lookup activity and can already run the pipeline. To do so, select the **> Run** button at the top and confirm by selecting **Save and run**.

<img src="./PNG/41%20Save%20and%20Run%20Get%20Company%20Details%20Pipeline.png" width="500">

While the pipeline is running, you can check out the other two activities - Executing the Get Company Details Notebook and the Changed_CompanyInfo KQL statement. In the settings section of each you'll see the connection is already established and the Notebook resp. KQL statement is also there. 

To make sure our pipeline run successfully and data has been loaded, we can either check the bronze_company table in our Lakehouse, our shortcut in our Eventhouse, or the Silver_Company table which gets loaded by calling the function in our last activity of the pipeline. I'm checking the data in the Eventhouse with following KQL query.

`Silver_Company`

<img src="./PNG/42%20Select%20Silver_Company%20talbe%20in%20EH.png" width="500">

Now, let's configure the next pipeline by heading back to our workspace overview and select the **Get Stock Details** pipeline. In there, we select the **Get_Ticker_List** Lookup activity, go to **Settings** and choose our existing FabrcSQL (or Azure SQL if no Fabric SQL DB has been created) connection.

<img src="./PNG/43%20Configure%20Get_Ticker_List%20Lookup%20Connection.png" width="500">

Next, we select the SQL Database **SQLDB_StockMetaData**, choose **Stored procedure** and configure the **[Core].[GetLoadObjects] stored procedure.

<img src="./PNG/44%20Lookup%20Get_Ticker_List%20settings.png" width="500">

As last step, we need to configure the ForeEach loop activity by selecting the pencil icon.

<img src="./PNG/45%20Edit%20ForEach%20loop.png" width="500">

Please make sure to configure following activities as described in the table below. Once the stored procedure is selected, hit the **Import parameter** button to get the parameters automatically. The screen shot below shows the Start Transfer activity.

| Activity | Connection | SQL Database | Use query | Stored Procedure |
|--------------|----------|--------------|----------|----------|
| Start Transfer | FabricSql | SQLDB_StockMetaData | Stored procedure | [Core].[StartTransfer] |
| EHDatabaseLoaded | FabricSql | SQLDB_StockMetaData | Stored procedure | [Core].[EHDatabaseLoaded] |
| ErrorInTransfer | FabricSql | SQLDB_StockMetaData | Stored procedure | [Core].[ErrorInTransfer] |

<br>

| Activity | Parameter Name | Type | Value |
|--------------|----------|--------------|----------|
| Start Transfer | HighWatermark | Datetime | @item().HighWaterMark
| Start Transfer | PipelineRunId | String | @pipeline().RunId
| Start Transfer | SourceObjectName | String | @item().SourceObjectName
| EHDatabaseLoaded | RowsInsertedKQL | Int32 | @activity('Get Stock Details').output.result.exitValue
| EHDatabaseLoaded | SourceObjectName | String | @item().SourceObjectName
| EHDatabaseLoaded | StartTime | Datetime | @activity('Start Transfer').output.firstRow.StartTime
| ErrorInTransfer | ErrorMessage | String | @activity('Get Stock Details').output.result.error.evalue
| ErrorInTransfer | SourceObjectName | String | @item().SourceObjectName
| ErrorInTransfer | SparkMonitoringURL | String | @activity('Get Stock Details').output.SparkMonitoringURL
| ErrorInTransfer | StartTime | Datetime | @activity('Start Transfer').output.firstRow.StartTime

<br>

<img src="./PNG/46%20Start%20Transfer%20Activity%20settings.png" width="500">

In order to minimize the amount of time it takes to execute the notebook job, you could optionally set a session tag. Setting the session tag will instruct Spark to reuse any existing Spark session thereby minimizing the startup time. Any arbitrary string value can be used for the session tag. If no session exists a new one would be created using the tag value. To configure a session tag, select the Notebook activity, go to **Settings**, expand **Advanced settings** and add a session tag.

<img src="./PNG/46%201%20Session%20Tag.png" width="500">

To make sure our pipeline can run multiple Notebooks in high concurrency, we need to enable this setting in the Workspace settings. Go back to the Workspace overview, select **Workspace settings**, expand **Data Engineering/Science**, go to **High concurrency**, and enable **For pipeline running multiple notebooks**. Hit the **Save** button to save the changes.

<img src="./PNG/46%202%20Workspace%20Settings%20Spark.png" width="500">

Before we run our pipeline, we need to add one last action to the pipeline to refresh the Semantic Model because our Date dimension is a calculated table and needs to be processed. Head back to the pipeline and select **Activities** in the Ribbon and choose the **Semantic Model refresh** activity. Position it to the right of the ForEach activity and connect them by drag and dropping the **On success** line.

<img src="./PNG/48%20Add%20Semantic%20Model%20Refresh%20Activity.png" width="500">

Lastly, rename and configure the activity as follow:

| Property | Value |
|--------------|----------|
| Name | Refresh Semantic Model |
| Connection | _Create new Power BI Semantic Model connection_ |
| Semantic model | Stock Use Case Analysis |
| Max parallelism | 4 |

<img src="./PNG/49%20Sem.%20Model%20Properties.png" width="500">

The Max parallelism property parallelize the refresh of tables and decrease therefore the refresh time (improving performance) of the Semantic Model. The **Table(s)** property allows us to refresh only a specific table or partition if wished, which is not necessary in our case.

Let's test if the pipeline runs successfully by selecting the **> Run** button in the Ribbon and confirm by selecting **Save and run**. As we're loading now data for multiple tickers starting from 2020 until today, this will take a few minutes to complete. In the meantime, we can configure a schedule for the two pipelines. To do so, let's go back to our Workspace overview, select the three dots next to the pipeline, and hit **Schedule**. 

<img src="./PNG/50%20Get%20Stock%20Details%20Schedule.png" width="500">

Now, configure the schedule as you wish. For example, on a daily base at 7am starting from 3rd April 2025 until end of 2030. Confirm with the **Apply** button. Repeat the same steps for the other pipeline to configure a schedule as well.

<img src="./PNG/51%20Schedule%20configuration.png" width="500">

Let's see if our job completed successfully by going back to the pipeline and check out the **Output** tab. Once successfully completed, we have finished the setup of the environment! If we open our report, we'll see some data in there.

<img src="./PNG/Slide4.PNG" width="500">

As a last, bonus step we want to add now further companies, for example Google (GOOG), Nvidia (NVDA), and Apple (APPL). We need to modify just one Stored Procedure **[Helper].[LoadTransferObject]** and update the **enabled** value for each of them. We could also write and **update** or **insert** statement to put further companies into our **[Core].[TransferObject]** table, but this time we modify the stored procedure showcasing that we also consider existing entries and do not overwrite the Watermark for example. You can find the needed script [here](./SQL%20Script/Update%20Stock%20Table%20through%20Stored%20Procedure.sql). Just run it in your SQL Database, execute the stored procedure once successfully modified, and start afterwards the **Get Company Details** and **Get Stock Details** pipeline. Once finished, you should see more companies in your Power BI report.