# Interview questions

## Jenkins: Real-Time Scenario Interview Q\&A <a href="#jenkins-real-time-scenario-interview-qa" id="jenkins-real-time-scenario-interview-qa"></a>

1. **Scenario:** You have a Jenkins pipeline that sometimes fails during the deployment stage. How do you debug and handle this?
   * **Answer:** Review stage-level logs, rerun the job with verbose/debug output, check workspace for leftover files, and add conditional post-blocks to handle failed deployments (e.g., send alerts, roll back).
2. How would you securely store and use secret credentials like AWS keys in a Jenkins pipeline?
   * Use Jenkins Credentials Manager to store secrets, reference them with environment variables or credentials binding plugins, and never hardcode them in the Jenkinsfile.
3. **Scenario:** The Jenkins build agent disconnects frequently when builds run for too long. What are your steps?
   * Examine agent logs, increase timeout, check network stability, optimize build tasks to run faster, or switch to more reliable nodes.
4. How do you automatically trigger a Jenkins build when a change is pushed to the Git repository?
   * Set up webhooks in the repository to notify Jenkins, configure the job to poll SCM, or use GitHub/GitLab integration plugins.
5. **Scenario:** You want to parallelize browser testing in a Jenkins pipeline. How do you achieve this?
   * Use the `parallel` step in the Jenkinsfile to run multiple test stages in parallel with different browser configurations.
6. How do you handle rolling deployments in Jenkins?
   * Implement pipeline logic (using Blue Ocean or scripted pipelines) to deploy to portions of servers (batches), monitor health, and proceed or roll back as needed.
7. **Scenario:** Builds are failing due to plugin conflicts after upgrading Jenkins. What is your process?
   * Check for plugin compatibility on Jenkins' update center, revert to a stable version, and update plugins one-by-one with backup.
8. How do you use Jenkins for continuous deployment (CD)?
   * Set up pipelines with post-success deployment steps to production/staging environments, use plugins for artifact upload, and include rollback strategies.
9. **Scenario:** You must run builds for multiple microservices with shared libraries. How do you structure this in Jenkins?
   * Use pipeline shared libraries for reusable logic, create multi-branch pipelines, and parameterized jobs for different services.
10. How would you protect Jenkins pipelines from unauthorized modification?
    * Implement role-based access control (RBAC), apply folder and job permissions, and restrict pipeline script modification to trusted users.
11. **Scenario:** A Jenkins job is slow due to multiple external shell script executions. How do you optimize?
    * Use pipeline steps or plugins instead of shell calls where possible, cache results, and run independent scripts in parallel.
12. How do you enable auditing of Jenkins job changes and build triggers?
    * Use the Audit Trail plugin, enable system logs, and store build artifacts and logs with metadata for traceability.
13. **Scenario:** Different teams want isolated build environments. What is your approach?
    * Use folders with individual permissions, possibly run separate Jenkins controllers or dedicated nodes for isolation.
14. How do you run builds in Docker containers using Jenkins?
    * Use the Docker Pipeline plugin to define builds inside containers, specify images in Jenkinsfile, and clean up containers after use.
15. **Scenario:** You must deploy to AWS from Jenkins, but want restricted access for the build user. How?
    * Assign minimal IAM permissions to Jenkins build users, use role-based access, and environment-specific credentials.
16. How do you enforce code quality checks before deployment in Jenkins pipelines?
    * Integrate static analysis tools (e.g., SonarQube), make stages fail if checks don't pass, block deployment until all checks succeed.
17. **Scenario:** Jenkins master performance is degrading as the job count increases. What do you do?
    * Move jobs to dedicated agents, reduce log retention, archive old builds, and possibly deploy additional masters or controllers.
18. How do you rollback a failed production deployment from Jenkins?
    * Track deployed artifacts/versions, trigger rollback scripts or jobs that redeploy previous stable builds, and automate rollback pipeline stages.
19. **Scenario:** Developers want to run specific pipeline stages manually post build. Is this possible?
    * Use input steps in the pipeline for manual approval, or define separate jobs for manual stage execution.
20. How do you manage secrets and sensitive variables in multi-branch pipelines?
    * Use credentials binding, avoid echoing secrets in logs, and rotate credentials regularly.
21. **Scenario:** Your Jenkins dashboard is flooded with old inactive jobs. How do you clean up efficiently?
    * Set up build discard policies, use scripts to identify and delete inactive jobs, and review with job owners.
