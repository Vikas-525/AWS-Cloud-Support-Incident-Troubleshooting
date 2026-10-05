# Incident 02 — Backend Server Failure

## 📌 Incident Summary

A customer reported that the website was returning an error.

Investigation showed that one backend EC2 server had its Nginx web service stopped, causing the Application Load Balancer to mark that target as unhealthy.

## 👤 Customer Complaint

> "The website is showing an error."

## 🔍 Symptoms

- Website returned **502 Bad Gateway** during the incident
- One EC2 target became **Unhealthy**
- The other EC2 target remained **Healthy**
- The affected EC2 instance was running normally

## 🧪 Investigation

1. Checked the Application Load Balancer.
2. Checked the Target Group health status.
3. Found one EC2 target marked **Unhealthy**.
4. Connected to the affected EC2 instance using AWS Systems Manager Session Manager.
5. Checked the Nginx service status.
6. Found that the Nginx service was **inactive (dead)**.
7. Tested the local web service using `curl`.

## 🎯 Root Cause

The Nginx web service was stopped on one backend EC2 instance.

Because Nginx was not running, the Application Load Balancer could not successfully communicate with that backend, causing the target to fail its health check.

## 🛠️ Resolution

Started the Nginx service on the affected EC2 instance:

```bash
sudo systemctl start nginx

## 📸 Evidence

### 1. Nginx Service Stopped
![Nginx Service Stopped](01-nginx-service-stopped.png)

### 2. Customer Symptom
![Customer Symptom](02-customer-symptom.png)

### 3. Backend Investigation
![Backend Investigation](03-backend-investigation.png)

### 4. Targets Recovered
![Targets Recovered](04-targets-recovered.png)

### 5. Service Restored
![Service Restored](05-service-restored.png)
