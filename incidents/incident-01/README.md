# Incident 01 — Website Inaccessible

## 📌 Incident Summary

A customer reported that the website was inaccessible.

The Application Load Balancer was reachable, but the backend EC2 instances were not passing health checks.

## 👤 Customer Complaint

> "The website is not loading."

## 🔍 Symptoms

- ALB returned **504 Gateway Timeout**
- Both EC2 targets were **Unhealthy**
- Target health-check reason showed **Request timed out**
- EC2 instances were running normally

## 🧪 Investigation

1. Checked the Application Load Balancer.
2. Checked the Target Group health status.
3. Both EC2 instances were marked **Unhealthy**.
4. Checked the health-check failure reason.
5. Reviewed the EC2 Security Group.
6. Found that HTTP port 80 traffic from the ALB Security Group was not allowed.

## 🎯 Root Cause

The EC2 Security Group was missing an inbound HTTP (port 80) rule allowing traffic from the Application Load Balancer Security Group.

Because the ALB health-check requests could not reach the Nginx service on the EC2 instances, the health checks timed out and both targets became unhealthy.

## 🛠️ Resolution

Restored the following inbound rule in the EC2 Security Group:

- Protocol: TCP
- Port: 80
- Source: Application Load Balancer Security Group

## ✅ Verification

After restoring the Security Group rule:

- Both EC2 targets became **Healthy**
- ALB health checks succeeded
- The website became accessible again
- Service was successfully restored

## 📸 Evidence

Screenshots documenting the incident investigation and recovery are provided below.

### 1. Security Group Failure

![Security Group Failure](01-security-group-failure.png)

### 2. Customer Symptom

![Customer Symptom](02-customer-symptom.png)

### 3. Targets Unhealthy

![Targets Unhealthy](03-targets-unhealthy.png)

### 4. Health Check Timeout

![Health Check Timeout](04-health-check-timeout.png)

### 5. Service Restored

![Service Restored](05-service-restored.png)
