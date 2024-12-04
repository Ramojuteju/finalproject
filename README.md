# Project Scope and Objectives
# Scope
# Resource Group:
* Created a resource group named datalakewarehouse in Azure to manage all related resources.
* Data Sources:
* Consolidate data from Azure SQL DB, SQL Server, and APIs.
* Azure SQL Database:
<img width="1711" alt="SQL DataBase" src="https://github.com/user-attachments/assets/a855d575-daa9-4cb4-990f-ab0324cf49b8">
<img width="1668" alt="SQL Server" src="https://github.com/user-attachments/assets/6a4bb4e2-35e9-4e58-b0a7-ac5a23ae56f8">

* Stores structured data in CSV format.
* API Endpoint:
* Fetches JSON product data from the API at https://fakestoreapi.com/products.
* SQL Server:
* Houses additional transactional or historical data.
* Storage Account:
<img width="1728" alt="Storage Account" src="https://github.com/user-attachments/assets/97512f7b-6830-4079-b72e-261db08c8ab8">

* Created a storage account mywarehouse23 to manage all data storage.
* Azure Data Lake:
* Implemented with raw, curated, and staging containers:
* Raw: Stores unprocessed data directly ingested from sources.
* Curated: Holds cleaned, validated, and partially transformed data.
* Staging: Contains business-ready, aggregated data for analytics.
<img width="1762" alt="Containers" src="https://github.com/user-attachments/assets/4ad9e687-cb4f-4135-9994-89948acb8a65">

# Microsoft Entra ID:
<img width="1728" alt="MEntraID" src="https://github.com/user-attachments/assets/9d801a5a-2c52-4fdb-ab83-4bd52e0bcbc9">

* Used for the Azure Default Directory application to manage the Service Principal.
* Azure Key Vault:
* Securely stores Service Principal credentials.
* Manages secrets, passwords, and other sensitive credentials.

* Azure SQL Database: 
* Managed within Azure SQL Server for hosting and managing structured data.
* Analytics: 
* Azure Synapse Analytics workspace created and named newretailsynapse for data querying, aggregation, and dashboarding.
* Utilizes a dedicated pool for optimized data querying and analytics.

# Microsoft SQL Server Management Studio (SSMS)
* Download SSMS:
* Go to the official SSMS download page.
* Click on Download SQL Server Management Studio.
* Install SSMS:
* Run the downloaded installer file.
* Follow the installation wizard steps, choosing the default options (unless custom configurations are needed).
* Click Install to begin the installation process.
* Launch SSMS:
* After installation, open SQL Server Management Studio from the Start menu.

# Objectives
* Automate Data Ingestion and Transformation:
* Use Azure Data Factory (ADF) pipelines to automate the process of extracting data from Azure SQL DB, SQL Server, and the API. Transform the data as required and load it into appropriate Azure Data Lake containers.
* Perform Advanced Aggregations and Join Operations:
* Join and aggregate data for business insights using Databricks.
* Ensure Secure Access:
* Use Azure Key Vault for managing API keys, database credentials, and other sensitive information.
* Implement Role-Based Access Control (RBAC) to restrict access to data and resources.

# Technology Stack
* Azure SQL Database:
* Used as the source for customer and product catalog data.
* Microsoft SQL Server:
* Houses transactional data, including sales, orders, and other business-critical information.
* Azure Data Factory:
* Instance: retailfact23
* Used for data integration, orchestrating ETL/ELT processes for data movement and transformation.
* Azure Data Lake:
* Used for data storage in three layers:
* Raw: Stores unprocessed, raw data ingested from various sources.
* Curated: Contains cleaned and partially transformed data for further processing.
* Staging: Holds business-ready data, optimized for analytics.
* Databricks:
* Instance: retailfact23
* Cluster: Ramoju
* Used for advanced transformations, aggregations, and machine learning tasks on the data.
* Azure Synapse Analytics:
* Workspace: newretailsynapse
* Provides data warehousing and query engine capabilities for large-scale analytics and reporting.
* Azure Key Vault:
* Instance: retailkeyvault
* Securely stores credentials and configurations, including service principal credentials and other secrets.

# Service Principal
* Authentication and Access Control:
* Use a Service Principal for automated authentication with Azure services (Azure Data Factory, Databricks, Azure Synapse) without needing a user to log in.
* Role-Based Access Control (RBAC):
* Assign the Service Principal the necessary roles to access resources:
* Contributor or Reader for Azure SQL Database and Data Lake.
* Data Factory Contributor for managing pipelines.
* Storage Blob Data Contributor for accessing containers in Data Lake.
* Key Vault Secrets User for accessing secrets in Azure Key Vault.
* Service Principal Setup:
* Create a Service Principal in Azure Active Directory.
<img width="1742" alt="Secretscope" src="https://github.com/user-attachments/assets/773cc39e-fe19-4a32-bf54-44827bebd259">

