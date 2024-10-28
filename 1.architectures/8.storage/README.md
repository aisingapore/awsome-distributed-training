# Deploy FSX for Lustre with S3 Integration

This sample creates a FSX storage with S3 Data repository integration.

## Prerequesites

This solution requires a security group with the following rules:

Inbound rules

TCP Port 988

Outbound rules

Leave as default

## Deploy

1. Configure the "Defaults" section of the `1.fsx.yaml` file with the desired values.

2. Deploy the CloudFormation stack using the following command:
```bash
aws cloudformation create-stack --stack-name create-fsx-lustre-s3 \
--template-body file://1.fsx.yaml \
--parameters ParameterKey=SecurityGroupId,ParameterValue=<key_name>,ParameterKey=SubnetId,ParameterValue=<key_name>,ParameterKey=VpcId,ParameterValue=<key_name> \
--region ap-southeast-1
```

## Connect to the UI

1. Retrieve the `LdapUIUrl` to connect to the LDAP User Interface.
  ```bash
  aws cloudformation describe-stacks --stack-name ldap-server \
  --query 'Stacks[0].Outputs[?OutputKey==`LdapUIUrl`].OutputValue' \
  --output text
  ```
  Copy URL into a Web Browser.


## Get the LDAP Password
The password to access the LDAP was generated randomly and stored in AWS Secret Manager under `LdapPassword` output of the cloudformation stack.

1. Get the Secret ARN
	```bash
	 SECRET_ARN=$(aws cloudformation describe-stacks --stack-name ldap-server \
	 --query 'Stacks[0].Outputs[?OutputKey==`LdapPassword`].OutputValue' \
	 --output text)
	```

1. Get the password that you will use to login
	```bash
	aws secretsmanager get-secret-value --secret-id $SECRET_ARN\
	  --query SecretString \
	  --output text
	```
