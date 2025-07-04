# 🛍️ AWS Auto Scaling and Load Balancing for Node.js E-Commerce App

## 📌 Overview

A hands-on AWS DevOps project that demonstrates deploying a Node.js e-commerce application using EC2 instances, Application Load Balancer (ALB), Auto Scaling Group (ASG), IAM roles, and CloudWatch Alarms with SNS email notifications. The project ensures high availability, fault tolerance, and monitoring.

It ensures high availability, fault tolerance, and automated monitoring and alerting.

---

## 🛠️ Technologies & Services Used

- **Amazon EC2** – for hosting web servers
- **Elastic Load Balancer (ALB)** – to distribute traffic
- **Auto Scaling Group (ASG)** – to handle dynamic instance scaling
- **Amazon CloudWatch** – for monitoring and alerting
- **Amazon SNS** – for email notifications
- **IAM Roles** – for EC2 permissions
- **Launch Template** – for launching new EC2 instances
- **Amazon Linux AMI** – as base image for EC2
- **Node.js** – E-Commerce App backend

---

## 🌐 Architecture Overview

![Image](https://github.com/user-attachments/assets/e0fc40c4-1c32-4c1d-aa37-23984aa99271)
              

---

## ⚙️ Setup Breakdown

### 1. Load Balancer: `Ecomm-A.L.B`
- Type: **Application**
- Scheme: **Internet-facing**
- Status: ✅ *Active*
- Hosted in **2 AZs** (us-east-1a, us-east-1b)
- DNS: `ecomm-alb-656131775.us-east-1.elb.amazonaws.com`

### 2. Target Group: `tg`
- Target type: **Instance**
- Protocol: **HTTP (port 80)**
- Total targets: 4
  - ✅ 1 Healthy
  - ❌ 3 Unhealthy

### 3. Auto Scaling Group: `ecomm-asg`
- Launch Template: `ecomm-asg`
- AMI: `ami-000ec6c25978d5999`
- Instance Type: `t2.micro`
- Capacity: min=2, max=4, desired=2
- Zones: **us-east-1a & us-east-1b**

### 4. Launch Template
- Name: `ecomm`
- Version: `v0.1 (Default)`
- Security Groups:
  - `sg-03cd7f12cd91ee1f0`
  - `sg-05440c9f9581e351d`
  - `sg-077a0750357b2b282`
  - `sg-0e523fb5ee345e569`
- Key Pair: `project`

### 5. IAM Role: `ecomm`
- Policies attached:
  - `AmazonEC2RoleforSSM`
  - `AmazonSNSFullAccess`
  - `CloudWatchAgentServerPolicy`

### 6. EC2 Instances
- `Web-app-1`, `Web-app-2`, etc.
- All instances: ✅ *Running*
- Instance types: `t2.micro`, `t3.micro`
- IAM Role attached: `ecomm`

### 7. CloudWatch Alarms
| Alarm Name                             | Status     | Condition                             |
|----------------------------------------|------------|----------------------------------------|
| Status check failed <Web-app-1>        | ⏸️ Insufficient Data | StatusCheckFailed_Instance >= 1         |
| high-memory-usage <Web-app-1>          | ✅ OK       | mem_used_percent > 80 (10 mins)        |
| high-cpu-utilization <Web-app-1>       | ✅ OK       | disk_used_percent > 70 (10 mins)       |
| TargetTracking-AS-AlarmLow             | ❌ In Alarm | CPUUtilization < 49 for 15 datapoints  |
| TargetTracking-AS-AlarmHigh            | ✅ OK       | CPUUtilization > 70 for 3 datapoints   |

### 8. SNS Topic Subscription
- Topic ARN: `arn:aws:sns:us-east-1:471112556180:ecomm`
- Confirmed Email: ✅ `u.kommineni@ajacs.in`

---

## 📸 Screenshots Included in `/images`

- Load Balancer Configuration
<img width="1913" height="911" alt="Image" src="https://github.com/user-attachments/assets/d4bebf0e-bac4-4e82-88db-d77347dfe100" />
![alt text](<A.L.B status active image-13.png>)
- Target Group Health:

![alt text](<Create Target group image-2.png>)

- Auto Scaling Group Settings:

![alt text](<Auto scaling group size image-3.png>)

- IAM Role and policies:
![alt text](<ecomm iam role image-15.png>)

- CloudWatch Alarms history:

![alt text](<cloudwatch alaram image-12.png>)

- Email Confirmation for SNS:

![alt text](<confirm subscription email notification image-7.png>)

![alt text](<Ecomm-sns confrmation status email notification image-8.png>)

![alt text](<Email notification alert image -10.png>)

---
---

## 🧩 Main AWS Components Used

Service

Purpose

EC2

Runs the Node.js-based e-commerce app

Launch Template

Template to launch EC2 instances with pre-configured settings

Auto Scaling Group

Automatically scales instances up/down based on CPU usage

Application Load Balancer (ALB)

Distributes traffic across instances in multiple AZs

Target Group

Registers EC2 instances behind the ALB

CloudWatch Agent

Collects system-level metrics like CPU, memory, and disk

CloudWatch Alarms

Triggers actions based on performance thresholds (e.g., CPU > 70%)

SNS

Sends email alerts when alarms are triggered:

![alt text](Email-alert.png)

IAM Role

Grants EC2 permissions to write logs and send alerts


---

## 📬 Author

**Uday sairam**  
Trainee Software Engineer | AWS Cloud Practitioner  
Location: India  
Email: saikommineni5@gmail.com 

LinkedIn: www.linkedin.com/in/uday-sai-ram-kommineni-uday-sai-ram

---
