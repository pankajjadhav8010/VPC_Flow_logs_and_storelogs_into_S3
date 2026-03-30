🌐 AWS VPC Flow Logs to S3 using IAM Role
📌 Project Overview

This project demonstrates how to enable VPC Flow Logs in Amazon Web Services and securely store them in an Amazon S3 bucket using an IAM Role.

The solution provides deep visibility into network traffic for security auditing, troubleshooting, and compliance purposes.

🎯 Objective

To:

Enable VPC Flow Logs for a VPC
Store logs securely in Amazon S3
Use an IAM Role for controlled access
Verify and analyze captured traffic logs
🏗️ Architecture Overview

Components Used:

Amazon VPC (Flow Logs)
Amazon S3 (Log Storage)
AWS Identity and Access Management (IAM Role)
EC2 Instance (for traffic generation)
⚙️ Step-by-Step Implementation
1️⃣ Create or Select VPC
Navigate to VPC Dashboard
Select an existing VPC or create a new one
2️⃣ Create S3 Bucket for Logs
📌 Bucket Details:
Name: vpc-flow-logs-bucket
Region: Same as VPC
📂 Folder Structure:
s3://vpc-flow-logs-bucket/<account-id>/<region>/<vpc-id>/
3️⃣ S3 Bucket Policy

Attach the following policy to allow VPC Flow Logs delivery:

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
4️⃣ Create IAM Role
🔐 Trust Relationship
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
📜 Permissions Policy
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
5️⃣ Enable VPC Flow Logs
📌 Configuration:
Resource Type: VPC
Traffic Type: ALL (to capture full visibility)
Destination: S3
S3 Bucket: vpc-flow-logs-bucket
IAM Role: Select created role
6️⃣ Generate Traffic for Testing
Launch or use an existing EC2 instance
Perform:
Ping external IP
Access websites
SSH connections
7️⃣ Verify Logs in S3
Navigate to S3 bucket
Check folder structure:
AWSLogs/<account-id>/vpcflowlogs/<region>/<year>/<month>/<day>/
Confirm log files are being generated
📊 Sample VPC Flow Log Record
2 123456789 eni-abc123 10.0.1.10 172.217.160.142 443 51532 6 10 840 1678901234 1678901294 ACCEPT OK
🔍 Log Fields Explanation
Field	Description
Version	Flow log version
Account ID	AWS account ID
Interface ID	ENI ID
Source IP	Source address
Destination IP	Destination address
Source Port	Source port
Destination Port	Destination port
Protocol	Protocol (6 = TCP)
Packets	Number of packets
Bytes	Data transferred
Start	Start time
End	End time
Action	ACCEPT / REJECT
Status	OK / NODATA
📸 Deliverables (Included in Repo)
✅ IAM Role JSON (Policy & Trust)
✅ S3 Bucket Policy JSON
✅ Flow Logs Configuration Screenshot
✅ Logs in S3 Screenshot
✅ Sample Log Explanation
🔐 Importance of This Setup
Enables network-level visibility
Helps in security investigations
Supports compliance requirements
Detects suspicious traffic patterns
📌 Traffic Type Used

ALL (Accepted + Rejected Traffic)

💡 Why?
Provides complete visibility
Helps detect unauthorized attempts
Useful for auditing and troubleshooting
🚀 Key Learnings
How VPC Flow Logs capture network metadata
Secure log delivery using IAM roles
Structured log storage in S3
Real-world security monitoring setup
🔮 Future Enhancements
Integrate logs with Amazon Athena for querying
Use Amazon CloudWatch for real-time monitoring
Enable alerts using SNS
Visualize logs using dashboards
🤝 Conclusion

This project showcases how to implement a secure and scalable logging solution using AWS services. It provides critical insights into network traffic, helping organizations improve security posture and operational visibility.