22. How do you implement blue-green deployment with Jenkins?
    * Use two sets of environments (blue/green), deploy to idle environment, switch traffic after health checks, and automate using pipeline logic.
23. **Scenario:** You need to trigger test jobs across multiple Jenkins instances. How do you coordinate?
    * Use the Jenkins Remote Build Trigger plugin, or invoke builds via REST API across controllers.
24. What plugins would you recommend for Git integration in Jenkins pipelines?
    * Git plugin, GitHub plugin, and pipeline Git step.
25. **Scenario:** How do you recover Jenkins from failure or corruption?
    * Restore from backups (keep regular home/data backups), scripts for job re-creation, and disaster recovery playbooks.
26. How do you restrict Jenkins builds to certain agent labels?
    * Use agent labels in pipeline definitions or restrict job execution via job configuration.
27. **Scenario:** Developers' builds often fail due to outdated dependencies. How to mitigate?
    * Use private artifact repositories (Nexus, Artifactory), rebuild dependencies on schedule, and notify devs for updates.
28. How do you visualize pipeline flow and job dependencies?
    * Use Blue Ocean visualization or build pipelines plugin for clear DAG (Directed Acyclic Graph) views.
29. **Scenario:** You need dynamic environment provisioning for each build. How would you set this up?
    * Use pipeline to provision ephemeral environments (e.g., Docker, cloud instances), run tests, and then teardown resources automatically.
30. How do you secure and audit Jenkins configuration as code?
    * Use Jenkinsfile in source control for auditable pipeline definitions, monitor changes, and use plugins for config export/import.

## Git: Real-Time Scenario Interview Q\&A <a href="#git-real-time-scenario-interview-qa" id="git-real-time-scenario-interview-qa"></a>

1. **Scenario:** A teammate accidentally pushed secrets to the repository. How do you remove it from history?
   * **Answer:** Use `git filter-branch`, `git rebase`, or the `BFG Repo-Cleaner` to purge secrets from all commit history, then force-push and inform other contributors to re-clone.
2. How do you handle and resolve merge conflicts in a large codebase?
   * Review conflicted files, communicate with team, manually edit conflicts, use `git status` for guidance, and commit the resolved files.
3. **Scenario:** You want to squash all commits in a feature branch into a single commit before merging. How?
   * Use `git rebase -i <base-branch>` and choose 'squash', then merge the squashed branch.
4. How do you revert a specific commit on a shared branch without affecting later commits?
   * Use `git revert <commit-hash>`, which creates a new commit undoing the chosen commit only.
5. **Scenario:** You want to test changes without switching branches. What Git feature helps?
   * Use `git stash` to temporarily save changes and work on another branch, then apply stash later.
6. How do you ensure your local branch is up to date with remote?
   * Use `git fetch` to update local refs and `git merge` or `git pull` to integrate changes.
7. **Scenario:** You want to resolve "detached HEAD" and keep your changes. What steps do you take?
   * Create a new branch from the detached state: `git checkout -b <branch-name>`.
8. What is the difference between `git pull` and `git fetch` followed by `git merge`?
   * `git pull` fetches and merges automatically; `fetch` followed by `merge` allows manual review.
9. **Scenario:** Team members are working on the same file, causing frequent conflicts. How do you mitigate?
   * Break functionalities into smaller files/modules, communicate early, and use code review to synchronize changes.
10. How do you safely delete a remote branch?
    * Run `git push origin --delete <branch-name>`; locally, use `git branch -d <branch-name>`.
11. **Scenario:** You want to view the history of a file including renames. What command do you use?
    * Use `git log --follow <file>`.
12. How do you cherry-pick a commit from one branch to another?
    * Use `git cherry-pick <commit-hash>` on the target branch.
13. **Scenario:** A feature is ready on a separate repository. How do you bring it into your repo with history?
    * Use `git remote add` to add the repo, fetch, and `git merge` or `git cherry-pick` as needed.
14. How do you tag a specific commit and push tags to remote?
    * Use `git tag <tag-name> <commit>`, then `git push origin <tag-name>`.
15. **Scenario:** You must resolve a divergence between your branch and the remote. What approaches exist?
    * Use either `git merge` for a merge commit or `rebase` to make history linear depending on workflow.
16. How do you undo a local commit but keep work in the working directory?
    * Use `git reset --soft HEAD~1`.
