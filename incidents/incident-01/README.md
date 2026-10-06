# Incident 01 — Website Inaccessible Due to Security Group Misconfiguration

## 📌 Incident Summary

A customer reported that the website was not accessible.

During the investigation, the Application Load Balancer was unable to reach the backend EC2 instances because the EC2 Security Group was not allowing HTTP traffic from the ALB Security Group.

The issue was resolved by restoring the required HTTP inbound rule.

---

## 👤 Customer Complaint

> "The website is not accessible."

---

## 🔍 Symptoms

- Website returned **504 Gateway Timeout**
- Both backend EC2 targets were marked **Unhealthy**
- ALB health checks were failing
- Health check reason indicated a **request timeout**
- EC2 instances themselves were running

---

## 🧪 Investigation

### 1. Customer-Side Symptom

The website was accessed using the Application Load Balancer DNS name.

The request resulted in a **504 Gateway Timeout**.

This indicated that the ALB was unable to successfully communicate with a healthy backend target.

### 2. Target Group Health Check

The Target Group was checked to determine the health status of the backend EC2 instances.

Both targets were found to be **Unhealthy**.

### 3. Health Check Investigation

The health check details showed **Request timed out**.

This indicated that the ALB health-check request was not successfully reaching the backend web server.

### 4. Security Group Investigation

The Security Group attached to the backend EC2 instances was inspected.

The required HTTP inbound rule allowing traffic from the **Application Load Balancer Security Group** was missing.

The ALB was sending health-check traffic, but the EC2 Security Group was blocking the traffic.

---

## 🎯 Root Cause

The root cause was an incorrect inbound rule in the backend EC2 Security Group.

The Security Group did not allow **HTTP traffic on port 80 from the ALB Security Group**.

Because the ALB health checks could not reach the backend EC2 instances, the health checks timed out and both targets were marked **Unhealthy**.

As a result, the Application Load Balancer could not route customer requests to a healthy backend.

---

## 🛠️ Resolution

The required HTTP inbound rule was restored in the EC2 Security Group.

The restored rule allowed:

- **Type:** HTTP
- **Protocol:** TCP
- **Port:** 80
- **Source:** Application Load Balancer Security Group

After restoring the rule, the ALB health checks were allowed to reach the backend EC2 instances.

---

## ✅ Verification

After the Security Group rule was restored:

- ALB health checks successfully reached the backend servers
- Both EC2 targets became **Healthy**
- Health checks passed successfully
- The website became accessible again
- The **504 Gateway Timeout** error was resolved

---

## 🔄 Incident Resolution Flow

**Customer Complaint → ALB Investigation → Target Group Health Check → Health Check Timeout → Security Group Investigation → Root Cause Identified → HTTP Rule Restored → Health Checks Passed → Service Restored**

---

## 📸 Evidence

### 1. Security Group Misconfiguration

![Security Group Failure](01-security-group-failure.png)

The required HTTP inbound rule from the ALB Security Group was missing from the backend EC2 Security Group.

### 2. Customer Symptom

![Customer Symptom](02-customer-symptom.png)

The customer-facing website returned a **504 Gateway Timeout**.

### 3. Targets Unhealthy

![Targets Unhealthy](03-targets-unhealthy.png)

Both backend EC2 instances were marked **Unhealthy** by the Target Group.

### 4. Health Check Timeout

![Health Check Timeout](04-health-check-timeout.png)

The ALB health check showed that the request was timing out while attempting to reach the backend.

### 5. Service Restored

![Service Restored](05-service-restored.png)

After restoring the required Security Group rule, the backend targets became healthy and the website was restored.

---

## 🧰 AWS Services & Technologies Used

- Amazon VPC
- Amazon EC2
- Application Load Balancer
- Target Groups
- Security Groups
- Linux
- Nginx
- HTTP/HTTPS

---

## 📚 Skills Demonstrated

- AWS infrastructure troubleshooting
- Application Load Balancer troubleshooting
- Target health-check analysis
- Security Group troubleshooting
- Network connectivity investigation
- Root-cause analysis
- Incident remediation
- Service recovery verification
- Customer issue investigation
- Technical documentation
