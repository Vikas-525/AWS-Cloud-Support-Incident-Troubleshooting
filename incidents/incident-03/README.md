# Incident 03 — HTTP 500 Internal Server Error

## 📌 Incident Summary

A customer reported that the website was returning an HTTP 500 Internal Server Error.

During the investigation, the backend EC2 instance was reachable and healthy, but the Nginx configuration was causing the main website path to return an HTTP 500 response.

The issue was resolved by restoring the correct Nginx configuration and verifying that the website returned successfully.

---

## 👤 Customer Complaint

> "The website is showing an Internal Server Error."

---

## 🔍 Symptoms

- Website returned **HTTP 500 Internal Server Error**
- The Application Load Balancer was reachable
- Backend EC2 instances remained **Healthy**
- Nginx was running
- The `/health` endpoint continued to return **200 OK**
- The issue affected the customer-facing website path

---

## 🧪 Investigation

### 1. Application Load Balancer Check

The Application Load Balancer was checked to confirm that the customer request was reaching the application infrastructure.

### 2. Target Group Health Check

The Target Group was checked to determine whether the backend instances were healthy.

The backend instances were found to be **Healthy**.

This helped rule out a basic backend availability or network connectivity problem.

### 3. Local Web Server Investigation

The affected EC2 instance was accessed using AWS Systems Manager Session Manager.

The local website endpoint was tested using `curl -I http://localhost`.

The request returned **HTTP 500 Internal Server Error**.

This confirmed that the issue existed on the backend web server.

### 4. Health Endpoint Verification

The health endpoint was tested using `curl -i http://localhost/health`.

The endpoint returned **HTTP 200 OK**.

This confirmed that Nginx was running and responding to requests.

### 5. Nginx Configuration Investigation

The Nginx configuration was inspected.

The main `/` location had been configured to return an HTTP 500 response:

`location / { return 500; }`

This configuration caused requests to the main website path to return an Internal Server Error.

---

## 🎯 Root Cause

The root cause was an incorrect Nginx web-server configuration.

The main website path `/` was configured to return **HTTP 500**, causing customer requests to fail even though the EC2 instance and Nginx service were running normally.

The `/health` endpoint continued to return **200 OK**, allowing the Target Group health check to confirm that the backend was available.

---

## 🛠️ Resolution

The incorrect Nginx configuration was restored.

The main website location was changed back to the normal configuration:

`location / { try_files $uri $uri/ =404; }`

The Nginx configuration was then validated using `sudo nginx -t`.

After the configuration test passed, Nginx was reloaded using `sudo systemctl reload nginx`.

---

## ✅ Verification

After restoring the Nginx configuration:

- Nginx configuration test passed
- Nginx successfully reloaded
- The local website returned **HTTP 200 OK**
- The `/health` endpoint continued to return **HTTP 200 OK**
- The backend EC2 instance remained **Healthy**
- The website was successfully restored
- The HTTP 500 error was resolved

---

## 🔄 Incident Resolution Flow

**Customer Complaint → ALB Investigation → Target Group Health Check → Backend Investigation → Local HTTP Test → Nginx Configuration Check → Root Cause Identified → Configuration Restored → Nginx Reloaded → HTTP 200 Verified → Service Restored**

---

## 📸 Evidence

### 1. Nginx Configuration

![Nginx Configuration](01-nginx-configuration.png)

The Nginx configuration was intentionally modified so that the main website path returned HTTP 500.

---

### 2. Customer 500 Error

![Customer 500 Error](02-customer-500-error.png)

The customer-facing website returned an **HTTP 500 Internal Server Error**.

---

### 3. Local Health and 500 Response

![Local Health and 500 Response](03-local-health-and-500.png)

The `/health` endpoint returned **200 OK**, while the main `/` endpoint returned **500**.

---

### 4. Nginx Logs

![Nginx Logs](04-nginx-logs.png)

The Nginx access logs showed requests to the website and the corresponding HTTP 500 responses.

---

### 5. Service Restored

![Service Restored](05-service-restored.png)

After restoring the Nginx configuration and reloading the service, the website returned successfully.

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
- Linux troubleshooting
- Nginx configuration troubleshooting
- HTTP 5xx error investigation
- Root-cause analysis
- Incident remediation
- Service recovery verification
- Technical documentation
