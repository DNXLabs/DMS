# L2H_DMS_event
Repository for AWS L2H DMS event


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

and launch the stack:
```
    [dms.yaml](CFN_Templates/dms.yaml)
```
