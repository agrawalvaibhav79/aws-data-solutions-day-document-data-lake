+++
title = "Visualize data using Quicksight"
date = 2021-03-17T18:13:14-04:00
weight = 45
chapter = true
+++

In this lab, you will create Quicksight account and use the data from Athena lab to create some visuals and dashboard. Let's get started.

#### Create QuickSight account
1.  Search for Quicksight from the AWS Management console search bar at the top. Right click on the search output and open in a new tab.
    Then on the new tab, click on Sign up button.    
    {{< img "search-qs.png" "search-qs" >}} 
    {{< img "qs-signup-button.png" "qs-signup-button" >}} 
1.1 Select Enterprise option and Continue
1.2 Keep everything as default, except 
    QuickSight account name - it should be a unique name, typically your email-id without domain+date in DDMMYYYY format should be good enough
    Notification email address - it should be your office email
    Click Finish
    {{< img "qs-account.png" "qs-account" >}} 
    {{< img "qs-account2.png" "qs-account2" >}} 
    In couple minutes you should have a new Quicksight account. Click on Go to Amazon QuickSight
    {{< img "qs-congrats.png" "qs-account-done" >}} 
1.3 You are ready to work with QuickSight now once you see this
    {{< img "qs-land.png" "qs-land" >}} 

#### Create dataset
1.  Click on Datasets on the left pane, then click on **New dataset** button on top right
1.1 Ensure you are in Ohio or us-east-2 region
    {{< img "qs-region.png" "qs-region" >}} 
1.2 You should various data sources it can connect to, click on Athena
    {{< img "qs-ds-1.png" "qs-ds1" >}} 
    Provide a Data source name eg: `labdatalake_qs`
    Click on validate connection button. In few seconds it will validate Quicksight's connection to Athena
    Click on Create data source blue button.
    {{< img "qs-ds-2.png" "qs-ds2" >}} 
    Next, you are connected and select the `dms_docdb` database and pick the view you have created in the previous Athena lab. Click Select.
    {{< img "qs-table.png" "qs-table" >}} 
    Next, keep **Import to SPICE for quicker analytics** option if it is available OR select **Directly query your data** option. Click Visualize.
    {{< img "qs-visual.png" "qs-visual" >}} 
    If you select SPICE option in last step, it will take some time for data to get loaded into SPICE.
    This is where you will land up to
    {{< img "qs-visual-land.png" "qs-visual-land" >}} 