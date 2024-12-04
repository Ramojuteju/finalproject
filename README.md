[ls_HttpServer.json](https://github.com/user-attachments/files/18001918/ls_HttpServer.json)* Project Scope and Objectives
Scope
Resource Group:
Created a resource group named datalakewarehouse in Azure to manage all related resources.
Data Sources:
Consolidate data from Azure SQL DB, SQL Server, and APIs.
Azure SQL Database:
Stores structured data in CSV format.
API Endpoint:
Fetches JSON product data from the API at https://fakestoreapi.com/products.
SQL Server:
Houses additional transactional or historical data.
Storage Account:
Created a storage account mywarehouse23 to manage all data storage.
Azure Data Lake:
Implemented with raw, curated, and staging containers:
Raw: Stores unprocessed data directly ingested from sources.
Curated: Holds cleaned, validated, and partially transformed data.
Staging: Contains business-ready, aggregated data for analytics.
Microsoft Entra ID:
Used for the Azure Default Directory application to manage the Service Principal.
Azure Key Vault:
Securely stores Service Principal credentials.
Manages secrets, passwords, and other sensitive credentials.
Azure SQL Database: 
Managed within Azure SQL Server for hosting and managing structured data.
Analytics: 
Azure Synapse Analytics workspace created and named newretailsynapse for data querying, aggregation, and dashboarding.
Utilizes a dedicated pool for optimized data querying and analytics.

* Microsoft SQL Server Management Studio (SSMS)
Download SSMS:
Go to the official SSMS download page.
Click on Download SQL Server Management Studio.
Install SSMS:
Run the downloaded installer file.
Follow the installation wizard steps, choosing the default options (unless custom configurations are needed).
Click Install to begin the installation process.
Launch SSMS:
After installation, open SQL Server Management Studio from the Start menu.

* Objectives
Automate Data Ingestion and Transformation:
Use Azure Data Factory (ADF) pipelines to automate the process of extracting data from Azure SQL DB, SQL Server, and the API. Transform the data as required and load it into appropriate Azure Data Lake containers.
Perform Advanced Aggregations and Join Operations:
Join and aggregate data (e.g., customer details, sales transactions, and product details) for business insights using Azure Synapse SQL or Databricks.
Ensure Secure Access:
Use Azure Key Vault for managing API keys, database credentials, and other sensitive information.
Implement Role-Based Access Control (RBAC) to restrict access to data and resources.

* Technology Stack
Azure SQL Database:
Used as the source for customer and product catalog data.
Microsoft SQL Server:
Houses transactional data, including sales, orders, and other business-critical information.
Azure Data Factory:
Instance: retailfact23
Used for data integration, orchestrating ETL/ELT processes for data movement and transformation.
Azure Data Lake:
Used for data storage in three layers:
Raw: Stores unprocessed, raw data ingested from various sources.
Curated: Contains cleaned and partially transformed data for further processing.
Staging: Holds business-ready data, optimized for analytics.
Databricks:
Instance: retailfact23
Cluster: Ramoju
Used for advanced transformations, aggregations, and machine learning tasks on the data.
Azure Synapse Analytics:
Workspace: newretailsynapse
Provides data warehousing and query engine capabilities for large-scale analytics and reporting.
Azure Key Vault:
Instance: retailkeyvault
Securely stores credentials and configurations, including service principal credentials and other secrets.

* Service Principal
Authentication and Access Control:
Use a Service Principal for automated authentication with Azure services (e.g., Azure Data Factory, Databricks, Azure Synapse) without needing a user to log in.
Role-Based Access Control (RBAC):
Assign the Service Principal the necessary roles to access resources:
Contributor or Reader for Azure SQL Database and Data Lake.
Data Factory Contributor for managing pipelines.
Storage Blob Data Contributor for accessing containers in Data Lake.
Key Vault Secrets User for accessing secrets in Azure Key Vault.
Service Principal Setup:
Create a Service Principal in Azure Active Directory.
Generate client ID, tenant ID, and client secret (store it in Key Vault).
Assign required RBAC roles to the Service Principal for accessing Azure resources.

* Using Azure Key Vault
Create a Key Vault:
In Azure, create a Key Vault (retailkeyvault) to securely store secrets.
Store Secrets:
Add secrets to the vault, such as a Service Principal client secret, with a name (client-secret).
Access Secrets:
Applications or services (Azure Data Factory) retrieve the secret from the Key Vault using its name (client-secret) and the vault's name (retailkeyvault).
Assign Permissions:
Assign the necessary access roles to allow services to read the secrets stored in the vault.

* Implementation Plan for Data Movement and Transformation Using Azure Data Factory Pipeline
1. Create an Azure Data Factory (ADF) Instance
In the Azure portal, create an Azure Data Factory instance (e.g., retailfact23).
This will be the environment where your pipelines, datasets, and linked services are defined.
2. Define Linked Services
Linked Service for Source:
Create a linked service for your source data (Azure SQL Database, API).
This defines the connection to your data source.
Linked Service for Sink (Data Lake Storage):
Create a linked service to Azure Data Lake Storage Gen2 where your raw data will be stored (mywarehouse23).
This will define how ADF connects to your Data Lake storage.
3. Define the Pipeline with Copy Activity
Create a Pipeline:
Create a new pipeline in Azure Data Factory.
Add a Copy Activity to the pipeline that moves and transforms data from the source to the raw container.
Configure the Copy Activity:
Source: Define the source dataset and transformation logic
Sink: Define the sink dataset (the Raw Container in the Data Lake).
Example of the Copy Activity:
Linked Service http:
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
Linked Service sqldb:
{
	"name": "ls_sqldb",
	"properties": {
		"annotations": [],
		"type": "AzureSqlDatabase",
		"typeProperties": {
			"server": "retailserver23.database.windows.net",
			"database": "retaildbase",
			"encrypt": "mandatory",
			"trustServerCertificate": false,
			"authenticationType": "SQL",
			"userName": "sqluseradmin",
			"password": {
				"type": "AzureKeyVaultSecret",
				"store": {
					"referenceName": "ls_keyvault",
					"type": "LinkedServiceReference"
				},
				"secretName": "client-secret"
			}
		}
	}
}
Linked Service keyvault:
{
	"name": "ls_keyvault",
	"properties": {
		"annotations": [],
		"type": "AzureKeyVault",
		"typeProperties": {
			"baseUrl": "https://retailkeyvault.vault.azure.net/"
		}
	}
}
4. Add Data Flow 
If transformations are needed (cleaning or data aggregation), add a Data Flow activity before the Copy Activity in the pipeline.
Use Data Flow to filter out invalid records or to transform the data format before moving it to the Raw container.
5. Trigger the Pipeline
Trigger: Set up triggers (manual or scheduled) to run the pipeline at a specific time or when an event occurs.
Set a scheduled trigger to run the pipeline every night to move the daily data.
6. Monitor the Pipeline Execution
Monitor the pipeline execution and log any errors or issues using the Monitoring feature in Azure Data Factory.
Review activity run details for success or failure.
7. Validate Data in Raw Container
Once the pipeline successfully runs, validate that the data has been copied correctly to the Raw container in Azure Data Lake.
You can check the data using Azure Storage Explorer or through the Azure portal.












Create a Service Principal in Azure Active Directory.
Generate client ID, tenant ID, and client secret (store it in Key Vault).
Assign required RBAC roles to the Service Principal for accessing Azure resources.
------
Scope
Resource Group: Created a resource group named datalakewarehouse in Azure to manage all related resources
Data Sources: Consolidate data from Azure SQL DB, SQL Server and APIs.
Azure SQL Database: Structured data in CSV format.
API Endpoint: JSON product data from the API at https://fakestoreapi.com/products.
SQL Server: Additional transactional or historical data.
Storage Account: mywarehouse23
Implement Azure Data Lake with raw, curated(processed) and staging containers.
Raw: Stores unprocessed data directly ingested from sources.
Curated: Holds cleaned, validated, and partially transformed data.
Staging: Contains business-ready, aggregated data for analytics.
Microsoft Entra ID:
Used for the Azure Default Directory application to manage the Service Principal.
Azure Key Vault:
Securely stores the Service Principal credentials.
Manages secrets, passwords, and other sensitive credentials.
Azure SQL Database:
Managed within Azure SQL Server for hosting and managing structured data.

Analytics: Azure Synapse Analytics workspace created and named newretailsynapse for data querying, aggregation, and dashboarding
Utilizes a dedicated pool for optimized data querying and analytics.

Microsoft SQL Server Management Studio (SSMS):
Download SSMS:
Go to the official SSMS download page.
Click on Download SQL Server Management Studio.

Install SSMS:
Run the downloaded installer file.
Follow the installation wizard steps, choosing the default options (unless you need custom configurations).
Click Install to start the installation process.

Launch SSMS:
After installation, open SQL Server Management Studio from the Start menu.

Connect to SQL Server:
In SSMS, click Connect and enter your SQL Server instance name (e.g., localhost or a server name).
Provide the necessary authentication details (Windows Authentication or SQL Server Authentication).

Start Managing Databases:
Once connected, you can start creating, managing, and querying SQL databases through the SSMS interface.
















* GitHub for:
    * Continuous-Integration/Continuous-Deployment (CI/CD)
    * HTTP API Replication
    * Important Project Documents
* Microsoft Entra ID with:
    * Azure Default Directory application for Service Principal
* Azure Key Vault for:
    * Service Principal
    * Storing other secrets, passwords and credentials
* Azure SQL Database with Azure SQL Server
* Microsoft SQL Server with:
    * Microsoft SQL Server Management Studio
    * Microsoft SQL Server Configuration Manager
    * ANSI SQL
    * DBeaver for the ER Diagram
    * Microsoft Integration Runtime
    * Windows Firewall
* Azure Data Lake Gen2 Storage Account
* Azure Data Factory
* Azure Databricks with
    * PySpark
    * Delta Tables
* Azure Synapse Analytics with
    * Serverless Database Pool
    * Transact-SQL
Abstract

This project uses GitHub for Continuous-Integration/Continuous-Deployment (CI/CD) throughout its course. For each major development change, a new dev branch was created, and for final testing, a separate qa branch was used with its own environment setup.
For security, Service Principal was used with the combination of Microsoft Entra ID with Azure Default Directory App and Azure Key Vault was used. Azure Key Vault was also used to store other important secrets, passwords and credentials.
Dynamic links, triggers, datasets, data flows and other pipeline activities were used wherever permissible to avoid hard coding. They were tested thoroughly with the required credentials.
The project involves data ingestion from multiple sources:
* HTTP API with JSON softline product data
* Azure SQL Database
* On-Premise Microsoft SQL Server

Note
GitHub HTTP API Replication
To replicate the HTTP API, I created and uploaded the JSON documents to GitHub, which can be found here. I used their direct raw HTTP links to pull data.
The direct links to the JSON files are here:
* accessories.json
* clothing.json
* footwear.json
* home_decor.json
The raw data from the various sources is then stored in the "raw" container of the Azure Data Lake Storage Gen2 Account using Azure Data Factory pipeline.
Lookup, Foreach and Switch activities are used to automate the dynamic pipeline for each dataset and data source.

Note
Lookup JSON Document
A JSON document (lookup.json) was created and uploaded to the "metadata" container of the Storage Account with all the datasets and their respective sources. A sample of this document can be found here.
The JSON raw data are stored first as it is using the Copy data activity, and then as CSV using the Data flow to ensure that the nested objects and arrays are saved properly under their respective CSV headers.
Copy data activity is also used for the raw data ingestion from the Azure SQL Database* and Microsoft SQL Server as parquet formats.
Using PySpark in an Azure Databricks cluster, the raw data is then cleaned and stored in Delta Tables onto the "curated" container of the Storage Account.
With another Databricks cluster, ETL is performed on the Delta Tables (as per the business case scenario). The transformed Delta Tables are stored in the "staging" container.
The two Azure Databricks clusters are incorporated into the Azure Data Factory pipeline, and the parameterized pipeline is again automated using a trigger that runs at the end of each day (at 08:00 pm) with the respective values for each parameter.
Azure Synapse Analytics with a Serverless Database Pool is used to create external tables from the Delta Tables in the curated and staging containers of the Storage Account.
Using Transac-SQL (T-SQL) in the Azure Synapse Analytics, a thorough analysis was performed on the available data from the various sources.
Methodology

On-Premise Microsoft SQL Server Database Setup

On a new Microsoft SQL Server sample database was created using Microsoft SQL Server Management Studio with SQL Server Authentication credentials. The DDL could be found here.
Here's what the sample code from the DDL looks like:
CREATE TABLE Customer (
   CustomerID INT IDENTITY(1,1) PRIMARY KEY,
   FirstName NVARCHAR(50),
   LastName NVARCHAR(50),
   Email NVARCHAR(100),
   Phone NVARCHAR(15),
   AddressLine1 NVARCHAR(200),
   AddressLine2 NVARCHAR(200),
   City NVARCHAR(100),
   StateID INT FOREIGN KEY REFERENCES StateProvinceRolling(StateID),
   PostalCode NVARCHAR(20),
   CountryID INT FOREIGN KEY REFERENCES CountryRolling(CountryID)
);

INSERT INTO Customer (FirstName, LastName, Email, Phone, AddressLine1, AddressLine2, City, StateID, PostalCode, CountryID)
VALUES
   ('John', 'Doe', 'john.doe@example.com', '1234567890', '123 Main St', '', 'Toronto', 1, 'M5H 2N2', 1),
   ('Jane', 'Smith', 'jane.smith@example.com', '9876543210', '456 Elm St', '', 'Los Angeles', 4, '90001', 2);

SELECT * FROM Customer;

The following ER Diagram shows the overview schema of the on-premise database:
￼
DBeaver was used to visualize the ER Diagram.
HTTP API — GitHub Replication Setup

To replicate an HTTP API with softline product datasets, GitHub was used.
Some sample JSON datasets were created and uploaded to the GitHub Repository, which were later accessed through their raw links. The information about the data has been provided above.
Service Principal and Azure Key Vault Setup

Created an Azure Default Directory* application using Microsoft Entra ID, and then created a client secret.
Used Azure Key Vault in an Azure Resource Group to store the following credentials:
* Client ID, also known as Application ID and Service Principal ID
* Directory ID, also known as Tenant ID
* Client Secret
* Self-hosted on-premise Microsoft SQL Server Authentication Password
Azure Data Lake Gen2 Storage Account Setup

Created an Azure Data Lake Gen2 Storage Account with the following Blob Storage Containers:
* metadata
* raw
* curated
* staging
Under IAM Access Control, appropriate roles were assigned to the Azure Default Directory application to give other resources (such as Azure Data Factory, Azure Databricks and Azure Synapse Analytics) required access through the Service Principal.
Azure SQL Database with SQL Server Setup

An Azure SQL Database with SQL Server and a sample dataset was created. The password credentials were stored in the Azure Key Vault.
Lookup File Setup

Since the Azure Data Factory pipelines would be parameterized for reusability and modular purposes, a lookup JSON document was created to include all the datasets with their sources. The file was uploaded to the metadata container. The link to the sample file has been provided above.
This is what a part of the JSON document looks like:
{
    "source": "onprem_sqlserver",
    "filename": "Seller"
},

Azure Data Factory Setup

An Azure Data Factory resource was created in the Resource Group, and linked to the GitHub Repository. The principles of Continuous Integration/Continuous Development (CI/CD) were followed throughout the development and testing process. For each stage of the development, testing and deployment, GitHub Repository branches were created and used. Each stage was tested thoroughly under separate environments and conditions.
The Azure Data Factory was given access to the Azure Key Vault credentials through Key Vault Access Policies. At the same time, the access was created for AzureDatabricks for later use.
All the linked services, integration runtime environment, datasets, data flow and pipelines were parameterized and created dynamically.
Linked services for the following resources were created:
* Azure Key Vault
* Azure Data Lake Gen2 Storage Account
* HTTP API — JSON (GitHub)
* Azure SQL Database
* Microsoft SQL Server (Selfhosted Integration Runtime Environment)
While creating the Selfhosted Microsoft SQL Server linked service, an integration runtime was also created and linked to an on-premise Microsoft Integration Runtime.
A linked service for Databricks was created later on.
A trigger was created (but not started, since no pipeline attached yet) to run at the end of each day at 08:00 pm.
Azure Data Factory Initial Pipeline Setup

￼
To handle data from multiple sources, a Lookup activity was used with a loopup.json file on the metadata container of the Storage account, as mentioned.
To go through the array output, Foreach activity was used with a Switch activity. Foreach data source, the switch statement would run certain activities.
￼
For the JSON data from the HTTP API, two sequenced parameterized activities were used:
* A Copy data activity to simply copy the JSON data as it is.
* A Data flow to ensure that the nested objects and arrays were saved properly under their CSV headers.
￼
For the hardline datasets from both the Azure SQL Database and on-premise, self-hosted Microsoft SQL Server, Copy data activities were used to ingest data in the parquet format.
The default activity was left to Wait for 1 second.
￼
Azure Databricks — Data Cleaning Cluster Setup

An Azure Databricks resource was created in the Resource Group. An access token was created in the settings, and a linked service in the Azure Data Factory for later use.
A secret scope was created to access the credentials from the Azure Key Vault*.
A notebook was created to clean the raw data from various sources. The notebook could be accessed here.
The cleaned data was stored in Delta Tables on the "curated" Blob Storage Container of the Data Lake Storage.
Azure Databricks — Data Cleaning Cluster and ADF Pipeline Integration

A Databricks Notebook activity was used to automate the process of data cleaning at the end of each pipeline run.
￼
Azure Databricks — ETL Cluster

Another Databricks Notebook was created for the business case ETL (Extract, Transform and Load). The data was extracted from the Delta Tables on the "curated" container of the Storage Account, transformed as per the business use cases, and loaded in the Delta Tables onto the "staging" container. The notebook could be found here.
Complete Azure Data Factory Pipeline with Databricks Clusters

The ETL Azure Databricks notebook was also added to the Azure Data Factory pipeline.
￼
Azure Synapse Analytics Integration

An Azure Synapse Analytics resource was created in the Azure Resource Group, and access to the Key Vault was granted through the Key Vault Access Policies for the Service Principal credentials.
Parameterized dynamic linked services for the Key Vault and Data Lake Storage Account were created.
A serverless database pool was created.
Used T-SQL to create external tables for al the datasets from various sources available on the "curated" and "staging" Delta Tables. The DDL could be found here.
Then performed thorough analysis on the available data using aggregated functions and joins. Then performed some advanced analysis.
Here's a sample of some of the advanced analyses performed:
-- Top 5 Products by Total Sales Revenue

SELECT TOP 5 p.Name AS ProductName, 
    SUM(sod.LineTotal) AS TotalRevenue,
    COUNT(DISTINCT soh.SalesOrderID) AS NumberOfSales
FROM [azsqldb_Product] p
JOIN [azsqldb_SalesOrderDetail] sod ON p.ProductID = sod.ProductID
JOIN [azsqldb_SalesOrderHeader] soh ON sod.SalesOrderID = soh.SalesOrderID
WHERE soh.Status = 5  -- 5 indicates completed orders
GROUP BY p.Name
ORDER BY TotalRevenue DESC;

-- Year-Over-Year Sales Growth

WITH SalesByYear AS (
    SELECT YEAR(OrderDate) AS SalesYear, 
        SUM(TotalDue) AS TotalSales
    FROM [azsqldb_SalesOrderHeader]
    WHERE OrderDate IS NOT NULL
    GROUP BY YEAR(OrderDate)
)
SELECT SalesYear, 
    TotalSales, 
    LAG(TotalSales, 1) OVER (ORDER BY SalesYear) AS LastYearSales,
    ((TotalSales - LAG(TotalSales, 1) OVER (ORDER BY SalesYear)) / LAG(TotalSales, 1) OVER (ORDER BY SalesYear)) * 100 AS SalesGrowthPercentage
FROM SalesByYear
ORDER BY SalesYear;

Pipeline Monitoring and Troubleshooting Measures

* Monitor logs for the Data Factory pipeline runs were checked thoroughly for detailed activity logs and error messages.
* Used ADF Debug mode to test individual activities, and checked for runtime errors.
* Ensured that the TCP/UDP ports for the Microsoft SQL Server were properly configured in the Firewall.
* The on-premise server was ensured to be available most of the time so that the data is available and accessible when needed.
* Pipeline parameters and credentials were ensured to be entered correctly.
* Data formats in the Copy data and Data flow activities were ensured to match properly.
* Ensure no mismatch between the source and sink schema for consistent mapping.
* Ensured that the Integration Runtime and Service Principal credentials were configured correctly.
* Conditional statements and error handling were used in the Databricks Notebooks to handle day-to-day activities.
* Adjusted the Databricks cluster size to handle larger workloads.
* Analyzed Databricks job and cluster logs for detailed error information. Used Databricks monitoring tools for real-time insights.
