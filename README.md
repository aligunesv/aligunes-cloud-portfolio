# AWS Static Website Hosting – Zero Cost Cloud & DevOps Project

This project demonstrates a **production-grade static website architecture** built on AWS using **S3, CloudFront (OAC), and GitHub Actions**, with a strong focus on **security, automation, and real-world troubleshooting**. All within the AWS Free Tier.

---

## 🚀 Project Goals

- Build a secure and scalable static website using AWS
- Practice real-world **Cloud & DevOps** workflows
- Implement **CI/CD automation** with GitHub Actions
- Understand and debug common **CloudFront + S3 production issues**
- Keep the entire system **zero-cost** (AWS Free Tier)

---

## 🏗 Architecture Overview

User Browser
	 ↓ HTTPS
CloudFront (OAC enabled)
    ↓ Signed Requests
Private S3 Bucket

- **Amazon S3**: Stores static website files (private, no public access)
- **Amazon CloudFront**: Global CDN with HTTPS
- **Origin Access Control (OAC)**: Restricts S3 access to CloudFront only
- **GitHub Actions**: Automated CI/CD deployment pipeline

---

## 🔐 Security Design

This setup follows AWS security best practices for serving private S3 content through CloudFront.
- S3 bucket is **not publicly accessible**
- **S3 Static Website Hosting disabled**
- CloudFront accesses S3 via **Origin Access Control (OAC)**
- Access controlled exclusively through **bucket policies**
- **Object Ownership: Bucket owner enforced**
- ACLs fully disabled

---

## 🔄 CI/CD Pipeline

On every push to the `main` branch:

1. GitHub Actions checks out the repository
2. AWS credentials are securely injected via GitHub Secrets
3. Static files are synced to S3 using `aws s3 sync`
4. CloudFront cache is invalidated to reflect changes instantly

```yaml
aws s3 sync . s3://<bucket-name> --exclude ".git/*" --exclude ".github/*"
```

---

## 🧠 Key Learnings
- Difference between S3 Static Website Hosting and S3 REST API origins
- How CloudFront Origin Access Control (OAC) works in production
- Why CloudFront requires S3 REST origins instead of website endpoints
- Understanding CloudFront caching behavior and propagation delays
- Debugging 403 AccessDenied errors caused by:
  -  Misconfigured origins
  - Missing default root objects
  - CloudFront cache behavior
  - Incorrect behavior–origin mappings

---

## ⚠️ Real-World Issues & Resolutions

### Issue: CloudFront returns 403 AccessDenied

Cause:
CloudFront origin was misidentified, causing signed requests to fail.

Resolution:
- Reconfigured the origin using AWS-managed S3 origin selection
- Correctly attached Origin Access Control (OAC)
- Ensured bucket policies allowed CloudFront access only

---

### Issue: Changes not reflected after deployment

Cause:
CloudFront cache still served stale content.

Resolution:
- Created CloudFront invalidations (/*)
- Waited for global propagation

---

## 💰 Cost
- $0 / month
- Fully within AWS Free Tier
- No Route 53, WAF, or logging enabled
Note: Costs may apply if traffic exceeds AWS Free Tier limits.

---

## 📌 Why This Project Matters

This project goes beyond basic setup tutorials by focusing on:
- Production-level security
- CI/CD automation
- Hands-on debugging of real AWS issues

It reflects how static websites are deployed and maintained in real-world cloud environments.

---

👤 Author

Ali Ihsan Gunes
IT Specialist | Cloud & DevOps Enthusiast