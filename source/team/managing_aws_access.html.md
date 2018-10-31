# IAM

Each AWS account has [root account](http://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html) credentials, which we don't use.

Instead we access AWS resources using [IAM](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html).

## IAM Roles for VMs

We use [IAM roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) via [intance profiles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) on our EC2 instances to delegate particular restricted permissions to processes running on those instances.

## IAM Roles for Humans

We use separate IAM roles for users on our team to use via AWS' assume-role feature. See the [RE manual](https://reliability-engineering.cloudapps.digital/iaas.html#access-aws-accounts) for details on how to do this.

The available role ARNs that you'll need for this are as follows:

| AWS account | Role | ARN |
|---|---|---|
| dev | Admin | arn:aws:iam::<dev account ID>:role/Admin |
| dev | PowerUser | arn:aws:iam::<dev account ID>:role/PowerUser |
| dev | ReadOnly | arn:aws:iam::<dev account ID>:role/ReadOnly |
| ci | Admin | arn:aws:iam::<ci account ID>:role/Admin |
| ci | PowerUser | arn:aws:iam::<ci account ID>:role/PowerUser |
| ci | ReadOnly | arn:aws:iam::<ci account ID>:role/ReadOnly |
| staging | Admin | arn:aws:iam::<staging account ID>:role/Admin |
| staging | PowerUser | arn:aws:iam::<staging account ID>:role/PowerUser |
| staging | ReadOnly | arn:aws:iam::<staging account ID>:role/ReadOnly |
| prod | Admin | arn:aws:iam::<prod account ID>:role/Admin |
| prod | PowerUser | arn:aws:iam::<prod account ID>:role/PowerUser |
| prod | ReadOnly | arn:aws:iam::<prod account ID>:role/ReadOnly |

Once you're setup with access to the gds-users account, and have been granted permission to assume one or more of these roles, you can add a section like the following to your `~/.aws/config` file:

```
[profile dev]
source_profile = gds-users
role_arn = arn:aws:iam::<dev account ID>:role/Admin
mfa_serial = arn:aws:iam::<gds-users account ID>:mfa/<gds-users-username>
```

## Role configuration

We manage all these IAM roles, and the corresponding policies using Terraform. The config is in the [account-wide-terraform](https://github.com/alphagov/paas-aws-account-wide-terraform) repo. This includes defining who is allowed to assume the above roles.
