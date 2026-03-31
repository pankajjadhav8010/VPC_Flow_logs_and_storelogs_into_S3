# AWS VPC Flow Logs to S3 using IAM Role

## Project Overview

This project demonstrates how to enable VPC Flow Logs in AWS and store them securely in an Amazon S3 bucket using an IAM role. The solution provides visibility into network traffic, which is useful for monitoring, troubleshooting, and security analysis.

It is designed to reflect a real-world setup where organizations need to track and audit network-level activity across their infrastructure.

## Objective

* Enable VPC Flow Logs for a VPC
* Store logs securely in Amazon S3
* Use an IAM role for controlled access
* Verify and analyze captured traffic logs

## Architecture Overview

### Components Used

* Amazon VPC (Flow Logs)
* Amazon S3 (Log Storage)
* AWS Identity and Access Management (IAM Role)
* Amazon EC2 (Traffic generation)

## Implementation Steps

### 1. Create or Select VPC

* Navigate to the VPC Dashboard
* Select an existing VPC or create a new one

### 2. Create S3 Bucket for Logs

#### Bucket Details

* Name: vpc-flow-logs-bucket
* Region: Same as VPC

#### Expected Folder Structure

```id="t0a8ws"
s3://vpc-flow-logs-bucket/<account-id>/<region>/<vpc-id>/
```

### 3. Configure S3 Bucket Policy

Attach the following policy to allow delivery of flow logs:

```json id="4grz5u"
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VPCFlowLogsDelivery",
      "Effect": "Allow",
      "Principal": {
        "Service": "vpc-flow-logs.amazonaws.com"
      },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::vpc-flow-logs-bucket/*"
    }
  ]
}
```

### 4. Create IAM Role

#### Trust Relationship

```json id="3h6xw9"
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "vpc-flow-logs.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

#### Permissions Policy

```json id="r1wxqt"
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::vpc-flow-logs-bucket/*"
    }
  ]
}
```

### 5. Enable VPC Flow Logs

Configure the following:

* Resource Type: VPC
* Traffic Type: ALL
* Destination: S3
* Select the created S3 bucket
* Attach the IAM role

### 6. Generate Traffic for Testing

Use an EC2 instance to generate network activity:

* Ping external IP addresses
* Access web applications
* Perform SSH connections

### 7. Verify Logs in S3

* Navigate to the S3 bucket
* Check the generated folder structure:

```id="ozr9oq"
AWSLogs/<account-id>/vpcflowlogs/<region>/<year>/<month>/<day>/
```

* Confirm that log files are being created

## Sample VPC Flow Log Record

```id="vkn3f7"
2 123456789 eni-abc123 10.0.1.10 172.217.160.142 443 51532 6 10 840 1678901234 1678901294 ACCEPT OK
```

## Log Fields Explanation

| Field            | Description                |
| ---------------- | -------------------------- |
| Version          | Flow log version           |
| Account ID       | AWS account ID             |
| Interface ID     | Network interface ID       |
| Source IP        | Source address             |
| Destination IP   | Destination address        |
| Source Port      | Source port                |
| Destination Port | Destination port           |
| Protocol         | Protocol (6 indicates TCP) |
| Packets          | Number of packets          |
| Bytes            | Data transferred           |
| Start            | Start time                 |
| End              | End time                   |
| Action           | ACCEPT or REJECT           |
| Status           | OK or NODATA               |

## Verification

* Confirm that flow logs are delivered to S3
* Validate log format and content
* Ensure traffic entries reflect generated activity

## Importance of This Setup

* Provides network-level visibility
* Supports security investigations
* Helps meet compliance requirements
* Enables detection of unusual traffic patterns

## Traffic Type Selection

Traffic type used: ALL

This ensures that both accepted and rejected traffic is captured, providing complete visibility for analysis and auditing.

## Security Considerations

* Use least privilege IAM roles
* Restrict S3 bucket access
* Enable encryption for S3 objects
* Monitor access to logs

## Optional Enhancements

* Query logs using Amazon Athena
* Stream logs to CloudWatch for real-time monitoring
* Configure alerts using SNS
* Visualize traffic patterns using dashboards

## Key Learnings

* Understanding VPC Flow Logs and network monitoring
* Secure log delivery using IAM roles
* Organizing and storing logs in S3
* Implementing real-world security monitoring practices

## Conclusion

This project demonstrates a scalable and secure approach to capturing and storing network traffic logs using AWS. It provides valuable insights into infrastructure activity and strengthens overall security and monitoring capabilities.
