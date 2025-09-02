# AWS

## Real-Time Scenario-Based DevOps Interview Questions on AWS <a href="#real-time-scenario-based-devops-interview-question" id="real-time-scenario-based-devops-interview-question"></a>

**Main Recommendation:** To excel in real-world AWS DevOps interviews, focus on understanding how services integrate to solve infrastructure challenges, automate operations at scale, and ensure security and reliability.

### 1. EC2 Instance Provisioning and Configuration <a href="#id-1-ec2-instance-provisioning-and-configuration" id="id-1-ec2-instance-provisioning-and-configuration"></a>

**Question:** Your application experiences variable traffic spikes. How would you design an EC2 provisioning workflow to handle sudden surges while minimizing cost?\
**Answer:**\
Design a combination of On-Demand and Spot Instances behind an Auto Scaling group.

* Use Launch Templates with dynamic parameters (AMI ID, instance type).
* Define scaling policies based on CloudWatch metrics (CPUUtilization >70% → add instances; <30% → remove).
* Leverage Spot Instances with fallback to On-Demand if capacity is unavailable.
* Bake configuration via user data scripts or AWS Systems Manager State Manager for consistency.
* Integrate with Elastic Load Balancer to distribute traffic.

### 2. Auto Scaling Groups (ASG) and Health Checks <a href="#id-2-auto-scaling-groups-asg-and-health-checks" id="id-2-auto-scaling-groups-asg-and-health-checks"></a>

**Question:** A subset of your EC2 instances is serving errors, but ASG isn’t replacing them. How would you troubleshoot and ensure unhealthy instances are replaced?\
**Answer:**

* Verify ASG health check type (EC2 vs. ELB); switch to ELB health checks for application-level validation.
* Confirm health check grace period is sufficient for instance initialization.
* Inspect CloudWatch metrics and ELB health reports to pinpoint failing instances.
* Ensure application returns proper HTTP status codes.
* Adjust health check thresholds and enable instance termination policies.

### 3. VPC Design and Isolation <a href="#id-3-vpc-design-and-isolation" id="id-3-vpc-design-and-isolation"></a>

**Question:** You need to host a multi-tier application (web, application, database) with strict isolation. Describe your VPC layout.\
**Answer:**

* Create a VPC with three subnets per Availability Zone: public (web), private (app), and isolated (DB).
* Associate an Internet Gateway with the VPC for public subnet access.
* Place NAT Gateways in public subnets for private subnet outbound internet.
* Use Security Groups and Network ACLs:
  * Web SG allows inbound HTTP/HTTPS from 0.0.0.0/0.
  * App SG allows inbound only from Web SG on application port.
  * DB SG allows inbound only from App SG on DB port.
* Enable VPC Flow Logs to monitor traffic.

### 4. CloudWatch Monitoring and Alarming <a href="#id-4-cloudwatch-monitoring-and-alarming" id="id-4-cloudwatch-monitoring-and-alarming"></a>

**Question:** How would you detect and respond to a sudden drop in application throughput?\
**Answer:**

* Create a CloudWatch metric filter on request count or latency.
* Set an alarm for a throughput decrease (e.g., Requests <100/min for 5 minutes).
* Use CloudWatch Events (EventBridge) to trigger an AWS Lambda that:
  * Executes a health check script.
  * Reboots or replaces unhealthy instances.
  * Notifies on-call via SNS.

### 5. EBS Volume Performance Optimization <a href="#id-5-ebs-volume-performance-optimization" id="id-5-ebs-volume-performance-optimization"></a>

**Question:** Your application shows high I/O wait and slow disk writes. Which EBS volume type would you choose and how would you tune it?\
**Answer:**

* Switch to Provisioned IOPS SSD (io1/io2) for consistent high performance.
* Allocate sufficient IOPS (e.g., 10,000 IOPS for heavy workloads).
* Enable EBS optimization on the instance.
* Stripe multiple EBS volumes using RAID 0 via Linux MDADM for aggregated throughput.
* Monitor disk metrics (VolumeReadOps, VolumeWriteOps, Throughput) in CloudWatch.

### 6. S3 Data Lifecycle Management <a href="#id-6-s3-data-lifecycle-management" id="id-6-s3-data-lifecycle-management"></a>

