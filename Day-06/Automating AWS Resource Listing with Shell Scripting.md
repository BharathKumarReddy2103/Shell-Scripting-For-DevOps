**Automating AWS Resource Listing with Shell Scripting**

**Introduction**

In the DevOps world, automation is crucial for efficiency, cost optimization, and operational visibility. One of the common automation tasks performed by DevOps engineers is writing shell scripts to interact with cloud platforms like AWS, Azure, and GCP.

In this guide, we will build a **Shell Script to list active AWS resources** dynamically based on user input. This script can be used by **managers, developers, or anyone interested in understanding active resources** in an AWS account.

We will cover:

•	Real-world **use cases** of Shell Scripting in DevOps.

•	**Best practices** for writing production-grade shell scripts.

**•	Security considerations** while handling AWS CLI commands.

•	How to **validate inputs and restrict services** to only those used in your organization.

**•	Execution and permission settings** to ensure secure script usage.

---

**Why Automate AWS Resource Listing?**

Companies spend significant amounts on cloud services, and **cost optimization** is a key responsibility of DevOps Engineers. By automating resource listing:

**•	Cost monitoring** becomes easier.

**•	Unused resources** can be identified and cleaned up.

**•	Daily reports** can be generated via Cron Jobs and emailed to management.

**•	Security compliance** is maintained by ensuring only required resources are active.

This script can be extended to **Azure or GCP** with minor modifications.

---

**Script Functionality**

The script performs the following actions:

**1.	Takes two inputs:** AWS Region & Service Name.

**2.	Validates inputs** to ensure proper usage.

**3.	Checks if AWS CLI is installed and configured.**

**4.	Uses AWS CLI** to list resources dynamically.

**5.	Restricts services** to those used by the organization.

**6.	Enhances security** by implementing proper file permissions.

---

**Prerequisites**

Before running the script, ensure:

•	You have an **AWS Account** with appropriate IAM permissions.

•	You have **AWS CLI installed and configured.**

•	Your machine has **Bash Shell** (Linux/MacOS or Windows with WSL/Git Bash).

---

**Shell Script: AWS Resource Listing**

Below is the well-structured shell script:

```sh
#!/bin/bash

# ----------------------------------------
# AWS Resource Listing Shell Script
# Author: Bharath
# Version: 1.0.0
# ----------------------------------------

# Description:
# This script lists active AWS resources based on user input.
# Usage: ./aws_resource_list.sh <AWS_REGION> <SERVICE_NAME>
# Example: ./aws_resource_list.sh us-east-1 ec2

# Supported AWS Services:
# - EC2
# - S3
# - EBS
# - RDS
# - DynamoDB
# - VPC
# - Lambda
# - EKS
# - CloudFormation
# - IAM

# ----------------------------------------

# Validate Inputs
if [ $# -ne 2 ]; then
    echo "Usage: $0 <AWS_REGION> <SERVICE_NAME>"
    echo "Example: $0 us-east-1 ec2"
    exit 1
fi

AWS_REGION=$1
SERVICE_NAME=$2

# Validate AWS CLI Installation
if ! command -v aws &> /dev/null; then
    echo "Error: AWS CLI is not installed. Please install AWS CLI first."
    exit 1
fi

# Validate AWS CLI Configuration
if [ ! -d "$HOME/.aws" ]; then
    echo "Error: AWS CLI is not configured. Run 'aws configure' to set it up."
    exit 1
fi

# Define Allowed Services
ALLOWED_SERVICES=("ec2" "s3" "ebs" "rds" "dynamodb" "vpc" "lambda" "eks" "cloudformation" "iam")

# Check if provided service is allowed
if [[ ! " ${ALLOWED_SERVICES[@]} " =~ " ${SERVICE_NAME} " ]]; then
    echo "Error: Invalid service name. Allowed services: ${ALLOWED_SERVICES[*]}"
    exit 1
fi

# Execute AWS CLI Commands Based on Service
case "$SERVICE_NAME" in
    ec2)
        aws ec2 describe-instances --region "$AWS_REGION" --query 'Reservations[*].Instances[*].[InstanceId,State.Name,InstanceType,PublicIpAddress]' --output table
        ;;
    s3)
        aws s3 ls --region "$AWS_REGION"
        ;;
    ebs)
        aws ec2 describe-volumes --region "$AWS_REGION" --query 'Volumes[*].[VolumeId,State,Size,VolumeType]' --output table
        ;;
    rds)
        aws rds describe-db-instances --region "$AWS_REGION" --query 'DBInstances[*].[DBInstanceIdentifier,DBInstanceClass,Engine,DBInstanceStatus]' --output table
        ;;
    dynamodb)
        aws dynamodb list-tables --region "$AWS_REGION"
        ;;
    vpc)
        aws ec2 describe-vpcs --region "$AWS_REGION" --query 'Vpcs[*].[VpcId,State,CidrBlock]' --output table
        ;;
    lambda)
        aws lambda list-functions --region "$AWS_REGION"
        ;;
    eks)
        aws eks list-clusters --region "$AWS_REGION"
        ;;
    cloudformation)
        aws cloudformation list-stacks --region "$AWS_REGION" --query 'Stacks[*].[StackName,StackStatus]' --output table
        ;;
    iam)
        aws iam list-users
        ;;
    *)
        echo "Invalid service name."
        exit 1
        ;;
esac
```
---

**How to Use the Script**

**Step 1: Save the Script**

Save the script as aws_resource_list.sh in your working directory.

**Step 2: Grant Execution Permission**

```sh
chmod +x aws_resource_list.sh
```

**Step 3: Install AWS CLI (If Not Installed)**

```sh
sudo apt update && sudo apt install awscli -y
```

For **MacOS**, use:

```sh
brew install awscli
```

For **Windows**, use the AWS CLI Installer.

**Step 4: Configure AWS CLI**

Run the following command to set up your credentials:

```sh
aws configure
```

Provide:

**•	AWS Access Key**

**•	AWS Secret Key**

**•	Default Region** (e.g., us-east-1)

**•	Default Output Format** (json or table)

**Step 5: Run the Script**

Execute the script with a region and service name:

```sh
./aws_resource_list.sh us-east-1 ec2
```

**Expected output:**

```sh
---------------------------------------------------
| InstanceId    | State  | Type   | Public IP     |
---------------------------------------------------
| i-abcdef12345 | running | t3.micro | 52.12.34.56 |
---------------------------------------------------
```

---

**Additional Enhancements**

**1.	Automate with Cron Jobs**

o	To schedule a **daily report**, add this line to crontab -e:

```sh
0 21 * * * /path/to/aws_resource_list.sh us-east-1 ec2 >> /var/log/aws_resources.log
```

o	This runs the script daily at **9:00 PM UTC** and logs the output.

**2.	Email Reports**

o	Use mailx or sendmail to send daily reports to management.

**3.	Secure Script Execution**

o	Restrict file permissions:

```sh
chmod 750 aws_resource_list.sh
```

o	Use AWS IAM roles instead of storing credentials locally.

---

**Conclusion**

This shell script **simplifies AWS resource monitoring** by providing an **efficient and automated way** to list cloud resources. It adheres to **best practices**, ensuring security and maintainability.

🔹 **Next Steps:**

•	Extend this script for **Azure CLI or GCP CLI.**

•	Integrate with **Slack or email alerts** for proactive monitoring.

•	Deploy as a **Lambda function** to trigger dynamically.
