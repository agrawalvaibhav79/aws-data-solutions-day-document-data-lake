+++
title = "Introduction to Workshop"
date = 2021-03-17T18:13:14-04:00
weight = 9
chapter = true
+++

Below is the workshop Architecture we will follow today -
{{< img "workshop-arch.png" "architecture diag" >}}

The flow diagram for today’s data solution day is divided into 6 domains – Sources, Ingestion, Storage, Metadata, Analytics and Users/Consumers

1.	Typically, customers use document database like Amazon DocumentDB, which is a MongoDB compatible database, as backend for web applications. It is fast and efficient for web applications to store and access data in JSON formats. Today we will be using Covid data captured by organizations like NY Times, which publishes them for research and training purposes in JSON format. This data is stored in a DocumentDB data store as shown. While this works well for purpose-built data storage, for analytics we will ingest the data into an S3 based data lake. To do that, we will use our database migration service DMS to replicate the data into S3. This will save the DocumentDB to serve its purpose without getting bogged down by analytical query workload. This will allow customers to separate transaction and analytical workloads
2.	Though DMS, when it replicates the data on S3, allows to store the data in Parquet format, we will use the default feature to store it as csv files. S3- which is an object store and a very popular choice for building data lakes on AWS
3.	The data stored on S3 is in the form of files, which to an end user, may not mean much and would be hard to navigate to get the data they need. So next we will use AWS Glue crawlers to crawl these files and create a metadata layer which will represent the schema of data in files in an easy to consume manner using databases and table constructs. This way we create a Glue Data catalog
4.	End users can then directly use Amazon Athena console to query using SQL to start querying the data on S3 in-situ. This helps in democratization of data access without the need for complex ETL to move data into a relational database. 
5.	Another type of end user who likes to visualize data can use Amazon Quicksight to create visuals and dashboards either using Athena for data connection or directly querying data on S3 depending on their needs
6.	Finally, the business users can either use SQL query engines of their choice or access dashboards giving them the rich analytics they need for their business functions or decision making needs

