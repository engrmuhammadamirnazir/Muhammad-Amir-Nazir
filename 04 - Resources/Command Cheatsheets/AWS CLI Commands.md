---
type: cheatsheet
tags: [commands, aws, cloud, s3, ec2, cheatsheet]
created: 2026-03-02
---

# AWS CLI Commands

---

## Configuration

```bash
aws configure
```

```bash
aws configure --profile myprofile
```

```bash
aws sts get-caller-identity
```
> Check who you're authenticated as

```bash
aws configure list
```

---

## S3 (Storage)

```bash
aws s3 ls
```
> List buckets

```bash
aws s3 ls s3://bucket-name/
```
> List objects in bucket

```bash
aws s3 ls s3://bucket-name/ --recursive --human-readable
```

```bash
aws s3 cp file.txt s3://bucket-name/
```
> Upload file

```bash
aws s3 cp s3://bucket-name/file.txt ./
```
> Download file

```bash
aws s3 cp s3://bucket-name/file.txt s3://other-bucket/
```
> Copy between buckets

```bash
aws s3 mv file.txt s3://bucket-name/
```

```bash
aws s3 rm s3://bucket-name/file.txt
```

```bash
aws s3 rm s3://bucket-name/ --recursive
```
> Delete all objects in bucket

```bash
aws s3 sync ./local-dir/ s3://bucket-name/
```
> Sync local to S3

```bash
aws s3 sync s3://bucket-name/ ./local-dir/
```
> Sync S3 to local

```bash
aws s3 sync ./local-dir/ s3://bucket-name/ --delete
```
> Sync and delete removed files

```bash
aws s3 mb s3://new-bucket-name
```
> Create bucket

```bash
aws s3 rb s3://bucket-name --force
```
> Delete bucket and all contents

```bash
aws s3 presign s3://bucket-name/file.txt --expires-in 3600
```
> Generate pre-signed URL (1 hour)

---

## EC2 (Instances)

```bash
aws ec2 describe-instances
```

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId,State.Name,PublicIpAddress,Tags[?Key=='Name'].Value|[0]]" --output table
```
> Clean table output

```bash
aws ec2 start-instances --instance-ids i-1234567890abcdef0
```

```bash
aws ec2 stop-instances --instance-ids i-1234567890abcdef0
```

```bash
aws ec2 reboot-instances --instance-ids i-1234567890abcdef0
```

```bash
aws ec2 terminate-instances --instance-ids i-1234567890abcdef0
```

```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --output table
```
> List running instances

```bash
aws ec2 describe-security-groups
```

```bash
aws ec2 describe-key-pairs
```

### Launch Instance
```bash
aws ec2 run-instances --image-id ami-12345678 --instance-type t2.micro --key-name mykey --security-group-ids sg-12345678 --count 1
```

---

## IAM (Users & Roles)

```bash
aws iam list-users
```

```bash
aws iam create-user --user-name newuser
```

```bash
aws iam create-access-key --user-name newuser
```

```bash
aws iam list-access-keys --user-name newuser
```

```bash
aws iam delete-access-key --user-name newuser --access-key-id AKIAIOSFODNN7EXAMPLE
```

```bash
aws iam attach-user-policy --user-name newuser --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
```

```bash
aws iam list-roles
```

```bash
aws iam list-groups
```

```bash
aws iam add-user-to-group --user-name newuser --group-name mygroup
```

---

## Lambda

```bash
aws lambda list-functions
```

```bash
aws lambda invoke --function-name myfunction output.json
```

```bash
aws lambda invoke --function-name myfunction --payload '{"key":"value"}' output.json
```

```bash
aws lambda update-function-code --function-name myfunction --zip-file fileb://function.zip
```

```bash
aws lambda get-function --function-name myfunction
```

```bash
aws lambda delete-function --function-name myfunction
```

---

## CloudFormation

```bash
aws cloudformation create-stack --stack-name mystack --template-body file://template.yaml
```

```bash
aws cloudformation update-stack --stack-name mystack --template-body file://template.yaml
```

```bash
aws cloudformation delete-stack --stack-name mystack
```

```bash
aws cloudformation describe-stacks --stack-name mystack
```

```bash
aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE
```

```bash
aws cloudformation validate-template --template-body file://template.yaml
```

---

## Route53 (DNS)

```bash
aws route53 list-hosted-zones
```

```bash
aws route53 list-resource-record-sets --hosted-zone-id Z1234567890
```

---

## Useful Flags

```bash
--region us-east-1
```

```bash
--output json|table|text
```

```bash
--query "expression"
```
> JMESPath query to filter output

```bash
--profile myprofile
```

```bash
--dry-run
```
> Preview without executing (EC2)

---

*See also: [[SSH & SCP Commands]] | [[Docker Commands]]*
