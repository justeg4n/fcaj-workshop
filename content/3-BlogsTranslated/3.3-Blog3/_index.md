---
title: "Blog 3"
date: 2026-04-04
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Automate AWS Systems Manager activation for hybrid-managed node registration
*by Justin Thomas on 09 MAR 2026 in Advanced (300), Amazon DynamoDB, AWS Lambda, AWS Systems Manager, Best Practices, Technical How-to*

AWS Systems Manager (formerly known as SSM) is an AWS service that you can use to view and control your servers on AWS cloud and on-premises infrastructure. Systems Manager makes it easy to manage a hybrid environment.

To set up servers and virtual machines (VMs) in your hybrid environment as Systems Manager managed instances, you create a managed-instance activation. Creating and managing Systems Manager Hybrid Activations credentials for your on-premises servers and VMs can be a manual and tedious task. The Hybrid Activations credentials can reach its activation expiration date or registration limit value after which the credentials can no longer be used to register the servers. The core ask is to automate the creation and management of the System Manager Hybrid Activations credentials, reducing the operational support needed in this task.

In this post, I will walk through the solution on automating the System Manager Hybrid Activations creation.

## Solution overview
The solution is enabled using **AWS CloudFormation** stack. The CloudFormation stack creates the AWS resources on your account needed for the solution:
* **Amazon API Gateway:** REST API of Private Type, Integrated with AWS Lambda function. When the web client from the on-premises server performs a `GET` request, it returns the Hybrid Actions Code/ID.
* **AWS Lambda:** Provides the Hybrid Activations Code/ID combination to the on-premises server. It creates a new activation code if the existing one is expired.
* **Amazon DynamoDB:** To store the state. The Lambda updates the table to 'Locked' if it’s serving a request, and 'Unlocked' after completion.
* **Amazon VPC Endpoint:** VPC Endpoint for privately accessing the API gateway URL from the on-premises network.
* **AWS Systems Manager Parameter Store:** To store the Hybrid Activations ID/Code.

### Execution Flow
1. The web client calls the private API Gateway endpoint.
2. The request is sent to the private IP address of the VPC Endpoint.
3. API Gateway resource policies are checked.
4. API Gateway passes the request to Lambda through an integration request.
5. Lambda updates the state key in DynamoDB to 'Locked'.
6. Lambda retrieves the credentials from the Parameter Store and sends them back to the client.

## Example scripts for automatic activation
You can use the API Gateway Invoke URL in your Shell/PowerShell script when installing SSM Agent.

**Linux (Shell Script):**
```bash
#!/bin/bash
sudo yum erase amazon-ssm-agent --assumeyes
credentials=$(curl -s [https://cla9phiczg.execute-api.us-east-1.amazonaws.com/lambdastage](https://cla9phiczg.execute-api.us-east-1.amazonaws.com/lambdastage))
activationcode=$(echo $credentials | jq -r '.ActivationCode')
activationid=$(echo $credentials| jq -r '.ActivationId')
mkdir /tmp/ssm
curl [https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm](https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm) -o /tmp/ssm/amazon-ssm-agent.rpm
sudo dnf install -y /tmp/ssm/amazon-ssm-agent.rpm
sudo systemctl stop amazon-ssm-agent
sudo -E amazon-ssm-agent -register -code $activationcode -id $activationid -region us-east-1
sudo systemctl start amazon-ssm-agent