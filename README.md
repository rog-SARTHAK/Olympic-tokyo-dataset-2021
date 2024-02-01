# Olympic-tokyo-dataset-2021

**Azure Data Engineering ETL Project Date: Jan 2024**

Github Repo: [https://github.com/rog-SARTHAK/Olympic-tokyo-dataset-2021](https://github.com/rog-SARTHAK/Olympic-tokyo-dataset-2021)

![](RackMultipart20240201-1-240zec_html_48def2d412483f8c.png)

**Architecture**

**Step 1) Data Extraction**

We go to Storage accounts

![](RackMultipart20240201-1-240zec_html_7edeb843ffd52be2.png)

Name the storage account and create a resource group

![](RackMultipart20240201-1-240zec_html_b9a007806b898d41.png)

Enable Hierarchical namespace

![](RackMultipart20240201-1-240zec_html_35ad7d13bbe3bb2a.png)

This is how the storage account looks like after deployment

![](RackMultipart20240201-1-240zec_html_888153ef33285427.png)

We will be storing our data in a container for object-storage

![](RackMultipart20240201-1-240zec_html_92b41b421c3204d5.png)

Create two directories, one for storing raw data and other for storing transformed data

![](RackMultipart20240201-1-240zec_html_8a0b26636fe4bcc2.png)

Open Data Factory

![](RackMultipart20240201-1-240zec_html_555d2ce030c0704f.png)

![](RackMultipart20240201-1-240zec_html_8be755b29a71b110.png)

![](RackMultipart20240201-1-240zec_html_73d22d8f628128d1.png)

Azure Data Factory console:

![](RackMultipart20240201-1-240zec_html_957b5ea78ed217ec.png)

To create the pipeline,

Go to Author -\> Add new resource -\> Pipeline

![](RackMultipart20240201-1-240zec_html_5a6174c54a43e6d.png)

Expand Move and transform -\> Drag drop Copy data into canvas

We will do a Copy-Data from Data source to Data storage

We have uploaded our dataset from Kaggle to Github repo in CSV format

