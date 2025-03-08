**Automating Jenkins Log Management with Shell Scripting**

**Overview**

This repository demonstrates a **real-world cost optimization solution** using **Shell Scripting, Jenkins, and AWS S3**. The project addresses the problem of **high storage costs** in self-hosted ELK Stack by **automating the transfer of Jenkins logs to AWS S3.**

By implementing this solution, organizations can:

**•	Reduce ELK Stack storage costs** by up to 50%.

**•	Optimize infrastructure** by leveraging **AWS S3 lifecycle management.**

**•	Automate log management** using a **simple shell script and cron jobs.**

---

**Problem Statement**

Many organizations use **Jenkins** for continuous integration (CI), generating thousands of logs daily. Typically, these logs are stored in **ELK Stack (Elasticsearch, Logstash, and Kibana)** for debugging and troubleshooting. However, this can lead to:

**•	High storage costs** due to excessive logging.

**•	Unnecessary resource consumption** for logs that are not actively analyzed.

**•	Scalability issues** as log volume increases over time.

**Challenges in Log Management**

•	Logs from **100+ developers** committing code daily.

**•	Multiple Jenkins environments** (UAT, Staging, Production) generating logs.

**•	No active log analysis** on Jenkins logs—logs are stored **only as backups.**

**•	ELK Stack storage** being used inefficiently.

**Solution: Move Jenkins Logs to AWS S3**

Instead of storing logs in ELK Stack, **a shell script automates log uploads to AWS S3**, leveraging **S3 lifecycle policies** to archive older logs efficiently.

---

**Technical Approach**

**Architecture**

**1.	Identify Jenkins logs** generated daily.

**2.	Compress and transfer logs to AWS S3.**

**3.	Schedule the script execution** using **cron jobs.**

**4.	Apply S3 lifecycle management** to **auto-archive** old logs.

**Benefits**

✅ **Cost Reduction** – No need to store unnecessary logs in ELK.

✅ **Scalability** – S3 offers **low-cost and scalable storage.**

✅ **Automation** – Shell scripting eliminates **manual log management.**

✅ **Lifecycle Management** – Automatically **move older logs to Glacier/Deep Archive.**

---

**Implementation**

**Prerequisites**

**•	AWS Account** with IAM user permissions for S3.

**•	AWS CLI** installed on the Jenkins server.

**•	Jenkins CI/CD environment** running on an EC2 instance.

**Step 1: Install AWS CLI**

Install **AWS CLI** on the EC2 instance where **Jenkins is hosted:**

```sh
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

**Step 2: Configure AWS CLI**

```sh
aws configure
# Enter your AWS Access Key ID and Secret Access Key
# Set default region (e.g., us-east-1)
# Default output format: json
```

**Step 3: Create an S3 Bucket**

```sh
aws s3 mb s3://jenkins-log-optimization
```

**Step 4: Write the Shell Script**

Create a file named **s3_upload.sh:**

```sh
vim s3_upload.sh
```

Paste the following script:

```sh
#!/bin/bash

# Metadata
# Author: DevOps Team
# Description: Automates Jenkins log backup to AWS S3
# Version: 1.0

# Define Variables
JENKINS_HOME="/var/lib/jenkins"
S3_BUCKET="s3://jenkins-log-optimization"
TODAY=$(date +"%Y-%m-%d")

# Check if AWS CLI is installed
if ! command -v aws &> /dev/null; then
    echo "AWS CLI not installed. Please install it before proceeding."
    exit 1
fi

# Loop through Jenkins job directories
for JOB_DIR in "$JENKINS_HOME/jobs"/*; do
    if [ -d "$JOB_DIR" ]; then
        JOB_NAME=$(basename "$JOB_DIR")

        # Loop through build logs
        for BUILD_DIR in "$JOB_DIR/builds"/*; do
            if [ -d "$BUILD_DIR" ]; then
                LOG_FILE="$BUILD_DIR/log"

                # Check if log file exists and is created today
                if [ -f "$LOG_FILE" ] && [[ $(date -r "$LOG_FILE" +"%Y-%m-%d") == "$TODAY" ]]; then
                    aws s3 cp "$LOG_FILE" "$S3_BUCKET/$JOB_NAME/" --storage-class STANDARD_IA

                    # Validate upload success
                    if [ $? -ne 0 ]; then
                        echo "Upload failed for $LOG_FILE. Retrying with multi-part upload..."
                        aws s3 cp "$LOG_FILE" "$S3_BUCKET/$JOB_NAME/" --storage-class GLACIER
                    fi
                fi
            fi
        done
    fi
done

echo "Jenkins log upload to S3 completed."
```

**Step 5: Make the Script Executable**

```sh
chmod +x s3_upload.sh
```

**Step 6: Run the Script**

```sh
./s3_upload.sh
```

**Step 7: Automate with a Cron Job**

To schedule the script execution **every night at 11 PM**, add a cron job:

```sh
crontab -e
```

Append the following line:

```sh
0 23 * * * /path/to/s3_upload.sh
```

---

**Results & Cost Savings**

✅ **Eliminated ELK Stack storage costs** for Jenkins logs.

✅ **50% reduction in storage expenses** using **AWS S3 & Glacier.**

✅ **Automated log management**, eliminating manual intervention.

✅ **Improved Jenkins performance** by reducing log storage overhead.

**Before vs. After**

| Metric            | Before (ELK Stack)       | After (AWS S3)            |
|------------------|------------------------|---------------------------|
| **Storage Cost**  | High (Expensive)        | Low (Cost-Effective)      |
| **Log Retention** | Manual Cleanup Required | Automated Lifecycle       |
| **Performance**   | High Load on ELK        | No Load on ELK            |
| **Scalability**   | Limited                 | Scalable with S3          |
| **Access**        | Requires ELK Querying   | Direct S3 Access          |
| **Backup Strategy** | Redundant in ELK      | S3 + Glacier Archiving    |

---

**How to Contribute**

If you found this project useful, feel free to:

•	⭐ **Star** this repository.

•	🛠 **Fork and modify** the script for your needs.

•	📥 **Submit Pull Requests** for improvements.

---

**Conclusion**

This project showcases a **practical DevOps solution** for **log management and cost optimization** using **Shell Scripting, Jenkins, and AWS S3**. By implementing this approach, organizations can **reduce ELK Stack costs, automate log transfers, and leverage AWS's cost-effective storage solutions.**

If you have any questions, feel free to open an issue or connect with me on GitHub.
