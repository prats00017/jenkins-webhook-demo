

# Jenkins Installation

Update packages:

```bash
sudo apt update
```

Install Java:

```bash
sudo apt install openjdk-17-jdk -y
```

Add Jenkins repository:

```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

```bash
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
```

Install Jenkins:

```bash
sudo apt update
sudo apt install jenkins -y
```

Start Jenkins:

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

Verify:

```bash
sudo systemctl status jenkins
```

---

# Create GitHub Repository

Repository:

```
jenkins-webhook-demo
```

---

# Source Files

## script.sh

```bash
#!/bin/bash

date
hostname

cat file.txt
```

Make executable:

```bash
chmod +x script.sh
```

---

## file.txt

```text
First Commit
```

---

# Jenkins Freestyle Project Configuration

## Source Code Management

Repository URL:

```text
https://github.com/<USERNAME>/jenkins-webhook-demo.git
```

---

## Build Trigger

Enable:

```
GitHub hook trigger for GITScm polling
```

---

## Build Step

Execute Shell:

```bash
chmod +x script.sh
./script.sh
```

---

# GitHub Webhook Configuration

Navigate to:

```
GitHub Repository
→ Settings
→ Webhooks
→ Add webhook
```

Payload URL:

```text
http://<PUBLIC-IP>:8080/github-webhook/
```

Content Type:

```text
application/json
```

Events:

```text
Just the push event
```

---

# Email Notification Configuration

## Manage Jenkins → System

SMTP Server:

```text
smtp.gmail.com
```

Port:

```text
465
```

Security:

```text
Use SSL ✓
```

Credentials:

```text
Gmail Address
Gmail App Password
```

---

# Editable Email Notification

Add Post Build Action:

```
Editable Email Notification
```

## Triggers

### Always

Recipient:

```text
your-email@gmail.com
```

### Failure - Any

Recipient:

```text
your-email@gmail.com
```

---

## Attach Build Log

Enable:

```
Attach Build Log
```

---

## Email Content

```text
Build Status: ${BUILD_STATUS}

Job Name: ${JOB_NAME}
Build Number: ${BUILD_NUMBER}

Console Output:
${BUILD_LOG}

Build URL:
${BUILD_URL}
```

---

# Workflow

```text
Developer Pushes Code
         │
         ▼
GitHub Repository
         │
         ▼
GitHub Webhook
         │
         ▼
Jenkins Triggered
         │
         ▼
Clone Repository
         │
         ▼
Execute Shell Script
         │
         ▼
Build Success / Failure
         │
         ▼
Send Email Notification
```

---

# Testing

Commit changes:

```bash
git add .

git commit -m "Updated file"

git push origin main
```

GitHub push automatically triggers Jenkins build.

<img width="1908" height="965" alt="image" src="https://github.com/user-attachments/assets/b54d58b1-3c08-4773-b24a-7291227a75d5" />



<img width="1918" height="1032" alt="image" src="https://github.com/user-attachments/assets/bcab709c-5b0f-4666-9da0-19674257355c" />



<img width="1462" height="756" alt="image" src="https://github.com/user-attachments/assets/f5463a1a-47a7-473b-a298-f1d85b87155f" />



<img width="1917" height="922" alt="image" src="https://github.com/user-attachments/assets/54a7afc0-4fcd-42c6-bb11-c733b75bb24c" />



<img width="1918" height="917" alt="image" src="https://github.com/user-attachments/assets/a5a89185-d566-46ff-9fc4-9fd9713e591c" />



<img width="1918" height="962" alt="image" src="https://github.com/user-attachments/assets/9cf826a6-7cb7-424b-b9ce-a67609b0ddda" />



<img width="1896" height="917" alt="Screenshot 2026-06-19 114407" src="https://github.com/user-attachments/assets/e9fa1d68-e01b-45ca-b290-6d3c00f94e68" />



<img width="1917" height="907" alt="Screenshot 2026-06-19 114431" src="https://github.com/user-attachments/assets/c167b795-d08c-4d8d-bed9-6f8f3644fc3a" />

---

# Conclusion

This project successfully implements a Continuous Integration (CI) pipeline using Jenkins and GitHub Webhooks. Whenever code changes are pushed to GitHub, Jenkins automatically fetches the latest code, executes the build process, and sends detailed email notifications containing build status and console logs. This setup demonstrates core DevOps concepts including automation, monitoring, and continuous integration.
