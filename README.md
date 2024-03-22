[comment]: # "Auto-generated SOAR connector documentation"
# AWS Athena

Publisher: Splunk  
Connector Version: 2\.2\.6  
Product Vendor: AWS  
Product Name: Athena  
Product Version Supported (regex): "\.\*"  
Minimum Product Version: 4\.9\.39220  

This app supports investigative actions on AWS Athena

[comment]: # " File: readme.md"
[comment]: # "  Copyright (c) 2017-2024 Splunk Inc."
[comment]: # ""
[comment]: # "  Licensed under Apache 2.0 (https://www.apache.org/licenses/LICENSE-2.0.txt)"
[comment]: # ""
## Asset Configuration

There are two ways to configure an AWS Athena asset. The first is to configure the **access_key** ,
**secret_key** and **region** variables. If it is preferred to use a role and Phantom is running as
an EC2 instance, the **use_role** checkbox can be checked instead. This will allow the role that is
attached to the instance to be used. Please see the [AWS EC2 and IAM
documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)
for more information.  
  
This app also requires some integration with AWS S3. Query results will be stored in a folder on S3
specified during a query action. These results can be encrypted using various encryption schemes,
some of which require a KMS key. This KMS key should be provided when configuring the asset and must
be applicable for the given region.

## Assumed Role Credentials

The optional **credentials** action parameter consists of temporary **assumed role** credentials
that will be used to perform the action instead of those that are configured in the **asset** . The
parameter is not designed to be configured manually, but should instead be used in conjunction with
the Phantom AWS Security Token Service app. The output of the **assume_role** action of the STS app
with data path **assume_role\_\<number>:action_result.data.\*.Credentials** consists of a dictionary
containing the **AccessKeyId** , **SecretAccessKey** , **SessionToken** and **Expiration** key/value
pairs. This dictionary can be passed directly into the credentials parameter in any of the following
actions within a playbook. For more information, please see the [AWS Identity and Access Management
documentation](https://docs.aws.amazon.com/iam/index.html) .


### Configuration Variables
The below configuration variables are required for this Connector to operate.  These variables are specified when configuring a Athena asset in SOAR.

VARIABLE | REQUIRED | TYPE | DESCRIPTION
-------- | -------- | ---- | -----------
**access\_key** |  optional  | password | AWS Access Key
**secret\_key** |  optional  | password | AWS Secret Key
**region** |  required  | string | Default Region
**kms\_key** |  optional  | password | KMS key
**use\_role** |  optional  | boolean | Use attached role when running Phantom in EC2

### Supported Actions  
[test connectivity](#action-test-connectivity) - Validate the asset configuration for connectivity using supplied configuration  
[list queries](#action-list-queries) - List named queries on Athena  
[run query](#action-run-query) - Run a named query on Athena  

## action: 'test connectivity'
Validate the asset configuration for connectivity using supplied configuration

Type: **test**  
Read only: **True**

#### Action Parameters
No parameters are required for this action

#### Action Output
No Output  

## action: 'list queries'
List named queries on Athena

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**credentials** |  optional  | Assumed role credentials | string |  `aws credentials` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.status | string | 
action\_result\.data\.\*\.NamedQuery\.Database | string | 
action\_result\.data\.\*\.NamedQuery\.Description | string | 
action\_result\.data\.\*\.NamedQuery\.Name | string | 
action\_result\.data\.\*\.NamedQuery\.NamedQueryId | string |  `athena named query` 
action\_result\.data\.\*\.NamedQuery\.QueryString | string |  `athena query` 
action\_result\.data\.\*\.ResponseMetadata\.HTTPHeaders\.connection | string | 
action\_result\.data\.\*\.ResponseMetadata\.HTTPHeaders\.content\-length | string | 
action\_result\.data\.\*\.ResponseMetadata\.HTTPHeaders\.content\-type | string | 
action\_result\.data\.\*\.ResponseMetadata\.HTTPHeaders\.date | string | 
action\_result\.data\.\*\.ResponseMetadata\.HTTPHeaders\.x\-amzn\-requestid | string | 
action\_result\.data\.\*\.ResponseMetadata\.HTTPStatusCode | numeric | 
action\_result\.data\.\*\.ResponseMetadata\.RequestId | string | 
action\_result\.data\.\*\.ResponseMetadata\.RetryAttempts | numeric | 
action\_result\.summary\.num\_queries | numeric | 
action\_result\.message | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric | 
action\_result\.parameter\.credentials | string |  `aws credentials`   

## action: 'run query'
Run a named query on Athena

Type: **investigate**  
Read only: **True**

The <b>query</b> parameter can take either a named query ID, or a query string\.<br><br>If the <b>database</b> parameter is not included, the query will run on the default database configured on Athena\.<br><br>The AWS API requires Athena query results be stored in a location on S3\. Use the <b>s3\_location</b> parameter to specify this location\.<br><br>To encypt the files containing the results, specify the desired encryption scheme with the <b>encryption</b> parameter\. If the given encryption scheme requires a KMS key, the action will use the <b>kms\_key</b> app configuration parameter\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**query** |  required  | Query to run | string |  `athena query`  `athena named query` 
**database** |  optional  | Database to run query on | string | 
**s3\_location** |  required  | S3 location to save results | string | 
**encryption** |  optional  | Encyption scheme of S3 location | string | 
**credentials** |  optional  | Assumed role credentials | string |  `aws credentials` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.status | string | 
action\_result\.parameter\.database | string | 
action\_result\.parameter\.encryption | string | 
action\_result\.parameter\.query | string |  `athena query`  `athena named query` 
action\_result\.parameter\.s3\_location | string | 
action\_result\.data\.\*\.\*\.VarCharValue | string | 
action\_result\.summary\.num\_rows | numeric | 
action\_result\.message | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric | 
action\_result\.parameter\.credentials | string |  `aws credentials` 