
# 📘 What is Amazon EC2?

Amazon EC2 (Elastic Compute Cloud) is a web service that provides resizable compute capacity in the cloud.

Allows launching virtual machines (called instances) with configurable CPU, memory, storage, and networking.

Supports multiple operating systems, architectures, and instance types.

Integrates with other AWS services for storage, security, monitoring, and auto-scaling.

## 🧠 Key Concepts
| Concept | Description |
|--------|-------------|
| Instance | A virtual server in EC2 |
| AMI (Amazon Machine Image) | Template for the root volume of an instance |
| Instance Type | Defines CPU, memory, storage, and network capacity |
| EBS (Elastic Block Store) | Persistent block-level storage for EC2 |
| Security Group | Virtual firewall that controls inbound/outbound traffic |
| Key Pair | Used for SSH access to Linux instances or RDP to Windows |
| Elastic IP | Static IPv4 address for dynamic cloud computing |
| Placement Group | Controls instance placement within the AWS infrastructure |
| EC2 Auto Scaling | Automatically scales instances up/down based on demand |

## 🛠️ EC2 Launch Workflow
1. **Select AMI** - Choose an OS image (Linux, Windows, custom).
2. **Choose Instance Type** - Select appropriate vCPU/memory/network combo (e.g., t3.micro, m5.large).
3. **Configure Instance** - Add networking, IAM role, user data scripts, shutdown behavior, etc.
4. **Add Storage** - Add or modify EBS volumes (root and additional).
5. **Configure Security Group** - Open required ports (e.g., 22 for SSH, 80 for HTTP).
6. **Launch** - Use a key pair to enable secure login (SSH or RDP).

## 📦 EC2 Instance Types
| Instance Family | Purpose | Examples |
|-----------------|---------|----------|
| General Purpose | Balanced CPU/memory | t3, t4g, m5, m6i |
| Compute Optimized | High performance CPU | c5, c6g |
| Memory Optimized | High memory workloads | r5, r6g, x1e |
| Storage Optimized | High disk throughput | i3, d3en |
| Accelerated Computing | GPUs / FPGAs | p4, inf1, g5 |

## 📂 EC2 Storage Options
### 1. EBS (Elastic Block Store)
Persistent, high-performance block storage.
- Supports gp3, io1/io2, sc1, and st1 volume types.

### 2. Instance Store (Ephemeral)
- Temporary storage that is lost when the instance stops or terminates.

### 3. EFS (Elastic File System)
- Scalable NFS file system that can be mounted to EC2 instances.

### 4. S3
- Object storage used for backups, static assets, etc.

## 🔐 EC2 Security
- **IAM Role**: Grant temporary AWS credentials to EC2 to access other services.
- **Security Group**: Stateful firewall, rules apply to inbound and outbound traffic.
- **Key Pair**: SSH or RDP access for users.
- **VPC/Subnets**: Define networking and isolation.
- **NACLs**: Optional stateless firewalls at subnet level.
- **Encryption**: EBS volumes, snapshots, and AMIs can be encrypted with KMS.

## 🧾 EC2 Pricing Models
| Model | Description |
|-------|-------------|
| On-Demand | Pay per second/minute with no long-term commitment |
| Reserved Instances | 1 or 3-year commitment with discount |
| Spot Instances | Buy unused capacity at up to 90% discount |
| Savings Plans | Flexible commitment-based discount model |
| Dedicated Hosts/Instances | Physical server isolation for compliance |

## 🔄 EC2 Auto Scaling
Launch or terminate instances automatically based on demand using Auto Scaling Groups (ASGs).

- Set min, max, desired capacity.
- Tie scaling policies to CloudWatch metrics (CPU, ALB requests, etc.).

## 🧪 EC2 Monitoring & Troubleshooting
| Tool | Purpose |
|------|---------|
| CloudWatch Metrics | Monitor CPU, disk, network, etc. |
| CloudWatch Logs | Collect logs via EC2 agents |
| Status Checks | Detect hardware/software issues |
| Elastic Load Balancer | Distributes traffic to instances |
| Systems Manager (SSM) | Automate tasks and access EC2 without SSH |

## 🧠 EC2 Best Practices
- Use latest AMIs for security and performance.
- Regularly rotate SSH keys and use IAM roles.
- Store critical data on EBS, enable backups.
- Monitor and alert on key metrics.
- Use Auto Scaling and Load Balancers.
- Tag resources for cost tracking.
- Use Spot Instances for fault-tolerant workloads.

## 🧾 Common EC2 CLI Commands
```bash
aws ec2 run-instances --image-id ami-12345678 --instance-type t2.micro --key-name MyKey --security-groups my-sg
aws ec2 describe-instances
aws ec2 stop-instances --instance-ids i-12345678
aws ec2 terminate-instances --instance-ids i-12345678
```

## 📚 Helpful Resources
- [Amazon EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [EC2 Pricing Calculator](https://calculator.aws.amazon.com/)
- [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)
- [EC2 Auto Scaling Guide](https://docs.aws.amazon.com/autoscaling/)
- [EC2 Best Practices](https://docs.aws.amazon.com/whitepapers/latest/ec2-best-practices/)