* Generate client ID, tenant ID, and client secret (store it in Key Vault).
<img width="1719" alt="SecretID" src="https://github.com/user-attachments/assets/6dbf3efd-9379-4c38-b53a-fe9f3914fdaf">

* Assign required RBAC roles to the Service Principal for accessing Azure resources.

# Using Azure Key Vault
* Create a Key Vault:
* In Azure, create a Key Vault (retailkeyvault) to securely store secrets.
<img width="1717" alt="Key Vault" src="https://github.com/user-attachments/assets/23bcc021-d437-4d19-bc36-252e85ecd03d">

* Store Secrets:
* Add secrets to the vault, such as a Service Principal client secret, with a name (client-secret).
* Access Secrets:
* Applications or services (Azure Data Factory) retrieve the secret from the Key Vault using its name (client-secret) and the vault's name (retailkeyvault).
* Assign Permissions:
<img width="1762" alt="Key - DF Acess" src="https://github.com/user-attachments/assets/0d1a226c-74e4-4e7b-9973-99541096d1de">

* Assign the necessary access roles to allow services to read the secrets stored in the vault.
  <img width="1725" alt="Client Secret" src="https://github.com/user-attachments/assets/5f7042bc-0cc6-4cbd-a261-5e586d58d132">

# Implementation Plan for Data Movement and Transformation Using Azure Data Factory Pipeline
# 1. Create an Azure Data Factory (ADF) Instance
* In the Azure portal, create an Azure Data Factory instance (e.g., retailfact23).
This will be the environment where your pipelines, datasets, and linked services are defined.
<img width="1740" alt="Data Factory" src="https://github.com/user-attachments/assets/ce3c2385-24a9-46d2-8bd4-81f4c8ecb53a">

# 2. Define Linked Services
* Linked Service for Source:
* Create a linked service for your source data (Azure SQL Database, API).
<img width="1728" alt="ls_http" src="https://github.com/user-attachments/assets/2a214f6a-d7e4-4c5d-bafa-99b42c75d3dc">
<img width="1726" alt="ls_sqldb" src="https://github.com/user-attachments/assets/869802f8-c500-4a67-bdb1-880ba1ba71f3">
<img width="1746" alt="ls_gen2" src="https://github.com/user-attachments/assets/f967e944-1c05-4699-848b-d44692f70e11">
<img width="1735" alt="ls_keyvault" src="https://github.com/user-attachments/assets/88be4512-7edd-43ff-ad19-2e9a7ace9e46">
<img width="1725" alt="ls_dbricks" src="https://github.com/user-attachments/assets/cc502e02-53aa-457a-96a2-e8534e44c196">

* This defines the connection to your data source.
* Linked Service for Sink (Data Lake Storage):
* Create a linked service to Azure Data Lake Storage Gen2 where your raw data will be stored (mywarehouse23).
* This will define how ADF connects to your Data Lake storage.
# 3. Define the Pipeline with Copy Activity
* Create a Pipeline:
* Create a new pipeline in Azure Data Factory.
* Add a Copy Activity to the pipeline that moves and transforms data from the source to the raw container.
* Configure the Copy Activity:
<img width="1725" alt="DF - Pipelinehttp" src="https://github.com/user-attachments/assets/25437472-cc85-41a8-a6b6-d468245593e2">
<img width="1737" alt="Df - Pipelinesql" src="https://github.com/user-attachments/assets/3ca00aa4-fc4f-4317-8c8f-399ee710dc6a">

* Source: Define the source dataset and transformation logic
* Sink: Define the sink dataset (the Raw Container in the Data Lake).
* Example of the Copy Activity:
# Linked Service http:
<img width="637" alt="ht" src="https://github.com/user-attachments/assets/53beda14-5183-4848-a27c-ff669c95308c">

# Linked Service sqldb:
<img width="776" alt="sql" src="https://github.com/user-attachments/assets/acccaddc-9c67-4b33-812c-e6d6b0ed250e">

# Linked Service keyvault:
<img width="747" alt="key" src="https://github.com/user-attachments/assets/2f98cd01-baf1-462e-b0e3-a2ecf5f3f183">

