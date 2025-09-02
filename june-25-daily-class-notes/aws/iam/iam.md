# IAM

## AWS IAM: Identities, Roles & Policies explained in simple terms <a href="#e6b9" id="e6b9"></a>

<figure><img src="https://miro.medium.com/v2/resize:fit:700/1*d5jAvOKMjhi_1Ibvsn8SoQ.png" alt="" height="258" width="700"><figcaption></figcaption></figure>

## Basic concepts <a href="#id-9b72" id="id-9b72"></a>

### Identities <a href="#id-15e2" id="id-15e2"></a>

> An identity refers to an entity that can make requests and interact with AWS resources. It can be a person, application, or service that needs to authenticate itself to access AWS resources.

* **Users:** End users, such as people, employees of an organization etc. These are identities with long-term credentials who can interact with AWS through accounts.
* **Groups:** A collection of users. Each user in the group will inherit the permissions of the group.
* **Roles**: An IAM role is an identity you can create that has specific permissions with credentials that are valid for short durations. Roles can be assumed by entities that you trust.

### Authorizations <a href="#cc9b" id="cc9b"></a>

> Authorizations are granted through policies. Authorizations dictates what actions can be performed on which AWS resources.

* **Policies:** A policy is an object in AWS that defines permissions. Policies are made up of documents (called ‘Policy documents’), which are stored in JSON format.

### How everything works together? <a href="#id-9ff2" id="id-9ff2"></a>

* **Step 1**: Create identities (User, User Group or Roles).
* **Step 2**: (Optional) Define custom (Customer Managed) policies.
* **Step 3**: Attach policies (can be AWS managed or Customer managed policies) to identities.

## Policies: Deep dive <a href="#id-5840" id="id-5840"></a>

### **Policy creation and management** <a href="#e6b7" id="e6b7"></a>

**Policy creation and management** can be done by either AWS (AWS managed policies) or customers (Customer Managed policies).

### Policy types <a href="#ac8e" id="ac8e"></a>

The 2 most important policy types are —

1. [**Identity-based policies**](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_id-based) — Identity-based policies grant permissions to an **identity**. To create identity-based policy, attach [managed](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#managedpolicy) and [inline](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#inline) policies to IAM identities (users, groups to which users belong, or roles).
2. [**Resource-based policies**](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_resource-based) — Resource-based policies grant permissions to a **resource**. To create resource-based policy, attach inline policies to resources.

For other policy types, refer to the AWS documentation [here](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html).

### Policy Documents: How does those look? <a href="#c5f4" id="c5f4"></a>

Here is a sample policy document:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::example-bucket/*",
                "arn:aws:s3:::example-bucket"
            ]
        }
    ]
}
```

1. **Version: “2012–10–17”** — Specifies the policy language version.
2. **Effect** — This can be set to either **allow** or **deny**. By default, all\
   IAM policies are set to deny. The effect either grants or restricts\
   access to the **“Action”** defined in the “Statement”. _Note: the deny policy always takes precedence over allow._
3. **Action** — Lists the specific AWS actions that an identity is allowed to perform.
4. **Resource** — This specifies the resources you wish the **“Action”** and **“Effect”** to be applied to using ARNs (Amazon Resource Names) specified in the form of _**arn:partition:service:region:account-id:resource**._

### Creating an IAM Policy Statement <a href="#id-8aac" id="id-8aac"></a>

IAM Policies can be created using any of the below 3 methods:

1. **Copying an Existing AWS Managed Policy** — You can use a pre-existing policy statement provided by AWS and customize it to meet your specific needs. This can save time and effort in creating a policy from scratch, while still providing the necessary permissions for your use case.
2. Using the [**AWS Policy Generator**](https://awspolicygen.s3.amazonaws.com/policygen.html) tool.
3. **Creating your own policy from scratch** — This can be achieved in one of two ways; You can either write the policy JSON document manually or use the AWS visual editor to duplicate an existing policy and modify it according to your requirements.[\
   ](https://medium.com/tag/aws-iam-policy?source=post_page-----a925e984845e---------------------------------------)
