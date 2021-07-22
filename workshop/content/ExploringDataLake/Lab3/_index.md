+++
title = "Catalog transformed data with Glue crawler"
date = 2021-03-17T18:13:14-04:00
weight = 43
chapter = true
+++

#### Create crawler for the processed data
1.  Go to [Glue crawler](https://us-east-2.console.aws.amazon.com/glue/home?region=us-east-2#catalog:tab=crawlers) again to create another crawler.  
1.1 Click on Add crawler and input as below
    On Crawler info> Crawler name = `lab_parquet_crawler`. Click Next.
{{% notice info %}}
Screenshot is only for illustration purpose, the actual bucket name comes from the Cloudformation Outputs>Bucket name key's value as shown in previous database lab
{{% /notice %}}
    For Crawler store type, keep everything default. Click Next
    For Data store, Choose as S3 and browse S3 bucket path and pick one with **`dmslabs3bucket`** in the name and **`parquet`** folder under it. Choose No for add another store and click Next.
    For IAM role, select Choose an existing IAM role and pick the one with **`GlueLabRole`** in the name we have been using all along.
    For Schedule, keep default Run on demand and click Next
    For Output, select the `dms_docdb` from drop down and for prefix enter `parquet_` to differentiate tables in the same database for the lab.
    Finally, Finish and run the crawler as before.   
1.2 Crawler will run and add 2 tables as shown below:
    {{< img "crawler-parquet.png" "crawler-parquet" >}}   
1.3 For validation, From Glue console - click on Databases> Click the database `dms_docdb`. Click on Tables in dms_docdb  
1.4 You should see two tables with parquet_ as prefix. Click on them to explore the schema. For example: schema for parquet_enigma_jhu will look like this below 
    {{< img "parquet-schema1.png" "parquet-schema1" >}} 
    {{< img "parquet-schema2.png" "parquet-schema2" >}} 

You are now ready to use consumption services like Athena or Quicksight to explore transformed data and do analytics on top of it in next labs using the Glue data catalog objects.