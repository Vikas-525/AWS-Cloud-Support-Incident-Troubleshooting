# Incident 02 — Backend Server Failure

## 📌 Incident Summary

A customer reported that the website was returning an error.

During the investigation, one backend EC2 server had its Nginx web service stopped, causing the Application Load Balancer to mark that target as unhealthy.

The issue was resolved by starting the Nginx service and verifying that the backend became healthy again.

---

## 👤 Customer Complaint

> "The website is showing an error."

---

## 🔍 Symptoms

- Website returned **502 Bad Gateway**
- One EC2 target became **Unhealthy**
- The other EC2 target remained **Healthy**
- The affected EC2 instance was still running
- The issue was isolated to the web service running on the affected backend

---

## 🧪 Investigation

### 1. Application Load Balancer Check

The Application Load Balancer was checked to confirm that the customer request was reaching the application infrastructure.

### 2. Target Group Health Check

The Target Group showed that one backend EC2 instance was **Unhealthy**, while the other remained **Healthy**.

This indicated that the issue was isolated to one backend instance.

### 3. EC2 Instance Investigation

AWS Systems Manager Session Manager was used to connect to the unhealthy EC2 instance.

### 4. Nginx Service Check

The Nginx service status was checked using `sudo systemctl status nginx`.

The Nginx service was found to be **inactive (dead)**.

### 5. Local Web Server Test

The local web server was tested using `curl -I http://localhost`.

The request failed because the Nginx service was not running.

---

## 🎯 Root Cause

The Nginx web service was stopped on one backend EC2 instance.

Because Nginx was not running, the Application Load Balancer could not successfully communicate with that backend, causing the target to fail its health check.

As a result, the target was marked **Unhealthy**, and the ALB stopped routing traffic to that backend.

---

## 🛠️ Resolution

The Nginx service was started on the affected EC2 instance using `sudo systemctl start nginx`.

The service status was then verified using `sudo systemctl status nginx`.

Nginx was confirmed to be **active (running)**.

---

## ✅ Verification

After starting Nginx:

- Nginx was **active (running)**
- The local HTTP request returned **200 OK**
- The affected EC2 target became **Healthy**
- Both backend targets were **Healthy**
- The website was successfully restored

---

## 🔄 Incident Resolution Flow

**Customer Complaint → ALB Investigation → Target Group Health Check → EC2 Investigation → Nginx Service Check → Root Cause Identified → Nginx Started → Health Check Passed → Service Restored**

---

## 📸 Evidence

### 1. Nginx Service Stopped

![Nginx Service Stopped](01-nginx-service-stopped.png)

The Nginx service was found to be inactive on the affected backend EC2 instance.

---

### 2. Customer Symptom

![Customer Symptom](02-customer-symptom.png)

The customer-facing website returned a **502 Bad Gateway** error.

---

### 3. Backend Investigation

![Backend Investigation](03-backend-investigation.png)

The affected backend was investigated using AWS Systems Manager Session Manager and Linux commands.

---

### 4. Targets Recovered

![Targets Recovered](04-targets-recovered.png)

After starting Nginx, the affected target became healthy and both backend targets were healthy.

---

### 5. Service Restored

![Service Restored](05-service-restored.png)

The website was successfully restored after the Nginx service was started.

---

## 🧰 AWS Services & Technologies Used

- Amazon EC2
- Application Load Balancer
- Target Groups
- Amazon VPC
- Security Groups
- AWS Systems Manager Session Manager
- Linux
- Nginx
- HTTP

---

## 📚 Skills Demonstrated

- AWS Cloud troubleshooting
- Application Load Balancer troubleshooting
- Target health-check analysis
- EC2 troubleshooting
- Linux service management
- Nginx troubleshooting
- HTTP 5xx error investigation
- Root-cause analysis
- Incident remediation
- Service recovery verification
- Technical documentation
