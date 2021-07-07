+++
title = "Lab 02: Migrate from DocumentDB to S3 using Database Migration Service (DMS)"
date = 2021-03-17T18:13:14-04:00
weight = 41
chapter = true
+++

#### Let's connect to DocumentDB cluster and prepare the data
1. Navigate to [CloudFormation](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2). 
2. Click on the stack and then the outputs tab.
{{< img "cfn.png" "Cloudformation" >}}
3. Copy the outputs to a text editor for later use.
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
    7a. aws s3 ls s3://covid19-lake/enigma-jhu/json/ #(This will get us the current file name that we use in step b)
    7b. aws s3 cp s3://covid19-lake/enigma-jhu/json/UseTheFileNameFromThePreviousStep.json UseTheFileNameFromThePreviousStep.json #Note use the file name returned from the previous step.
    7c. aws s3 cp s3://covid19-lake/rearc-usa-hospital-beds/json/usa-hospital-beds.geojson usa-hospital-beds.geojson
8. Now let's load the data sets. Be sure to replace the file name in 8a with the output of 7a. Be sure to replace the host from the cloudformation output.
    8a. mongoimport --ssl --host=labdatalake-YourClusterFromCFNOutput.docdb.amazonaws.com:27017 --collection=enigma-jhu --db=Covid19 --file=UseTheFileNameFrom7a.json --numInsertionWorkers=4 --username=dbmaster --sslCAFile rds-combined-ca-bundle.pem --password=dbmaster123
    8b. mongoimport --ssl --host=labdatalake-YourClusterFromCFNOutput.docdb.amazonaws.com:27017 --collection=rearc-usa-hospital-beds --db=Covid19 --file=usa-hospital-beds.geojson --numInsertionWorkers 4 --username=dbmaster —sslCAFile=rds-combined-ca-bundle.pem --password=dbmaster123
9. Let’s create a DMS replication instance. Go to the [DMS console](https://us-east-2.console.aws.amazon.com/dms/v2/home?region=us-east-2#firstRun) and click on create replication instance.
{{< img "dms1.png" "DMS step1" >}}
10. Fill out the form as shown:
{{< img "dms2.png" "DMS step2" >}}
{{< img "dms3.png" "DMS step3" >}}
