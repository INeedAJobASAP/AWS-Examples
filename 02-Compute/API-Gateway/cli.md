# AWS API & CLI Examples

## Check AWS CLI Version

```bash
aws --version
```

---

## Configure AWS CLI

```bash
aws configure
```

Configure:

```text
AWS Access Key ID
AWS Secret Access Key
Default region name
Default output format
```

---

## List Configured Profiles

```bash
aws configure list-profiles
```

---

## Use a Specific Profile

```bash
aws s3 ls --profile exampro
```

---

## View Current CLI Configuration

```bash
aws configure list
```

For a specific profile:

```bash
aws configure list --profile exampro
```

---

## Set the Default Region

```bash
aws configure set region us-east-1
```

For a specific profile:

```bash
aws configure set region ca-central-1 --profile exampro
```

---

## Set the Default Output Format

```bash
aws configure set output json
```

---

## Use Environment Variables

```bash
export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY_ID
export AWS_SECRET_ACCESS_KEY=YOUR_SECRET_ACCESS_KEY
export AWS_DEFAULT_REGION=us-east-1
```

> Avoid placing real credentials directly into scripts or source code.

---

## EC2 API Example

```bash
aws ec2 describe-instances
```

The AWS CLI sends the corresponding API request to the Amazon EC2 service.

---

## Filter EC2 Instances

```bash
aws ec2 describe-instances \
  --filters Name=tag-key,Values=Name
```

---

## Query API Response

```bash
aws ec2 describe-instances \
  --query 'Reservations[*].Instances[*].{Instance:InstanceId,AZ:Placement.AvailabilityZone,Name:Tags[?Key==`Name`]|[0].Value}'
```

---

## Format Output as a Table

```bash
aws ec2 describe-instances \
  --query 'Reservations[*].Instances[*].{Instance:InstanceId,AZ:Placement.AvailabilityZone,Name:Tags[?Key==`Name`]|[0].Value}' \
  --output table
```

---

## Available Output Formats

```bash
--output json
--output yaml
--output text
--output table
```

Example:

```bash
aws ec2 describe-instances --output json
```

---

## Use a Profile with an AWS Command

```bash
aws ec2 describe-instances \
  --profile exampro
```

---

## Verify AWS Identity

```bash
aws sts get-caller-identity
```

This is useful for verifying which AWS identity the CLI is currently using.
