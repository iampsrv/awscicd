
# 📘 What is AWS ECR?

Amazon Elastic Container Registry (ECR) is a fully managed Docker container image registry that makes it easy to store, manage, and deploy container images.

Integrated with Amazon ECS, EKS, Lambda, and AWS Fargate.  
Secure, scalable, and highly available.  
Supports both public and private registries.

---

## 🎯 Key Features of AWS ECR

### 1. Private and Public Repositories
- **Private repositories**: Store container images accessible only to your AWS account or shared with other accounts.
- **Public repositories**: Allow hosting and sharing public images (similar to Docker Hub).

### 2. Lifecycle Policies
- Automatically remove unused or old images based on rules (e.g., retain only last N images).

### 3. Image Tagging
- Supports semantic versioning with tags (v1.0, latest, etc.).
- Tags are mutable and can be overwritten.

### 4. Integration with CI/CD
- Easily integrated with:
  - CodeBuild
  - CodePipeline
  - GitHub Actions
  - Jenkins

### 5. Security & IAM
- Supports IAM-based access control and resource-based policies.
- Image scanning for vulnerabilities (via Amazon Inspector).
- KMS encryption for images at rest.

### 6. Replication Across Regions
- Supports cross-region image replication to improve performance and disaster recovery.

---

## 🧱 Core Components

| Component     | Description                                                    |
|---------------|----------------------------------------------------------------|
| Repository     | Logical container for storing Docker images                   |
| Image          | Built container image with metadata and layers                |
| Tag            | Identifier used to version and retrieve images                |
| Registry URI   | `<aws_account_id>.dkr.ecr.<region>.amazonaws.com`             |

---

## 🛠️ Basic Workflow with AWS ECR

**Step 1: Authenticate Docker to ECR**
```bash
aws ecr get-login-password --region us-east-1 \
| docker login --username AWS \
--password-stdin <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com
```

**Step 2: Create an ECR Repository**
```bash
aws ecr create-repository --repository-name my-app
```

**Step 3: Build and Tag the Docker Image**
```bash
docker build -t my-app .
docker tag my-app:latest <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/my-app:latest
```

**Step 4: Push the Image**
```bash
docker push <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/my-app:latest
```

**Step 5: Use in ECS/EKS**

Reference the image in ECS task definitions or Kubernetes manifests.

---

## 🔒 Security and Access

- **IAM policies**: Define who can push/pull images.
- **Repository policies**: Resource-level access control (similar to S3 policies).
- **Encryption**:
  - At rest: AWS-managed or customer-managed KMS keys.
  - In transit: TLS encryption by default.

---

## 🧪 Image Scanning

- Automatically scan images on push or on-demand.
- Detect Common Vulnerabilities and Exposures (CVEs).
- Integrated with Amazon Inspector.
- Vulnerability reports include CVSS scores and remediation suggestions.

---

## 📈 Monitoring and Logging

- Integrated with Amazon CloudWatch for:
  - Repository activity
  - Image scan findings
  - Pull and push events
- Use CloudTrail for auditing API calls.

---

## 🌐 Public ECR (Amazon ECR Public)

- Alternative to Docker Hub.
- Host and share public container images globally.
- Public URL format: `public.ecr.aws/<namespace>/<repository>`

---

## 💵 Pricing Overview

| Resource        | Pricing Description                                     |
|-----------------|----------------------------------------------------------|
| Storage         | Per GB-month for images stored                          |
| Data Transfer   | Charged when pulling images (cross-region/outbound)     |
| Image Scanning  | Free for basic scan, charges for enhanced scan          |
| Public ECR      | Free up to certain limits (then tiered pricing)         |

👉 [Official Pricing](https://aws.amazon.com/ecr/pricing)

---

## ✅ Best Practices

- Use lifecycle policies to clean up unused images.
- Enable image scanning to detect vulnerabilities early.
- Use semantic versioning tags to manage releases.
- Leverage cross-region replication for latency and resilience.
- Secure access using IAM policies and KMS encryption.
- Integrate with CI/CD pipelines for automated builds and pushes.

---

## 🔗 Integration with Other AWS Services

| Service       | Integration Purpose                             |
|---------------|--------------------------------------------------|
| ECS/EKS       | Use container images in tasks/pods              |
| CodeBuild     | Build images and push to ECR                    |
| CodePipeline  | Automate the entire CI/CD workflow              |
| Lambda        | Use container images stored in ECR              |
| Inspector     | Perform vulnerability scans                     |

---

## 📚 Helpful Resources

- [AWS ECR Documentation](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)
- [Amazon ECR Public Gallery](https://gallery.ecr.aws/)
- [Amazon ECR Pricing](https://aws.amazon.com/ecr/pricing)
- [Best Practices for ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/best-practices.html)
