# AWS DMS Training

Repository for AWS DMS Training

# Day 2

**1 - Log in to your AWS Account**

https://aws.amazon.com/console/

And select ap-southeast-2 Region (Sydney).

**2 - Create Key Pair Manually following the steps below:**

   - Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
   - In the navigation pane, under NETWORK & SECURITY, choose Key Pairs.
   - Choose Create Key Pair.
   - Enter a name for the new key pair in the Key pair name field of the Create Key Pair dialog box, and then choose Create.
      
      ***The private key file is automatically downloaded by your browser. The base file name is the name you specified as the name of your key pair, and the file name extension is .pem. Save the private key file in a safe place.

Important

      ***This is the only chance for you to save the private key file. You'll need to provide the name of your key pair when you launch an instance and the corresponding private key each time you connect to the instance.***

     If you will use an SSH client on a Mac or Linux computer to connect to your Linux instance, use the following command to set the permissions of your private key file so that only you can read it.
```
      chmod 400 my-key-pair.pem
```

   If you use windows you probably need convert your pem file to ppk file following the steps below:
   
   https://aws.amazon.com/premiumsupport/knowledge-center/convert-pem-file-into-ppk/
   
Reference:
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html

**3 - Access the Cloudformation Console:**

https://console.aws.amazon.com/cloudformation/home

and launch the stack below:

[Cloudformation Template](https://raw.githubusercontent.com/maiconrocha/DMS/master/CFN_Templates/dms.yaml).


  *You should choose a name for the stack and specify the key pair you previously launched
  
  This cloudformation stack is going to create the following AWS Resources in your account:
  
  - VPC(10.0.0.0/16)
  - Subnets
  - IGW
  - RDS MySQL
  - RDS SQLServer
  - RDS PostgreSQL
  - S3 Bucket
  - DMS Replication Instance
  - DMS Endpoints
  - Route 53 Domain
  - Route R3 RecordSets
  - IAM Roles
  - EC2 Instance (with all the clients pre configured)
  
  *Please note the DMS Tasks should be created manually.


## 4 - Connect to the EC2 Instance

Once we have source and target already configured to use DMS, we need populate some data on the source
and then migrate the data to the target using DMS.

You have to first access the EC2 Instance:

How access the ec2 Instance:

Allow your IP on the Security Group of the EC2 Instance:

```
Go to EC2 Console: 
https://console.aws.amazon.com/ec2

Select the instance 'DMSClients'
Click on the Security Group attached to the Instance
Click in Inbound
Edit
Add Rule:
SSH Port 22 Source: My Ip
Now, you should be able to connect.
```


If you are not sure in how to connect click again on the ec2 instance
and click on 'Connect' for more instructions.

Once connect using ec2-user, connect to either mysql or postgresql instance:

The scripts mysql.sh and pg.sh are configured with the credentials to access RDS Mysql and RDS PostgreSQL instances.

## 5 - Inserting data on source:

 Connect to the RDS Mysql:

 . mysql.sh 

And then create the user and insert data running the commands from the following repository:

https://github.com/aws-samples/aws-database-migration-samples/tree/master/mysql/sampledb/v1

git clone https://github.com/aws-samples/aws-database-migration-samples

mysql -h rdsmysql.dms.com -uroot -p < install-rds.sql

To confirm the tables were created under dms_user schema you can run the following:
```
SQL> SELECT TABLE_SCHEMA, TABLE_NAME 
     FROM information_schema.tables 
     WHERE TABLE_SCHEMA = 'dms_sample';
```
## 6 - Enable pre requirements on the RDS Source

Before create DMS Tasks, you have to go through the AWS Documentation and make sure
Source and Target have the pre requirements 

For the source MySQL database, please refer to the section "Using an AWS-managed MySQL-compatible database as a source for AWS DMS"
on the link below:

Mysql as a Source: https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.MySQL.html#CHAP_Source.MySQL.AmazonManaged

For the target you don't need to configure anything, we are already using RDS Master User which has all the privileges to insert data.
The documentation is just for reference: 

PostgreSQL as a target: https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Target.PostgreSQL.html

## 7 - Start Creating DMS Resources

Once the cloudformation stack was completed, you can start creating DMS Resources

You have two options to create resources:
Using the console or using CLI(Command Line Interface).
I would recommend use the console if you are doing it for the first time
following the steps below:

## 8 - Go to the DMS Console 

 https://console.aws.amazon.com/dms/home


## 10 - Creating DMS task:

```
Go to the DMS Console 

https://console.aws.amazon.com/dms/home

On the left menu, click on Tasks:
Create Task:

To create a migration task

On the Create Task page, specify the task options.

For This Option	                  - Do This
Task name                           - Type a name for the task. (I would suggest DMSTASK + Your name)
Task description                    - Type a description for the task.
Source endpoint                     - sourcerdsmysql
Target endpoint                     - targetrdspostgresql
Replication instance                - Select the Replication Instance you have created
Migration type                      - Migrate Existent Data and Replicate Ongoing changes
Start task on create                - Yes
CDC stop mode                       - Don't use custom CDC stop mode
Create recovery table on target DB  - No
Target table preparation mode       - Drop tables on target
Stop task after full load completes - Don't stop
Include LOB columns in replication  - Limited LOB mode
Max LOB size (kb)                   - 54 KB
Enable validation                   - No
Enable logging                      - Yes

Table Mappings:

Selection rules: 

Schema name is: dms_source

If the dms_source schema is not on the list, you should go to the DMS Endpoint and click on Refresh Schemas
or click on Select Schema and manually inform: dms_sample
Table name is like - %
Action - Include

- Add Selection Rule

Transformation rules - Yes

Target: Schema
Schema_name: dms_sample
Action: Rename to: (I would suggest dms_target + Your name)

- Create Task

```


For more information, please refer: https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.Creating.html



## 11 - You should be able to answer the following questions:


   ###### 1 - Was the task completed without any errors?
   
   *** :confused: If there is a error, have you followed the steps on the section "Using an AWS-managed MySQL-compatible database as a source for AWS DMS"
on the link below ?
    MySQL as a Source: https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.MySQL.html#CHAP_Source.MySQL.AmazonManaged

    Once you have fixed the error, you can restart the task
    Click on Start/Resume - Restart***

   ###### 2 - Are you able to find the data on RDS PostgreSQL target?

   ###### 3 - Were the tables created in UPPER CASE or lower case? Why?

   ###### 4 - Do you think any transformation rule should be created to migrate tables in upper case?

     https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TableMapping.html

   
  ###### 5 - Was DMS able to migrate the view under dms_sample schema?
 ```  
   - SELECT TABLE_SCHEMA, TABLE_NAME 
     FROM information_schema.tables 
     WHERE TABLE_TYPE LIKE 'VIEW'
       AND TABLE_SCHEMA = 'dms_sample';
```
   If not, why not?

   ###### 6 - There is a way to migrate views from Mysql? How?

   ###### 7 - Can ongoing changes(CDC) be enabled for a view?
   
   ###### 8 - Insert some data on the source after full load finish to make sure CDC is working.
   
   ###### 9 - What happens when you drop a table on source? will the table be removed on target as well?
   
   ###### 10 - Create a table manually under dms_sample schema and include this table on the DMS Task. Make sure table was migrated to the target.


   # Thank you for your hard work. :thumbsup: :clap: :muscle:
   # Good luck. :four_leaf_clover:










