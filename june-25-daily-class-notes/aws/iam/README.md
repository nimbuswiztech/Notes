# IAM

### 1. **Introduction to AWS IAM** <a href="#undefined" id="undefined"></a>

Start with a concise overview:

* **Definition:** IAM enables you to securely control access to AWS services and resources.
* **Purpose:** It helps manage users, groups, roles, and permissions in your AWS environment.
* **Use Case:** Multi-user AWS environments (e.g., a company with developers, admins, and auditors).

### 2. **Key IAM Components with Examples** <a href="#undefined" id="undefined"></a>

Explain each main component with practical, relatable analogies:

| IAM Component | Description                                          | Example/Analogy                                                   |
| ------------- | ---------------------------------------------------- | ----------------------------------------------------------------- |
| **Users**     | Individual with credentials to access AWS resources. | _A software developer in your company_                            |
| **Groups**    | Collection of users sharing permissions.             | _All developers placed in a “Developers” group_                   |
| **Roles**     | AWS identity with permissions, assumed temporarily.  | _An application or EC2 instance temporarily needing access to S3_ |
| **Policies**  | JSON documents that define permissions.              | _A policy allowing only S3 Read access_                           |

### Exercise 1: Creating an IAM User

**Steps:**

1. Open the IAM service on AWS Console.
2. Create a new user (e.g., “student1”).
3. Assign console access and set a custom password.
4. Attach an existing policy (e.g., `AmazonS3ReadOnlyAccess`).
5. Log in as the new user and attempt to access S3 buckets.

**Discussion Point:** Why can/can’t this user perform actions on other services?

### Exercise 2: Using IAM Groups

**Steps:**

1. Create two groups: “Admins” (full access) and “Developers” (limited S3 and EC2 read).
2. Move users into each group.
3. Demonstrate permission changes by moving users between groups.

### Exercise 3: Creating and Assigning Custom Policies

**Steps:**

1. Write a custom policy (e.g., only allowing access to a specific S3 bucket).
2. Attach this policy to a user or group.
3. Test the permissions in real time.

### Exercise 4: IAM Roles and Role Switching

**Scenario:** An application on EC2 must access DynamoDB, but no hardcoded credentials are allowed.

**Steps:**

1. Create a role with DynamoDB permissions.
2. Attach the role to an EC2 instance.
3. Show how the EC2 instance now has permissions, with no credentials stored.

### Exercise 5: Enforcing MFA (Multi-Factor Authentication)

**Steps:**

1. Enable MFA for a sensitive user (e.g., an admin).
2. Show the process of login with MFA.

### 4. **Real-Time Scenarios To Discuss During Practice** <a href="#undefined" id="undefined"></a>

* **Restricting developers from deleting production resources.**
* **Granting temporary access to a third-party auditor.**
* **Allowing an application running on AWS Lambda to access only one S3 bucket.**
* **Implementing least privilege for sensitive financial data.**

### 5. **Scenario-Based Questions** <a href="#undefined" id="undefined"></a>

End your session with these to encourage critical thinking:

1. **A developer needs access to update code in a CodeCommit repository, but must not have delete permissions. How would you implement this with IAM?**
2. **You want to allow an external consultant to access only specific logs in CloudWatch for 7 days. What’s the best approach?**
3. **A web app running on an EC2 instance must upload files to an S3 bucket: How do you grant permissions without using static credentials?**
4. **If an employee leaves, explain steps to immediately revoke all their AWS access.**
5. **Describe how you would force MFA for all users accessing the AWS Management Console.**
6. **What is the best way to control permissions for different levels of access (e.g., read-only, admin) across many users?**
7. **What problems might arise if you assign ‘AdministratorAccess’ to all users, and how would you avoid it?**

AWS Identity and Access Management (IAM) is like the security guard of the cloud—it decides **who** can do **what** in **which** part of your AWS account.

### 1. Four Building Blocks <a href="#id-1-four-building-blocks" id="id-1-four-building-blocks"></a>

| Component  | Think of it as …           | Key Purpose                                                                                     | Where Policies Attach               |
| ---------- | -------------------------- | ----------------------------------------------------------------------------------------------- | ----------------------------------- |
| **User**   | A named employee badge     | Gives one person or an app a login and keys                                                     | Directly to the user                |
| **Group**  | A department badge drawer  | Lets you give the same permissions to many users at once                                        | To the group (flows to all members) |
| **Role**   | A temporary visitor badge  | Grants short-lived permissions that anyone (user, service, or account) can “put on” when needed | To the role                         |
| **Policy** | The rule sheet on the wall | JSON document that lists allowed/denied actions, resources, and conditions                      | Attaches to users, groups, or roles |

IAM Components at a Glance

### Why it matters

* **Least privilege:** start with zero access; add only what’s required.
* **Auditability:** each badge swipe (API call) is logged, so you know who did what.
* **Flexibility:** switch roles instead of sharing long-term passwords.

### 2. Story-Based Explanation <a href="#id-2-story-based-explanation" id="id-2-story-based-explanation"></a>

1. **New intern joins (User).**\
   Create an IAM user “intern-arya” — she gets her own username, console password, and OPTIONAL access keys for CLI.
2. **Intern joins “Developers” (Group).**\
   Instead of attaching 10 policies to Arya, add her to the “Developers” group that already has `AmazonEC2ReadOnlyAccess`. One change updates everyone in the group.
3. **EC2 instance uploads logs (Role).**\
   An EC2 server cannot store passwords safely. Instead, it assumes the role `EC2-S3-LogWriter` that holds `PutObject` rights to a specific S3 bucket. AWS automatically rotates the temporary keys every few hours.
4. **External auditor arrives (Cross-Account Role).**\
   You create role `Audit-Review` that trusts the auditor’s AWS account for one week. No new user in your account, and the role auto-expires.
5. **New policy needed (Policy).**\
   Security adds a JSON policy denying `s3:DeleteObject` in the production bucket. Attach it to the “Developers” group — instantly enforced for Arya and 49 other developers.

### 3. Mental Model Cheat-Codes <a href="#id-3-mental-model-cheat-codes" id="id-3-mental-model-cheat-codes"></a>

* **User = Person/App with long-term credentials**
* **Group = Container that hands down permissions**
* **Role = Borrowed identity with temporary keys**
* **Policy = If/Then rules for allow/deny**

Remember: _Groups cannot log in_ and _roles have no password_.

### 4. Quick Analogy <a href="#id-4-quick-analogy" id="id-4-quick-analogy"></a>

| Real-World Office         | AWS IAM Parallel |
| ------------------------- | ---------------- |
| Employee ID card          | IAM User         |
| Department list (IT, HR)  | IAM Group        |
| Visitor pass / Master key | IAM Role         |
| Building access matrix    | IAM Policy       |