17. **Scenario:** You want to see which branch contains a specific commit. What command?
    * Use `git branch --contains <commit-hash>`.
18. How do you set up a pre-commit hook to check code quality?
    * Add scripts in `.git/hooks/pre-commit`, e.g., for style or lint checks.
19. **Scenario:** You need to split one repository into multiple for microservices. How?
    * Use `git filter-branch --subdirectory-filter` or `git subtree split` to extract subdirectories.
20. How do you track code review comments in Git?
    * Use pull requests (on platforms like GitHub/GitLab), which allow inline code comments.
21. **Scenario:** Your history is full of merge commits and hard to read. How do you improve it?
    * Use `git rebase` for linear history before merging.
22. How do you clone a repository with all branches?
    * Use `git clone --mirror` or fetch all with `git fetch --all`.
23. **Scenario:** You wish to undo the last commit after pushing. What are your options?
    * Use `git revert` (safe) or force-push after a reset (not recommended for shared branches).
24. How do you fetch and work with a specific remote branch?
    * Use `git fetch origin <branch>`, then `git checkout <branch>`.
25. **Scenario:** Contributors accidentally rewrite git history. How do you recover?
    * Use reflog `git reflog` to find previous states, reset or revert as needed.
26. How do you find what changed in a particular commit?
    * Use `git show <commit-hash>`.
27. **Scenario:** A large binary file is added, bloating repo size. How do you clean up?
    * Use `git filter-branch` or `BFG Repo-Cleaner` to remove large files from all history.
28. How do you set global ignore rules for Git?
    * Create a global `.gitignore` and set with `git config --global core.excludesfile ~/.gitignore`.
29. **Scenario:** You want to contribute to an open-source repo. What’s the recommended workflow?
    * Fork, clone, make a feature branch, push changes, and open PR.
30. How do you recover deleted commits?
    * Use `git reflog` to find and checkout commits or branches deleted by mistake.

## AWS EC2: Real-Time Scenario Interview Q\&A <a href="#aws-ec2-real-time-scenario-interview-qa" id="aws-ec2-real-time-scenario-interview-qa"></a>

1. **Scenario:** Your web server EC2 instance suddenly experiences a spike in traffic. How do you maintain availability without downtime?
   * **Answer:** Set up an Autoscaling Group to add instances automatically based on CPU/network utilization. Use a Load Balancer to distribute traffic evenly.
2. A Linux EC2 instance won’t boot after a failed OS update. How do you fix it?
   * Detach the root EBS volume, attach to a healthy instance, fix configs/rollback the update, reattach, and reboot.
3. **Scenario:** How do you enable secure, automated logins to EC2 without sharing the private key?
   * Use AWS Systems Manager Session Manager, IAM policies for access, or rotate and distribute SSH keys via EC2 Instance Connect.
4. Describe the ways to monitor EC2 instance health and performance.
   * Use CloudWatch for metrics and alarms, install CloudWatch agents for OS metrics/logs, and set up notifications for issues.
5. **Scenario:** You need to move an EC2 instance between VPCs. How?
   * Create an AMI of the instance, spin up a new EC2 in the target VPC using the AMI, and migrate associated elastic IPs.
6. What’s the difference between EBS- and Instance Store-backed root volumes?
   * EBS volumes are persistent across stop/start; instance store data is lost when the instance is stopped or terminated.
7. **Scenario:** How do you recover data from a terminated EC2 instance?
   * For EBS-backed, recover from attached EBS snapshots; for instance store, data cannot be recovered.
8. How do you resize an EC2 instance to improve performance?
   * Stop the instance, change instance type in EC2 console or via CLI, then restart.
9. **Scenario:** You must ensure an instance always has a static IP address. What do you do?
   * Assign an Elastic IP address to the EC2 instance.
10. How do you automate instance updates across a fleet?
    * Use AWS Systems Manager (SSM) Run Command or Automation, or user data scripts on launch.
11. **Scenario:** An app must run on Spot Instances but must not lose critical data. What’s your plan?
    * Store data on resilient, external services like EBS, S3, RDS, or EFS, and regularly back up important data.
12. How do you secure SSH access to your EC2 instances?
    * Restrict inbound rules (security groups), use Key Pairs, and implement bastion hosts for private subnet access.
13. **Scenario:** An EC2 instance in a private subnet needs internet access. What do you do?
    * Set up a NAT Gateway or NAT Instance in a public subnet, update route tables accordingly.
