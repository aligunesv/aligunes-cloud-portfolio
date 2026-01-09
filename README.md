# AWS Static Website Hosting – Zero-Cost Cloud & DevOps Project

This project demonstrates a **production-grade static website architecture** built on **AWS** using **Amazon S3, CloudFront (OAC), Route 53, ACM, and GitHub Actions**, with a strong focus on **security, automation, and real-world troubleshooting**. All within the AWS Free Tier.

🌐 Live Demo: https://aligunes.cloud

---

## 🚀 Project Objectives

- Build a **secure and scalable** static website on AWS
- Practice real-world **Cloud & DevOps workflows**
- Implement **CI/CD automation** using GitHub Actions
- Understand and debug common **CloudFront + S3 production issues**
- Keep the entire infrastructure **zero-cost**


---

## 🏗 Architecture Overview

                    ┌──────────────────────────┐
                    │        User Browser      │
                    │  https://aligunes.cloud  │
                    └─────────────┬────────────┘
                                  │ HTTPS (443)
                                  ▼
                    ┌───────────────────────────┐
                    │      Amazon CloudFront    │
                    │  - Global CDN             │
                    │  - HTTPS (ACM Certificate)│
                    │  - Origin Access Control  │
                    └─────────────┬─────────────┘
                                  │ Signed Requests
                                  ▼
                    ┌───────────────────────────┐
                    │        Amazon S3          │
                    │  - Private Bucket         │
                    │  - No Public Access       │
                    │  - ACLs Disabled          │
                    │  - Bucket Policy Only     │
                    └───────────────────────────┘

            ┌──────────────────────────────────────────────────┐
            │                    CI / CD                       │
            │                                                  │
            │   GitHub Repository                              │
            │        │                                         │
            │        ▼                                         │
            │   GitHub Actions                                 │
            │   - aws s3 sync                                  │
            │   - CloudFront cache invalidation (/*)           │
            │                                                  │
            └──────────────────────────────────────────────────┘

### Components

- **Amazon S3** – Stores static files (private, no public access)
- **Amazon CloudFront** – Global CDN with HTTPS
- **Origin Access Control (OAC)** – Restricts S3 access to CloudFront only
- **Route 53** – DNS and custom domain
- **GitHub Actions** – Automated CI/CD pipeline


---

## 🔐 Security Design

This setup follows AWS best practices for serving **private S3 content via CloudFront**:
- S3 bucket is **not publicly accessible**
- **S3 Static Website Hosting disabled**
- Access granted only through **CloudFront OAC**
- Permissions managed exclusively via **bucket policies**
- **Object Ownership: Bucket owner enforced**
- ACLs fully disabled


---

## 🔄 CI/CD Pipeline

On every push to the `main` branch:

1.	Repository is checked out by GitHub Actions
2.	AWS credentials are injected via GitHub Secrets
3.	Static files are synced to S3
4.	CloudFront cache is invalidated

```yaml
aws s3 sync . s3://<bucket-name> \
  --exclude ".git/*" \
  --exclude ".github/*"
```

---

## 🧠 Key Learnings
- Difference between **S3 Static Website Hosting** and **S3 REST API origins**
- How **CloudFront Origin Access Control (OAC)** works in production
- Why CloudFront requires S3 **REST origins** instead of website endpoints
- Understanding CloudFront caching behavior and propagation delays
- Debugging **403 AccessDenied** errors caused by:
  - Misconfigured origins
  - Missing default root objects
  - CloudFront cache behavior
  - Incorrect behavior–origin mappings

---

## ⚠️ Real-World Issues & Resolutions

### Issue: CloudFront returns 403 AccessDenied

**Cause:**
Origin misconfiguration caused signed requests to fail.

**Solution:**
- Recreated the origin using AWS-managed S3 origin selection
- Correctly attached Origin Access Control
- Updated bucket policy to allow CloudFront access only


---

### Issue: Updates not visible after deployment

**Cause:**
CloudFront was serving cached content.

**Solution:**
- Created CloudFront invalidations (/*)
- Waited for global CDN propagation

---

## 💰 Cost
- $0 / month
- Fully within AWS Free Tier
- No Route 53, WAF, or logging enabled

⚠️ Costs may apply if traffic exceeds Free Tier limits

---

## 📌 Why This Project Matters

This project goes beyond basic tutorials by focusing on:
- Production-level security
- CI/CD automation
- Hands-on debugging of real AWS issues

It reflects how static websites are deployed, secured, and maintained in real-world cloud environments.

---

👤 Author  
**Ali Ihsan Gunes**  
IT Specialist | Cloud & DevOps Enthusiast  
🔗 LinkedIn: https://linkedin.com/in/aligunesv1  
🔗 Portfolio: https://aligunes.cloud