# Week 0 — Billing and Architecture

## Create AWS user
There is a difference in creating an IAM user and a user in the Identity Center. According to AWS it is recommended to use IAM users for programmatic access and users in the Identity Center for general access rights. For this bootcamp, I will use the IAM user as it gets access to the AWS dashboard. Identity Center users seem to have only limited view on applications that get assigned (--> need more time to understand how this works).

## Setting environment variables in AWS 

Relevant env vars to identify a user in AWS are:
```
export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
export AWS_DEFAULT_REGION=us-west-2
```

Using `aws sts get-caller-identity` then displays the user's information (if valid).
