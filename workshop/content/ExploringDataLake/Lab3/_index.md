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
    For Data store, Include path as `s3://labdatalake/parquet/`
    For IAM role, select Choose an existing IAM role and pick the one with `labdatalake` in the name we have been using all along.
    For output, select the `dms_docdb` and for prefix input `parquet_` to differentiate tables in the same database for the lab.
    Finally, Finish and run the crawler as before.   
1.2 Crawler will run and add 2 tables as shown below:
    {{< img "crawler-parquet.png" "crawler-parquet" >}}   
1.3 For validation, From Glue console - click on Databases> Click the database `dms_docdb`. Click on Tables in dms_docdb  
1.4 You should see two tables with parquet_ as prefix. Click on them to explore the schema. For example: schema for parquet_enigma_jhu will look like this below 
    {{< img "parquet-schema1.png" "parquet-schema1" >}} 
    {{< img "parquet-schema2.png" "parquet-schema2" >}} 

You are now ready to use consumption services like Athena or Quicksight to explore transformed data and do analytics on top of it in next labs using the Glue data catalog objects.