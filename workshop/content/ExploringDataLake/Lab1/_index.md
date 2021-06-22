+++
title = " Catalog raw data with Glue crawler "
date = 2021-03-17T18:13:14-04:00
weight = 41
chapter = true
+++


#### Create Glue crawler #1
1.  Use AWS Management Console to search for `AWS Glue` and if using Google Chrome, right click and open in new tab in the browser as shown. Make sure region is Ohio.
{{< img "search-glue.png" "search glue" >}}
1.1 On [Glue console](https://us-east-2.console.aws.amazon.com/glue/home?region=us-east-2), in left navigation pane, click on Cralwers>Add crawler
{{< img "crawler-console.png" "go to crawler" >}}
1.2 Enter Crawler name as `dms_source_jhu_crawler` and click Next
{{< img "crawler1.png" "go to crawler1" >}}
1.3 Select default selection of Crawler source type as Data stores and Repeat crawls of S3 data stores as Crawl all folders. Click Next 
{{% notice info %}}
Screenshot is only for illustration purpose, the actual bucket name for your lab will be similar NOT exactly the same
{{% /notice %}}      
1.4 For Add a data store- 
    keep S3 selected, 
    Connection as blank, 
    Crawl data in with Specified path in my account, 
    For Include path select the S3 bucket and folder where the DMS ingested data is stored; similar to s3://**<<dmslabs3bucket>>**/Covid19/enigma-jhu and click Next
    {{< img "crawler-datastore.png" "jhu crawler data store" >}}
1.5 For Add another data store keep default as No and click Next
1.6 For Choose an IAM role - select a Role with **GlueLabRole** in the name as shown below and click Next
    {{< img "crawler-role.png" "crawler role" >}}
1.7 For Schedule, pick Run on demand option and click Next    
1.8 For Output, click on Add database, and input `dms_docdb` as Database name and click Create  
    button. Then click Next after leaving everything thing else as default
    {{< img "crawler-add-database.png" "crawler database" >}}
    {{< img "crawler-add-database2.png" "crawler database2" >}}
1.9 At last, review everything and click Finish.   
1.10 It brings you back to Crawler console with a new crawler added. You select it using checkbox 
    before it and click on Run crawler button to run it. You will see as below -
    {{< img "crawler-run.png" "crawler run" >}}
    {{< img "crawler-starting.png" "crawler starting" >}}
    {{< img "crawler-stopping.png" "crawler stopping" >}}
1.11 Once you see ( takes about 1 min.) crawler is in Ready status and 1 table was added
    {{< img "crawler-ready.png" "crawler ready" >}}

#### Create Glue crawler #2
2. On Glue console, in left navigation pane, click on Crawlers>Add crawler   
 2.1 Enter Crawler name as `dms_source_hosp_bed_crawler` and click Next
{{< img "crawler2.png" "go to crawler2" >}}
 2.2 For Crawler source type keep everything as default and Click Next  
 {{% notice info %}}
Screenshot is only for illustration purpose, the actual bucket name for your lab will be similar NOT exactly the same
{{% /notice %}}     
 2.3 For Add a data store- 
    keep S3 selected, 
    Connection as blank, 
    Crawl data in with Specified path in my account, 
    Include path select the S3 bucket and folder where the DMS ingested data is stored; similar to s3://**<<dmslabs3bucket>>**/Covid19/rearc-usa-hospital-beds and click Next
    {{< img "crawler2-datastore.png" "jhu crawler data store" >}}
2.4 For Add another data store keep default as No and click Next   
2.5 For Choose an IAM role - select an existing Role with **GlueLabRole** in the name as you did with the previous crawler. Click Next
2.6 For Schedule, pick Run on demand option and click Next   
2.7 For Output, from drop down pick previously created database `dms_docdb` as Database 
    Then click Next after leaving everything thing else as default
    {{< img "crawler-existing-pick-database.png" "crawler pick existing database" >}}
2.8 At last, review everything and click Finish   
2.9 It brings you back to Crawler console with the new crawler added. You select it using checkbox 
    before it and click on Run crawler button to run it as before. It goes through same stages as it did previously.  
2.10 Once you see (takes about 1 min.) crawler is in Ready status and 1 table was added
    {{< img "hosp-crawler-ready.png" "hosp-crawler ready" >}}


3. From left navigation, click Databases and click on the `dms_docdb` and then click on Tables in dms-docdb to see the two tables got added by the crawler. Click on each to 
   explore the schema. For example, enigma_jhu schema would look like this -
   {{< img "enigma_jhu_schema.png" "enigma_jhu_schema" >}}

After you have crawled raw data, lets use the catalog objects as source to transform the data using Glue Studio and ETL jobs next.