14. How do you automate daily snapshots of EBS volumes?
    * Use AWS Backup or Lambda with CloudWatch Events to schedule and manage snapshots.
15. **Scenario:** The root EBS volume is full, but data must be retained. Your actions?
    * Stop instance, create a snapshot, extend volume size, and resize partition/filesystem on boot.
16. Describe steps to move EC2 instances across availability zones securely.
    * Create an AMI, launch into the target AZ, and transfer Elastic IPs and EBS volumes as needed.
17. **Scenario:** Your EC2 instance is running but not reachable over SSH. Troubleshoot.
    * Check security group and NACLs, ensure correct key pair, verify public IP, and check OS firewall.
18. How do you back up and restore an EC2 instance quickly?
    * Use AMIs or EBS snapshots, and scripting with AWS CLI for rapid backup/restore.
19. **Scenario:** EC2 Autoscaling is not triggering as expected. Troubleshoot approach?
    * Review CloudWatch metric alarms, instance health status, scaling policies, IAM roles, and error logs.
20. How do you reduce AWS EC2 usage costs?
    * Use right-sizing, reserved/spot instances, auto-scaling, and terminate unused resources.
21. **Scenario:** Instance volumes are lost after a restart. Why?
    * Data is on instance store (ephemeral storage) which is wiped; use EBS for data persistence.
22. How would you configure alarm-based scaling for a high-CPU workload?
    * Create CloudWatch alarms for CPU thresholds, and set scaling actions (scale-out/scale-in) on triggers.
23. **Scenario:** You must move EC2 data securely offsite. How?
    * Use encrypted EBS snapshots, S3 bucket with encryption, or secure upload scripts.
24. How do you automatically recover a failed EC2 instance?
    * Use EC2 recovery actions or Health Checks in Auto Scaling Groups.
25. **Scenario:** Instances in a VPC can’t reach each other after launch. Why?
    * Misconfigured security groups or subnet NACLs; review and correct rules to allow internal traffic.
26. How do you assign IAM roles to EC2 instances?
    * Attach a role with least-privilege permissions during instance launch, or attach/detach later via console/CLI.
27. **Scenario:** Ec2 stops unexpectedly every day. What do you check?
    * Billing issues, maintenance events, instance scheduled actions, CloudWatch Events, and check application logs.
28. How do you handle software patching for hundreds of EC2s?
    * Use Systems Manager Patch Manager for automated, scheduled patching across fleets.
29. **Scenario:** An instance is running as a mail server and blacklisted. Your steps?
    * Check security, patch vulnerabilities, rotate IPs, and apply for IP delisting; review email sending policies.
30. How do you ensure HA (High Availability) for EC2-based applications?
    * Use multiple AZs, load balancers, autoscaling, and monitor instance health.

## AWS VPC: Real-Time Scenario Interview Q\&A <a href="#aws-vpc-real-time-scenario-interview-qa" id="aws-vpc-real-time-scenario-interview-qa"></a>

1. **Scenario:** Deploy a highly available web app across multiple AZs in a VPC. Configuration?
   * **Answer:** Design public and private subnets in each AZ, deploy EC2s across subnets, use load balancer, and NAT for private subnet internet access.
2. How do you restrict access to a VPC to only specific IPs?
   * Configure security group and NACL rules to allow only trusted IPs.
3. **Scenario:** Need secure, encrypted connection between your on-prem network and AWS VPC. How?
   * Set up IPSec VPN or Direct Connect with VPN overlay.
4. To get internet into a VPC, what must be configured?
   * Attach Internet Gateway, add routes in route tables, and configure security groups.
5. **Scenario:** Two VPCs in different AWS regions need to communicate. What’s your solution?
   * Set up VPC peering or use AWS Transit Gateway for secure inter-region connectivity.
6. How do you allow EC2 in private subnet to access S3 securely?
   * Set up a VPC Endpoint (S3), update routing and policies accordingly.
7. **Scenario:** Monitor and log all VPC network traffic. What do you use?
   * Enable VPC Flow Logs, configure CloudWatch/log destination, and analyze logs for unusual activity.
8. How is a default VPC different from a custom VPC?
   * Default VPC comes pre-configured with subnets and internet access; custom is user-defined for granular control.
9. **Scenario:** EC2 in public subnet is failing to connect to the internet. Next steps?
   * Verify IGW attachment, correct route tables, assign public IP, and configure security groups.
