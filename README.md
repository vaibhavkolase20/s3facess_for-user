# *AWS S3 IAM Policy Guide*

## *Overview*
This repository provides a guide on how to configure an *AWS IAM policy* that allows a specific user to *list all S3 buckets* but grants access to only *one specific bucket*. This is useful for managing permissions while maintaining security.

## *Features*
- ✅ *List all S3 buckets* available in the AWS account
- ✅ *Allow access to a specific bucket* for a designated user
- ✅ *Prevent access to other buckets*
- ✅ *Step-by-step guide using AWS Policy Generator*

## *Prerequisites*
- *AWS Account*
- *IAM user* for whom the policy will be applied
- *AWS S3 bucket* (e.g., dipak-cronjob)

## *Steps to Create IAM Policy Using AWS Policy Generator*

### *Step 1: Open AWS Policy Generator*
1. Go to *[AWS Policy Generator](https://awspolicygen.s3.amazonaws.com/policygen.html)*
2. Select *IAM Policy*

### *Step 2: Add Permissions to List All Buckets*
1. *AWS Service* → Select *Amazon S3*
2. *Actions* → Select s3:ListAllMyBuckets
3. *ARN* → Enter *
4. Click *Add Statement*

### *Step 3: Add Permissions to Access a Specific Bucket*
1. *AWS Service* → Select *Amazon S3*
2. *Actions* → Select s3:ListBucket
3. *ARN* → Enter arn:aws:s3:::dipak-cronjob
4. Click *Add Statement*

### *Step 4: Generate and Attach IAM Policy*
1. Click *Generate Policy*
2. Copy the generated JSON policy
3. In AWS Console, go to *IAM* > *Policies*
4. Click *Create Policy, select **JSON*, and paste the policy
5. Click *Review Policy*, name it (e.g., S3-ListAll-AccessOne), and save it
6. Attach this policy to the IAM user (shivam)

## *IAM Policy JSON*
json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:ListAllMyBuckets",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::dipak-cronjob"
        }
    ]
}


## *(Optional) Add Bucket Policy for Cross-Account Access*
If the IAM user (shivam) is in *another AWS account, use a **bucket policy*:

### *Steps to Create the Bucket Policy*
1. Open *AWS Policy Generator*
2. *Select Policy Type* → Choose *S3 Bucket Policy*
3. *AWS Service* → Select *Amazon S3*
4. *Actions* → Select s3:ListBucket
5. *ARN* → Enter arn:aws:s3:::dipak-cronjob
6. *Principal* → Enter IAM user ARN:
   
   arn:aws:iam::149536472881:user/shivam
   
7. Click *Add Statement, then **Generate Policy*
8. Copy the policy and paste it in *S3 Console > Permissions > Bucket Policy*

### *Bucket Policy JSON*
json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::149536472881:user/shivam"
            },
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::dipak-cronjob"
        }
    ]
}


## *Testing Permissions*
Run the following *AWS CLI* commands to verify permissions:

1️⃣ *Check all available buckets:*
sh
aws s3 ls


2️⃣ **Try listing objects in the dipak-cronjob bucket:**
sh
aws s3 ls s3://dipak-cronjob


3️⃣ *Try listing objects in another bucket (should be denied):*
sh
aws s3 ls s3://other-bucket


## *Conclusion*
This guide provides step-by-step instructions to configure an *AWS IAM policy* that allows a user to *list all S3 buckets* but restricts access to *a specific one. If **cross-account access* is needed, a *bucket policy* should be applied.

---

