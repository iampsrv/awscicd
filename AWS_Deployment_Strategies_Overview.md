# 📘 Overview: What Are Deployment Strategies?

A deployment strategy defines how new application versions are released to users, aiming to reduce downtime, avoid failures, and support rollback if needed.

---

# 🚀 AWS CodeDeploy Deployment Strategies

AWS CodeDeploy supports multiple strategies depending on the target platform:

---

## 🔹 1. EC2 / On-Premises

### a. In-Place Deployment

- **How it works**: Stops the application on each instance, deploys the new version, and restarts it.
- **Pros**: Simple, cost-effective.
- **Cons**: Downtime during deployment; can't serve traffic during update.

### b. Blue/Green Deployment

- **How it works**: New version is deployed to a separate environment (green), tested, and then traffic is switched from old (blue) to new.
- **Pros**: Zero downtime, safe rollback.
- **Cons**: Requires load balancer and more infrastructure.
- Traffic shifting not native here — you use ALB/NLB with autoscaling for full blue/green control.

---

## 🔹 2. AWS Lambda

Lambda offers automated traffic shifting during deployments.

### a. All-at-Once
- Deploys the new version and shifts 100% of traffic immediately.
- **Use case**: Fast updates where risk is low.

### b. Canary Deployment
- Gradually shifts traffic (e.g., 10% → wait → 100%).
- **Options**:
  - `Canary10Percent5Minutes`
  - `Canary10Percent30Minutes`
- **Use case**: Safer rollouts with monitoring time.

### c. Linear Deployment
- Shifts traffic in equal increments over time.
- **Options**:
  - `Linear10PercentEvery1Minute`
  - `Linear10PercentEvery10Minutes`
- **Use case**: Controlled rollouts with multiple checkpoints.

---

## 🔹 3. Amazon ECS (Blue/Green)

ECS integrates with CodeDeploy for blue/green deployments.

- Old task set (blue) keeps running.
- New task set (green) is deployed and tested.
- After approval or test success, traffic shifts using an Application Load Balancer.

### Hooks available:
- `BeforeInstall`, `AfterInstall`
- `AfterAllowTestTraffic`, `BeforeAllowTraffic`, `AfterAllowTraffic`

### Strategy benefits:
- Zero downtime.
- Instant rollback if failure detected.
- Smooth container upgrades.

---

# 🌿 Elastic Beanstalk Deployment Policies

Elastic Beanstalk supports several environment deployment policies:

### a. All at Once
- Deploys the new version to all instances simultaneously.
- **Fast**, but causes **downtime**.

### b. Rolling
- Updates instances in batches.
- **Can cause brief downtime** per batch.

### c. Rolling with Additional Batch
- Launches extra batch to maintain capacity during deployment.
- **Zero downtime**.

### d. Immutable
- Launches a full new environment for the new version.
- Replaces old only after success.
- **Most reliable**, costlier.

### e. Blue/Green
- Manual process with environment cloning.
- Swap environment URLs after testing.

---

# 🔁 Deployment Strategies in CodePipeline

AWS CodePipeline doesn't define deployment strategies directly, but integrates with CodeDeploy, Lambda, or Elastic Beanstalk, letting you specify strategies in those services.

Example pipeline:
- Build with CodeBuild
- Deploy with CodeDeploy (specify strategy via `appspec.yml`)

---

# 📋 Strategy Comparison Table

| Strategy      | Services                     | Downtime | Rollback | Complexity | Ideal For       |
|---------------|------------------------------|----------|----------|------------|------------------|
| All-at-once   | Lambda, Beanstalk            | Yes      | No       | Low        | Small apps       |
| Rolling       | EC2, Beanstalk               | Some     | Partial  | Medium     | Mid-size apps    |
| Canary        | Lambda                       | No       | Yes      | Medium     | Production       |
| Linear        | Lambda                       | No       | Yes      | Medium     | Gradual rollout  |
| Blue/Green    | CodeDeploy, ECS, Beanstalk   | No       | Yes      | High       | Critical apps    |

---

# ✅ Best Practices

- Use **Blue/Green** or **Canary** for production.
- Monitor deployments with **CloudWatch alarms** and rollback on failure.
- Automate approvals in CodePipeline for safe production pushes.
- For Lambda and ECS, use hooks in `appspec.yml` to run tests during deployment.
- Avoid **All-at-once** unless downtime is acceptable.

---

# 🔗 Helpful Resources

- [CodeDeploy Deployment Types](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-steps.html)
- [Lambda Traffic Shifting](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations.html)
- [Elastic Beanstalk Deployment Policies](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html)
- [Amazon ECS Blue/Green Deployments](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-group-ecs-bluegreen.html)