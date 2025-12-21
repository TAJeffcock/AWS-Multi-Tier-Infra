# AWS Multi-Tier Application Infrastructure with Terraform

[![Terraform](https://img.shields.io/badge/Terraform-1.6+-623CE4?logo=terraform&logoColor=white)](https://www.terraform.io/)
[![AWS](https://img.shields.io/badge/AWS-Cloud-FF9900?logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Maintained](https://img.shields.io/badge/Maintained-Yes-green.svg)](https://github.com/yourusername/aws-terraform-multitier-app)

A production-grade, multi-tier web application infrastructure built with Terraform on AWS. This project demonstrates Infrastructure as Code (IaC) best practices, AWS architectural patterns, and DevOps operational excellence across multiple environments.

## 🏗️ Architecture Overview

This infrastructure implements a highly available, scalable three-tier architecture:

```
┌─────────────────────────────────────────────────────────────┐
│                        Internet                              │
└────────────────────────┬────────────────────────────────────┘
                         │
                    ┌────▼────┐
                    │   ALB   │ (Application Load Balancer)
                    └────┬────┘
                         │
        ┌────────────────┼────────────────┐
        │                │                │
   ┌────▼────┐      ┌────▼────┐     ┌────▼────┐
   │   EC2   │      │   EC2   │     │   EC2   │  (Auto Scaling)
   └────┬────┘      └────┬────┘     └────┬────┘
        │                │                │
        └────────────────┼────────────────┘
                         │
              ┌──────────┴──────────┐
              │                     │
         ┌────▼────┐          ┌────▼────┐
         │   RDS   │          │  Cache  │
         │Multi-AZ │          │ (Redis) │
         └─────────┘          └─────────┘
```

### Key Components

- **Networking**: VPC with public/private subnets across multiple Availability Zones
- **Compute**: Auto Scaling Groups with Application Load Balancer
- **Database**: RDS Multi-AZ deployment with automated backups
- **Caching**: ElastiCache Redis cluster for performance optimization
- **Storage**: S3 buckets for static assets and application logs
- **Monitoring**: CloudWatch dashboards, alarms, and SNS notifications
- **Security**: Security groups, IAM roles, encryption at rest and in transit

## 🚀 Features

- ✅ **Multi-Environment Support**: Separate configurations for dev, staging, and production
- ✅ **High Availability**: Multi-AZ deployment across all critical components
- ✅ **Auto Scaling**: Dynamic scaling based on CPU and custom metrics
- ✅ **Security**: Principle of least privilege, network segmentation, encryption everywhere
- ✅ **Monitoring**: Comprehensive CloudWatch alarms and SNS notifications
- ✅ **Modular Design**: Reusable Terraform modules for easy maintenance
- ✅ **Cost Optimized**: Environment-specific resource sizing
- ✅ **Production Ready**: Backup strategies, disaster recovery, and operational excellence

## 📋 Prerequisites

- [Terraform](https://www.terraform.io/downloads.html) >= 1.6.0
- [AWS CLI](https://aws.amazon.com/cli/) configured with appropriate credentials
- AWS Account with appropriate permissions
- Basic understanding of AWS services and Terraform

## 🛠️ Project Structure

```
.
├── environments/
│   ├── dev/                    # Development environment configuration
│   ├── staging/                # Staging environment configuration
│   └── prod/                   # Production environment configuration
├── modules/
│   ├── networking/             # VPC, subnets, routing, NAT gateways
│   ├── compute/                # ALB, Auto Scaling Groups, Launch Templates
│   ├── database/               # RDS Multi-AZ, parameter groups
│   ├── cache/                  # ElastiCache Redis configuration
│   ├── storage/                # S3 buckets, lifecycle policies
│   ├── monitoring/             # CloudWatch dashboards, alarms, SNS
│   └── security/               # Security groups, IAM roles and policies
├── docs/
│   ├── architecture.md         # Detailed architecture documentation
│   ├── deployment.md           # Deployment procedures
│   └── troubleshooting.md      # Common issues and solutions
├── scripts/
│   ├── init.sh                 # Initialize Terraform backend
│   ├── plan.sh                 # Run terraform plan for environment
│   ├── apply.sh                # Apply infrastructure changes
│   └── destroy.sh              # Destroy infrastructure (with safeguards)
└── examples/
    └── terraform.tfvars.example # Example variable configurations
```

## 🚦 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/aws-terraform-multitier-app.git
cd aws-terraform-multitier-app
```

### 2. Configure AWS Credentials

```bash
aws configure
# OR export credentials
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
export AWS_DEFAULT_REGION="eu-west-"
```

### 3. Initialize Terraform

```bash
cd environments/dev
terraform init
```

### 4. Configure Variables

```bash
# Copy example variables file
cp ../../examples/terraform.tfvars.example terraform.tfvars

# Edit with your specific values
vim terraform.tfvars
```

### 5. Plan and Apply

```bash
# Review the execution plan
terraform plan

# Apply the infrastructure
terraform apply
```

## 📊 Cost Estimation

Estimated monthly costs per environment (us-east-1):

| Environment | Estimated Cost | Key Resources |
|-------------|----------------|---------------|
| **Dev**     | $50-100/month  | t3.micro instances, db.t3.micro, single NAT |
| **Staging** | $150-250/month | t3.small instances, db.t3.small, multi-AZ |
| **Prod**    | $300-500/month | t3.medium+ instances, db.m5.large, multi-AZ |

*Costs vary based on usage, data transfer, and enabled features.*

### Cost Optimization Tips

- Use AWS Cost Explorer for detailed breakdowns
- Implement auto-shutdown for dev environments during off-hours
- Utilize Reserved Instances for predictable workloads
- Monitor and optimize data transfer costs
- Review and adjust CloudWatch log retention policies

## 🔒 Security Features

- **Network Security**: Private subnets for application and database tiers
- **Encryption**: Encryption at rest (EBS, RDS, S3) and in transit (TLS)
- **IAM**: Least privilege access with role-based permissions
- **Security Groups**: Restrictive inbound/outbound rules
- **Secrets Management**: Integration with AWS Secrets Manager (documented)
- **Monitoring**: CloudWatch alarms for security-relevant metrics
- **Compliance**: Following AWS Well-Architected Framework principles

## 📈 Monitoring and Alerts

CloudWatch alarms configured for:

- High CPU utilization (>80%)
- Unhealthy target count in ALB
- Database connection exhaustion
- Cache hit ratio degradation
- HTTP 4xx/5xx error rates
- Auto Scaling events

SNS topics configured for:
- Critical alerts (immediate action required)
- Warning notifications (investigation needed)

## 🔄 CI/CD Integration

- GitHub Actions workflow for automated `terraform plan` on pull requests
- Automated testing and validation
- Cost estimation in PR comments
- Security scanning with tfsec/checkov

## 📚 Documentation

- [Architecture Details](docs/architecture.md) - Comprehensive component explanations
- [Deployment Guide](docs/deployment.md) - Step-by-step deployment procedures
- [Troubleshooting](docs/troubleshooting.md) - Common issues and solutions

## 🗺️ Roadmap

- [] Core networking infrastructure
- [] Compute layer with auto-scaling
- [] Database and caching layers
- [] Monitoring and alerting
- [] CI/CD pipeline integration
- [] Enhanced security with WAF
- [] Backup and disaster recovery automation
- [] Multi-region deployment support
- [] Container orchestration (ECS/EKS) option

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/TAJeffcock/aws-terraform-multitier-app/issues).

## 📝 License

This project is [MIT](LICENSE) licensed.

## 👤 Author

**Your Name**

- GitHub: [@TAJeffcock](https://github.com/TAJeffcock)
- LinkedIn: [LinkedIn](https://www.linkedin.com/in/thomas-jeffcock-32277b106/)

## 🙏 Acknowledgments

- AWS Well-Architected Framework for architecture guidance
- Terraform AWS Provider documentation
- HashiCorp best practices

## 📞 Support

If you have questions or need help with deployment, please:

1. Check the [documentation](docs/)
2. Review [troubleshooting guide](docs/troubleshooting.md)
3. Open an [issue](https://github.com/yourusername/aws-terraform-multitier-app/issues)

---

**⭐ If you find this project helpful, please consider giving it a star!**

Built with ☕ and Terraform