10. Describe isolating workloads within a VPC.
    * Use private subnets, dedicated NACLs/security groups, and restrict routing and access for isolation.
11. **Scenario:** Design multi-tier architecture (web, app, db) in VPC. How?
    * Create separate subnets for each tier, restrict access via security groups, allow only necessary traffic between subnets.
12. How do you peer VPCs?
    * Establish VPC peering connection, update route tables, and ensure no overlapping CIDR blocks.
13. **Scenario:** Hundreds of VPCs need secure communication. What’s recommended?
    * Use AWS Transit Gateway for scalable and secure interconnectivity.
14. Enable internal DNS resolution within VPC: steps?
    * Use Route 53 Resolver or enable DNS hostnames/hostname resolution in VPC settings.
15. **Scenario:** Migrate on-prem app to AWS while maintaining IP range. Steps?
    * Create VPC with matching CIDR, establish VPN/Direct Connect for seamless transition.
16. How do you enforce security across all subnets in VPC?
    * Use centrally managed security groups, NACLs, and automate compliance with Lambda/Config rules.
17. **Scenario:** Setup of VPC with IPv6 support: how?
    * Enable IPv6 in VPC, assign prefixes to subnets, and update security group/NACL rules.
18. For cross-account VPC resource access, solutions?
    * Use VPC Resource Access Manager (RAM) or peering with IAM roles as appropriate.
19. **Scenario:** Subnet route table misconfigured; instances can't reach targets. Find and fix?
    * Diagnose with route tables, update destination/correct target, ensure propagations.
20. How do you isolate databases from direct internet exposure in VPC?
    * Place DBs in private subnets, restrict security groups to only allow app servers.
21. **Scenario:** Application must support both IPv4 and IPv6. How do you enable?
    * Enable IPv6 for VPC and assign to desired subnets/instances; adjust security policies.
22. Create a scalable NAT solution for private subnet egress traffic:
    * Use managed NAT Gateway in public subnet with route tables pointing to NAT.
23. **Scenario:** Need low-latency AWS to on-prem connectivity. What do you choose?
    * Use AWS Direct Connect.
24. Traffic monitoring and anomaly detection in VPC. How?
    * VPC Flow Logs, CloudWatch, GuardDuty, and custom Lambda triggers for alerting.
25. **Scenario:** A new subnet can’t reach other VPC resources. Troubleshoot?
    * Check route tables, NACL rules, and make sure correct associations are set.
26. Difference between security groups and network ACLs?
    * Security groups are stateful, instance-level; NACLs are stateless and subnet-level.
27. **Scenario:** Developer needs isolated sandbox in VPC. What’s your approach?
    * Allocate dedicated subnet, separate NACLs, and restrict VPC peering/workload access.
28. How do you enable private connectivity to AWS services without public IP?
    * Use VPC Endpoints (interface/gateway).
29. **Scenario:** Traffic between AZs is failing in your VPC; possible causes?
    * Route table misconfiguration, NACL restrictions, or lack of correct subnets.
30. How do you enforce compliance and auditing in VPC configurations?
    * Use AWS Config, CloudTrail, GuardDuty, and regular scripted audits.

## AWS Volumes (EBS, Storage Gateway, S3): Real-Time Scenario Interview Q\&A <a href="#aws-volumes-ebs-storage-gateway-s3-real-time-scena" id="aws-volumes-ebs-storage-gateway-s3-real-time-scena"></a>

1. **Scenario:** EBS volume attached to EC2 is nearly full; what are your steps?
   * **Answer:** Stop instance if needed (snapshots advisable), increase EBS volume size, resize filesystem inside the instance, and verify disk usage before bringing app back online.
2. How do you migrate an EBS volume between EC2 instances in different AZs?
   * Snapshot the EBS volume, copy the snapshot to the desired AZ, create new volume, attach to the new instance.
3. **Scenario:** You want to automatically take EBS snapshots every day. How can you achieve this?
   * Schedule Lambda with CloudWatch Events, or use AWS Backup to automate daily snapshots.
4. How do you restore from a failed EBS volume?
   * Use latest EBS snapshot to create a new volume and attach it to the instance.
5. **Scenario:** Application requires high IOPS on volume, but performance is poor. Solution?
   * Use provisioned IOPS (io1/io2) volumes, or switch to EBS-optimized instances for better throughput.
6. How do you attach a second volume to a running EC2 instance?
   * Create and attach via console/AWS CLI, mount it on the OS, and format if new.
