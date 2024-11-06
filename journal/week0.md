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

---

## Enable billing
We need to turn on billing alerts to receive such alerts:

- in Root account go to billing page
- under `Billing Preferences` in section `Alert Preferences` choose `Receive AWS Free Tier Alerts`
- save preferences

---

## Create AWS SNS topic
SNS is AWS's pub/sub message que service. To create a topic
```
aws sns create-topic --name {topic-name}
```

Returns the topic ARN.

---

## Create AWS Budget

Get AWS account ID:
```
AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
```

To create the budget using CLI:
```
aws budgets create-budget \
    --account-id $AWS_ACCOUNT_ID \
    --budget file://aws/json/budget.json \
    --notifications-with-subscribers file://aws/json/budget-notifications-with-subscribers.json
```

We can use an SNS topic and subscribe to it to get the alerts:

```
TOPIC_ARN=$(aws sns create-topic --name billing-alarm --query "TopicArn" --output text)
```

To set up the subscription:

```
aws sns subscribe \
    --topic-arn=$TOPIC_ARN
    --protocol email
    --notification-endpoint {email-address}
```
---

## CloudWatch alarm
The CloudWatch component completes the cycle to actually receive the alerts (?)

```
aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm-config.json
```

---

# Week0 - Cloud Security - Best practices from day 0

**Goal of security**: Identify for and inform the business any technical risk that the business may be exposed to.

- reducing impact of breach
- protect against data breaches / theft
- reducing human error (responsible for data leaks)

## Why it needs practice?
- complexity
- always chasing our tail (new services)
- hackers are improving their game (AI)

## Useful steps and actions
1. MFA for root user (!) -> IAM
2. Create organization unit -> AWS Organizations
  - how many types of OU?
  - account management
  - automate vending accounts (designated owner)
3. AWS Cloud Trail -> incurrs costs
  - monitor data security & residence
  - understand region vs. availability zone (sub-network in region) vs. global services concepts
  - audit logs for IR/Forensics
4. Create IAM users
  - 3 kinds of users: human user, system users
5. IAM Roles
  - 2 types of IAM roles and policies
  - policy can be attached to groups;
  - principle of least privilidge (!)
6. Enable AWS organization SCP -> AWS Organizations
  - SCP = service control policies
