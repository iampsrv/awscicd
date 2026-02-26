# 📘 Overview of AWS CodeBuild

AWS CodeBuild is a fully managed continuous integration (CI) service that compiles source code, runs tests, and produces software packages ready for deployment.

## ☁️ Serverless
- No need to provision or manage build servers.

## 📈 Scalable
- Automatically scales to handle multiple concurrent builds.

## 💰 Pay-as-you-go
- Charges are based on build time and compute resources used.

## 🔒 Secure
- Integrates with IAM, VPC, and KMS for secure builds.

---

# ⚙️ Core Features

## 1. Build Projects
- Central unit of work in CodeBuild.
- Includes source, environment, buildspec file, artifacts, and logs.
- Supports manual and automatic triggers (e.g., via CodePipeline or Git webhooks).

## 2. Build Environments
- Choose between:
  - Managed images (AWS provided Docker images)
  - Custom Docker images hosted in Amazon ECR or Docker Hub
- Environment variables and secrets can be configured.
- Support for Linux, Windows, and ARM environments.

## 3. Buildspec File (`buildspec.yml`)
A YAML file that defines the build steps.

### Example:
\`\`\`yaml
version: 0.2

phases:
  install:
    runtime-versions:
      nodejs: 18
    commands:
      - npm install
  build:
    commands:
      - npm run build
artifacts:
  files:
    - '**/*'
\`\`\`

## 4. Source Code Providers
- AWS CodeCommit
- GitHub / GitHub Enterprise
- Bitbucket
- Amazon S3
- GitHub Actions integration supported

## 5. Artifacts
- Store build output in:
  - Amazon S3
  - CodePipeline
- Can include single files, folders, or zipped bundles.

## 6. Logs
- Build logs are sent to:
  - Amazon CloudWatch Logs
  - Amazon S3 (optional)

---

# 🔐 Security Features

- **IAM roles**:
  - Execution role for CodeBuild to access resources.
  - Use policies for least privilege access.
- **KMS encryption**: For environment variables and logs.
- **VPC support**: Run builds in private VPC for secure access to internal services.

---

# 🔄 CI/CD Integration

## 1. With AWS CodePipeline
- CodeBuild acts as the build stage.
- Automatically picks up source and deploys artifacts.
- Supports parameter overrides, artifact inputs/outputs.

## 2. With GitHub
- Use webhooks to trigger builds on push.
- Monitor pull requests with build statuses.

## 3. With AWS CodeDeploy / Lambda
- Output from CodeBuild can be deployed directly via CodeDeploy.
- Or used to update Lambda function versions.

---

# 🧪 Testing & Reports

- **Test Reporting**:
  - Supports JUnit and Cucumber report formats.
  - Integrated test result visualization in AWS Console.
- **Code Coverage**:
  - With tools like `coverage.py`, `nyc`, etc.

---

# 🧠 Typical Use Cases

- Compiling and packaging software (Java, Node.js, Python, Go, etc.)
- Running automated tests
- Building Docker images (and pushing to ECR)
- CI workflows for microservices
- Build-and-deploy automation in DevOps pipelines

---

# 💵 Pricing

- Charged based on:
  - Build duration (in minutes)
  - Compute type used (e.g., `BUILD_GENERAL1_SMALL`, `LARGE`, `ARM`)
- Includes free tier: 100 build minutes/month on `BUILD_GENERAL1_SMALL`
- Refer to: [AWS CodeBuild Pricing](https://aws.amazon.com/codebuild/pricing/)

---

# 🏗️ Compute Types

| Type                   | vCPU | Memory (GiB) | Use Case                    |
|------------------------|------|--------------|-----------------------------|
| BUILD_GENERAL1_SMALL   | 3    | 3            | Basic builds                |
| BUILD_GENERAL1_MEDIUM  | 4    | 7            | Standard builds             |
| BUILD_GENERAL1_LARGE   | 8    | 15           | Heavy builds (e.g., Java)   |
| ARM variants           | 6    | 12           | ARM architecture builds     |

---

# ✅ Best Practices

- Use small images to reduce startup time.
- Cache dependencies (e.g., NPM, pip, Maven) to speed up builds.
- Encrypt everything (logs, artifacts, environment variables).
- Set up build badges for GitHub/Bitbucket repos.
- Limit build timeout to prevent runaway costs.
- Enable notifications with CloudWatch Events or Amazon SNS.

---

# 🛠️ Example Workflow

1. **Source**: CodeCommit repo is updated.
2. **Trigger**: CodePipeline invokes CodeBuild.
3. **Buildspec**: CodeBuild installs dependencies and compiles code.
4. **Test**: Unit tests are run, results published.
5. **Artifact**: Output stored in S3.
6. **Deploy**: Passed to CodeDeploy or Lambda.

---

# 🚫 Limitations

- No built-in GUI for pipelines (relies on CodePipeline).
- Some complex language/tool combinations may need custom images.
- Build environments have cold-start latency.
- Limited to AWS ecosystem (external runners not supported).

---

# 🔗 Helpful Resources

- [AWS CodeBuild Documentation](https://docs.aws.amazon.com/codebuild/)
- [Buildspec Reference](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)
- [Best Practices Guide](https://docs.aws.amazon.com/codebuild/latest/userguide/best-practices.html)
- [CI/CD Pipeline Sample](https://aws.amazon.com/blogs/devops/creating-a-ci-cd-pipeline/)