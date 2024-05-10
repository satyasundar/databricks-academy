# Data Engineeering with Databricks

## Medalliion Architecture

    We will have 3 stages of a table.
     - Bronze - In this stage data is ingested from raw file into a table.
     - Silver - In this stap, data is copied from bronze table. But it only copies the new data. Then we can apply transformations on this data to make it clean and usable.
     - Gold - This data is ready for use for different workloads.

## while creating catalog

     Predefined path cariables -
        - DA.paths.working_dir: dbfs:/mnt/dbacademy-users/satyasundar.nayak@gmail.com/get-started-with-data-engineering-with-databricks/v01/
        - DA.paths.datasets:    dbfs:/mnt/dbacademy-datasets/get-started-with-data-engineering-with-databricks/v01

## create schema:

    CREATE TABLE IF NOT EXISTS getting_started;

    USE SCHEMA getting_started;

## create table:

    CREATE TABLE IF NOT EXISTS customers_bronze;

    COPY_INTO customers_bronze
        FROM 'dbfs:/mnt/dbacademy-datasets/get-started-with-data-engineering-with-databricks/v01/'
        FILEFORMAT = CSV
        FORMAT_OPTIONS('inferSchema' = 'true', 'header' = 'true')
        COPY_OPTIONS('merge_schema' = 'true');

Note - COPY_INTO will only add new files if it runs again. If run again not, it will habe 0 affected rows.

## querying the data

    select * from customers_bronze;

## Clone our bronze table

    Lets create a deep clone of our bronze table. Deep clone is an independent copy of our table for cleaning the data.

        CREATE TABLE IF NOT EXISTS customer_silver
            DEEP CLONE customers_bronze;

        SELECT * FROM customers_silver;

    Deep clone will override all the data in customers_silver table. Instead of that lets merge so that it can add only neew records.

## merge into existing table

    To continue with the above example, lets first drop this silver table.

        DROP TABLE customer_silver;

        CREATE TABLE IF NOT EXISTS customers_silver_merged
            (customer_id int,
            tax_id double,
            ... ,
            ... ,
            unit_puchased double,
            loyality_segment int);

        MERGE INTO customers_silver_merged
            USING customers_bronze
            ON customers_bronze.customer_id = customers_silver_merged.customer_id
            WHEN NOT MATCHED THEN INSERT *;

## Databricks Grant and revike permission

    - Data Explorer -> Catalog -> Schema -> Table -> Permissions
        - Select principal as user and privilege to either give permission or revoke it.
        - Best practice is to give permissoin to groups instead of individuals. That will be easy to maintain.

## Workflows

    - 2 types of workflow
        - Job workflow ( any task/job. Can run Jar, ML task, spark-submit etc..)
        - DLT pipeline ( batch and streaming pipeline. Created with notebook. )

## Workflows demo

        - Run the classroom setup scripts
        - DA.print_job_config_v1() :
            Job Name: Example Job
            Reset Notebook Path:

        - To configure the job manually
            Workflows -> Jobs -> Create Job
                - Task Name: Example Job
                - Type: Notebook
                - Source: Workspace
                - Path: My Notebook path
                - Cluster: select the cluster
                - Job Name: enter a name provided above
                - Create button
                - Run Now button

        - To run the job automatically
            - DA.create_job_v1()
        - To validate the job configiration
            - DA.validate_job_v1_config()

        - For scheduling the jobs
            - workflows -> Jobs -> Schedules & Triggers -> Add Trigger -> "Trigger Type = None(manula) -> Scheduled" -> This will open up an UI for cron job

## Databricks SQL

    - New -> Query
    - Select warehouse
    - select catalog and schema name
    - Run a query
    - add visualisation
    - add to dashboard