7. **Scenario:** EBS volumes are being left orphaned after instance termination. How do you clean up?
   * Enable "delete on termination" setting for non-critical volumes, and automate audits to find/detach/delete orphaned volumes.
8. Explain the difference between EBS, S3, and instance store.
   * EBS: block storage; S3: object storage; instance store: ephemeral storage tied to EC2 instance lifecycle.
9. **Scenario:** Volume snapshots are rapidly consuming budget. Your approach?
   * Set policies for snapshot retention (delete old, retain recent), automate cleanup, and optimize backup schedule.
10. How do you encrypt EBS volumes in-flight and at-rest?
    * Enable encryption on creation (or for snapshot restore), and ensure traffic is only over secure protocols.
11. **Scenario:** Need data replication across regions for disaster recovery. What storage solution?
    * Copy EBS snapshots/S3 objects to other regions as part of DR strategy; automate with scripts or Lambda.
12. How do you mount an EBS volume to multiple EC2s?
    * Use EBS Multi-Attach (for io1/io2), or for file sharing, use EFS instead.
13. **Scenario:** You need seamless on-prem and cloud file sharing with versioning. What AWS storage product?
    * AWS Storage Gateway File Gateway or S3 with versioning and lifecycle policies.
14. Explain using Storage Gateway-cached vs stored volumes.
    * Gateway-cached: data in S3, local cache for hot data; Gateway-stored: all data local, backed up to S3 as point-in-time snapshots.
15. **Scenario:** Instance store volume is erased after reboot. Why?
    * Instance store is ephemeral and non-persistent; use EBS for key data.
16. How do you increase storage performance for RDS or transactional database?
    * Use provisioned IOPS SSD (io1/io2), scale size for better throughput, and ensure optimized EBS settings.
17. **Scenario:** EBS snapshot creation consistently fails. Troubleshoot path?
    * Check AWS limits, permissions (IAM), ongoing write ops on the volume, and try stopping the instance if safe.
18. How do you automate S3 lifecycle transitions for old data?
    * Use S3 lifecycle rules to transition data to IA/Glacier after X days.
19. **Scenario:** User says S3 object delete is not working. What could be the case?
    * Check bucket policies, versioning (may only add delete marker), and object lock (WORM).
20. How do you restore a deleted S3 object?
    * If versioning enabled, restore from previous versions; otherwise, not possible.
21. **Scenario:** S3 bucket objects need to be searchable by metadata. How?
    * Tag S3 objects with custom metadata and use it as search/filter through S3 APIs, or maintain external metadata DB.
22. How do you optimize costs for EBS and S3 storage?
    * Clean up unattached EBS, apply S3 lifecycle policies, and use tiered storage appropriately.
23. **Scenario:** S3 event notifications are not being triggered. How would you debug?
    * Check event config, permission on bucket, SNS/SQS/Lambda integration setup, and CloudWatch logs.
24. Explain how you would migrate on-prem storage to S3 with minimal downtime.
    * Use AWS DataSync, S3 Transfer Acceleration, or Storage Gateway with hybrid access.
25. **Scenario:** Need an audit trail of all object access in S3. Solution?
    * Enable S3 server access logging and CloudTrail for auditing.
26. How do you ensure S3 data is securely shared with external partners?
    * Use pre-signed URLs, bucket policies, and enable encryption for data at rest.
27. **Scenario:** S3 bucket replication between regions is slow. How to improve?
    * Enable S3 Transfer Acceleration and check for bandwidth/connectivity bottlenecks.
28. How do you check and enforce S3 bucket encryption?
    * Use AWS Config rules, bucket policy enforcement, and block unencrypted object uploads.
29. **Scenario:** You’re planning Hadoop on EC2. Which volume type and config to use?
    * Use EBS throughputs optimized (st1/sc1) or instance store, size and stripe for bulk throughput.
30. How do you perform bulk deletion of thousands of S3 objects efficiently?
    * Use `aws s3 rm` with recursive, or S3 Batch Operations for large scale deletion tasks.

### Additional EC2 Real-Time Scenario Q\&A

1. **Scenario:** You must automate the stopping of non-production EC2 instances after business hours to save costs. How would you implement this?
   * **Answer:** Use AWS Lambda with an EventBridge (CloudWatch Events) rule to schedule a Lambda function that lists and stops tagged EC2 instances during specified hours.
