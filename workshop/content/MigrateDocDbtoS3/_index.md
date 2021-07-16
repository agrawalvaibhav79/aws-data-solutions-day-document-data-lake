+++
title = "Lab 02: Migrate from DocumentDB to S3 using Database Migration Service (DMS)"
date = 2021-03-17T18:13:14-04:00
weight = 41
chapter = true
+++

#### Let's connect to DocumentDB cluster and prepare the data
1. Navigate to [CloudFormation](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2). 
2. Click on the stack(mod-XXXXXXX) and then the outputs tab.
{{< img "cfn.png" "Cloudformation" >}}
3. Copy the output to a text editor for later use.
4. Now let’s install a mongo client and connect to DocumentDB. Navigate to the [Cloud9 aws console](https://us-east-2.console.aws.amazon.com/cloud9/home?region=us-east-2).
5. Open your IDE. The command prompt is in the bottom pane. You can adjust size so it is more usable.
6. Let’s run these three commands to set the repository, install the clients and get the certificate.
```bash
    echo -e "[mongodb-org-4.0] \nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/amazon/2013.03/mongodb-org/4.0/x86_64/\ngpgcheck=1 \nenabled=1 \ngpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc" | sudo tee /etc/yum.repos.d/mongodb-org-4.0.repo
    sudo yum install -y mongodb-org-shell
    sudo yum install -y mongodb-org-tools.x86_64
    wget https://s3.amazonaws.com/rds-downloads/rds-combined-ca-bundle.pem
```
7. Let’s get the Covid 19 data sets. 
    ```bash 
    aws s3 ls s3://covid19-lake/enigma-jhu/json/ #(This will get us the current file name that we use in next step) 

    aws s3 cp s3://covid19-lake/enigma-jhu/json/UseTheFileNameFromThePreviousStep.json UseTheFileNameFromThePreviousStep.json #Note use the file name returned from the previous step. 

    aws s3 cp s3://covid19-lake/rearc-usa-hospital-beds/json/usa-hospital-beds.geojson usa-hospital-beds.geojson
    ```
8. Now let's load the data sets. Be sure to replace the file name in the first command with the output step 7 above. Be sure to replace the host from the cloudformation output.
    ```bash
    mongoimport --ssl --host=mod-YourClusterFromCFNOutput.docdb.amazonaws.com:27017 --collection=enigma-jhu --db=Covid19 --file=UseTheFileNameFromStep7.json --numInsertionWorkers=4 --username=dbmaster --sslCAFile rds-combined-ca-bundle.pem --password=dbmaster123 

    mongoimport --ssl --host=labdatalake-YourClusterFromCFNOutput.docdb.amazonaws.com:27017 --collection=rearc-usa-hospital-beds --db=Covid19 --file=usa-hospital-beds.geojson --numInsertionWorkers 4 --username=dbmaster --sslCAFile=rds-combined-ca-bundle.pem --password=dbmaster123
    ```
9. Let’s create a DMS replication instance. Go to the [DMS console](https://us-east-2.console.aws.amazon.com/dms/v2/home?region=us-east-2#firstRun) and click on create replication instance.

{{< img "dms1.png" "DMS step1" >}}
10. Fill out the dialog as shown:
{{< img "dms2.png" "DMS step2" >}} 

{{< img "dms3.png" "DMS step3" >}} 

11. Click on endpoints and then create endpoint. Create the source endpoint to DocumentDB by filling out the dialog as below using the endpoint of your cluster. Add new CA certificate with this [file.](https://s3.amazonaws.com/rds-downloads/rds-combined-ca-bundle.pem) After the dialog is filled out, please test the connection and then hit create endpoint.
{{< img "docsourceendpoint.png" "Source Endpoint" >}}
12. Click on endpoints and then create endpoint. Create the target endpoint to S3 by filling out the dialog as below. After the dialog is filled out, please test the connection and then hit create endpoint.
{{< img "TargetEndpoint.JPG" "S3 target" >}}
13. Click on database migration tasks and then create task.
14. Fill out the task configuration as show below.
{{< img "TaskConfig.png" "Task Config" >}}
15. Enable CloudWatch logs under task settings as shown below.
{{< img "TaskSettings.png" "Task Settings" >}}
16. On table mappings, add a new selection rule with % wild cards for schema and table as seen below.
{{< img "TableMappings.png" "Table Mappings" >}}
17. Click create task.
18. After your task is created, click on it and then click on the the table statistics tab. We want to verify the status.
{{< img "TaskResults.PNG" "Task Results" >}}
