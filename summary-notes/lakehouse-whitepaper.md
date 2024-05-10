# Lakehouse Whitepaper

## Abstract

    - New architecture patter will be based on
        - open direct-access data format apache parquet
        - first-class support for machine learning and data science
        - offer state-of-the-art performance

    - Major challenges with data warehouses
        - data staleness
        - data reliablity
        - total cost of ownership
        - data lock-in
        - limited use-case support

## Introudction

    - Data Warehouse
        Helping business leaders get analytical insights by collecting data from operational databases into centralized data          warehouses, which then could be used for decision support and business intelegince.

    - data warehose is schema-on-read where as data lake is on schema-on-write

    - During Hadoop movement, data warehouse was trying to offload to Data-Lake(HDFS) for better cost optimization

    - During 2015/1016, HDFS slowly being replaced by Cloud storage providers like AWS, GCP, Azure

    - Till this time, two-tier arhitecture was like : Cloud Date Lake + Data Warehouse

    - Today's data architecture have four major problems:
        - Reliability:
        - Data Staleness:
        - Limited support for advanced analytics:
        - Total cost of ownership:
