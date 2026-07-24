# AWS Account Setup

This folder documents the initial setup of my AWS learning environment while preparing for the AWS Certified Solutions Architect – Associate (SAA-C03).

## Completed Setup

- Created an AWS account
- Secured the root account with MFA
- Created a Zero Spend Budget
- Created an IAM administrator user
- Created IAM access keys for AWS CLI usage
- Installed AWS CLI v2 on Windows
- Configured AWS CLI using `aws configure`
- Verified AWS CLI authentication using `aws sts get-caller-identity`

## AWS CLI Commands Used

```bash
aws configure
```

This command configures the AWS CLI using IAM user credentials.

### Verify AWS Identity

```bash
aws sts get-caller-identity
```

This command confirms the AWS account and IAM user currently authenticated.