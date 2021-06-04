+++
title = "Create Glue Data Catalog"
date = 2021-03-17T18:13:14-04:00
weight = 41
chapter = true
+++



1.  Use AWS Management Console to search for `AWS Glue` and if using Google Chrome, right click and open in new tab in the browser as shown
{{< img "search-glue.png" "search glue" >}}
1.1 On Glue console, in left navigation pane, click on Cralwers>Add crawler
{{< img "crawler-console.png" "go to crawler" >}}
1.2 Enter Crawler name as `dms-source-crawler` and click Next
{{< img "crawler1.png" "go to crawler1" >}}
1.3 Select default selection of Crawler source type as Data stores and Repeat crawls of S3 data stores as Crawl all folders. Click Next       
1.4 For Add a data store- 
    keep S3 selected, 
    Connection as blank, 
    Crawl data in with Specified path in my account, 
    Include path select the S3 bucket and folder where the DMS ingested parquet data is stored as shown and click Next
    {{< img "crawler-datastore.png" "crawler data store" >}}
1.5 For Add another data store keep default as No and click Next
1.6 For Choose an IAM role - select as show below and click Next
    {{< img "crawler-role.png" "crawler role" >}}
1.7 For Schedule, pick Run on demand option and click Next
1.8 For Output, click on Add database, and input `dms-docdb` as Database name and click Create  
    button. Then click Next after leaving everything thing else as default
    {{< img "crawler-add-database.png" "crawler database" >}}
    {{< img "crawler-add-database2.png" "crawler database2" >}}
1.9 At last, review everything and click Finish
    {{< img "crawler-review.png" "crawler review" >}}
1.10 It brings you back to Crawler console with a new crawler added. You select it using checkbox 
    before it and click on Run crawler to run it. You will see as below -
    {{< img "crawler-run.png" "crawler run" >}}
    {{< img "crawler-starting.png" "crawler starting" >}}
    {{< img "crawler-stopping.png" "crawler stopping" >}}
1.11 Once you see (about in 1 min.) crawler is in Ready status and 2 tables are added
    {{< img "crawler-ready.png" "crawler ready" >}}

2. From left navigation, click Databases and you will new database got added as below
    {{< img "check-database.png" "check database" >}}
    2.1 Click on the database `dms-docdb` and then click on Tables in dms-docdb to see the two tables got added by the crawler. Click on each to explore the schema.
        {{< img "click-database.png" "click database" >}}
        {{< img "view-tables.png" "view tables" >}}
    for example: table1 schema is like this. Similary you can check schema for table2 as well -
    {{< img "table1-schema1.png" "table1-schema1" >}}
    {{< img "table1-schema.png" "table1 schema" >}}



