# 📘 What is CI/CD?

CI/CD stands for:

- **Continuous Integration (CI)**: The practice of automatically integrating code changes from multiple contributors into a shared repository and verifying them with automated builds and tests.
- **Continuous Delivery (CD)**: The practice of automatically preparing code for deployment to staging or production environments.
- **Continuous Deployment (CD)** (optional extension): The practice of automatically deploying every change that passes all stages of your pipeline directly to production.

---

# 🎯 Goals of CI/CD

- Faster and safer software delivery  
- Higher code quality and reliability  
- Automated testing and deployment  
- Early detection of bugs  
- Improved developer productivity  

---

# 🔁 CI/CD Pipeline Lifecycle

## 1. Source Stage
- Detects changes in version control (e.g., GitHub, CodeCommit)
- Triggers the CI/CD pipeline

## 2. Build Stage
- Compiles source code
- Installs dependencies
- Produces artifacts

## 3. Test Stage
- Runs automated tests:
  - Unit tests
  - Integration tests
  - UI/End-to-end tests

## 4. Package/Artifact Stage
- Bundles the application into deployable packages (e.g., Docker image, JAR)

## 5. Deploy Stage
- Pushes the code to staging or production
- May involve approval steps or full automation

## 6. Monitor Stage
- Observes performance, logs, and metrics post-deployment

---

# 🧩 Components of CI/CD

| Component         | Role                                       |
|------------------|--------------------------------------------|
| Source Control    | Manages code versions (Git, GitHub, GitLab)|
| CI Server         | Automates builds/tests (Jenkins, GitLab CI)|
| Artifact Repository | Stores build output (Nexus, Artifactory) |
| Deployment Tool   | Automates release (CodeDeploy, Spinnaker)  |
| Monitoring Tool   | Observes post-release metrics (Prometheus) |
| Notification Tool | Communicates build status (Slack, Email)   |

---

# 🔧 Common CI/CD Tools

| Category       | Tools                                                  |
|----------------|--------------------------------------------------------|
| CI Servers     | Jenkins, GitHub Actions, GitLab CI, CircleCI, Travis CI|
| CD Tools       | Spinnaker, AWS CodePipeline, Azure DevOps, Argo CD     |
| Build Tools    | Maven, Gradle, npm, pip, Docker                         |
| Testing Tools  | JUnit, Selenium, Cypress, Jest                          |
| Monitoring     | Prometheus, Grafana, CloudWatch, Datadog               |
| Notification   | Slack, Microsoft Teams, Email, SNS                     |

---

# 🧪 CI/CD Testing Types

| Test Type         | Purpose                          | Speed   |
|-------------------|----------------------------------|---------|
| Unit Tests        | Test small units of code         | Fast    |
| Integration Tests | Test component interactions      | Medium  |
| End-to-End Tests  | Test full app workflows          | Slow    |
| Security Tests    | Scan for vulnerabilities         | Medium  |
| Performance Tests | Evaluate speed/load handling     | Slow    |

---

# ✅ Best Practices for CI/CD

## ✅ Continuous Integration
- Commit small, frequent changes  
- Run fast, reliable tests  
- Use feature branches and pull requests  
- Automate linting, testing, and merging  

## ✅ Continuous Delivery
- Use staging environments to mirror production  
- Automate approvals and rollback processes  
- Generate and store build artifacts consistently  

## ✅ Continuous Deployment
- Monitor health after each release  
- Use blue/green or canary deployments  
- Log everything for observability and debugging  

---

# 🧠 Benefits of CI/CD

- Faster release cycles  
- Reduced manual effort  
- Improved collaboration  
- Better product quality  
- Lower mean time to recover (MTTR)  

---

# 📈 CI/CD Metrics to Track

| Metric                  | Description                              |
|-------------------------|------------------------------------------|
| Build Time              | Time to complete builds                  |
| Test Coverage           | % of code covered by automated tests     |
| Deployment Frequency    | How often code is deployed               |
| Lead Time for Changes   | Time from commit to deployment           |
| Change Failure Rate     | % of deployments causing errors          |
| MTTR                    | Time to fix a production issue           |

---

# 🔐 CI/CD and DevSecOps

DevSecOps integrates security into CI/CD pipelines:

- Static analysis (SAST)  
- Dependency scanning  
- Container vulnerability scans  

**Tools**: SonarQube, Snyk, Aqua Security, Checkmarx

---

# 🚀 CI/CD Example with AWS

Pipeline:
- **CodeCommit** – source repo  
- **CodeBuild** – compile and test code  
- **CodeDeploy** – deploy to EC2, ECS, or Lambda  
- **CloudWatch** – monitor and alert  

---

# 🔗 Helpful Resources

- [CI/CD on AWS](https://aws.amazon.com/devops/continuous-delivery/)
- [CI/CD with GitHub Actions](https://docs.github.com/en/actions)
- [Google Cloud CI/CD](https://cloud.google.com/solutions/devops/devops-tech-stack)
- [Azure DevOps Pipelines](https://learn.microsoft.com/en-us/azure/devops/pipelines/)

---

# 💬 Summary Table: CI vs CD

| Feature         | Continuous Integration | Continuous Delivery | Continuous Deployment |
|-----------------|------------------------|---------------------|------------------------|
| Focus           | Merge and test code    | Prepare for release | Automatically release  |
| Deployment      | Manual or automated    | Manual              | Fully automated        |
| Risk            | Low                    | Medium              | High                   |
| Feedback Speed  | Fast                   | Faster              | Fastest                |