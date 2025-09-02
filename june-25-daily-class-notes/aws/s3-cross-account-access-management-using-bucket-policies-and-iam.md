# S3 Cross-Account Access Management Using Bucket Policies and IAM

Managing access to Amazon S3 buckets across AWS accounts is a common requirement in multi-account architectures. AWS provides several tools and mechanisms to grant access, including bucket policies, IAM roles, and access control lists (ACLs).

## Concepts <a href="#id-1bf6" id="id-1bf6"></a>

**Bucket Policies**:

* JSON-based access control statements are applied directly to an S3 bucket.
* This allows us to specify who can access the bucket and under what conditions.

**IAM Roles**:

* Enable temporary access to AWS resources for entities (users, applications, or AWS accounts).
* Roles can be assumed by an external AWS account, granting permissions defined in the role’s policy.

**Typical Use Cases**:

* Account A needs to read/write objects in Account B’s S3 bucket.
* Cross-account collaboration is where data is shared between departments or teams.

## Setup Process <a href="#id-9a2d" id="id-9a2d"></a>

### Grant Access Using Bucket Policies <a href="#ee26" id="ee26"></a>

A bucket policy allows us to define permissions for external accounts directly on the S3 bucket.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": "s3:Get*",
      "Resource": "arn:aws:s3:::example-bucket/*"
    }
  ]
}
```

**Explanation:**

* **Effect**: Defines whether to allow or deny the request.
* **Principal**: Specifies the AWS account (e.g., `123456789012`) that will be granted access.
* **Action**: Defines the permitted operations (e.g., `s3:Get*`).
* **Resource**: Identifies the bucket and objects to which the policy applies.

**Steps:**

1. Open the **S3 console** in Account B.
2. Select the bucket for cross-account access.
3. Edit the **Bucket Policy**.
4. Paste the bucket policy JSON and save changes.

### Use IAM Roles for Cross-Account Access <a href="#b2db" id="b2db"></a>

IAM roles provide more flexibility and security for cross-account access compared to bucket policies alone.

**Steps:**

**1. Create a Role in Account B:**

* Go to the IAM console in Account B.
* Create a new role and select **Another AWS Account** as the trusted entity.
* Enter the Account A ID (e.g., `123456789012`) as the trusted account.
* Attach an appropriate policy granting access to the S3 bucket (e.g., `s3:ListBucket`, `s3:GetObject`).

**IAM Policy**:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::example-bucket/*"
      ]
    }
  ]
}
```

**2. Assume the Role in Account A:**

In Account A, set up permissions for a user or application to assume the role in Account B.

**IAM Policy in Account A:**

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::987654321098:role/RoleName"
    }
  ]
}
```

**3. Access S3 Resources in Account B:**

Use AWS SDK, CLI, or other tools to assume the role and interact with the S3 bucket.

**Assume Role Using AWS CLI:**

```
aws sts assume-role \
  --role-arn arn:aws:iam::987654321098:role/RoleName \
  --role-session-name CrossAccountSession
```

Use the temporary credentials returned by the command to access S3 resources.

## Best Practices <a href="#eda8" id="eda8"></a>

* **Use Least Privilege**: Grant only the permissions required for the intended operation.
* **Monitor and Audit**: Enable logging using Amazon S3 server access logs.
* **Encrypt Data**: Use server-side encryption (SSE) or client-side encryption.
* **Rotate IAM Roles**: Regularly rotate access credentials and review roles.

## Troubleshooting Tips <a href="#id-0466" id="id-0466"></a>

**Access Denied Errors**:

* Verify that both bucket policies and IAM policies grant the necessary permissions.
* Check the resource ARNs in policies for typos.

**Role Assumption Issues**:

* Ensure the trusted account is correctly specified in the role trust policy.
* Validate the role session credentials and permissions.

**Testing Permissions**:

* Use AWS Policy Simulator to validate access configurations.
