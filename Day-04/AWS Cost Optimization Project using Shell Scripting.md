**Automating AWS Cost Optimization: A Real-World Shell Scripting Project for DevOps Engineers**

**Introduction**

Cost optimization is one of the primary reasons organizations move to the cloud. Cloud providers like **AWS, Azure, and GCP** offer a **pay-as-you-go model**, ensuring businesses only pay for the resources they use. However, inefficient resource management can lead to unnecessary costs, making **cost tracking and resource monitoring** an essential task for **DevOps Engineers**.

In this article, we will implement an **AWS resource tracking solution** using **Shell Scripting** and **AWS CLI**, ensuring that organizations can monitor and optimize cloud resource usage. This is a common real-world **DevOps project** that can be added to your resume or GitHub portfolio.

---

**Why Cloud Cost Optimization Matters**

**1. Reducing Maintenance Overhead**

•	Traditional on-premise servers require constant **patching, monitoring, and upgrades.**

•	A dedicated **systems engineering team** is required to manage hardware, software, and networking.

**2. Cost Savings with Pay-As-You-Go**

•	Cloud providers charge based on resource consumption.

•	Unused resources, such as **idle EC2 instances, unattached EBS volumes, or inactive S3 buckets**, can lead to **unnecessary charges.**

**3. Challenges in Cloud Cost Management**

•	Developers often create resources but **forget to delete them.**

•	Large enterprises with **multiple teams and accounts** require **automated cost-tracking.**

---

**Project Overview: AWS Resource Tracking with Shell Scripting**

In this project, we will create a **Shell Script** to track the following AWS resources:

**•	EC2 Instances** (running instances)

**•	S3 Buckets** (existing storage buckets)

**•	Lambda Functions** (deployed serverless functions)

**•	IAM Users** (active user accounts)

The script will:
**1.	Retrieve resource details** using AWS CLI.

**2.	Filter out unnecessary details** using jq.

**3.	Store the output in a structured report.**

**4.	Schedule the script using a Cron Job** to generate daily reports.

---

**Prerequisites**

Before proceeding, ensure you have:
•	An **AWS account** with IAM permissions to list resources.

**•	AWS CLI installed** and configured.

**•	Bash Shell** (bash should be available on your system).

**•	jq installed** (for parsing JSON output).

**Verify AWS CLI Installation:**

```sh
aws --version
```

**Configure AWS CLI:**

```sh
aws configure
```

You will need to enter:

**•	AWS Access Key**

**•	AWS Secret Key**

**•	Default AWS Region**

**•	Output format (JSON)**

---

**Shell Script: AWS Resource Tracker**

Below is the shell script to track AWS resources.

```sh
#!/bin/bash

# Author: Bharath
# Date: 11th January 2025
# Version: v1.0
# Description: This script tracks AWS resources and generates a daily report.

# Output file
REPORT_FILE="aws_resource_report.txt"

# Start Report
echo "AWS Resource Usage Report - $(date)" > $REPORT_FILE
echo "-------------------------------------" >> $REPORT_FILE

# Function to log output to both console and file
log() {
    echo "$1"
    echo "$1" >> $REPORT_FILE
}

# List all S3 Buckets
log "Listing S3 Buckets:"
aws s3 ls >> $REPORT_FILE 2>&1
log "-------------------------------------"

# List EC2 Instances (only Instance IDs)
log "Listing EC2 Instances:"
aws ec2 describe-instances --query 'Reservations[].Instances[].InstanceId' --output text >> $REPORT_FILE 2>&1
log "-------------------------------------"

# List Lambda Functions
log "Listing AWS Lambda Functions:"
aws lambda list-functions --query 'Functions[].FunctionName' --output text >> $REPORT_FILE 2>&1
log "-------------------------------------"

# List IAM Users
log "Listing IAM Users:"
aws iam list-users --query 'Users[].UserName' --output text >> $REPORT_FILE 2>&1
log "-------------------------------------"

# Completion message
log "AWS Resource Report generated: $REPORT_FILE"
```

**Explanation:**

**•	Functions for Logging**: The log function ensures messages are printed both on the console and stored in a report file.

**•	AWS CLI Commands**: Each resource is listed using AWS CLI, filtering unnecessary data using --query for better readability.

**•	Redirection (>> and 2>&1)** ensures errors are logged in the report.

---

**Scheduling the Script with Cron Job**

To ensure the script runs **daily at 6 PM**, we can schedule it using **Cron Jobs**.

**Step 1: Open Crontab**

```sh
crontab -e
```

**Step 2: Add the following Cron Job**

```sh
0 18 * * * /path/to/aws_resource_tracker.sh
```

•	0 18 * * * → Runs **every day at 18:00 (6 PM)**

•	Replace /path/to/aws_resource_tracker.sh with the actual script location.

**Verify Cron Job**

```sh
crontab -l
```

This will list all scheduled cron jobs.

---

**Enhancements & Best Practices**

**•	Use AWS Cost Explorer API**: Instead of just listing resources, integrate AWS Cost Explorer API to fetch detailed cost insights.

**•	Slack / Email Alerts**: Send resource usage reports to Slack channels or emails for better visibility.

**•	AWS Lambda Automation**: Convert the script into a Lambda function triggered by EventBridge Scheduler.

**•	Tagging and Cleanup Automation**: Extend the script to delete unused EBS volumes, orphaned snapshots, and idle EC2 instances.

---

**Real-World Use Cases**

**1.	Enterprise Cost Management**

o	Large enterprises use similar scripts or Lambda functions to monitor AWS costs.

**2.	Automated Governance & Compliance**

o	Ensuring IAM users and EC2 instances follow security policies.

**3.	Startups & SMBs**

o	Small teams benefit by tracking unused resources to optimize cloud expenses.

---

**Conclusion**

Cloud cost optimization is **critical** for organizations of all sizes. By implementing an **automated AWS resource tracking system**, DevOps Engineers can **reduce costs, improve efficiency, and prevent resource sprawl.**

This **Shell Scripting project** is a **great addition to your GitHub portfolio** and can serve as an entry point for mastering cloud automation.

**Next Steps:**

**•	Extend the script** to track additional resources like **EBS volumes, RDS, and Kubernetes clusters.**

**•	Deploy the script in production** using Lambda and EventBridge.

**•	Contribute to Open Source** by enhancing this script and sharing improvements.

If you found this useful, **star the GitHub repo** and share your feedback.
