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
1.2 You should see various data sources it can connect to, click on Athena
    {{< img "qs-ds-1.png" "qs-ds1" >}} 
    Provide a Data source name eg: `labdatalake_qs`
    Click on validate connection button. In few seconds it will validate Quicksight's connection to Athena
    Click on Create data source blue button.
    {{< img "qs-ds-2.png" "qs-ds2" >}} 
    Next, you are connected and select the `dms_docdb` database and pick the `parquet_enigma_jhu` table. Click on Edit/Preview data button.
    {{< img "qs-table.png" "qs-table" >}} 
    This brings you to visual query editor console. There on top change Dataset name to `labdatalake_dataset` and then go to top right to click on Add data blue button to add another table `parquet_hospital_beds`
    {{< img "qs-table2.png" "qs-table2" >}} 
    Next click on the two red bubbles, and you will see some options below to join. Select inner join and select from drop down columns `fips` to join the two tables.
    You can also use a view with all the join logic in Athena but this is a way to do it from QuickSight. Click Apply. It will preview all the columns with data below.
    {{< img "qs-table-join.png" "qs-table-join" >}} 

    Take a moment to scroll and check the data, then click on Save & visualize button on top right to go to visualize canvas. You will see [AutoGraph](https://docs.aws.amazon.com/quicksight/latest/user/autograph.html) is pre-selected
    {{< img "qs-save-visualize.png" "qs-save-visualize" >}} 
    {{< img "qs-visual-land.png" "qs-visual-land" >}} 

#### Create visual
{{% notice info %}}
Screenshot is only for illustration purpose, the actual data may change during the lab. The idea is to show to you ease of use of QuickSight, some of its features and give you hands-on experience using it.
{{% /notice %}}
1.  By default, AutoGraph is selected as visual type. It charts your data automatically using the most approprite visual for the data you choose.   
1.1 In the field list click on `# confirmed` field. By default, AutoGraph will show the Sum of confirmed covid cases as metric in the tile. Take a moment to review as shown below -
    {{< img "qs-autograph-number.png" "qs-autograph-number" >}} 
    You will see a blue outline around the visual tile, which means the tile is currently selected.
    Highlight the title `Sum of Confirmed` and change it to `Confirmed cases`   
1.2 Click outside the selected tile and click on `deaths` column from Field list. The Autograph will create another tile
    {{< img "qs-visual2.png" "qs-visual2" >}} 
1.3 Now you realized, you need to change data type for a column `last_update` from string to date. Click on penil icon next to Dataset. It will give you the dataset you have created. Click ellipses again and choose Edit option. It will take back to data editor console. In Data editor, from left search for `last_update`. Then next to column, click ellipses again>Change data type>Date options.
    {{< img "qs-visual-edit-ds.png" "qs-visual-edit-ds" >}} 
    {{< img "qs-visual-edit-ds2.png" "qs-visual-edit-ds2" >}} 

    After you select Date, it will ask you for the date format. Copy and paste below into the date format box and click Validate. If all good, click on Update.
    ```bash 
    yyyy-MM-dd'T'HH:mm:ss 
    ```
    {{< img "qs-visual-edit-ds3.png" "qs-visual-edit-ds3" >}} 
    {{< img "qs-visual-edit-ds4.png" "qs-visual-edit-ds4" >}} 

    Click on Save & visualize button again to go back to Visual canvas. In the visual canvas, you will notice icon next to last_update is changed to a calendar denoting it has Date datatype now.    
1.4 Next lets select Line chart for the next Visual type. First click on `+Add` button on top left of the page and choose Add visual to add another tile to the canvas.
For this visual, lets select `last_update` on X-axis and `confirmed` as the value which you are tracking by it.
    {{< img "qs-add-visual.png" "qs-add-visual" >}} 

    Then on the selected tile, Choose Line Chart
    {{< img "qs-visual-line1.png" "qs-visual-line1" >}} 

    Then from Fields list, pick `last_update`
    {{< img "qs-visual-line1.png" "qs-visual-line2" >}} 

    Then from Fields list, pick `confirmed`. The line chart will update to show distribution of confirmed covid cases by date.
    {{< img "qs-visual-line3.png" "qs-visual-line3" >}}

    You can use mouse and drag to fill all the space with this visual.
    As things are now, last_update is shown at date granularity. You can change that to Week (or anything else), by clicking on the dropdown next to last_update column in the Field wells on the top of the tile.
    {{< img "qs-visual-line4.png" "qs-visual-line4" >}}
1.5 Next lets use some ML features out of the box.   
    1.5.1 Forecast   
            While you have the Line chart tile selected. On its right edge, click on ellipses and Add forecast
            {{< img "qs-visual-line5.png" "qs-visual-line5" >}}
    QuickSight used the existing data and created an Orange colored line forecasting for next 14 periods (defaul) in future. The more the data, the better is forecasting as it is with any ML algorithm. You can change the Periods and use this feature without writing a single line of code.   

            {{< img "qs-visual-line6.png" "qs-visual-line6" >}}
    1.5.2 Natural language narrative   
            While it is always good to have charts and visuals. You can enrich them by adding narratives in English. QuickSight automatically generates them for you depending on the data for a visual. Lets click on the left pane, on Insights button. You will see a list of suggested insights, which you can add using the `=` next to it.
            Lets click `+` next to Highest Week and Lowest Week metrics. You can then position the tiles right above line chart as it is related to that.
            {{< img "qs-visual-line7.png" "qs-visual-line7" >}}
            {{< img "qs-visual-line8.png" "qs-visual-line8" >}}
            {{< img "qs-visual-line9.png" "qs-visual-line9" >}}
    1.5.3 Geospatial visual   
            Finally, lets create a geospatial visual. Click outside the current selected visual in empty area.
            Again from top left corner click on `+ Add visual` button to add a new visual. Select `Point on map` visual type from the menu.
            Then from Fields list, click on State_name, Confirmed and county_name columns. Expand the tile by dragging the edges, to cover more space on the canvas.
            {{< img "qs-visual-geo1.png" "qs-visual-geo1" >}}

            Click on the gear icon on the visual tile, to open Format visual menu. From there, click on Legend and uncheck `Show legend` box.   

            {{< img "qs-visual-geo2.png" "qs-visual-geo2" >}}
            {{< img "qs-visual-geo3.png" "qs-visual-geo3" >}}

            On the map, you can now hover on any state, and see information about the counties under the state.   

            {{< img "qs-visual-geo4.png" "qs-visual-geo4" >}}

This lab gave you a good overview of visuals and how to use them on QuickSight. You can add actions for end users on QuickSight visuals too. Please read more about [filters](https://docs.aws.amazon.com/quicksight/latest/user/filtering-visual-data.html) and [drill-downs](https://docs.aws.amazon.com/quicksight/latest/user/adding-drill-downs.html)

#### Publish dashboad
After you have created individual visuals, you would need to share them using a dashboard with your end users to start using these visuals.
1.  If you are not still in the `labdatalake_dataset analysis`, you can go there using QuickSight>Analyses>labdatalake_dataset analysis.
    once you are in the analyses, click on top right `Share` button
    {{< img "qs-publish-dash1.png" "qs-publish-dash1" >}}

    Give a name for the dashboard like - `labdatalake-covid-cases-dashboard` and Click Publish dashboard button

    {{< img "qs-publish-dash2.png" "qs-publish-dash2" >}}

    Skip the `Share dashboard` screen by `x` on the pop up.

    What you see next is the published dashboard as it will appear to a consumer once shared.
    {{< img "qs-publish-dash3.png" "qs-publish-dash3" >}}

Congratulations!! You finished all the labs to explore document data using your data lake.

