[![Build Status](https://travis-ci.com/dentalwings/aws-cloudformation-sentry.svg?branch=master)](https://travis-ci.com/dentalwings/aws-cloudformation-sentry)

# AWS Cloudformation Template for [Sentry.io](https://sentry.io/)

This Cloudformation template will create an [Sentry.io](https://sentry.io/) instance.

## Requirements
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
      --parameters ParameterKey=DNSZoneId,ParameterValue=XXXXXXXXXX ParameterKey=DNSZoneDomain,ParameterValue=aws.example.com
      --capabilities CAPABILITY_IAM \
      --stack-name sentry \
      --template-body file://cloudformation_sentry.yml

## Parameters

| Variable | Default | Description |
| --- |  --- | --- |
| `DNSZoneId` | | HostZoneID from DNS |
| `DNSZoneDomain` | | HostZoneDomain from DNS |
| `CustomDNSHostname` | `${AWS::Region}-${AWS::StackName}-sentry` | Custom DNS Hostname, which overrides the dynamic generated one |
| `Tags` |  `''` | A comma seperated list of key=value pairs |
| `RedisClusterNodes` | `1` | Number of Redis Cluster Nodes |
| `RedisInstanceType` | `cache.t2.micro` | InstanceClass for each Redis node |
| `SSHKeyPairName` | `None` | EC2 SSH KeyPair Name (`None` for disable key deployment) |
| `DBUsername` | `sentryuser` | Default database user |
| `DBPassword` | `SentryPassword` | Password for default database user |
| `SentryAdminUser` | `admin` | Username for root sentry access |
| `SentryAdminPassword` | `supersectretadminpassword` | Password for root sentry access - minimum 20 characters |
| `SentrySecretKey` | `#this-secret-key-must-be-at-least-fifty-characters!` | Private key for encrypting user sessions |
| `SentryGithubAppId` | | Description: GitHub API App ID for SSO |
| `SentryGithubApiSecret` | | GitHub API secret key for SSO |
| `SentryMailHost` | `email-smtp.eu-west-1.amazonaws.com` | SMTP host for sentry to use to send email |
| `SentryMailPort` | `25` | SMTP port for sentry to use to send email |
| `SentryMailUsername` | | SMTP username for sentry to use to send email |
| `SentryMailPassword` | | SMTP password for sentry to use to send email |
| `SentryMailFrom` | `noreply@example.com` | Sending email address for sentry to use to send email
| `SentryInstanceType` | `t2.medium` | The size of the EC2 instances |
| `SentryScalingMinNodes` | `1` | Minium size of the auto scaling group |
| `SentryScalingMaxNodes` | `2` | Maximum size of the auto scaling group |
