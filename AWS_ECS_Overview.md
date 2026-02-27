# 📘 What is Amazon ECS?

Amazon Elastic Container Service (ECS) is a fully managed container orchestration service that allows you to run, manage, and scale Docker containers on AWS infrastructure.

- Supports both serverless (Fargate) and managed EC2 launch types.
- Integrates deeply with AWS services (CloudWatch, IAM, ALB, ECR, etc.).
- Alternative to Kubernetes (but simpler to use for AWS users).

## 🧠 Key Concepts and Components

| Component         | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| Cluster          | Logical grouping of container instances or tasks                            |
| Task Definition  | Blueprint describing container(s), image, CPU, memory, and network settings |
| Task             | Instantiation of a task definition (running containers)                     |
| Service          | Maintains desired count of tasks and handles scheduling                     |
| Container Agent  | Installed on EC2 instances to register with ECS                             |
| Launch Types     | Determines compute platform: EC2 or Fargate                                 |
| Capacity Provider| Manages scaling of compute capacity                                         |

## 🚀 ECS Launch Types

### 1. Fargate (Serverless)
- No server or cluster management
- Pay per use (vCPU + memory)
- Suitable for most workloads
- Simplifies auto-scaling and isolation

### 2. EC2
- Use self-managed EC2 instances as compute
- More control over the environment (e.g., GPU, custom AMIs)
- Requires managing instance lifecycle and scaling

## 🧾 Task Definition Example

```json
{
  "family": "my-task",
  "containerDefinitions": [
    {
      "name": "web",
      "image": "my-image",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80
        }
      ],
      "memory": 512,
      "cpu": 256
    }
  ]
}
```

## 🧱 ECS Cluster Setup Options

| Option         | Description                            |
|----------------|----------------------------------------|
| Default (Fargate) | No provisioning; just run tasks     |
| EC2-backed     | Requires setting up autoscaling groups |
| ECS Anywhere   | Run ECS tasks on on-prem or other clouds |

## 🔧 ECS Service Scheduler

- Ensures desired number of tasks is always running.
- Handles task replacement on failure.
- Supports rolling updates and blue/green deployments (via CodeDeploy).

## 🔐 Security & Networking

### IAM Integration
- **Task Role**: Assigned to the container to access AWS services
- **Execution Role**: Used by ECS to pull images and log data

### Networking Modes
- `bridge`: Default for EC2
- `host`: High performance, EC2 only
- `awsvpc`: Assigns ENI per task — default for Fargate

### Security Groups
- Applied to tasks in awsvpc mode

## 📦 Storage

- **EFS volumes**: Shared storage across tasks
- **Ephemeral storage**: For temporary use during task execution
- **Docker volumes**: Used in EC2 mode

## 🔗 Integration with Other AWS Services

| Service            | Purpose                                  |
|--------------------|------------------------------------------|
| ECR                | Stores container images                  |
| ALB/NLB            | Load balances traffic across tasks       |
| CloudWatch         | Logs and metrics                         |
| CloudTrail         | API call auditing                        |
| IAM                | Secure access to AWS services            |
| Secrets Manager    | Manage secrets and configs               |
| CodeDeploy         | Automates blue/green deployments         |

## 📈 Monitoring and Observability

- **CloudWatch Logs**: Capture container logs
- **CloudWatch Metrics**: Task CPU, memory usage, etc.
- **AWS X-Ray**: Trace requests across microservices
- **Container Insights**: Advanced performance and health metrics

## 🧪 Deployment Strategies

- **Rolling Update (default)**: ECS replaces tasks one at a time
- **Blue/Green Deployment**:  
  - Requires AWS CodeDeploy  
  - Canary/test traffic supported  
  - Safer for production changes

## 📊 Cost Considerations

| Launch Type | Pricing Model                         |
|-------------|----------------------------------------|
| Fargate     | Pay per vCPU and memory used           |
| EC2         | Pay for provisioned instances (EC2 + EBS) |

## 🛠️ CLI and SDK Support

**Common CLI Commands**
```bash
aws ecs list-clusters
aws ecs describe-clusters --clusters my-cluster
aws ecs register-task-definition --cli-input-json file://taskdef.json
aws ecs run-task --launch-type FARGATE --cluster my-cluster ...
aws ecs update-service --service my-service --force-new-deployment
```

## ✅ Best Practices

- Use Fargate for simplicity unless deep control is needed.
- Secure your containers with IAM roles, VPCs, and SGs.
- Monitor using CloudWatch + Container Insights.
- Define resource limits (CPU/memory) to avoid contention.
- Keep task definitions versioned and documented.
- Use ALBs for fine-grained traffic routing.

## 📚 Helpful Resources

- [Amazon ECS Documentation](https://docs.aws.amazon.com/ecs/)
- [ECS Pricing](https://aws.amazon.com/ecs/pricing/)
- [ECS vs. EKS Comparison](https://aws.amazon.com/ecs/eks/)
- [ECS Workshop](https://ecsworkshop.com/)