Testing IAM User Permissions Lab
Introduction
This README documents the steps and outcomes of the "Testing IAM user permissions Lab." The purpose of this lab is to demonstrate how to manually create and manage AWS Identity and Access Management (IAM) resources and test their permissions using the AWS Cloud Shell and a local machine with the AWS Command Line Interface (CLI).
Lab Tasks
Task 1: Create IAM User and Attach S3 Policy (via AWS Cloud Shell)
Objective: Create a new IAM user with a set of credentials and permissions to manage S3 buckets.
Commands Used:
Create IAM user:
aws iam create-user --user-name <your-new-user-name>


Create access keys:
aws iam create-access-key --user-name <your-new-user-name>


Attach S3 access policy:
aws iam attach-user-policy \
    --user-name <your-new-user-name> \
    --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess


Task 2: Test Permissions (via AWS CLI on Local Machine)
Objective: Configure a local machine to use the new user's credentials and verify S3 permissions while demonstrating a lack of permissions for another service (EC2).
Commands Used:
Create a new CLI profile:
aws configure --profile <your-new-user-name>


Note: This command will prompt you to enter the Access Key ID, Secret Access Key, default region, and output format.
Verify logged-in user identity:
aws sts get-caller-identity --profile <your-new-user-name>


List S3 buckets (Success):
aws s3 ls --profile <your-new-user-name>


Create S3 buckets (Success):
aws s3 mb s3://<your-unique-bucket-name> --profile <your-new-user-name>


List EC2 instances (Failure/Access Denied):
aws ec2 describe-instances --profile <your-new-user-name>


Task 3: Modify Permissions (Challenge)
Objective: Modify the user's permissions to allow viewing of EC2 instances and verify the change.
Commands Used:
Attach EC2 access policy:
aws iam attach-user-policy \
    --user-name <your-new-user-name> \
    --policy-arn arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess


List EC2 instances again (Success):
aws ec2 describe-instances --profile <your-new-user-name>


Note: The command should now return a successful output, proving the permission change was effective.
Deliverables
Full_Name_Image_1: Screenshot of aws iam create-user command output.
Full_Name_Image_2: Screenshot of aws iam create-access-key command output.
Full_Name_Image_3: Screenshot of aws configure command line output.
Full_Name_Image_4: Screenshot of aws sts get-caller-identity command output.
Full_Name_Image_5: Screenshot of aws s3 ls command output.
Full_Name_Image_6: Screenshot of aws s3 mb command output.
Full_Name_Image_7: Screenshot of aws ec2 describe-instances command output showing Access Denied.
Full_Name_Image_8: Screenshot of aws ec2 describe-instances command output showing successful permission change.
