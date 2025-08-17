# Testing IAM User Permissions Lab

## Introduction

This README documents the steps and outcomes of the "Testing IAM User Permissions Lab." The purpose of this lab is to demonstrate how to manually create and manage AWS Identity and Access Management (IAM) resources and test their permissions using the AWS Cloud Shell and a local machine with the AWS Command Line Interface (CLI).

## Prerequisites

- Active AWS account with administrative privileges
- AWS CLI installed on local machine
- Access to AWS Cloud Shell
- Basic understanding of IAM concepts

## Lab Tasks

### Task 1: Create IAM User and Attach S3 Policy (via AWS Cloud Shell)

**Objective:** Create a new IAM user with a set of credentials and permissions to manage S3 buckets.

#### Commands Used:

1. **Create IAM user:**
   ```bash
   aws iam create-user --user-name <your-new-user-name>
   ```

2. **Create access keys:**
   ```bash
   aws iam create-access-key --user-name <your-new-user-name>
   ```

3. **Attach S3 access policy:**
   ```bash
   aws iam attach-user-policy \
       --user-name <your-new-user-name> \
       --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
   ```

### Task 2: Test Permissions (via AWS CLI on Local Machine)

**Objective:** Configure a local machine to use the new user's credentials and verify S3 permissions while demonstrating a lack of permissions for another service (EC2).

#### Commands Used:

1. **Create a new CLI profile:**
   ```bash
   aws configure --profile <your-new-user-name>
   ```
   > **Note:** This command will prompt you to enter the Access Key ID, Secret Access Key, default region, and output format.

2. **Verify logged-in user identity:**
   ```bash
   aws sts get-caller-identity --profile <your-new-user-name>
   ```

3. **List S3 buckets (Success):**
   ```bash
   aws s3 ls --profile <your-new-user-name>
   ```

4. **Create S3 bucket (Success):**
   ```bash
   aws s3 mb s3://<your-unique-bucket-name> --profile <your-new-user-name>
   ```

5. **List EC2 instances (Failure/Access Denied):**
   ```bash
   aws ec2 describe-instances --profile <your-new-user-name>
   ```

### Task 3: Modify Permissions (Challenge)

**Objective:** Modify the user's permissions to allow viewing of EC2 instances and verify the change.

#### Commands Used:

1. **Attach EC2 access policy:**
   ```bash
   aws iam attach-user-policy \
       --user-name <your-new-user-name> \
       --policy-arn arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess
   ```

2. **List EC2 instances again (Success):**
   ```bash
   aws ec2 describe-instances --profile <your-new-user-name>
   ```
   > **Note:** The command should now return a successful output, proving the permission change was effective.

## Learning Outcomes

By completing this lab, you will have demonstrated:

- Creating IAM users programmatically
- Managing access keys securely
- Attaching managed policies to users
- Testing permissions using AWS CLI profiles
- Understanding the principle of least privilege
- Modifying user permissions and validating changes

## Deliverables

Submit the following screenshots as evidence of lab completion:

- **Full_Name_Image_1:** Screenshot of `aws iam create-user` command output
- **Full_Name_Image_2:** Screenshot of `aws iam create-access-key` command output
- **Full_Name_Image_3:** Screenshot of `aws configure` command line output
- **Full_Name_Image_4:** Screenshot of `aws sts get-caller-identity` command output
- **Full_Name_Image_5:** Screenshot of `aws s3 ls` command output
- **Full_Name_Image_6:** Screenshot of `aws s3 mb` command output
- **Full_Name_Image_7:** Screenshot of `aws ec2 describe-instances` command output showing Access Denied
- **Full_Name_Image_8:** Screenshot of `aws ec2 describe-instances` command output showing successful permission change

## Security Best Practices

- Always follow the principle of least privilege when assigning permissions
- Regularly rotate access keys
- Use IAM roles instead of users when possible for applications
- Enable MFA for sensitive operations
- Monitor and audit IAM activities using CloudTrail

## Cleanup

After completing the lab, remember to:

1. Delete the test S3 bucket
2. Remove the IAM user's access keys
3. Delete the IAM user
4. Remove the AWS CLI profile from your local machine

```bash
# Cleanup commands
aws s3 rb s3://<your-unique-bucket-name> --profile <your-new-user-name>
aws iam delete-access-key --user-name <your-new-user-name> --access-key-id <access-key-id>
aws iam detach-user-policy --user-name <your-new-user-name> --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
aws iam detach-user-policy --user-name <your-new-user-name> --policy-arn arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess
aws iam delete-user --user-name <your-new-user-name>
```

## Troubleshooting

Common issues and solutions:

- **Access Denied errors:** Verify the correct policy is attached and permissions have propagated
- **Invalid credentials:** Ensure access keys are correctly configured in the CLI profile
- **Bucket name conflicts:** S3 bucket names must be globally unique
- **Region mismatches:** Ensure consistent region configuration across commands