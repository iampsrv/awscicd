
# 📘 AWS CloudWatch Overview

Amazon CloudWatch is a monitoring, logging, and observability service for AWS cloud resources and applications. It provides metrics, logs, alarms, dashboards, and events to help you monitor and manage infrastructure, improve system performance, and reduce downtime.

## 🎯 Primary Use Cases

- Monitoring EC2 instances, Lambda functions, ECS tasks, etc.
- Visualizing custom and AWS service metrics
- Tracking application logs and system logs
- Setting alarms on thresholds and anomalies
- Automating incident response
- Performing root cause analysis

## 🔧 Core Components of CloudWatch

### 1. Metrics
- Numerical data points representing system or application performance.
- Examples: `CPUUtilization`, `MemoryUsage`, `Latency`, `RequestCount`
- Granularity: high-resolution (1s) or standard (1m/5m)

**Sources:**
- AWS Services (EC2, RDS, Lambda, etc.)
- Custom metrics (via API or CLI)
- Application metrics (e.g., from Prometheus)

### 2. Logs
- Collects and stores logs from EC2, Lambda, ECS, on-premises apps
- Real-time ingestion, search, filter, metric filters

### 3. Dashboards
- Custom interactive metric visualizations
- Supports line graphs, text widgets, cross-account dashboards

### 4. Alarms
- Monitor metrics against thresholds
- States: `OK`, `ALARM`, `INSUFFICIENT_DATA`
- Trigger: SNS, Auto Scaling, Lambda

### 5. Events (EventBridge)
- Real-time routing of AWS and custom events
- Examples: EC2 state changes, Auto Scaling, custom events

### 6. CloudWatch Synthetics
- Simulates user behavior using canaries (browser scripts)
- Monitors endpoints, APIs

### 7. Application Insights
- Automated monitoring and diagnostics

### 8. ServiceLens
- Full-stack observability using X-Ray integration

## 🔐 Security and Access Control

- IAM for user/role permissions
- KMS for encrypting logs
- Resource policies for cross-account access

## 🧠 Common AWS Metrics

| Service | Key Metrics |
|--------|--------------|
| EC2 | CPUUtilization, DiskReadOps, NetworkIn |
| RDS | DatabaseConnections, FreeStorageSpace |
| Lambda | Invocations, Duration, Errors |
| ECS | CPUUtilization, MemoryUtilization |
| ALB/ELB | RequestCount, TargetResponseTime, HTTPCode_ELB_5XX |

## 🛠️ Sending Custom Metrics

```bash
aws cloudwatch put-metric-data \
  --namespace "MyApp" \
  --metric-name "PageLoadTime" \
  --value 2.7 \
  --unit Seconds
```

## 🧪 Metric Math

```text
AVG(CPUUtilization) + AVG(MemoryUtilization)
```

## 🧾 Sample Alarm Setup (CLI)

```bash
aws cloudwatch put-metric-alarm \
  --alarm-name "HighCPUAlarm" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2 \
  --alarm-actions arn:aws:sns:region:account-id:my-topic
```

## 🔍 CloudWatch Logs Insights (Query Logs)

```sql
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20
```

## 🔁 Integration with Other Services

| Integrated With | Purpose |
|-----------------|---------|
| AWS Lambda | Trigger on alarms |
| SNS | Notifications |
| Auto Scaling | Scaling triggers |
| CloudTrail | API monitoring |
| EventBridge | Workflow automation |
| Systems Manager | Run automation documents |

## 📋 Pricing Overview

- Metrics: Free defaults; custom billed per metric/month
- Logs: Based on ingestion + storage
- Alarms: Per alarm (high-resolution billed separately)
- Synthetics & ServiceLens: Extra cost

[Full Pricing](https://aws.amazon.com/cloudwatch/pricing/)

## ✅ Best Practices

- Group metrics in namespaces
- Use high-resolution metrics where needed
- Extract metrics from logs using filters
- Use composite alarms to reduce noise
- Encrypt logs with KMS
- Create status dashboards
- Set log retention policies

## 📚 Helpful Resources

- [AWS CloudWatch Docs](https://docs.aws.amazon.com/cloudwatch/)
- [Metric and Alarm Reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html)
- [Logs Insights Query Syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html)
- [CloudWatch Agent Setup Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html)