**Question:** You store logs in S3 that every month grow by 1 TB. How would you manage costs over time?\
**Answer:**

* Implement Lifecycle Policies:
  * Transition objects older than 30 days to S3 Standard-IA.
  * After 90 days, move to S3 Glacier.
  * After 365 days, expire/delete logs if no longer needed.
* Use object tagging to apply granular policies.
* Enable S3 Storage Lens for cost visibility.

### 7. IAM Least Privilege Enforcement <a href="#id-7-iam-least-privilege-enforcement" id="id-7-iam-least-privilege-enforcement"></a>

**Question:** A developer needs read-only access to S3 and CloudWatch logs for one specific bucket. How do you grant access securely?\
**Answer:**

* Create an IAM policy with:
  * `s3:GetObject`, `s3:ListBucket` actions scoped to the specific bucket ARN.
  * `logs:FilterLogEvents`, `logs:GetLogEvents` scoped to the log group ARN.
* Attach policy to a user or group.
* Enforce MFA and require IAM Access Analyzer to validate policies.

### 8. Blue/Green Deployment on EC2 <a href="#id-8-bluegreen-deployment-on-ec2" id="id-8-bluegreen-deployment-on-ec2"></a>

**Question:** Describe how you’d implement a blue/green deployment strategy with EC2 and ELB.\
**Answer:**

* Provision a parallel (green) environment identical to production (blue).
* Register green instances with a new Target Group on the Application Load Balancer.
* Perform validation tests.
* Update listener rule to switch traffic to green target group.
* Deregister blue instances and decommission or reuse them.

### 9. VPC Peering and Cross-Account Access <a href="#id-9-vpc-peering-and-cross-account-access" id="id-9-vpc-peering-and-cross-account-access"></a>

**Question:** Two AWS accounts need to share a service hosted in a VPC. How would you configure secure connectivity?\
**Answer:**

* Create a VPC peering connection between the two VPCs.
* Update route tables to send relevant CIDR traffic through the peering connection.
* Adjust Security Groups to allow only specific ports/IP ranges.
* For cross-region or transitive scenarios, use AWS Transit Gateway instead.

### 10. Elastic Load Balancer SSL Termination <a href="#id-10-elastic-load-balancer-ssl-termination" id="id-10-elastic-load-balancer-ssl-termination"></a>

**Question:** How would you enable SSL for your web tier without managing certificates on each EC2?\
**Answer:**

* Use an Application Load Balancer with an ACM-managed certificate.
* Terminate SSL at the ALB listener (HTTPS).
* Forward decrypted HTTP to backend instances over a private network.
* Configure automatic certificate renewal in ACM.

### 11. CloudWatch Logs Insights for Troubleshooting <a href="#id-11-cloudwatch-logs-insights-for-troubleshooting" id="id-11-cloudwatch-logs-insights-for-troubleshooting"></a>

**Question:** Your application logs errors in CloudWatch. How do you quickly identify which error types are most frequent?\
**Answer:**

*   Use CloudWatch Logs Insights with a query:

    ```
    fields @message
    | parse @message "[ERROR] *: *" as module, errorMsg
    | stats count() by errorMsg
    | sort by count_desc
    | limit 10
    ```
* Visualize results in the console dashboard.

### 12. Data Encryption at Rest and In Transit <a href="#id-12-data-encryption-at-rest-and-in-transit" id="id-12-data-encryption-at-rest-and-in-transit"></a>

**Question:** Explain how you ensure data is encrypted both at rest and in transit for an EC2-hosted application interacting with S3.\
**Answer:**

* Enable SSE-S3 or SSE-KMS for S3 buckets.
* On EC2, mount EBS volumes with encryption enabled (AWS-managed CMK or custom CMK).
* Configure the application to use HTTPS endpoints (`https://s3.amazonaws.com`).
* Enforce TLS protocols via security policies.

### 13. Automated AMI Creation and Patching <a href="#id-13-automated-ami-creation-and-patching" id="id-13-automated-ami-creation-and-patching"></a>

**Question:** How would you automate AMI updates for nightly patching?\
**Answer:**

