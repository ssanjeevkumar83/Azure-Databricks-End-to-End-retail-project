# Azure Databricks End-to-End Retail Data Warehouse Project
This project demonstrates a real-world, production-ready data warehouse built on Azure Databricks using the Medallion Architecture. Unlike a "hobby project", this implementation includes administrative tasks, security configurations, and advanced data engineering patterns like Slowly Changing Dimensions (SCD) and Delta Live Tables (DLT).

--------------------------------------------------------------------------------
### Project Architecture
The project follows the industry-standard Medallion Architecture, progressing through three distinct layers:
1. Bronze (Raw): Incremental ingestion from source files using Autoloader.
   
2. Silver (Enriched): Data cleaning, PySpark transformations, and Object-Oriented Programming (OOP) for reusable logic.
   
3. Gold (Modelled): A Star Schema with dimension and fact tables, implementing SCD Type 1 and Type 2.

--------------------------------------------------------------------------------
### Key Features & Technologies
• Data Governance: Managed via Unity Catalog for centralised auditing and access control.

• Incremental Loading: Leveraging Spark Structured Streaming and Autoloader to ensure Exactly-Once processing (Idempotency).

• Delta Live Tables (DLT): A declarative framework used to automate complex ETL tasks and track data history.

• SCD Implementation: Manual PySpark code for SCD Type 1 and DLT for SCD Type 2.

• Orchestration: Multi-task Databricks Workflows with parallel execution and task dependencies.

• Data Quality: Defined through DLT Expectations (Warn, Drop, Fail).

--------------------------------------------------------------------------------
### Tech Stack
• Cloud Provider: Azure (Resource Groups, ADLS Gen2, Access Connectors).

• Data Processing: Azure Databricks, PySpark, Spark SQL.

• Storage: Delta Lake (Parquet-based columnar storage).

• BI & Analytics: SQL Warehouses, Power BI (via Partner Connect).

--------------------------------------------------------------------------------
### Step-by-Step Implementation:

1. **Infrastructure & Security**
   
• Provisioned an Azure Data Lake Storage (ADLS) Gen2 account with hierarchical namespaces.

• Configured Unity Catalog by creating a Metastore and assigning it to the workspace.

• Established External Locations and Storage Credentials using an Access Connector to allow Databricks to read/write securely without access keys.

2. **Bronze Layer: Ingestion**
   
• Implemented Autoloader to ingest Parquet files from the source container.

• Used Checkpointing and RocksDB to maintain the state of processed files, ensuring only new data is ingested during subsequent runs.

• Developed a Dynamic Ingestion Notebook using Widgets to process multiple datasets (Orders, Customers, Products) via a single pipeline.

3. **Silver Layer: Transformation**
   
• Cleaned raw data by dropping redundant columns (e.g., _rescued_data) and converting data types.

• Implemented Python Classes (OOP) to handle reusable window functions like dense_rank and row_number.

• Registered Unity Catalog Functions (SQL/Python) to standardise logic, such as price discounts, across the workspace.

4. **Gold Layer: Dimensional Modelling**
   
• SCD Type 1 (Customers): Built a manual MERGE operation that overwrites existing data and calculates new Surrogate Keys based on the previous maximum ID.

• SCD Type 2 (Products): Utilised Delta Live Tables with the apply_changes API to automatically track historical changes using start_at and end_at columns.

• Fact Table (Orders): Created a fact table by joining refined silver data with gold dimension tables to replace natural keys with surrogate keys.

5. **Orchestration & Serving**
   
• Configured a Parent Pipeline in Databricks Workflows to manage dependencies across all layers.

• Deployed a Serverless SQL Warehouse for high-performance querying by data analysts.

• Connected the gold layer to Power BI for automated reporting.

--------------------------------------------------------------------------------
### How to Run:

1. Clone the Repository: Download the project files and data from GitHub.
2. Azure Setup: Create a Resource Group, ADLS Gen2 account, and Databricks Workspace.
3. Unity Catalog: Enable the Metastore and register external containers.
4. Upload Data: Place the source files in the source container of your ADLS account.
5. Import Notebooks: Import the PySpark and DLT notebooks into your Databricks workspace.

6. Execute Workflow: Run the "End-to-End" workflow to populate the Medallion layers.
