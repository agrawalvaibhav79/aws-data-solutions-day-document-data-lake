+++
title = "Interactive query using Athena"
date = 2021-03-17T18:13:14-04:00
weight = 44
chapter = true
+++

In this lab, we will use interactive query service Athena to query processed and crawled data from previous lab using SQL. Lets get started

1.  Go to [Athena console](https://us-east-2.console.aws.amazon.com/athena/home?region=us-east-2#query). First step is to get Athena setup to query successfully. This is a one-time setup for new accounts.   
{{% notice info %}}
Screenshot is only for illustration purpose, the actual bucket name for your lab will be similar NOT exactly the same
{{% /notice %}}
1.1 Click on Settings, input Query result location as s3://**<<labdatalake>>**/athenaqueryresults/ and click Save.
    {{< img "athena-settings.png" "athena-settings1" >}} 
    {{< img "athena-settings2.png" "athena-settings2" >}}   
1.2 Console is ready to query now. Select Database as `dms_docdb` from left navigation pane.
    {{< img "athena-query-editor.png" "athena-query-editor" >}}    
1.3 You cannot run multiple queries at the same time, so run below queries one by one OR copy and paste both but select and highlight one and run it one at a time by doing the same. OR use multiple query tabs
```bash
SELECT * FROM "dms_docdb"."parquet_enigma_jhu" limit 10;
```
```bash
SELECT * FROM "dms_docdb"."parquet_hospital_beds" limit 10;
```

This way you can query parquet files on S3 location directly using SQL on Athena. Notice the Run time and Data scanned metrics.
{{< img "athena-query1.png" "athena-query1" >}} 

1.4 Lets run another query using both the tables to see the growth of confirmed Covid cases joined side-by-side with hospital bed availability, broken down by US county

```bash
    SELECT 
    cases.fips, 
    admin2 as county, 
    province_state, 
    confirmed,
    growth_count, 
    sum(num_licensed_beds) as num_licensed_beds, 
    sum(num_staffed_beds) as num_staffed_beds, 
    sum(num_icu_beds) as num_icu_beds
    FROM 
    "covid-19"."hospital_beds" beds, 
    ( SELECT 
        fips, 
        admin2, 
        province_state, 
        confirmed, 
        last_value(confirmed) over (partition by fips order by last_update) - first_value(confirmed) over (partition by fips order by last_update) as growth_count,
        first_value(last_update) over (partition by fips order by last_update desc) as most_recent,
        last_update
        FROM  
        "covid-19"."enigma_jhu" 
        WHERE 
        country_region = 'US') cases
    WHERE 
    beds.fips = cases.fips AND last_update = most_recent
    GROUP BY cases.fips, confirmed, growth_count, admin2, province_state
    ORDER BY growth_count desc
```

{{< img "athena-query2.png" "athena-query2" >}} 

1.5 In Athena you can do several things with this query
    a.  You can use Save as to save this query, so that you can come back to it everytime you want to run it. Lets click on Save as button
    {{< img "athena-query-saveas1.png" "athena-query-saveas1" >}}   
    {{< img "athena-query-saveas2.png" "athena-query-saveas2" >}}  
    Click on Saved queries tab on the top, to access it  
    {{< img "athena-saved-query.png" "athena-saved-query" >}}  
    b.  You can create a View. From the same query editor tab, now click on Create button as shown to select `Create view from query` option.  
    {{< img "athena-create-view.png" "athena-create-view" >}}   
    Provide name as `covid_cases_by_usa_view` and click on Create button  
    {{< img "athena-create-view2.png" "athena-create-view2" >}} 
    {{< img "athena-create-view3.png" "athena-create-view3" >}} 
    c.  You can create a Table **(Try on your own)**
        You will provide S3 location and file format you want to store data into this new table. This gives analyst simple ETL mechanism using SQL where they can deploy ETL logic and create a table and data files on another S3 location. They may need for intermediate processing or sharing it with other users.

With this you are done trying out how you can leverage Athena for your interactive data query needs on your data lake. This gives you a powerful tool in the hands of your data analysts or data scientists to explore data using SQL. In the next lab we will use Amazon Quicksight to create some visuals and dashboards to show how easily you can consume same data on data lake via different types of tools.