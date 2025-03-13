**Shell Scripting for DevOps Engineers – A Practical Guide**

**Introduction**

Shell scripting is an essential skill for DevOps engineers, enabling them to automate repetitive tasks, manage infrastructure, and streamline workflows. Whether it's configuring servers, managing deployments, or monitoring system health, shell scripts play a critical role in day-to-day DevOps operations.

**In this guide, we’ll cover:**

**•	What is shell scripting?**

**•	Why DevOps engineers use shell scripting.**

**•	Essential Linux commands.**

**•	Writing your first shell script.**

**•	Practical use cases in DevOps.**

**•	Best practices for writing shell scripts.**

By the end, you'll have a solid understanding of **how to use shell scripting for automation in DevOps.**

---

**What is Shell Scripting?**

A **shell script** is a file containing a series of **Linux commands** that are executed sequentially. These scripts help in automating manual tasks, reducing human errors, and improving efficiency.

**Why Do DevOps Engineers Use Shell Scripting?**

DevOps engineers use shell scripting to:

•	Automate infrastructure provisioning and deployments.

•	Manage configurations and scheduled jobs.

•	Monitor system health (CPU, memory, and disk usage).

•	Automate backups and log rotations.

•	Perform CI/CD tasks using tools like GitLab CI/CD, Jenkins, and GitHub Actions.

---

**Essential Linux Commands for Shell Scripting**

Before diving into scripting, let’s cover some fundamental Linux commands.

**1. File Management Commands**

| Command | Description |
|---------|------------|
| `touch filename` | Create a new file |
| `ls -ltr` | List files with details |
| `cat filename` | View file contents |
| `rm filename` | Delete a file |
| `mkdir foldername` | Create a directory |
| `rmdir foldername` | Delete a directory |

**2. File Permissions**

In Linux, file permissions are managed using the chmod command.

| Command | Description |
|---------|------------|
| `chmod 777 filename` | Full permissions (read, write, execute) |
| `chmod 755 filename` | Read & execute for all, write for the owner |
| `chmod 444 filename` | Read-only access for all |

**3. Process and System Monitoring**

| Command | Description |
|---------|------------|
| `ps aux` | List running processes |
| `top` | View real-time system performance |
| `free -m` | Check memory usage |
| `df -h` | Display disk space usage |

**4. User and Group Management**

| Command | Description |
|---------|------------|
| `whoami` | Display the current user |
| `groups` | Show groups the user belongs to |
| `id username` | Get user details |

---

**Writing Your First Shell Script**

Let’s create a simple shell script that **creates a directory, adds files, and sets permissions.**

**Step 1: Create a Shell Script File**

Use the touch command to create a file:

```sh
touch script.sh
```

Open it with a text editor:

```sh
vim script.sh
```

**Step 2: Add the Shebang Line**

The first line of every shell script should specify which shell to use:

```sh
#!/bin/bash
```

The **shebang (#!)** tells the system to use **Bash** for execution.

**Step 3: Add Script Logic**

```sh
#!/bin/bash

# Create a directory
mkdir -p my_directory

# Navigate into the directory
cd my_directory

# Create two files
touch file1.txt file2.txt

# Set permissions (read, write, execute for all)
chmod 777 file1.txt file2.txt

# Print success message
echo "Directory and files created successfully!"
```

**Step 4: Save and Execute the Script**

Save the script in **Vim**:

•	Press **Esc**

•	Type :wq (write & quit)

•	Press **Enter**

Give execute permission:

```sh
chmod +x script.sh
```

Run the script:

```sh
./script.sh
```

Output:

```sh
Directory and files created successfully!
```

---

**Real-World Use Cases in DevOps**

**1. Monitoring System Health**

Automate checking **CPU & memory usage** and send alerts if thresholds exceed.

```sh
#!/bin/bash

# Check CPU & Memory Usage
echo "CPU Usage:"
top -b -n1 | grep "Cpu(s)"

echo "Memory Usage:"
free -m
```

**2. Automating Backups**

Create a script to back up files to a remote server.

```sh
#!/bin/bash

tar -czvf backup.tar.gz /home/user/data
scp backup.tar.gz user@backup-server:/backups/
```

**3. Automating Deployment**

Deploy an application using **Git, Docker, and Kubernetes.**

```sh
#!/bin/bash

git pull origin main
docker build -t myapp .
kubectl apply -f deployment.yaml
```

---

**Best Practices for Writing Shell Scripts**

**•	Use Shebang** (#!/bin/bash) to specify the shell.

**•	Comment your code** to explain complex sections.

**•	Use meaningful variable names** for better readability.

**•	Handle errors gracefully** using set -e or if-else conditions.

**•	Test scripts in a non-production environment** before deploying.

Example:

```sh
#!/bin/bash

# Set error handling
set -e

if [ -d "/data" ]; then
    echo "Directory exists"
else
    echo "Directory not found!"
fi
```

---

**Conclusion**

Shell scripting is an **indispensable skill for DevOps engineers**. It helps automate repetitive tasks, monitor infrastructure, and streamline CI/CD workflows.

By mastering shell scripting, **you can improve efficiency, reduce manual errors, and manage cloud environments effectively.**

💡 **Want to contribute?**

If you have improvements, **submit a PR** on this GitHub repository.

**Happy scripting** 🚀
