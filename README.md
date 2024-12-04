# Project Scope and Objectives
# Scope
# Resource Group:
* Created a resource group named datalakewarehouse in Azure to manage all related resources.
* Data Sources:
* Consolidate data from Azure SQL DB, SQL Server, and APIs.
* Azure SQL Database:
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
* Use a Service Principal for automated authentication with Azure services (e.g., Azure Data Factory, Databricks, Azure Synapse) without needing a user to log in.
* Role-Based Access Control (RBAC):
* Assign the Service Principal the necessary roles to access resources:
* Contributor or Reader for Azure SQL Database and Data Lake.
* Data Factory Contributor for managing pipelines.
* Storage Blob Data Contributor for accessing containers in Data Lake.
* Key Vault Secrets User for accessing secrets in Azure Key Vault.
* Service Principal Setup:
* Create a Service Principal in Azure Active Directory.
* Generate client ID, tenant ID, and client secret (store it in Key Vault).
* Assign required RBAC roles to the Service Principal for accessing Azure resources.

# Using Azure Key Vault
* Create a Key Vault:
* In Azure, create a Key Vault (retailkeyvault) to securely store secrets.
* Store Secrets:
* Add secrets to the vault, such as a Service Principal client secret, with a name (client-secret).
* Access Secrets:
* Applications or services (Azure Data Factory) retrieve the secret from the Key Vault using its name (client-secret) and the vault's name (retailkeyvault).
* Assign Permissions:
* Assign the necessary access roles to allow services to read the secrets stored in the vault.

# Implementation Plan for Data Movement and Transformation Using Azure Data Factory Pipeline
# 1. Create an Azure Data Factory (ADF) Instance
* In the Azure portal, create an Azure Data Factory instance (e.g., retailfact23).
This will be the environment where your pipelines, datasets, and linked services are defined.
# 2. Define Linked Services
* Linked Service for Source:
* Create a linked service for your source data (Azure SQL Database, API).
* This defines the connection to your data source.
* Linked Service for Sink (Data Lake Storage):
* Create a linked service to Azure Data Lake Storage Gen2 where your raw data will be stored (mywarehouse23).
* This will define how ADF connects to your Data Lake storage.
# 3. Define the Pipeline with Copy Activity
* Create a Pipeline:
* Create a new pipeline in Azure Data Factory.
* Add a Copy Activity to the pipeline that moves and transforms data from the source to the raw container.
* Configure the Copy Activity:
* Source: Define the source dataset and transformation logic
* Sink: Define the sink dataset (the Raw Container in the Data Lake).
* Example of the Copy Activity:
# Linked Service http:








{
	"name": "ls_HttpServer",
	"properties": {
		"annotations": [],
		"type": "HttpServer",
		"typeProperties": {
			"url": "https://fakestoreapi.com/products",
			"enableServerCertificateValidation": true,
			"authenticationType": "Anonymous"
		}
	}
} 

# Linked Service sqldb:
*{
*	"name": "ls_sqldb",
*	"properties": {
*		"annotations": [],
*		"type": "AzureSqlDatabase",
*		"typeProperties": {
*			"server": "retailserver23.database.windows.net",
*			"database": "retaildbase",
*			"encrypt": "mandatory",
*			"trustServerCertificate": false,
*			"authenticationType": "SQL",
*			"userName": "sqluseradmin",
*			"password": {
*				"type": "AzureKeyVaultSecret",
*				"store": {
*					"referenceName": "ls_keyvault",
*					"type": "LinkedServiceReference"
*				},
*				"secretName": "client-secret"
*			}
*		}
*	}
*}
# Linked Service keyvault:
*{
*	"name": "ls_keyvault",
*	"properties": {
*		"annotations": [],
*		"type": "AzureKeyVault",
*		"typeProperties": {
*			"baseUrl": "https://retailkeyvault.vault.azure.net/"
*		}
*	}
*}
# 4. Add Data Flow 
* If transformations are needed (cleaning or data aggregation), add a Data Flow activity before the Copy Activity in the pipeline.
* Use Data Flow to filter out invalid records or to transform the data format before moving it to the Raw container.
#. Trigger the Pipeline
* Trigger: Set up triggers (manual or scheduled) to run the pipeline at a specific time or when an event occurs.
* Set a scheduled trigger to run the pipeline every night to move the daily data.
# 5. Monitor the Pipeline Execution
* Monitor the pipeline execution and log any errors or issues using the Monitoring feature in Azure Data Factory.
* Review activity run details for success or failure.
# 6. Validate Data in Raw Container
* Once the pipeline successfully runs, validate that the data has been copied correctly to the Raw container in Azure Data Lake.
* You can check the data using Azure Storage Explorer or through the Azure portal.



























