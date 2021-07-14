+++
title = "Lab 03: Work with in data lake using Glue, Athena and Quicksight"
date = 2021-03-17T18:13:14-04:00
weight = 12
chapter = true
+++

With exploring data lake labs, below is the architecture flow we are going to cover ->
{{< img "exploring-data-lake-arch.png" "exploring-data-lake-arch" >}}

Step1: You will use the data replicated by DMS into an S3 location as raw data. You will crawl that dataset using AWS Glue to catalog the S3 location and data files as tables in Glue catalog.

Step2: Then you will use Glue Studio to create ETL jobs using visual ETL style to convert the .csv raw data files to analytics optimized parquet file format. The job will write to a different S3 location.

Step3: Next you will again use Glue crawler to crawl the parquet files to catalog as tables in Glue catalog.
The best practice is to crawl all data types to ensure end user can easily access any data they need to analyze.

Step4: Then you will use Athena to query the data sitting in S3 location in-situ. Athena is an interactive query engine which ensures SQL access to big data on S3 and supports many open file formats.

Step5: Finally you will use QuickSight to build some visuals and a dashboard to visually analyze the sample dataset.