# Identity and Access Management (IAM)

Service to manage access to AWS resource.

* Every user has access rights
* Users/Groups/Permissions/Roles
* Reliability
* Free

## Users

Good practice when you 1st time AWS access.
Go to IAM, create new root user and use it this user only.

* Users - all accounts. p.s. it's better to create root-company user

## Security credentials

Go to `IAM -> Users -> Security credentials`

Create access key

It generates access key id and secret access key

## Add user

Key | Value | Notes
--- | ----- | -----
Name | SampleUser | 
AWS access type | `Programmatic access`, `AWS Management Console access` | Access id and secret key for AWS API. Or ability to sign in 
Console password | ****** |

### Add user to group

* Create group
* My Custom group | My Policies

### Set permissions for SampleUser

Policies:
* Create policy - ability to create your own access policy
* AmazonEC2FullAccess
* AmazonEC2ContainerRegistryFullAccess
* AmazonS3ReadOnlyAccess
* ...

```
statement: [
  {
    Action: "ec2:*",
    Effect: "Allow",
    Resource: "*"
  },
  {
    Effect: "Allow",
    Action: "elasticloadbalancing:*",
    Resource: "*"
  },
  {
    Effect: "Allow",
    Action: [
      "s3:PutObject",
      "s3:GetObject",
    ],
    Resource: "arn:aws:s3:::example_bucket/example?folder/*"
  }
  ...
]
```

*If you want to change resource policy, search for ARN*

## Roles

Role is a current access. User can have temporal credentials.

Example:
I want to allow EC2 termination only for user with role `Terminator`.
I want to access my own resources to business partners temporarily. No new access, no share my credentials.

## Create role

### Select type of trusted entity

* AWS service (EC2, Lambda, others). e.g. Only machine with role Deployer can deploy to EC2
* Another AWS account. Input account ID. (`Chosen`)
* Web identity. `Cognito` or `OpenID` provider.
* Saml 2.0 federation. SSO integration.

### Attach permission policies

* S3FullAccess

### Review

Key | Value | Notes
--- | ----- | -----
Name | MyCustomRole | 
Trusted entities | 131231.. | Id of user

### Summary

Key | Value | Notes
--- | ----- | -----
Role APN | arn:aws:iam:131231.. |

### Create policy to role

Visual Editor | JSON

Key | Value | Notes
--- | ----- | -----
Actions | Write Assume role |
Resources | All resources |

### Give this link to users who can switch the role

Go to `IAM -> Roles -> MyCustomRole`. 
Follow the link in `Give this link to users who can switch to the role`.
Now you have `MyCustomRole` :smile:

