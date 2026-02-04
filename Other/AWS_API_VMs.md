### Exposing APIs on AWS EC2: Generic Architectural Guide

This document provides a generic architectural overview for exposing APIs running on an AWS EC2 instance, applicable to any number of APIs (2, 5, or more). The guide covers infrastructure choices, security, and traffic management, ordered from the simplest/most cost-effective to the most complex/expensive solutions.

---

## 1. Public IP + Security Group Ports

**Description:**

* The EC2 instance has a public IP or Elastic IP
* Open the necessary ports in the Security Group
* Each API listens on a separate port

**Pros:**

* Simple and quick to set up
* No additional AWS services required

**Cons:**

* Exposes ports publicly, low security
* No automatic TLS
* No advanced routing

**Recommended use:**

* Testing, proof-of-concept, temporary environments

**Cost:** ⭐ (low)
**Complexity:** ⭐

---

## 2. AWS Network Load Balancer (Layer 4)

**Description:**

* L4 load balancing on TCP/UDP ports
* Forwards connections directly to the EC2 instance(s)

**Pros:**

* High reliability at TCP/UDP level
* Cost-effective

**Cons:**

* No HTTP routing based on path or hostname
* Does not handle HTTPS/TLS (must be terminated on the EC2 instance)
* Security limited to Security Groups

**Recommended use:**

* Exposing TCP/UDP ports for multiple APIs without advanced routing

**Cost:** ⭐⭐
**Complexity:** ⭐⭐

---

## 3. AWS Application Load Balancer (Layer 7)

**Description:**

* Managed Layer 7 load balancer
* Supports path-based or host-based routing to APIs
* Centralized TLS termination, security groups, health checks

**Pros:**

* Centralized HTTPS/TLS
* Flexible routing for multiple APIs
* Scalable and managed

**Cons:**

* Higher cost than L4 Load Balancer
* Overkill for very small setups

**Recommended use:**

* Production environments with security requirements
* Multiple internal or external APIs/microservices

**Cost:** ⭐⭐⭐
**Complexity:** ⭐⭐⭐⭐

---

## 4. Amazon CloudFront + ALB/EC2

**Description:**

* Global content delivery network (CDN) with caching
* HTTPS termination at the edge
* Routes traffic to ALB or EC2 instances

**Pros:**

* High performance and global access
* Automatic TLS and certificate management
* Optional WAF integration
* Supports multi-region deployments

**Cons:**

* Higher cost
* Best for global or highly available environments

**Recommended use:**

* Public APIs with global users
* High availability and performance scenarios

**Cost:** ⭐⭐⭐⭐
**Complexity:** ⭐⭐⭐⭐

---

## 5. Amazon API Gateway

**Description:**

* Fully managed API gateway
* Handles routing, versioning, throttling, authentication (IAM, Cognito, JWT)
* Backend can be one or more EC2 instances, Lambda functions, or other services

**Pros:**

* Advanced security and API governance
* TLS, authentication, throttling, analytics
* Versioning and management of multiple APIs

**Cons:**

* High cost
* More complex setup

**Recommended use:**

* Public APIs or customer-facing APIs
* Centralized management and governance of multiple APIs

**Cost:** ⭐⭐⭐⭐⭐
**Complexity:** ⭐⭐⭐⭐⭐

---

## Summary Table of Solutions

| Order | Solution                           | Cost  | Complexity | HTTP Routing | HTTPS/TLS | Security  | Recommended Use                                      |
| ----: | ---------------------------------- | ----- | ---------- | ------------ | --------- | --------- | ---------------------------------------------------- |
|     1 | Public IP + Security Group Ports   | ⭐     | ⭐          | ❌            | ❌         | ❌         | Testing, POC, temporary environments                 |
|     2 | AWS Network Load Balancer (L4)     | ⭐⭐    | ⭐⭐         | ❌            | ❌         | ⚠ Limited | Exposing TCP/UDP ports for multiple APIs             |
|     3 | AWS Application Load Balancer (L7) | ⭐⭐⭐   | ⭐⭐⭐⭐       | ✅            | ✅         | ⭐⭐⭐⭐      | Production with security requirements, multiple APIs |
|     4 | CloudFront + ALB/EC2               | ⭐⭐⭐⭐  | ⭐⭐⭐⭐       | ✅            | ✅         | ⭐⭐⭐⭐      | Global access, high performance, multi-API           |
|     5 | Amazon API Gateway                 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐      | ✅            | ✅         | ⭐⭐⭐⭐⭐     | Public/customer-facing APIs, multi-API governance    |

---

## Key Considerations

* L4 Network Load Balancer **does not handle HTTPS/TLS**; termination must occur on the EC2 instance.
* L7 solutions (Application Load Balancer, CloudFront, API Gateway) provide centralized TLS and enhanced security.
* API Gateway is designed for **full management of multiple APIs**, including versioning, access control, and monitoring.
* Architecture choice depends on **number of APIs, security requirements, scalability, and target audience**.

---

## General Recommendations

* **Testing or few internal APIs → Public IP or Network Load Balancer**
* **Production with HTTPS and security → Application Load Balancer**
* **Public/customer-facing APIs → API Gateway**
* **Global access + performance → CloudFront + ALB**