# 4. Add Data Flow 
* If transformations are needed (cleaning or data aggregation), add a Data Flow activity before the Copy Activity in the pipeline.
* Use Data Flow to filter out invalid records or to transform the data format before moving it to the Raw container.
# Trigger the Pipeline
* Trigger: Set up triggers (manual or scheduled) to run the pipeline at a specific time or when an event occurs.
* Set a scheduled trigger to run the pipeline every night to move the daily data.
<img width="1753" alt="Trigger" src="https://github.com/user-attachments/assets/9fdf73a4-c56f-4daf-8f8b-d9dedc7a4bf9">

# 5. Monitor the Pipeline Execution
* Monitor the pipeline execution and log any errors or issues using the Monitoring feature in Azure Data Factory.
* Review activity run details for success or failure.
# 6. Validate Data in Raw Container
* Once the pipeline successfully runs, validate that the data has been copied correctly to the Raw container in Azure Data Lake.
* You can check the data using Azure Storage Explorer or through the Azure portal.
<img width="1723" alt="raw" src="https://github.com/user-attachments/assets/cdfc39a9-461b-4a10-a689-d4e4b2d41223">
<img width="1722" alt="raw-http" src="https://github.com/user-attachments/assets/3ea06099-30ba-43ba-a393-82f441282451">
<img width="1735" alt="raw-sql" src="https://github.com/user-attachments/assets/38da9873-5a3d-478d-bd83-04dbd44950fc">

# Data Transformation and Loading using Databricks
* Source Data:
* Raw Container: Contains raw data files (http.txt and sqldb.txt).
* Destination:
* Curated Container: Stores cleaned and processed data in Delta format.
# Step 1: Set Up Databricks Environment
* Launch Databricks and connect to the Ramoju cluster.
<img width="1722" alt="Databricks" src="https://github.com/user-attachments/assets/c23cedb1-68d1-440e-b54b-4f2cdb262563">

* Ensure the cluster is running and libraries are up to date.
<img width="1![Uploading Databricks.png…]()
744" alt="Cluster" src="https://github.com/user-attachments/assets/dc4f9f8e-dc48-4735-8fea-d96494c30423">

# Step 2: Mount Azure Data Lake Storage
* Mount Raw Container: Mount the raw container from ADLS:
* Source: abfss://raw@mywarehouse23.dfs.core.windows.net/
* Mount Point: /mnt/mywarehouse23/raw
* Mount Curated Container: Mount the curated container:
* Source: abfss://curated@mywarehouse23.dfs.core.windows.net/
* Mount Point: /mnt/mywarehouse23/curated.
<img width="1438" alt="1 Mt" src="https://github.com/user-attachments/assets/ce808677-5a87-4a39-a0c7-41b2ee74ca32">
<img width="1676" alt="2 mt" src="https://github.com/user-attachments/assets/de34bf08-b91c-496e-aa59-02555e574552">


# Step 3: Read and Prepare Raw Data
* Rename Files: Rename files in the raw container for clarity:
* http.txt → http.csv
* sqldb.txt → sqldb.csv.
* Load Files: Read renamed files into Spark DataFrames:
* http.csv: Represents product details.
* sqldb.csv: Represents sales transaction data.
# Step 4: Data Cleaning and Transformation
* Infer or Apply Schemas:
* Define schemas for both DataFrames to ensure data type consistency.
* Drop Unnecessary Columns:
* Remove columns that are not required for analysis (image in HTTP data).
* Handle Missing or Null Values:
* Fill missing values in HTTP data.
* Drop rows with null values in SQLDB data.
# Step 5: Write to Curated Container
* Write the cleaned DataFrames into the curated container in Delta format:
* HTTP Data: Save at /mnt/mywarehouse23/curated/http_delta.
* SQLDB Data: Save at /mnt/mywarehouse23/curated/sqldb_delta.
* Ensure the files are written in overwrite mode for consistency.
# Step 6: Validate Data in Curated Container
* List files in the curated container to confirm successful writing:
* Load and preview data in Delta format to verify cleaning and transformation steps.
<img width="1466" alt="1In" src="https://github.com/user-attachments/assets/2ee7e5ae-2eca-4ebf-aa0f-11981afb6e15">
<img width="1552" alt="2In" src="https://github.com/user-attachments/assets/0b55a1a8-8d53-4454-ad5d-c703a5cb724c">
<img width="1510" alt="3 Inc" src="https://github.com/user-attachments/assets/b37972b3-34fd-4f4d-9884-7b7aee7472e9">

