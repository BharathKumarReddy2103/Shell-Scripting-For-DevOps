**Automating GitHub Repository Access Management Using Shell Scripting and GitHub API**

**Introduction**

In a DevOps role, managing multiple repositories and ensuring proper access control is a frequent task. While GitHub provides a **Graphical User Interface (GUI)** to view and manage repository access, doing this manually for **hundreds of repositories** can be inefficient.

To **automate and streamline** this process, we can leverage **GitHub API** and **Shell scripting**. In this guide, we will:

•	Understand how to use the **GitHub API** to retrieve repository access details

•	Write a **Shell script** to list users with access to a repository

•	Discuss **real-world use cases** and **best practices** for DevOps Engineers

By the end of this guide, you will have a **working Shell script** to automate **repository access management**, improving security and operational efficiency in your DevOps workflows.

---

**Why Automate GitHub Access Management?**

**Challenges of Manual Repository Access Management**

**•	Time-consuming**: Manually checking access for multiple repositories is tedious.

**•	Error-prone**: Human oversight can lead to **security risks.**

**•	Scalability issues**: As the number of repositories grows, tracking access manually becomes unmanageable.

**How Automation Helps**

**•	Efficiency:** Quickly fetch user access details using a simple script.

**•	Security:** Easily identify and revoke access for users who no longer need it.

**•	Scalability:** Automate access tracking across multiple repositories.

---

**Understanding GitHub API for Repository Access**

GitHub provides two primary ways to interact with repositories programmatically:

**1.	GitHub CLI (gh command)**

**2.	GitHub REST API (curl or HTTP requests)**

For this tutorial, we will use the **GitHub REST API**, as it allows **programmatic access** to repository details via simple **HTTP requests.**

**GitHub API Endpoint for Repository Collaborators**

To fetch the list of users with access to a repository, we use the following API endpoint:

```sh
GET https://api.github.com/repos/{owner}/{repo}/collaborators
```

•	{owner} → GitHub organization or user who owns the repository

•	{repo} → Name of the repository

The response returns a JSON list of users with their **roles** (admin, read, write).

---

**Writing the Shell Script to List Repository Collaborators**

**Prerequisites**

Before running the script, ensure you have:

•	A **GitHub Personal Access Token (PAT)** with repo access

•	jq installed on your system for parsing JSON responses

•	A **Linux/macOS terminal** or an **EC2 instance** with SSH access

**Creating the GitHub Personal Access Token**

**1.**	Go to **GitHub → Settings → Developer Settings → Personal Access Tokens**

**2.**	Click **Generate New Token**

**3.**	Select the necessary scopes (repo, read:org)

**4.**	Copy and store the token securely

---

**Shell Script**: list_github_users.sh

```sh
#!/bin/bash

# Script to list users with access to a GitHub repository using GitHub API
# Author: Bharath (Modified for GitHub Publication)
# Usage: ./list_github_users.sh <org_name> <repo_name>

# Exit script if any command fails
set -e

# Validate input parameters
if [[ $# -ne 2 ]]; then
  echo "Usage: $0 <org_name> <repo_name>"
  exit 1
fi

# GitHub API Base URL
API_URL="https://api.github.com"

# Read input parameters
ORG_NAME=$1
REPO_NAME=$2

# Ensure GitHub credentials are set
if [[ -z "$GITHUB_USERNAME" || -z "$GITHUB_TOKEN" ]]; then
  echo "Error: Please set GITHUB_USERNAME and GITHUB_TOKEN environment variables."
  exit 1
fi

# API Call to fetch collaborators
response=$(curl -s -u "$GITHUB_USERNAME:$GITHUB_TOKEN" "$API_URL/repos/$ORG_NAME/$REPO_NAME/collaborators")

# Parse response using jq to extract usernames and roles
users=$(echo "$response" | jq -r '.[] | select(.permissions.pull == true) | .login')

# Display results
if [[ -z "$users" ]]; then
  echo "No users found with read access to the repository."
else
  echo "Users with read access to $ORG_NAME/$REPO_NAME:"
  echo "$users"
fi
```

---

**Executing the Script**

**1. Set GitHub Credentials**

Before running the script, export your **GitHub Username** and **API Token**:

```sh
export GITHUB_USERNAME="your_github_username"
export GITHUB_TOKEN="your_github_token"
```

**2. Run the Script**

To list users for a specific GitHub repository:

```sh
./list_github_users.sh <org_name> <repo_name>
```

Example:

```sh
./list_github_users.sh devops-by-examples python
```

---

**Real-World Use Cases for DevOps Engineers**

**1. Offboarding Employees Securely**

When an employee **leaves an organization**, a DevOps Engineer can use this script to:

•	Identify if the employee has **access to critical repositories**

•	Revoke their access if necessary

**2. Periodic Security Audits**

•	Organizations can **schedule this script**to run periodically (e.g., using cron)

•	Audit and verify **who has access** to sensitive repositories

**3. Automating GitHub Access Reviews**

•	Use this script in a **CI/CD pipeline** to generate GitHub access reports

•	Automate security reviews and ensure only **authorized users** have access

---

**Enhancements & Best Practices**

**1. Handle Errors Gracefully**

•	Improve error handling using HTTP response codes (401 Unauthorized, 403 Forbidden, etc.)

•	Implement **logging** for better debugging

Example:

```sh
http_status=$(curl -o /dev/null -s -w "%{http_code}" -u "$GITHUB_USERNAME:$GITHUB_TOKEN" "$API_URL/repos/$ORG_NAME/$REPO_NAME/collaborators")

if [[ "$http_status" -ne 200 ]]; then
  echo "Error: API request failed with status code $http_status"
  exit 1
fi
```

**2. Extend to Support Multiple Repositories**

Modify the script to iterate over **multiple repositories** in an organization:

```sh
repos=("repo1" "repo2" "repo3")

for repo in "${repos[@]}"; do
  ./list_github_users.sh "$ORG_NAME" "$repo"
done
```

**3. Store API Tokens Securely**

•	Use a **secrets manager** (AWS Secrets Manager, HashiCorp Vault)

•	Avoid **hardcoding credentials** in scripts

---

**Conclusion**

By automating **GitHub repository access management** with Shell scripting and GitHub API, DevOps Engineers can:

✅ **Improve security** by regularly auditing repository access

✅ **Reduce manual work** by automating user access tracking

✅ **Enhance compliance** with security best practices

This script is a **practical solution** for managing repository permissions efficiently. Try implementing it in your DevOps workflows, and feel free to **extend it** for additional automation tasks
