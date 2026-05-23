# 🚀 Blue-Green Deployment using Jenkins & AWS

## 📖 Overview

This project showcases how to implement a **Blue-Green Deployment strategy** using **Jenkins CI/CD pipeline** along with **AWS EC2 instances and an Application Load Balancer (ALB)**.

It focuses on delivering applications with:

* Zero service interruption
* Safe and controlled releases
* Quick rollback in case of failure

---

## 🏗️ System Design

The deployment follows a dual-environment approach:

* **Blue Environment** → Running current stable version
* **Green Environment** → Receives new updates

Traffic is controlled via **AWS ALB**, allowing smooth switching between environments.

```
          Jenkins (CI/CD)
                 │
                 ▼
            GitHub Repo
                 │
     ┌───────────┴───────────┐
     ▼                       ▼
 BLUE Environment       GREEN Environment
 (Live Version)         (New Deployment)
     │                       │
     └──────────┬────────────┘
                ▼
        AWS Application Load Balancer
```

---

## 🛠️ Tech Stack

* Jenkins (Automation Server)
* AWS EC2 (Compute Instances)
* AWS ALB (Traffic Routing)
* GitHub (Source Code Management)
* Ubuntu Linux
* Shell Scripting (Bash)

---

## 📁 Repository Layout

```
blue-green-deployment/
├── Jenkinsfile
├── index.html
└── README.md
```

---

## ⚙️ Setup Guide

### 🔹 Step 1: Install Jenkins

```bash
sudo apt update
sudo apt install openjdk-21-jdk -y

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y

sudo systemctl start jenkins
sudo systemctl enable jenkins
```

---

### 🔹 Step 2: Configure SSH Access

```bash
sudo su - jenkins

ssh-keygen
ssh-copy-id ubuntu@<BLUE_SERVER_IP>
ssh-copy-id ubuntu@<GREEN_SERVER_IP>
```

---

### 🔹 Step 3: Install AWS CLI

```bash
sudo apt install awscli -y
aws configure
```

---

### 🔹 Step 4: Clone the Project

```bash
git clone https://github.com/rohandhenge/blue-green-deployment.git
cd blue-green-deployment
```

---

### 🔹 Step 5: Create Sample App

```bash
echo "<h1>Version 1 - BLUE Environment</h1>" > index.html
```

---

## ⚙️ Jenkins Pipeline Overview

The pipeline performs the following operations:

### 🔸 Deploy to Green

* Transfers application files to Green server
* Updates web directory

### 🔸 Health Check

* Waits for deployment stabilization
* Validates application using HTTP status

### 🔸 Traffic Switch

* Updates ALB listener to route traffic to Green

### 🔸 Rollback

* Automatically switches back to Blue if failure occurs

---

## 🔄 Deployment Workflow

1. Jenkins fetches latest code from GitHub
2. Deploys application to **Green environment**
3. Runs health checks
4. If successful → traffic shifts to Green
5. If failed → rollback to Blue

---

## 📸 Expected Outputs

### ✔️ Pipeline Execution

* Jenkins stages should show:

  * Deploy
  * Health Check
  * Traffic Switch

### ✔️ Traffic Routing

* ALB switches from Blue → Green target group

### ✔️ Application Behavior

* Before deployment → Blue version visible
* After deployment → Green version live

---

## ✨ Key Highlights

* Zero downtime deployment
* Automated CI/CD workflow
* Load balancer-based traffic control
* Built-in rollback mechanism
* Health check validation before release

---
