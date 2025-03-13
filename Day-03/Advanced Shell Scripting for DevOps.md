**Advanced Shell Scripting for DevOps**

**Introduction**

Shell scripting is an essential skill for **DevOps Engineers, Cloud Engineers, and System Administrators. It enables automation of repetitive tasks, system monitoring, and configuration management.

In this guide, we will explore **advanced shell scripting concepts**, including:

•	Best practices for writing efficient scripts

•	Debugging and error handling techniques

•	Working with system resources and processes

•	Automating log file analysis

•	Advanced shell scripting commands like awk, grep, find, and trap

By the end of this article, you will be equipped with the knowledge to write **robust, production-ready shell scripts.**

---

**Prerequisites**

Before diving into advanced shell scripting, ensure you have:

•	Basic understanding of Linux commands

•	Familiarity with **Bash scripting**

•	Access to a **Linux machine (Ubuntu, CentOS, or Oracle Linux)**
(You can create an **AWS EC2 instance** for practice)

---

**Setting Up Your Environment**

To follow along, launch a **Linux-based EC2 instance** and install Bash (if not already installed).

```sh
sudo apt update && sudo apt install -y bash  # Ubuntu
sudo yum install -y bash                     # RHEL / CentOS
```

Once installed, verify the version:

```sh
bash --version
```

---

**Best Practices for Writing Shell Scripts**

**1. Always Start with a Shebang**

A **shebang** (#!) tells the system which interpreter to use for execution.

```sh
#!/bin/bash
```
**2. Add Metadata Information**

Providing author details, version, and purpose makes scripts easier to understand.

```sh
#!/bin/bash
# Author: Bharath
# Created: March 2025
# Purpose: Node Health Check Script
# Version: v1.0
```

**3. Use Error Handling and Debugging**

Enable debugging and exit on errors:

```sh
set -e  # Exit immediately if a command fails
set -o pipefail  # Exit if any command in a pipeline fails
set -x  # Print each command before execution (debug mode)
```

---

**Monitoring System Resources with Shell Scripting**

Let's start by creating a **node health check script** that collects system resource information.

**Script**: node_health.sh

```sh
#!/bin/bash

# Metadata
set -e
set -o pipefail
set -x

# Print disk space
echo "Disk Space:"
df -h

# Print free memory
echo "Memory Info:"
free -g

# Print number of CPUs
echo "CPU Info:"
nproc
```

Make the script executable:

```sh
chmod +x node_health.sh
```

Run the script:

```sh
./node_health.sh
```

---

**Process Monitoring and Filtering**

In **real-world DevOps scenarios**, identifying resource-intensive processes is crucial. We can achieve this using ps, grep, **and** awk.

**Find all running processes**

```sh
ps -ef
```

**Filter specific processes (e.g., Amazon services)**

```sh
ps -ef | grep amazon
```

**Extract only process IDs (using awk)**

```sh
ps -ef | grep amazon | awk '{print $2}'
```

---

**Automating Log File Analysis**

Log files contain valuable insights for **troubleshooting applications**. As a DevOps engineer, extracting errors from large log files is essential.

**Example: Extract Errors from Logs**

```sh
cat /var/log/syslog | grep "ERROR"
```

**Fetch Logs from a Remote Source (e.g., S3, GitHub)**

Using curl, we can retrieve logs from a URL.

```sh
curl -s https://raw.githubusercontent.com/user/logs/sample.log | grep "ERROR"
```

**Alternative: Using wget to Download the Log**

```sh
wget -q https://raw.githubusercontent.com/user/logs/sample.log -O logs.txt
cat logs.txt | grep "ERROR"
```

---

**Using find to Locate Files in Linux**

The find command helps locate specific files within a large directory structure.

**Find a File by Name**

```sh
find / -name "config.yaml"
```

**Find Files Modified in the Last 7 Days**

```sh
find /var/log -mtime -7
```

---

**Writing Conditional Statements in Shell Scripts**

if-else **Condition in Bash**

Example: Compare Two Numbers

```sh
#!/bin/bash

a=4
b=10

if [ $a -gt $b ]; then
  echo "A is greater than B"
else
  echo "B is greater than A"
fi
```

---

**Looping Constructs in Shell Scripting**

for **Loop: Print Numbers from 1 to 10**

```sh
for i in {1..10}; do
  echo "Number: $i"
done
```

while **Loop: Run Until a Condition is Met**

```sh
count=1
while [ $count -le 5 ]; do
  echo "Iteration: $count"
  ((count++))
done
```

---

**Handling User Interruptions with trap**

The trap command **captures system signals** and prevents users from terminating a script accidentally.

**Prevent Script Termination Using** Ctrl + C

```sh
#!/bin/bash

trap "echo 'CTRL+C is disabled'; exit" SIGINT

while true; do
  echo "Running..."
  sleep 5
done
```

**How it Works:**

•	SIGINT is triggered when a user presses Ctrl + C.

•	The script prints a message instead of stopping.

---

**Debugging and Logging in Shell Scripts**

**Enable Debugging Mode:**

```sh
set -x
```

**Redirect Output to a Log File:**

```sh
./script.sh > output.log 2>&1
```

**Monitor Logs in Real Time:**

```sh
tail -f output.log
```

---

**Summary**

In this guide, we covered **advanced shell scripting** concepts used in **real-world DevOps:**

**•	System Monitoring**: df, free, nproc

**•	Process Management**: ps -ef | grep | awk

**•	Log Analysis**: curl, wget, grep

**•	File Search**: find

**•	Conditionals & Loops**: if-else, for, while

**•	Signal Handling**: trap

**•	Debugging**: set -x, set -e, set -o pipefail
