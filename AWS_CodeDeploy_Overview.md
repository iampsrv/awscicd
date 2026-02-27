# 📘 Overview of AWS CodeDeploy

AWS CodeDeploy is a fully managed deployment service that automates the deployment of applications to various compute services such as:

- Amazon EC2
- AWS Lambda
- On-premises servers
- Amazon ECS (via blue/green deployments)

It helps maintain high availability, automates rollbacks, and minimizes downtime during software releases.

---

# ⚙️ Core Features

## 1. Deployment Types

### In-place (rolling) deployments:
- Application is stopped on each instance and updated in place.
- Used for EC2 and on-premises.

### Blue/Green deployments:
- New version is deployed to a replacement environment.
- Traffic is rerouted once validation is complete.
- Supported for Lambda, ECS, and EC2 (via load balancer).

## 2. Deployment Configurations
Defines how CodeDeploy deploys to instances:

### For EC2/on-prem:
- `CodeDeployDefault.AllAtOnce`
- `CodeDeployDefault.HalfAtATime`
- `CodeDeployDefault.OneAtATime`

### For Lambda:
- Canaries, linear, and all-at-once options.
- Custom configs supported for fine-tuned control.

## 3. Deployment Groups
A logical set of instances or Lambda functions targeted by deployments. Associated with:
- IAM roles
- Load balancers (optional)
- Auto Scaling groups
- Tag filters or target groups

---

# 🔌 Supported Platforms

| Platform | Description |
|----------|-------------|
| EC2      | Deploy to EC2 or on-prem servers (Agent required) |
| Lambda   | Shift traffic between versions automatically |
| ECS      | Blue/green deployments of ECS services |

---

# 🧩 Integration with Other AWS Services

- **CodePipeline**: Automates deployment as a stage in CI/CD pipelines.
- **CloudWatch Alarms**: Monitor deployments and trigger rollbacks.
- **IAM**: Controls access to deployments, groups, roles.
- **SNS**: Notifies on deployment status changes.
- **S3 / GitHub / Bitbucket / CodeCommit**: As sources for revision bundles.

---

# 📦 Application Revisions

Must be stored in:
- Amazon S3 (ZIP format)
- GitHub / Bitbucket (public/private)
- CodeCommit

Each revision includes an `appspec.yml` file that defines how deployment should proceed.

---

# 📄 The `appspec.yml` File

Defines the deployment lifecycle and hooks.

### Example for EC2:
\`\`\`yaml
version: 0.0
os: linux
files:
  - source: /
    destination: /home/ec2-user/app

hooks:
  BeforeInstall:
    - location: scripts/before_install.sh
      timeout: 180
      runas: ec2-user
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 180
      runas: ec2-user
\`\`\`

### Example for Lambda:
\`\`\`yaml
version: 0.0
Resources:
  - myLambda:
      Type: AWS::Lambda::Function
      Properties:
        Name: "MyFunction"
        Alias: "Live"
        CurrentVersion: "1"
        TargetVersion: "2"
\`\`\`

---

# 🔁 Lifecycle Events (EC2/On-Prem)

| Hook Name         | Description                            |
|-------------------|----------------------------------------|
| ApplicationStop   | Stop current app before deployment     |
| BeforeInstall     | Backup/clean up tasks                  |
| AfterInstall      | Install dependencies or configs        |
| ApplicationStart  | Start updated application              |
| ValidateService   | Verify deployment success (e.g., health check) |

---

# ✅ Best Practices

- Use lifecycle hooks to control the deployment process.
- Implement rollback strategies with CloudWatch alarms or scripts.
- Use Blue/Green for zero-downtime deployments.
- Log deployment activity using CloudWatch and CodeDeploy console.
- Automate with CodePipeline for full CI/CD workflows.
- Test deployments in lower environments first.

---

# 🧠 Typical Use Cases

- Updating EC2-based applications.
- Deploying serverless functions using Lambda traffic shifting.
- Coordinating ECS service updates with zero downtime.
- Automating hybrid on-premise deployments.
- Integrating with Git-based workflows for continuous delivery.

---

# 📊 Monitoring & Rollbacks

Deployment status, events, and logs visible in:
- CodeDeploy console
- Amazon CloudWatch

### Automatic rollback options:
- On failure
- On alarm trigger
- Manual rollback support available

---

# 💰 Pricing

- No additional charge for CodeDeploy itself.
- Pay only for underlying compute resources (EC2, Lambda, etc.).
- Free tier eligible for basic usage.

---

# 🚧 Limitations & Considerations

- EC2/On-prem requires CodeDeploy agent installed and running.
- Blue/green for EC2 requires use of load balancers.
- ECS and Lambda support only blue/green.
- Learning curve with `appspec.yml` and lifecycle hooks.

---

# 🔗 Helpful Resources

- [AWS CodeDeploy Documentation](https://docs.aws.amazon.com/codedeploy/)
- [appspec.yml Reference](https://docs.aws.amazon.com/codedeploy/latest/userguide/app-spec-ref.html)
- [CodeDeploy Best Practices](https://docs.aws.amazon.com/codedeploy/latest/userguide/best-practices.html)
- [CodeDeploy Agent GitHub](https://github.com/aws/aws-codedeploy-agent)