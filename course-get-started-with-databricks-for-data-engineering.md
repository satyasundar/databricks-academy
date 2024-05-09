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
        ```sql
        CREATE TABLE IF NOT EXISTS customer_silver
            DEEP CLONE customers_bronze;
        SELECT * FROM customers_silver;
        ```

    Deep clone will override all the data in customers_silver table. Instead of that lets merge so that it can add only neew records.
