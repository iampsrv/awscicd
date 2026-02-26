# 📘 What is AWS IAM?

AWS IAM (Identity and Access Management) is a global AWS service that allows you to:

- Securely manage access to AWS services and resources.
- Create and manage users, groups, roles, and policies.
- Enforce the principle of least privilege through permissions.

IAM helps ensure secure, granular, and auditable control over who can do what in your AWS environment.

## 🧱 Core IAM Concepts

| Concept    | Description |
|------------|-------------|
| **User**   | An individual identity with long-term credentials (login/password, access keys). |
| **Group**  | A collection of IAM users. Policies attached to the group apply to all members. |
| **Role**   | An identity with temporary credentials, assumed by users, applications, or services. |
| **Policy** | A JSON document that defines permissions (what actions are allowed/denied). |
| **Principal** | An entity (user, role, service) that can make a request in AWS. |
| **Resource** | The AWS entity (e.g., S3 bucket, EC2 instance) being accessed. |
| **Action** | The specific operation being performed (e.g., s3:PutObject). |

## 🔐 Types of IAM Policies

| Type                | Description |
|---------------------|-------------|
| Identity-based      | Attached to IAM users, groups, or roles. |
| Resource-based      | Attached directly to AWS resources (e.g., S3 bucket policy, Lambda permission). |
| Permissions boundaries | Limits the max permissions a role or user can get. |
| Service Control Policies (SCPs) | Organization-wide permissions control (used in AWS Organizations). |
| Session policies    | Passed during role assumption to limit permissions temporarily. |

## 🧾 Example IAM Policy (S3 Read-Only)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::my-bucket",
        "arn:aws:s3:::my-bucket/*"
      ]
    }
  ]
}
```

## 👤 IAM Users

- Represents a single person or application.
- Can have:
  - Console password
  - Access keys (for CLI/API access)
- Should be avoided for programmatic access; use IAM roles instead.
- MFA (Multi-Factor Authentication) is highly recommended.

## 👥 IAM Groups

- Logical grouping of users.
- Policies attached to the group apply to all members.
- Good for managing permissions for teams (e.g., Admins, Developers).

## 🎭 IAM Roles

- Not associated with a user or group.
- Can be assumed temporarily by:
  - AWS services (e.g., EC2, Lambda)
  - IAM users (for switching roles)
  - External identities (federation/SAML)
- Uses STS (Security Token Service) to provide temporary credentials.

## 🔐 IAM Best Practices

- Enable MFA for all accounts.
- Use IAM roles instead of long-term credentials.
- Apply least privilege principle — give only the permissions needed.
- Use groups to assign permissions to users.
- Avoid using the root account for daily tasks.
- Rotate access keys regularly and audit their usage.
- Use IAM Access Analyzer to identify unused/over-permissive policies.
- Log and monitor IAM activity with CloudTrail.

## 📈 Monitoring IAM

- **AWS CloudTrail**: Records all IAM-related API calls.
- **IAM Access Analyzer**: Identifies resources shared outside the account.
- **Credential Reports**: Lists all users and the status of their credentials.
- **Access Advisor**: Shows services that a user/role has accessed.

## 🤝 IAM with AWS Services

| Service | IAM Usage |
|---------|-----------|
| EC2 | Use IAM roles to allow EC2 to access S3, DynamoDB, etc. |
| Lambda | Each Lambda function has an execution role with permissions. |
| S3 | Use IAM and bucket policies to control access. |
| STS | Supports temporary access to AWS via AssumeRole. |

## 🔗 IAM Policy Structure
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "OptionalStatementID",
      "Effect": "Allow",
      "Action": "service:operation",
      "Resource": "arn:aws:service:::resource",
      "Condition": {
        "StringEquals": {
          "aws:username": "alice"
        }
      }
    }
  ]
}
```

## 🧠 Common IAM Scenarios

| Scenario | IAM Feature Used |
|----------|------------------|
| Developer accesses S3 via CLI | IAM user + access key |
| EC2 instance accesses S3 | IAM role assigned to instance |
| External user logs in via Google | IAM identity federation |
| Automated pipeline deploys to ECS | IAM role assumed by CodePipeline |
| Prevent user from deleting resources | Deny policy or SCP |

## 🔐 IAM and Security Tools

- **AWS Config**: Monitors IAM policy changes.
- **Security Hub**: Evaluates IAM posture (e.g., open policies).
- **GuardDuty**: Detects anomalies like suspicious IAM activity.

## 🧮 IAM Quotas (Limits)

| Item | Default |
|------|---------|
| Users per account | 5,000 |
| Groups per account | 300 |
| Roles per account | 1,000 |
| Inline policies per user/group/role | 10 |

(Quotas can be increased by request.)

## 📚 Helpful Resources

- [AWS IAM Documentation](https://docs.aws.amazon.com/iam/)
- [IAM Policy Generator](https://awspolicygen.s3.amazonaws.com/policygen.html)
- [IAM Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer.html)
- [Best Practices for IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)