# 1. Configure Azure Data Lake Access
* This sets the Spark configuration to access an Azure Data Lake Storage (ADLS) Gen2 account (mywarehouse23) using an access key.
* The key provides authenticated access to the data lake for reading and writing files.
# 2. Display Raw Container Contents
Lists and displays the files in the raw container of the ADLS account in tabular format.
dbutils.fs.ls() fetches the contents of the specified ADLS path.
# 3. Rename Files
* Renames files in the raw container for easier identification:
* 78d9a3a4-7a07-4e1d-97ff-24bc6d2dd964.txt → http.csv
* SalesLT.Product.txt → salesproduct.csv
# 4. Read Data from Raw Container
* Reads the renamed CSV files into DataFrames (http_df and salesproduct_df) using Spark.
* header=True: Treats the first row as column headers.
* inferSchema="true": Automatically detects the column data types.
# 5. Define Explicit Schemas
* Explicit schemas are defined for the DataFrames to enforce column data types and structure.
# 6. Drop Unnecessary Columns
* Drops irrelevant or redundant columns to clean the datasets.
# 7. Reload Data with Defined Schemas
* Reloads the cleaned data from the raw container, applying the defined schemas.
# 8. Define Curated Container Paths
* Specifies the paths for saving cleaned datasets into the curated container in Delta format.
# 9. Save Data to Curated Container
* Writes the cleaned DataFrames (http_df_cleaned and salesproduct_df_cleaned) to the curated container in Delta format:
* mode="append": Appends new data if the Delta table already exists.
# 10. Verify Curated Data
* Lists the files in the curated container to verify successful data writing.
<img width="1746" alt="curated" src="https://github.com/user-attachments/assets/1268d1dc-af6a-433f-b176-1a401374758e">

# 1. Configure Azure Data Lake Access
*Configures the Spark session to authenticate with the Azure Data Lake Storage (ADLS) Gen2 account (mywarehouse23) using an access key.
# 2. Display Curated Data
*Lists and displays the contents of the curated container to verify the presence of http_delta and salesproduct_delta.
# 3. Read Data from Curated Container
* Loads the Delta-formatted curated datasets into Spark DataFrames:
* http_df: Contains HTTP API data.
* salesproduct_df: Contains sales product data.
# 4. Register Temporary Views
* Registers the DataFrames as temporary SQL views, allowing Spark SQL queries to manipulate the data.
# 5. Query Data Using Spark SQL
* Executes a SQL query on the http view to retrieve all rows.
* Assigns the result of the SQL query to a new DataFrame (httpstaging_df) for further processing.
* Retrieves all rows from the salesproduct view.
* Assigns the result of the SQL query to a new DataFrame (sales_df).
<img width="1544" alt="1 et" src="https://github.com/user-attachments/assets/7f761363-9b64-4e80-b4dc-2f73ef10d205">
<img width="1170" alt="2 et" src="https://github.com/user-attachments/assets/30016d5e-43ef-4921-8a8f-5470ece1d89e">

# 6. Define Paths for  Container
* Specifies paths in the  container for saving the transformed data in Delta format.
# 7. Save DataFrames to Staging Container
* Saves the SQL query results into the staging container:
* httpstaging_df → httpstaging_delta
* salesstaging_df → salesstaging_delta
* mode="overwrite" ensures that any existing data at these paths is replaced.
# 8. Display Staging Data
* Lists and displays the contents of the staging container to verify successful writes.
<img width="1751" alt="staging" src="https://github.com/user-attachments/assets/76b8e59a-5816-4409-9dea-f68b2a2d9e6b">

