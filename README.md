# AWS Learn to Hire - DMS Event

Repository for AWS L2H DMS event

# Day 2

**1 - Please inform your AWS Account ID to the instructor in order to whitelist your account.**

**2 - Log in to your AWS Account**

https://aws.amazon.com/console/

And select us-east-1 Region (N.Virginia).

**3 - Create Key Pair Manually following the steps below:**

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

[Cloudformation Template](https://raw.githubusercontent.com/maiconrocha/L2H_DMS_event/master/CFN_Templates/dms.yaml).


  *You should choose a name for the stack and specify the key pair you previously launched
  
  This cloudformation stack is going to create the following AWS Resources in your account:
  
  - VPC(10.0.0.0/16)
  - Subnets
  - IGW
  - RDS Oracle
  - RDS MySQL
  - RDS SQLServer
  - RDS PostgreSQL
  - Redshift Cluster
  - S3 Bucket
  - DMS Replication Instance
  - DMS Endpoints
  - Route 53 Domain
  - Route R3 RecordSets
  - EC2 Instance (with all the clients pre configured)
  
  *Please note the DMS Tasks should be created manually.
  
  
  # Day 3 - Challenge Day :muscle:

For today we expect you to be able to create DMS Replication Instance, DMS Endpoints
and DMS tasks manually.

All the infrastructure needed as a pre requirement to create DMS resources
will be created by the cloudformation template below

[challenge.yaml](https://raw.githubusercontent.com/maiconrocha/L2H_DMS_event/master/CFN_Templates/challenge.yaml).

This cloudformation stack is going to create the following AWS Resources in your account:

- VPC(10.0.0.0/16)
- Subnets
- IGW
- RDS Oracle
- RDS MySQL
- Route 53 Domain
- Route R3 RecordSets
- EC2 Instance (with all the clients pre configured)

So, you need to go through the following steps:

## 1 - Login to AWS Console in us-east-1(N.Virginia) region:

 https://aws.amazon.com/console/

## 2 - Make sure you have pre created a Key Pair. (You can use the same you created yesterday)

 If you are using windows you probably need convert your pem file to ppk file following the steps below:

 https://aws.amazon.com/premiumsupport/knowledge-center/convert-pem-file-into-ppk/

Reference: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html

## 3 - Access the Cloudformation Console:

 https://console.aws.amazon.com/cloudformation/home

and launch the stack below:

[challenge.yaml](https://raw.githubusercontent.com/maiconrocha/L2H_DMS_event/master/CFN_Templates/challenge.yaml).

You should save the file on your local machine and then 
inform this file when launching the cloudformation stack.

The time the stack takes to complete is about 15 - 20 minutes.

## 4 - Start Creating DMS Resources

Once the cloudformation stack was completed, you can start creating DMS Resources

You have two options to create resources:
Using the console or using CLI(Command Line Interface).
I would recommend use the console if you are doing it for the first time
following the steps below:

## 5 - Go to the DMS Console 

 https://console.aws.amazon.com/dms/home

## 6 - Creating Replication Instance

```
- On the left menu, click on Replication Instance
- Click on 'Create Replication Instance'

For This Option             - Do This

Name                        - Type a name for the replication instance that contains from 8 to 16 printable ASCII characters (excluding /,", and @).
Description                 - Type a brief description of the replication instance.
Instance class  		       - dms.t2.medium
Replication engine version  - 2.4.3
VPC                         - Choose the Amazon Virtual Private Cloud (Amazon VPC) which was created by the cloudformation. Should be the VPC with longer characters. If you are not sure go to VPC console(https://console.aws.amazon.com/dms) and find the VPC ID for CIDR (10.0.0.0/16).

Multi-AZ                    - no
Publicly accessible         - no

Advanced:

Allocated storage (GB)      - 50 GB
Replication Subnet Group    - dms-subnet-group
Availability zone           - no preference
VPC Security Group(s)       -  You should select the security group which have the string 'InstanceSecurityGroup' 
KMS master Key              - Use the default

Maintenance:

Leave the default

- Click on create Replication Instance
```


If you would like more information,
please refer to the section "Creating a Replication Instance" on the link: https://docs.aws.amazon.com/dms/latest/userguide/CHAP_ReplicationInstance.html



## 7 - Once the Replication Instance is created, you can start creating the DMS Endpoints:

```
- On the left menu, click on 'Endpoints'
- Create Endpoint

Creating the Source Endpoint:

- Endpoint type : Source
- Select RDS DB Instance: rdsoracle
- Endpoint identifier: rdsoracle-source
- Source engine - Oracle
- Server Name - Leave the option automatic filled
- Port: 1521
- User name: root
- Password: Root1234
- SID/Service Name: ORCL

Test Endpoint connection:


VPC: Choose the Amazon Virtual Private Cloud (Amazon VPC) which was created by the cloudformation. Should be the VPC with longer characters. If you are not sure go to VPC console(https://console.aws.amazon.com/dms) and find the VPC ID for CIDR (10.0.0.0/16).


Replication Instance: Select the  Replication Instance you have created

Run test: Connection should be successfull

- Save
```

```

Creating the Target Endpoint:

- Endpoint type : Target
- Select RDS DB Instance: rdsmysql
- Endpoint identifier: rdsmysql-target
- Source engine - mysql
- Server Name - Leave the option automatic filled
- Port: 3306
- SSL Mode; none
- User name: root
- Password: Root1234

Test Endpoint connection:


VPC: Choose the Amazon Virtual Private Cloud (Amazon VPC) which was created by the cloudformation. Should be the VPC with longer characters. If you are not sure go to VPC console(https://console.aws.amazon.com/dms) and find the VPC ID for CIDR (10.0.0.0/16).

Replication Instance: Select the  Replication Instance you have created

Run test: Connection should be successfull

- Save
 ```



## 8 - Enable pre requirements on the RDS Source

Before create DMS Tasks, you have to go through the AWS Documentation and make sure
Source and Target have the pre requirements 

For the source Oracle database, please refer to the section "Working with an Amazon-Managed Oracle Database as a Source for AWS DMS"
on the link below:
Oracle as a Source: https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.Oracle.html

Since the user on the endpoint is the RDS Master User, you don't need to grant the privileges on section "User Account Privileges Required on an Amazon-Managed Oracle Source for AWS DMS
"
You have just to follow steps on the section: "Configuring an Amazon-Managed Oracle Source for AWS DMS"
nothing else.


For the target you don't need to configure anything, we are already using RDS Master User which has all the privileges to insert data.
The documentation is just for reference: 
Mysql as a target: https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Target.MySQL.html


## 9 - Connect to the EC2 Instance

Once we have source and target already configured to use DMS, we need populate some data on the source
and then migrate the data to the target using DMS.

You have to first access the EC2 Instance:

How access the ec2 Instance:

Allow your IP on the Security Group of the EC2 Instance:

```
Go to EC2 Console: 
https://console.aws.amazon.com/ec2

Select the instance 'Clients'
Click on the Security Group attached to the Instance
Click in Inbound
Edit
Add Rule:
SSH Port 22 Source: My Ip
Now, you should be able to connect.
```


If you are not sure in how to connect click again on the ec2 instance
and click on 'Connect' for more instructions.

Once connect using ec2-user, go to the root account:

sudo su -

The scripts oracle.sh and mysql.sh are configured with the credentials to access RDS Oracle and RDS Mysql Instances.


## 10 - Inserting data on source:

 On the home directory of the root account, you should run:

 wget https://raw.githubusercontent.com/maiconrocha/L2H_DMS_event/master/data/hr_cre.sql
 wget https://raw.githubusercontent.com/maiconrocha/L2H_DMS_event/master/data/hr_popul.sql

This is going to download the files on the home directory:

hr_cre.sql
hr_popul.sql

After download the files,
Connect to the RDS Oracle:

 . oracle.sh 

And then create the user and insert data running the commands below:

SQL> create user HR identified by HR;
SQL> alter user HR quota unlimited on USERS;

SQL> ALTER SESSION SET CURRENT_SCHEMA = HR;

SQL> @hr_cre.sql
SQL> @hr_popul.sql

To confirm the tables were created under HR account you can run the following:

SQL> select table_name from dba_tables where owner = 'HR';


## 11 - Creating DMS task:

```
Go to the DMS Console 

https://console.aws.amazon.com/dms/home

On the left menu, click on Tasks:
Create Task:

To create a migration task

On the Create Task page, specify the task options.

For This Option	                  - Do This

Task name                           - Type a name for the task.

Task description                    - Type a description for the task.
Source endpoint                     - rdsoracle-source
Target endpoint                     - rdsmysql-target
Replication instance                - Select the  Replication Instance you have created
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

Schema name is: HR

If the HR schema is not on the list, you should go to the DMS Endpoint and click on Refresh Schemas
or click on Select Schema and manually inform: HR
Table name is like - %
Action - Include

- Add Selection Rule

Transformation rules - No

- Create Task

```


For more information, please refer: https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.Creating.html



## 12 - You should be able to answer the following questions:


   ###### Was the task complete without any errors?
   
   *** :confused: If there is a error, have you followed the steps on the section "Working with an Amazon-Managed Oracle Database as a Source for AWS DMS"
on the link below ?
    Oracle as a Source: https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.Oracle.html

    Once you have fixed the error, you can restart the task
    Click on Start/Resume - Restart***

   ###### Are you able to find the data on RDS Mysql target?

   ###### Were the tables created in UPPER CASE or lower case? Why?

   ###### Do you think any transformation rule should be created to migrate tables in lower case?

     https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TableMapping.html

   
  ###### Was DMS able to migrate the view under HR schema?
   
   - select VIEW_NAME from dba_views where owner ='HR';

   If not, why not?

   ###### There is a way to migrate views from Oracle? How?

   ###### Can ongoing changes(CDC) be enabled for a view?



   # Thank you for your hard work. :thumbsup: :clap: :muscle:
   # Good luck. :four_leaf_clover:










