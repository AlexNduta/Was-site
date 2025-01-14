# AWS: Identity and Access Management
**What is IAM?**

IAM stands for Identity and Access Management. It is a tool provided by cloud providers that you can use for user, role, and privilege management. This tool is very important as it can determine how vulnerable or strong your account can be. IAM is one of the main security tools provided by AWS as part of its shared responsibility model in the cloud.

**Usage of IAM**

1. **Manage IAM Users and Their Access:**

   * You have the ability to create users and assign security credentials such as access keys, passwords, and multi-factor authentication devices.
   * You can manage permissions to determine operations a user can perform.

2. **Manage IAM Roles and Their Permissions**

   * As the root user, you have the ability to assign roles to specific users. It is always advisable to assign the least privilege to users and only add more privileges upon request and verification. This reduces the attack surface.

**User Groups**

A user group consists of users that need to have access to the same data. Privileges can be set at the group level rather than giving them to an individual user. This feature gives more control over the policies we set and how we implement them. Giving a single user privileges is called an inline policy, but it's not the best practice. Attaching policies to groups reduces the load on the admin as it basically involves just adding and removing a user to a group.

**Managed Policies**

Managed policies are pre-built policies created by either AWS or your Admin.

* The policies are attached to an IAM user or user group.
* Making changes to a policy applies to all groups and users that are attached to the policy.
* Policies are in JSON format.
* It is advised to assign support users the ability to view but not modify the rules.

**Structure:**

* **Version:** Defines the version of the policy, this is normally the date the policy was created.
* **Statement:** This is an array of policies, enclosed in square brackets to indicate it's a list.
* **sid:** This is the identification name of the specific policy.
* **Effect:** Defines what is to be done, whether to allow or deny permissions.
* **Action:** Specifies the API call that can be made on an AWS service.
* **Principal:** Specifies the user who will have this policy applied to them.
* **Condition:** The condition in which the policy will be in effect.
* **Resource:** Defines the scope of entities covered by the policy rule.

An Admin policy has a customer inline policy, which is a policy that is assigned to one user or group.

```json
// This is an admin policy
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

The policy above is versioned for when it was created and gives administrative access to every resource. The `Effect` is marked as `Allow`, meaning all actions are allowed. `Action` is marked as '*', meaning all API calls will be allowed on all resources.

```json
// Policy to allow access to the s3 ivs resource
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::AWSIVS_*/ivs/*"
      ]
    }
  ]
}
```

The policy above allows putting objects to the `s3 ivs` resource.

```json
// Example of a custom policy
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VisualEditor0",
      "Effect": "Allow",
      "Action": [
        "iam:ListUsers",
        "iam:GetUser"
      ],
      "Resource": "*"
    }
  ]
}
```

The custom policy basically allows our users to list and get users in IAM by making the `ListUsers` and `GetUser` API calls.

**IAM Security**

Creating a user is the first step, and securing it is the next. With the AWS shared security model, AWS is more responsible for the security of the cloud, and they have a set of tools that an administrator can use to secure users. 
Some of these tools include multi-factor authentication (MFA) and password managers. But the most basic way to protect a user account is by coming up with effective password policies that can make it difficult for threat actors to try cracking the password.

Examples of password policies include:

* Set a minimum password length.
* Require specific character types such as numbers, upper & lowercase letters, and non-alphanumeric characters.
* Allow all IAM users to change their passwords.
* Require users to change their passwords after some time.
* Prevent password reuse.

**Multi-Factor Authentication (MFA)**

This involves using multiple forms of verification on top of the password to prove your identity. This could include using passkeys, email verification, or text verification.

**IAM Roles**

AWS services are expected to interact with each other or carry out tasks on our behalf. To do this, we use IAM Roles to assign permissions to users.

* Common roles include:
    * EC2 instance Roles
    * Lambda function Roles
    * Roles for CloudFormation

* Creating a role involves navigating to IAM -> Roles -> Create Role -> AWS Service.

* Roles define are in JSON formart.

```json
// Sample role for EC2
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "sts:AssumeRole"
      ],
      "Principal": {
        "Service": [
          "ec2.amazonaws.com"
        ]
      }
    }
  ]
}
```

* The role above tells AWS to let an EC2 service to `AssumeRole` when making API calls.

**IAM Best Practices**

1. Always create an IAM user with Administrator privileges to avoid using the root account for your day-to-day interaction in AWS.
2. Always set up MFA for all your users, including root and IAM users.
3. An IAM user is the same as a physical user. Avoid giving out root user credentials. Instead, create an IAM user for anyone who needs access.
4. For AWS services, use Roles to give permissions to specific services.
5. Generate Access keys when using the CLI or SDK. 
    * Access keys should be stored safely.


