# Incident 03 — HTTP 500 Internal Server Error

## 📌 Incident Summary

A customer reported that the website was returning an **HTTP 500 Internal Server Error**.

During the investigation, the backend EC2 instances were found to be reachable and healthy, but the web server configuration on one backend was intentionally configured to return HTTP 500 responses for the main website path.

The issue was resolved by restoring the correct Nginx configuration.

---

## 👤 Customer Complaint

> "The website is showing an Internal Server Error."

---

## 🔍 Symptoms

- Website returned **HTTP 500 Internal Server Error**
- The Application Load Balancer was reachable
- Backend EC2 instances remained **Healthy**
- Nginx was running
- The issue affected the customer-facing website path
- The `/health` endpoint continued to return **200 OK**

---

## 🧪 Investigation

### 1. Customer-Side Symptom

The website was accessed through the Application Load Balancer DNS name.

The customer-facing page returned:

**HTTP 500 Internal Server Error**

This indicated that the request was reaching the web server, but the server was returning an application-level error.

---

### 2. Target Group Health Check

The Target Group was checked to determine whether the backend instances were healthy.

The backend instances were found to be **Healthy**.

This helped rule out a basic network connectivity or backend availability problem.

---

### 3. Local Web Server Investigation

The affected EC2 instance was accessed using **AWS Systems Manager Session Manager**.

The local website endpoint was tested:

```bash
curl -I http://localhost


## 📸 Evidence

### 1. Nginx Configuration
![Nginx Configuration](01-nginx-configuration.png)

### 2. Customer 500 Error
![Customer 500 Error](02-customer-500-error.png)

### 3. Local Health and 500 Response
![Local Health and 500 Response](03-local-health-and-500.png)

### 4. Nginx Logs
![Nginx Logs](04-nginx-logs.png)

### 5. Service Restored
![Service Restored](05-service-restored.png)
