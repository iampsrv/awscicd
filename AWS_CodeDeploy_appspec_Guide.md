# 📘 What is appspec.yml?

The `appspec.yml` file is a YAML configuration file used by AWS CodeDeploy to:

- Specify deployment instructions (what to do and when).
- Define lifecycle event hooks and their associated scripts.
- Control how the application is deployed on EC2/on-prem, Lambda, or ECS platforms.

It's included in the application revision bundle, typically uploaded to Amazon S3 or pulled from CodeCommit/GitHub.

---

# 🧩 Structure Varies by Deployment Type

There are 3 versions of `appspec.yml`, depending on the deployment platform:

| Platform        | Use Case                             | Key Sections                   |
|------------------|--------------------------------------|--------------------------------|
| EC2/On-premises  | VM deployments (CodeDeploy agent required) | version, os, files, hooks     |
| Lambda           | Serverless deployments               | version, Resources, hooks      |
| ECS              | Containerized deployments on ECS     | version, Resources, hooks      |

---

# 🔹 1. EC2 / On-Premises appspec.yml

## ✅ Basic Structure

\`\`\`yaml
version: 0.0
os: linux
files:
  - source: /
    destination: /home/ec2-user/app
hooks:
  BeforeInstall:
    - location: scripts/before_install.sh
      timeout: 300
      runas: ec2-user
  AfterInstall:
    - location: scripts/after_install.sh
      timeout: 300
      runas: ec2-user
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: ec2-user
  ValidateService:
    - location: scripts/validate.sh
      timeout: 300
      runas: ec2-user
\`\`\`

## 🔸 Key Sections

- `version`: Always `0.0` for EC2/on-prem deployments.
- `os`: `linux` or `windows`.
- `files`:
  - `source`: Path inside revision archive.
  - `destination`: Where to copy on the instance.
- `hooks`: Define lifecycle event scripts.

## 🔸 Common Lifecycle Events

| Hook              | Purpose                                      |
|-------------------|----------------------------------------------|
| ApplicationStop   | Stop existing app processes (if needed)      |
| BeforeInstall     | Backup, delete files, or prep environment    |
| AfterInstall      | Set file permissions, configs, etc.          |
| ApplicationStart  | Start the app                                |
| ValidateService   | Health check, test endpoint, etc.            |

---

# 🔹 2. Lambda appspec.yml

Used to manage versioning and traffic shifting for Lambda functions.

## ✅ Example

\`\`\`yaml
version: 0.0
Resources:
  - myLambdaFunction:
      Type: AWS::Lambda::Function
      Properties:
        Name: MyFunction
        Alias: Live
        CurrentVersion: 1
        TargetVersion: 2
hooks:
  BeforeAllowTraffic:
    - location: scripts/before_traffic.sh
  AfterAllowTraffic:
    - location: scripts/after_traffic.sh
\`\`\`

## 🔸 Key Sections

- `Resources`: List of Lambda functions and traffic shifting properties.
  - Must specify `Name`, `Alias`, `CurrentVersion`, `TargetVersion`.
- `hooks`: Lifecycle event scripts (optional).

## 🔸 Lambda Lifecycle Hooks

| Hook                | When it Runs                         |
|---------------------|--------------------------------------|
| BeforeAllowTraffic  | Before routing traffic to new version|
| AfterAllowTraffic   | After routing traffic to new version |

---

# 🔹 3. ECS appspec.yml

Used to manage blue/green deployments in Amazon ECS (Fargate or EC2).

## ✅ Example

\`\`\`yaml
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: my-task-def
        LoadBalancerInfo:
          ContainerName: my-container
          ContainerPort: 80
hooks:
  BeforeInstall:
    - location: scripts/precheck.sh
  AfterAllowTestTraffic:
    - location: scripts/test_traffic.sh
\`\`\`

## 🔸 Key Sections

- `Resources`: Define the ECS service and task definition.
- `hooks`: Optional scripts to run during deployment.

## 🔸 ECS Lifecycle Hooks

| Hook                  | When It Runs                                  |
|-----------------------|-----------------------------------------------|
| BeforeInstall         | Before new task set is created                |
| AfterInstall          | After new task set is created                 |
| AfterAllowTestTraffic | After routing test traffic to new task set    |
| BeforeAllowTraffic    | Before full traffic is routed to new set      |
| AfterAllowTraffic     | After full traffic is routed to new set       |

---

# 🛡️ Permissions Required

Ensure CodeDeploy has IAM permissions to:
- Access S3, ECS, Lambda, or EC2
- Assume instance roles or deployment roles
- Run lifecycle event scripts (if accessing AWS services)

---

# ✅ Best Practices

- Use lifecycle hooks for custom tasks like config updates, service restarts, etc.
- Always test scripts locally or on a staging environment before deploying.
- Store scripts in a `scripts/` directory and reference them in the `appspec.yml`.
- Use log files or CloudWatch to troubleshoot hook failures.
- Separate responsibilities across hooks for clean, modular deployments.

---

# 🧪 Troubleshooting Tips

- **Deployment fails in ValidateService** → Check service health endpoint or log output.
- **Missing appspec.yml error** → Ensure it's in root of your revision bundle.
- **Hook scripts not running** → Verify script path, permissions, and runas user.
- **Use CodeDeploy logs**: Check `/opt/codedeploy-agent/deployment-root` on EC2 for details.

---

# 🔗 Helpful Resources

- [CodeDeploy appspec Reference](https://docs.aws.amazon.com/codedeploy/latest/userguide/app-spec-ref.html)
- [ECS Blue/Green Sample](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-group-ecs-bluegreen.html)
- [Lambda Deployment Sample](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-group-lambda.html)
- [CodeDeploy Logs](https://docs.aws.amazon.com/codedeploy/latest/userguide/troubleshooting.html)