2. **How do you check which security groups are attached to a given EC2 instance using CLI or SDK?**
   * Use the AWS CLI command `aws ec2 describe-instances --instance-ids <id>` and review the output’s security group mappings; in Python, use Boto3’s `describe_instances()` and inspect `SecurityGroups`.
3. **Scenario:** Your instance needs to access S3 privately without using a public IP. What do you configure?
   * **Answer:** Create an S3 VPC Endpoint, attach proper IAM/endpoint policies, and ensure routing does not traverse the public internet.
4. **How do you automate patch management for dozens of EC2s?**
   * Use AWS Systems Manager Patch Manager, attach SSM role, configure patch baseline and schedule maintenance windows.
5. **Scenario:** You need to deploy software to 100 EC2s at once. How do you do it?\*\*
   * Use AWS Systems Manager Run Command to execute scripts remotely and consistently across targeted instances, ensuring SSM agent is installed.
6. **How would you monitor access and API actions performed on your EC2s?**
   * Enable AWS CloudTrail for your region/account. CloudTrail logs all API actions, including those on EC2, which can be sent to S3 and analyzed.
7. **Scenario:** An instance has been compromised. What immediate action do you take?\*\*
   * Isolate by removing all security group ingress rules, take a snapshot of volumes for analysis, stop (but do not terminate) the instance, start forensics, and rotate all credentials.
8. **Describe how you’d resize a root EBS volume attached to a live EC2 instance.**
   * Modify the volume in the EC2 Console (or CLI/API), then use the OS’s file system tools (e.g., `resize2fs` on Linux, Disk Management on Windows) to extend the file system.
9. **Scenario:** You need to troubleshoot high CPU or memory on an EC2. Steps?\*\*
   * Review metrics in CloudWatch, install and check top/htop on instance, analyze running processes, scale vertically (larger instance) or horizontally (add more instances) if workload requires.
10. **How do you securely allow only certain users to SSH into EC2?**
    * Use security group rules to allow SSH only from trusted IPs, provision unique key pairs to individuals, and use IAM policies/Session Manager for additional access control.
11. **Scenario:** You want to track all changes to EC2 configuration and API activity for compliance. What AWS services do you enable?\*\*
    * Use AWS Config (with EC2 resources selected) to track configuration changes, and CloudTrail for API logs and event history.
12. **How would you script automated EC2 deployment and teardown?**
    * Use AWS CLI scripts or Infrastructure-as-Code tools like CloudFormation, Terraform, or the AWS CDK for reproducible deployments and automated destruction.
13. **Scenario:** You need temporary capacity for a batch job, but cost is a concern. Best EC2 pricing model?\*\*
    * Use EC2 Spot Instances for the batch job; these provide deep discounts over On-Demand but should be able to handle potential interruptions.
14. **What are the steps to attach multiple ENIs to an EC2 and why?**
    * Stop the instance (for primary), attach Secondary ENIs via Console/CLI, then start the instance. Useful for multi-homed servers, VPC security, or failover.
15. **How do you quickly identify which EC2s can be stopped or rightsized to reduce monthly AWS spend?**
    * Analyze CloudWatch metrics for CPU/network over time, review EC2 Cost & Usage Reports, and use AWS Compute Optimizer for actionable recommendations.
16. **Scenario:** You need to recover a lost key pair but cannot access a Linux EC2 instance. Steps?\*\*
    * Stop the instance, detach the root EBS, attach it to another instance, manually edit the `authorized_keys` file to insert a new key, reattach, and start the instance.
17. **Explain how you would migrate an EC2 instance to a different AWS region.**
    * Create an AMI of the instance, copy the AMI and snapshots to the target region, and launch a new instance using the copied AMI.
18. **Scenario:** Two EC2 instances in the same VPC but different subnets cannot communicate. Troubleshooting path?\*\*
    * Check Network ACLs (stateless, subnet-level), security group rules (ensure ingress/egress), and confirm correct routing tables and no overlapping CIDR.
19. **How do you create custom automation for regular instance health checks and replacements?**
    * Use an Auto Scaling Group with health checks set (EC2 or Elastic Load Balancer); if an instance is unhealthy, the group replaces it automatically.
20. **Scenario:** You must ensure no sensitive data is left on EBS volumes after instance termination. What do you recommend?\*\*
    * Enable EBS encryption, set "Delete on Termination," and if not using encryption, zero out disk space before termination. Consider Secure Delete utilities or AWS-provided disk wiping methods.

\
