[![Build Status](https://travis-ci.com/dentalwings/aws-cloudformation-sentry.svg?branch=master)](https://travis-ci.com/dentalwings/aws-cloudformation-sentry)

# AWS Cloudformation Template for [Sentry.io](https://sentry.io/)

This Cloudformation template will create an [Sentry.io](https://sentry.io/) instance.

## Requirements
* An existing VPC/Subnet setup created with https://github.com/dentalwings/aws-cloudformation-vpc-setup:<br/>[![Launch Stack](assets/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=infrastructure&templateURL=https%3A%2F%2Fs3.ca-central-1.amazonaws.com%2Fdentalwings-cloudformation-templates%2Faws-cloudformation-vpc-setup%2Fcloudformation_vpc_creation.yml)
* an Route53 managed DNS zone (top-level or subdomain) for setting up DNS Records and Amazon Certificate Manager Certificates<br/>See https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring.html how to configure Route53.

## How to launch this stack

[![Launch Stack](assets/launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=sentry&templateURL=https%3A%2F%2Fs3.ca-central-1.amazonaws.com%2Fdentalwings-cloudformation-templates%2Faws-cloudformation-sentry%2Fcloudformation_sentry.yml)

via CLI: *(replace at least the ParameterValue for the DNSZoneId)*

    aws cloudformation create-stack \
      --parameters ParameterKey=DNSZoneId,ParameterValue=XXXXXXXXXX ParameterKey=DNSZoneDomain,ParameterValue=aws.example.com
      --capabilities CAPABILITY_IAM \
      --stack-name sentry \
      --template-url https://s3.ca-central-1.amazonaws.com/dentalwings-cloudformation-templates/aws-cloudformation-sentry/cloudformation_sentry.yml

or

    aws cloudformation create-stack \
      --parameters ParameterKey=DNSZoneId,ParameterValue=XXXXXXXXXX
      --capabilities CAPABILITY_IAM \
      --stack-name sentry \
      --template-body file://cloudformation_sentry.yml

## Parameters

| Variable | Default | Description |
| --- |  --- | --- |
| `Tags` |  `''` | A comma seperated list of key=value pairs |
| `VpcCIDR` | `10.0.0.0/16` | IP range (CIDR notation) for this VPC |
| `CreateBastionHost` | `false` | Should we setup bastion hosts? |
| `BastionHostInstanceType` | `t2.micro` | InstanceType of Bastion Host |
| `BastionHostAllowedIPRange` | `t2.micro` | IP Range (CIDR notation) which is allowed connect from |
| `BastionHostKeyName` | `None` | Name of an existing EC2 KeyPair for SSH access |

## Cloudformation Stack Exports

| Variable | Description |
| --- | --- |
| `${AWS::StackName}-VPCID` | A reference to the created VPC |
| `AZ[1-6]PrivateSubnet` | AZ[1-6] Private Subnet |
| `AZ[1-6]PublicSubnet` | AZ[1-6] Public Subnet |
| `${AWS::StackName}-PrivateSubnets` | Comma seperated list of all created private subnets |
| `${AWS::StackName}-PublicSubnets` | Comma seperated list of all created public subnets |
