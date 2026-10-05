# AWS Cloud Support Incident Troubleshooting

## 📌 Project Overview

This project demonstrates a hands-on AWS Cloud Support environment designed to simulate and troubleshoot common cloud infrastructure and application incidents.

The environment was intentionally configured to reproduce real-world support issues. Each incident was investigated using a structured troubleshooting process:

**Customer Complaint → Symptom Identification → Evidence Collection → Root Cause Analysis → Remediation → Service Verification**

The project focuses on practical AWS Cloud Support skills rather than simply deploying infrastructure.

---

## 🎯 Project Objectives

- Troubleshoot common AWS infrastructure issues
- Investigate customer-reported application problems
- Analyze Application Load Balancer health checks
- Troubleshoot EC2 and Linux services
- Diagnose HTTP 5xx errors
- Perform root-cause analysis
- Apply corrective actions
- Verify service recovery
- Document troubleshooting evidence

---

## 🏗️ AWS Architecture

The environment was deployed across two Availability Zones.

### Architecture Components

- Amazon VPC
- 2 Availability Zones
- 2 Public Subnets
- 2 Private Subnets
- Internet Gateway
- NAT Gateway
- Application Load Balancer
- Target Group
- 2 Amazon EC2 Instances
- Security Groups
- Ubuntu Linux
- Nginx
- AWS Systems Manager Session Manager
- Amazon CloudWatch
- AWS CloudTrail
- AWS IAM

### Traffic Flow

```text
Internet
   │
   ▼
Application Load Balancer
   │
   ▼
Target Group
   │
   ├───────────────┐
   ▼               ▼
 EC2-1            EC2-2
 Nginx            Nginx
   │               │
Private Subnet   Private Subnet