* Use AWS Systems Manager Automation Document:
  * `aws:runPatchBaseline` to apply OS patches.
  * `aws:createImage` to capture a new AMI.
* Schedule via EventBridge rule at your desired cadence.
* Test AMIs in a staging ASG before promoting to production.

### 14. EBS Snapshot Management <a href="#id-14-ebs-snapshot-management" id="id-14-ebs-snapshot-management"></a>

**Question:** Your business requires daily backups of critical volumes, keeping 14 days of history. How do you automate this?\
**Answer:**

* Tag volumes (e.g., `Backup=true`).
* Create an EventBridge rule triggering an AWS Lambda daily.
* Lambda script:
  * Filter volumes by tag.
  * Use `create_snapshot` API.
  * Tag snapshots with creation date.
  * Clean up snapshots older than 14 days.

### 15. S3 Event-Driven Processing <a href="#id-15-s3-event-driven-processing" id="id-15-s3-event-driven-processing"></a>

**Question:** Describe a solution to process new files uploaded to S3 in real time.\
**Answer:**

* Configure S3 event notifications on `s3:ObjectCreated:*` to invoke a Lambda function.
* Lambda reads file metadata, processes data, and writes results to another bucket or database.
* For high throughput, use SQS or SNS intermediate buffering.

### 16. IAM Role for EC2 Service Access <a href="#id-16-iam-role-for-ec2-service-access" id="id-16-iam-role-for-ec2-service-access"></a>

**Question:** Your EC2 instances need to push metrics to CloudWatch and read from DynamoDB. How do you grant that?\
**Answer:**

* Create an IAM role with:
  * `cloudwatch:PutMetricData` for specific namespaces.
  * `dynamodb:GetItem`, `dynamodb:Query` scoped to the table ARN.
* Attach role to EC2 via instance profile.
* Ensure no long-term access keys are used on the instance.

### 17. Auto Scaling with Scheduled Scaling <a href="#id-17-auto-scaling-with-scheduled-scaling" id="id-17-auto-scaling-with-scheduled-scaling"></a>

**Question:** Your workload has predictable peaks at 9 AM and troughs after 6 PM. How would you optimize ASG sizing?\
**Answer:**

* Define scheduled scaling actions in ASG:
  * `StartTime=08:45AM`, `DesiredCapacity=50` to handle morning traffic.
  * `StartTime=06:15PM`, `DesiredCapacity=10` for the evening lull.
* Combine with dynamic scaling to accommodate unforeseen spikes.

### 18. Cross-Region Replication for S3 <a href="#id-18-cross-region-replication-for-s3" id="id-18-cross-region-replication-for-s3"></a>

**Question:** A compliance requirement mandates storing data in two geographic regions. How do you implement this for S3?\
**Answer:**

* Enable S3 Cross-Region Replication (CRR) on the source bucket.
* Assign IAM role for replication with `s3:ReplicateObject` permission.
* Choose a destination bucket in the secondary region.
* Ensure versioning is enabled on both buckets.

### 19. VPC Endpoint for Private S3 Access <a href="#id-19-vpc-endpoint-for-private-s3-access" id="id-19-vpc-endpoint-for-private-s3-access"></a>

**Question:** How can EC2 instances in a private subnet access S3 without traversing the internet?\
**Answer:**

* Create a Gateway VPC Endpoint for S3 in the VPC.
* Update route table for private subnet to direct S3 prefix list through the endpoint.
* Adjust IAM policy to require `aws:sourceVpce` condition matching the endpoint ID.

### 20. Incident Response with AWS Config and CloudTrail <a href="#id-20-incident-response-with-aws-config-and-cloudtrai" id="id-20-incident-response-with-aws-config-and-cloudtrai"></a>

**Question:** You suspect unauthorized creation of IAM users. How would you detect and respond automatically?\
**Answer:**

* Enable AWS Config rule `iam-user-no-policies-check` and CloudTrail for audit logs.
* Create CloudWatch EventBridge rule on `CreateUser` API in CloudTrail.
* Trigger a Lambda that:
  * Validates user tag or origin.
  * Deletes unauthorized user.
  * Sends an alert via SNS to the security team.

\