# 1. Retrieve Secrets from Azure Key Vault
* Retrieves the client-secret for the Service Principal from Azure Key Vault using Databricks Secret Scope.
# 2. Set Spark Configuration for ADLS OAuth
* Configures Spark to authenticate with ADLS using OAuth.
* ClientCredsTokenProvider enables token-based access via Service Principal credentials.
# 3. Define Configuration Dictionary
* Consolidates the Spark configuration into a reusable dictionary.
# 4. Mount Raw Container
* Mounts the raw container (abfss://raw@mywarehouse23.dfs.core.windows.net/) to /mnt/mywarehouse23/raw.
* Checks for an existing mount at the same location and unmounts it if found.
* Handles errors during the mounting process.
# 5. Mount Curated Container
* Mounts the curated container (abfss://curated@mywarehouse23.dfs.core.windows.net/) to /mnt/mywarehouse23/curated.
* Follows the same unmount-check and error-handling logic as for the raw container.
# Key Functionalities
* Secure Authentication: Uses a Service Principal for OAuth-based secure access to ADLS.
* Key Vault Integration: Ensures secrets are securely fetched without hardcoding.
* Mount Points: Simplifies file access via /mnt directories for Spark-based operations.

# Synapse Analytics Integration with GitHub and Staging Container
# Step 1: Create Synapse Analytics Workspace
* In the Azure portal, create a Synapse Analytics Workspace:
* Workspace Name: newretailsynapse
<img width="1734" alt="SynaServer" src="https://github.com/user-attachments/assets/d2b310d2-3971-4cf7-b096-614bb8072ea5">

* Link it to the existing Azure Data Lake Storage (ADLS) account mywarehouse23.
* Assign Contributor and Storage Blob Data Contributor roles to Synapse for seamless integration with ADLS.
# Step 2: Connect Synapse Workspace to GitHub
* Go to the Manage tab in Synapse Studio.
* Select Git Configuration under Source Control.
* Link your GitHub repository:
* Select your GitHub account and repository.
<img width="1733" alt="synapsegit" src="https://github.com/user-attachments/assets/1337350c-c0ba-4365-8d0d-edc5abc99353">

* Choose a collaboration branch (main or develop).
* Configure a Synapse project for CI/CD practices.
* Save the configuration to enable version control for your Synapse workspace.
# Step 3: Create and Link Staging Container
* Ensure the staging container exists in ADLS with required permissions:
* Container Name: staging
* Example files:httpstaging_delta (data from http.csv)
* salesstaging_delta (data from salesproduct.csv).
* Validate file access:
* Use Synapse's Data Hub to browse ADLS and verify staging files are accessible.
# create a dedicated SQL pool in Azure Synapse Analytics and define external tables:
# Step 1: Create a Dedicated SQL Pool
* Navigate to Synapse Workspace:
* Open the Azure portal and navigate to your Synapse workspace (newretailsynapse).
* Create a Dedicated SQL Pool:
* Go to SQL Pools under Synapse Studio > Manage.
* Click New SQL Pool.
* Provide details:
* Name: mypool
* Performance Level: Choose the desired performance level as 0
* Click Create and wait for deployment.
<img width="1707" alt="dedicatedpool" src="https://github.com/user-attachments/assets/58b945b4-1f34-4f5b-af1d-7bb5ac615fad">

# Step 2: Configure the Dedicated SQL Pool
* Open the Synapse Studio and connect to the mypool
* Ensure the pool is online and ready for queries.
# Step 3: Create External Tables
* 1. Create External Data Source
* Use the Data Hub in Synapse Studio to ensure access to the Azure Data Lake container (staging) is configured correctly.
* Run the following SQL script in the mypool
# Step 4: Query External Tables
* Validate that the external tables are created successfully and query their contents:
* SELECT * FROM StagingHttpData;
<img width="1892" alt="httpext" src="https://github.com/user-attachments/assets/5278cae1-ca1a-46bb-801f-690647eaff11">

* SELECT * FROM StagingSalesData;
<img width="1853" alt="salesext" src="https://github.com/user-attachments/assets/bf10d32e-94a1-4380-ba08-79dc4ad1459b">

  #
  Step 1: Connect Synapse Studio to GitHub
Navigate to Synapse Studio:

Open your Synapse workspace in the Azure portal and launch Synapse Studio.
Enable Git Integration:

Go to Manage > Git Configuration.
Select Git Repository and connect to your GitHub account.
Enter the repository details:
Repository Name: <repository_name>
Collaboration Branch: main
Working Branch: dev
Save the configuration.
Step 2: Commit and Push Changes from Synapse
Make Changes:

After creating the external tables, navigate to the Source Control panel in Synapse Studio.
Select the changes made (e.g., SQL scripts for external tables) and click Commit.
Push to GitHub:

After committing, ensure the changes are pushed to the dev branch on GitHub.
Step 3: Create a Pull Request (PR) on GitHub
Go to Your GitHub Repository:

Open your GitHub repository where the Synapse project is stored.
Create Pull Request:

Go to the Pull Requests tab and click New Pull Request.
Compare the branches:
Base: qa
Compare: dev
Review the changes and add a description of the updates (e.g., "Added external table definitions and staging logic").
Submit Pull Request:

Click Create Pull Request to submit it.
Step 4: Review Changes in GitHub
Team Review:

Have your QA team review the changes in the pull request.
Add comments or approve the PR if everything looks good.
Merge Changes:

Once approved, click Merge Pull Request to merge changes from the dev branch into the qa branch.
Step 5: Verify Changes in Synapse (QA Environment)
Switch to QA Branch:

In Synapse Studio, go to Git Configuration and switch to the qa branch.
Validate External Tables:

Run the external table queries in the QA environment to ensure the changes have been successfully deployed.























