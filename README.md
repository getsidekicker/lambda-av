 AWS Buckets Antivirus Scan Project  
====================================

Project summary
---------------
Aim of this project is to provide an antivirus scan tool to check uploaded to the AWS S3 buckets files using AWS Lambda 
function and free ClamAV antivirus.

External tools
----------
This project is based on [bucket-antivirus-function](https://github.com/upsidetravel/bucket-antivirus-function) 
which is a ClamAV antivirus AWS Lambda wrapper function written in Python.

[Serverless Framework](https://www.serverless.com/) is used to deploy the code to the AWS infrastructure. 

Scanned buckets
---------------

AWS S3 buckets to be included in the scan:
- arn:aws:s3:::getsidekicker.com-public
- arn:aws:s3:::getsidekicker.com-sensitive

How it works     
------------

The project consist of two main services: 
- update:  AWS Lambda Function to update the antivirus definition files 
- scan: Virus scanner AWS Lambda Function

After uploading files to one of the [buckets](#scanned-buckets) there is an event triggered which runs 
the scanner against the uploaded file. If the file is infected, the scanner function will send alert message on the 
Slack, to the "AWS Alert" channel. 

In addition, it's can block file download from the S3. This behavior to be decided.    
 
After [building](https://github.com/upsidetravel/bucket-antivirus-function#installation) the function from the source 
the output Lambda Function should be used   

Deploying using Serverless 
--------------------------

1. Install Serverless CLI as described [here](https://www.serverless.com/framework/docs/getting-started/).
2. Configure AWS credentials as described [here](https://www.serverless.com/framework/docs/providers/aws/guide/credentials/).

3. Run:
`bash
$ serverless deploy --aws-profile engineering
`

Note: `--aws-profile engineering` parameter might be different depending on the step 2. 

Buckets configuration
---------------------
It's only possible to configure one bucket per function using same event type, so the `getsidekicker.com-public` bucket event was manually configured to trigger scan on object creation. 

Testing
-------

Use [EICAR ANTI MALWARE TEST FILE](https://www.eicar.org/?page_id=3950) to download a "test virus" and do a real world test.

The EICAR is non-viral but is detected by anti-virus software as if it a virus code. 