[https://github.com/rog-SARTHAK/Olympic-tokyo-dataset-2021/upload/main](https://github.com/rog-SARTHAK/Olympic-tokyo-dataset-2021/upload/main)

[https://www.kaggle.com/datasets/arjunprasadsarkhel/2021-olympics-in-tokyo?resource=download](https://www.kaggle.com/datasets/arjunprasadsarkhel/2021-olympics-in-tokyo?resource=download)

Copy the raw link of the Atheletes.csv from Github

Azure Data Factory can source data from a huge variety of platforms

![](RackMultipart20240201-1-240zec_html_752f68d02d8694c.png)

Search HTTP in New Source Dataset

![](RackMultipart20240201-1-240zec_html_152dbe72dad306ee.png)

Click on continue and specify the file format. Ours is csv

![](RackMultipart20240201-1-240zec_html_c929dadd3b564491.png)

Next we create a new linked service

![](RackMultipart20240201-1-240zec_html_d074f75fd4ee288e.png)

Paste the URL in Linked Service

Click on Preview Data to view

![](RackMultipart20240201-1-240zec_html_afdd5dff99721c31.png)

After setting the "Source", we will next configure the "Sink".

Sink will basically load data to Azure Data Lake storage.

![](RackMultipart20240201-1-240zec_html_d952a1d9d54e292b.png)

Select Azure Data Lake Storage Gen 2 as Sink

![](RackMultipart20240201-1-240zec_html_2cda41607c8577f6.png)

Provide a name

![](RackMultipart20240201-1-240zec_html_fff60b6cc7f3a0da.png)

Select storage account name created earlier as Gen 2 storage destination. Here it is tokyoolympicstorageacc. Also choose a subscription.

When we repeat the step for other file ingestion we can re use the same linked service in sink.

![](RackMultipart20240201-1-240zec_html_4a42571ec444d4bc.png)

Next select the file path

![](RackMultipart20240201-1-240zec_html_56e27b733eb82e3a.png)

Provide a File name. We name it the same as atheletes.csv

Now since the Source and Sink is set up, click on Validate.

![](RackMultipart20240201-1-240zec_html_d73011cbc0e20249.png)

Once validation is done, click on Debug.

Go to the tokyoolympicstorageacc storage account -\> tokyo-olympic-container container to check the file is there on data lake gen 2

![](RackMultipart20240201-1-240zec_html_bd3353f4dbde47c9.png)

So we are able to extract data from Github by creating a copy activity in Azure Data Factory and then loaded the data to Azure Storage Account.

Similarly for Coaches.csv in github we copy the raw link and do all the steps

![](RackMultipart20240201-1-240zec_html_870891faad37de7d.png)

Ingestion and storage is done in azure storage container

![](RackMultipart20240201-1-240zec_html_8c27e472baca1fc0.png)

Next we create an azure databricks service

![](RackMultipart20240201-1-240zec_html_1d1d596db52ed8f1.png)

Create Azure Databricks Workspace

![](RackMultipart20240201-1-240zec_html_9b8aa29498f29ac8.png)

Go to resource

![](RackMultipart20240201-1-240zec_html_71bbba1b923a3ea6.png)

Resource is ready

![](RackMultipart20240201-1-240zec_html_ab2d92521630fdcb.png)

![](RackMultipart20240201-1-240zec_html_1319c72438653517.png)

Next we create a compute resource

![](RackMultipart20240201-1-240zec_html_76944d67a9aa90e8.png)

Apply the settings for compute

We will only use one worker so we go for single-node.

![](RackMultipart20240201-1-240zec_html_a93369d644754fe4.png)

We have our cluster deployed

![](RackMultipart20240201-1-240zec_html_b6569d5ad46e1b2.png)

Go to New -\> Notebook

![](RackMultipart20240201-1-240zec_html_b2224ea6622d4452.png)

Once we have our azure databricks service ready we have to mount the service with Azure Data Factory for authentication of accessing the data from storage

Go to App registrations service

![](RackMultipart20240201-1-240zec_html_7432fc5c365b9cb4.png)

Give a name and register

![](RackMultipart20240201-1-240zec_html_5de241874a497fdb.png)

We need client id and tenant id

![](RackMultipart20240201-1-240zec_html_7ca0fbcbf12e311e.png)

Next we will create a secret ID

Go to Certificates and secrets

![](RackMultipart20240201-1-240zec_html_bc92e1dd9a37d6d7.png)

Create a new client secret

![](RackMultipart20240201-1-240zec_html_47e97477b3dbb81.png)

Copy the value of secret key and note it somewhere

![](RackMultipart20240201-1-240zec_html_d5a563a83bde296c.png)

So overall we need we need **client id** and **tenant id** and **secret key** credentials to create a connection from Databricks to Azure Data Lake Service ADLS.

![](RackMultipart20240201-1-240zec_html_aa6c3852f040bae.png)

We are using the app registration and using the credentials of the app we are trying to access the data that is stored onto the data lake. Next we will give permission to that particular app to access the data from a data lake.

Go inside the Container -\> IAM

![](RackMultipart20240201-1-240zec_html_955b264675287e90.png)

Add -\> Add role assignment

![](RackMultipart20240201-1-240zec_html_2c392fc5d8ff8a97.png)

![](RackMultipart20240201-1-240zec_html_e5501d33f0c09e21.png)

Select the role "Storage Blob Data Contributor" -\> Next

![](RackMultipart20240201-1-240zec_html_748c3d953ba14fb2.png)

Select Members -\> type the name of the app created in App registrations service

![](RackMultipart20240201-1-240zec_html_5f199f5d4e4158fd.png)

Review + assign

This will add a new role inside IAM so that we can give permission to this app to access the data in our storage account

![](RackMultipart20240201-1-240zec_html_91a2fd0420592be9.png)

Now running an ls command gives all files in our storage container

![](RackMultipart20240201-1-240zec_html_8a1a5f3e0b0ee6f5.png)

Once transformation is done, we write the data to (ADLS) transformed-data folder in container tokyo-olympic-container

![](RackMultipart20240201-1-240zec_html_2ca2dcfb376965d2.png)

Next we will use Azure Synapse Analytics service to visualize our transformed data.

![](RackMultipart20240201-1-240zec_html_2e57bbcf88bfdcda.png)

Next we create a synapse workspace

![](RackMultipart20240201-1-240zec_html_83d808e1d0e13ae2.png)

We provide a new synapse workspace name, select form dropdown the resource group, Data Lake acc name and file system.

![](RackMultipart20240201-1-240zec_html_3cd8f986bc1c7069.png)

Review and Create

![](RackMultipart20240201-1-240zec_html_d42c2b834d7fa4cf.png)

This screenshot of Resource Group shows all the Azure services used till yet

![](RackMultipart20240201-1-240zec_html_bf4bc6a30be6b574.png)

Launch the Synapse Studio

![](RackMultipart20240201-1-240zec_html_8d3f37fa17159f75.png)

We will use the Synapse service only for analytics here.

![](RackMultipart20240201-1-240zec_html_5008c507d702cc74.png)

Synapse also lets us create Spark pools in Monitor section.

![](RackMultipart20240201-1-240zec_html_c8e36bf1d9e7cb8f.png)

The Integrate section in Synapse studio also provides same features as Azure Data Factory.

Now we navigate to Data -\> Linked -\> + -\> Lake database

![](RackMultipart20240201-1-240zec_html_bdc668adafdd5b7e.png)

After creating new database

![](RackMultipart20240201-1-240zec_html_6a1a84fb924b889d.png)

Rename the database and click on +Table -\> From data lake

![](RackMultipart20240201-1-240zec_html_3fbe72573e320b34.png)

Provide a table name and select the athlete folder in transformed-data folder.

![](RackMultipart20240201-1-240zec_html_24ad0c8cb06f03.png)

We make sure to infer first row as header

![](RackMultipart20240201-1-240zec_html_f006be2c19accad0.png)

After validating and publishing the Workspace would get populated with the table

![](RackMultipart20240201-1-240zec_html_d98ce5e40b29c72a.png)

Now we can run SQL script on our table

![](RackMultipart20240201-1-240zec_html_f614c64d7866549b.png)

The query editor

![](RackMultipart20240201-1-240zec_html_91cabde8d2cf3a95.png)

Make sure the column names does not contain any special characters

![](RackMultipart20240201-1-240zec_html_9b080070c8265550.png)

Likewise we publish rest of the tables in schema DatabaseTokyo

![](RackMultipart20240201-1-240zec_html_9c3a3ee8770cefe.png)

We can now query from the tables imported

Say Total number of athletes participated from each country

Data Analytics Executed:

-- Average number of entries by gender for each disclipline

SELECT Discipline,

AVG(Male) Avg\_Male,

AVG(Female) Avg\_Female

FROM TableEntriesgender

Group by Discipline;

![](RackMultipart20240201-1-240zec_html_8e3c8796f64f490b.png)

![](RackMultipart20240201-1-240zec_html_fe3f625b44a6807c.png)

-- Total number of medals won by each country

SELECT NOC,

SUM(Gold) Total\_Gold,

SUM(Silver) Total\_Silver,

SUM(Bronze) Total\_Bronze

FROM TableMedals

GROUPBY NOC

ORDERBY Total\_Gold DESC;

![](RackMultipart20240201-1-240zec_html_bd704a1015ed97f8.png)

![](RackMultipart20240201-1-240zec_html_998e0e5267a244fc.png)
