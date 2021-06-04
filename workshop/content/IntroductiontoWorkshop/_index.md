+++
title = "Introduction to Workshop"
date = 2021-03-17T18:13:14-04:00
weight = 9
chapter = true
+++

Below is the workshop Architecture we will follow today -
{{< img "workshop-arch.png" "architecture diag" >}}

The flow diagram for today’s data solution day is divided into 6 domains – Sources, Ingestion, Storage, Metadata, Analytics and Users/Consumers

1.	Typically, customers use document database like DocDB which is also MongoDB compatible as backend for web applications. It is quicker for web applications to store and access data in JSON formats. Today we will be using Covid data captured by organizations like Ny times, which publishes them for research and training purposes in JSON format. This data is stored in a documentDB data store as shown. While this works well for purpose-built data storage, for analytics we will ingest the data into an S3 based data lake. For that purpose, we will use our database migration service DMS to replicate the data into S3. This will save the DocDB to serve its purpose without getting bogged down by analytical query workload
2.	Then DMS when it replicates the data on S3 we will use an out of the box feature to store convert and store the data from JSON to Parquet format. We will talk about benefits later during other presentations today. Data gets stored and organized on S3 which is an object store and a very popular choice for building data lakes on AWS
3.	The data stored on S3 is in the form of files, which to a consumer may not mean much and would be hard to navigate to get the data they need. So next we will use Glue crawlers to crawl these thousands or millions of files and create a metadata layer which will represent the schema of data in files in easy to consume manner using databases and table constructs and create a Glue Data catalog
4.	End users can then directly use Athena console to fire ANSI compliant SQL queries to start querying the data on S3 in-situ. This helps in democratization of data access without the need for complex ETL. This really solves the problem where a data analyst now doesn’t really need to wait for ETL team to load data in to a relational database for them to query.
5.	Another type of end user who likes to visualize data can use Quicksight to create visuals and dashboards either using Athena or directly querying data on S3 depending on their needs
6.	Finally, the business users can either use SQL query engines of their choice or access dashboards giving them the rich analytics they need for their business functions or decision making.

