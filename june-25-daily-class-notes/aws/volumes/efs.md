# EFS

## &#x20;EFS Practical Session <a href="#delivering-an-effective-efs-practical-session" id="delivering-an-effective-efs-practical-session"></a>

Amazon Elastic File System (EFS) is a scalable, cloud-native NFS file system for use with AWS Cloud services and on-premises resources. Here’s how you can organize and deliver an engaging, hands-on EFS training session, including a structured demo and scenario-based questions for participants.

### Session Structure <a href="#session-structure" id="session-structure"></a>

### 1. Introduction to Amazon EFS

* Explain what EFS is, its features, and core use cases.
* Compare EFS with other AWS storage solutions (EBS, S3).

### 2. Setting Up EFS: Step-by-Step Demo

### Prerequisites

* An AWS account with appropriate permissions
* At least two Amazon EC2 instances in the same VPC

### Steps

1. **Create a File System**
   * Navigate to the EFS Console.
   * Click “Create file system”.
   * Select the VPC and network settings.
   * Review and create.
2. **Configure Mount Targets**
   * Ensure each Availability Zone has a mount target.
   * Adjust security groups to allow NFS traffic (default port 2049).
3. **Install NFS Utilities on EC2 Instances**
   * Use `sudo yum install -y nfs-utils` or `sudo apt-get install nfs-common` as appropriate.
4. **Mount the EFS File System**
   * Obtain file system DNS from EFS console.
   *   Run:

       ```
       sudo mount -t nfs4 -o nfsvers=4.1 <EFS-DNS>:/ /mnt/efs
       ```
5. **Test File Sharing**
   * Create files/folders from Instance A.
   * Access and verify those files from Instance B.
6. **Basic Performance Testing (optional)**
   * Copy files of various sizes to observe throughput and latency.

### 3. Best Practices and Security

* Review managing permissions with AWS IAM and POSIX.
* Discuss backup strategies (AWS Backup).
* Network security (restricting access, encryption options).

### Tips for Running a Great Practical Session <a href="#tips-for-running-a-great-practical-session" id="tips-for-running-a-great-practical-session"></a>

* **Prepare all resources in advance**, including EC2 instances and necessary IAM roles.
* **Use a whiteboard or diagrams** to illustrate EFS architecture and data flow.
* Allow participants to follow along and perform each step themselves.
* Pause frequently for questions and clarify each step.
* If possible, simulate a “real” scenario (e.g., sharing media files between web servers).
* **Highlight troubleshooting tips:** e.g., NFS mount failures, permission errors.
* Provide a quick reference sheet for key CLI commands and console actions.

### Scenario-Based Questions for Discussion <a href="#scenario-based-questions-for-discussion" id="scenario-based-questions-for-discussion"></a>

| Scenario                                                                | Question                                                                          |
| ----------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Multiple web servers must serve the same static content                 | How would you set up EFS so files are accessible and writable by all servers?     |
| A user reports high latency from one EC2 instance accessing EFS         | What troubleshooting steps would you follow?                                      |
| You need to restrict access to EFS only to certain applications/servers | Which security mechanisms (AWS and OS level) would you use to enforce this?       |
| Unexpectedly high storage costs are observed with EFS                   | What tools/features help monitor and control EFS storage costs?                   |
| A requirement mandates all file transfers be encrypted in transit       | Does EFS support encryption in transit, and how would you enable and verify it?   |
| A file system needs backup and recovery support                         | How can you integrate EFS with AWS native backup and disaster recovery solutions? |

By following this structure and encouraging hands-on participation, your EFS practical session will be clear, engaging, and impactful. Use the scenario questions to drive discussion and ensure deeper understanding.

wxplain step by step for the demo handson. ubuntuwxplain step by step for the demo handson. ubuntu

## Amazon EFS Hands-On Demo Guide (Ubuntu ) <a href="#amazon-efs-hands-on-demo-guide-ubuntu-edition" id="amazon-efs-hands-on-demo-guide-ubuntu-edition"></a>

Amazon Elastic File System (EFS) offers fully managed, elastic, NFS-compatible shared storage for AWS workloads. This in-depth guide walks you through a complete, production-grade hands-on demonstration on Ubuntu 22.04 LTS EC2 instances. It covers everything from initial networking and security to mounting with TLS, automating re-mounts on reboot, benchmarking, monitoring, troubleshooting, and cleanup. Scenario-based quiz questions at the end help reinforce each concept.

### Overview <a href="#overview" id="overview"></a>

Running a live demo is the fastest way to learn how EFS behaves. In the following pages you will:

* Build a secure, multi-AZ architecture that lets two Ubuntu servers share the same filesystem.
* Install and compare the EFS mount helper (`amazon-efs-utils`) and the native NFS client (`nfs-common`).
* Enable encryption in transit with one command.
* Persist mounts across reboots using `/etc/fstab`.
* Run a simple `fio` benchmark to observe I/O patterns under different throughput modes.
* Collect CloudWatch metrics and set alerts on throughput saturation.
* Resolve common errors such as `mount.nfs4: Connection timed out`.

The sequence is intentionally verbose so you can reuse individual steps in future workshops.

### Demo Prerequisites <a href="#demo-prerequisites" id="demo-prerequisites"></a>

| Component   | Minimum Requirement                                             | Why It’s Needed                      |
| ----------- | --------------------------------------------------------------- | ------------------------------------ |
| AWS account | IAM user/role with permission to create EFS, EC2 and CloudWatch | Resource provisioning                |
| Region      | Any region that supports EFS (e.g., `us-east-1`)                | Service availability                 |
| VPC         | Default VPC or custom VPC with at least two public subnets      | Multi-AZ demo topology               |
| Ubuntu AMI  | 22.04 LTS or newer                                              | Package names match tutorial         |
| Local tools | SSH client, AWS CLI v2                                          | Instance access & optional CLI steps |

> **Time estimate:** 40-60 minutes for the full workflow, including benchmark and cleanup.

### Environment Architecture <a href="#environment-architecture" id="environment-architecture"></a>

### Logical Diagram&#x20;

1. **VPC (10.0.0.0/16)**
   * Subnet-A (10.0.1.0/24) in **us-east-1a** – houses **EC2-A**.
   * Subnet-B (10.0.2.0/24) in **us-east-1b** – houses **EC2-B**.
2. **EFS File System (`fs-XYZ123`)**
   * One mount target per Availability Zone, each with a dedicated Elastic Network Interface.
3. **Security Groups**
   * `sg-ec2` for instances: outbound **TCP 2049** allowed.
   * `sg-efs` for mount targets: inbound **TCP 2049** sourced from `sg-ec2` only.
4. **IAM Role**
   * `EC2EFSRole` attaches the AWS-managed policy `AmazonElasticFileSystemsUtils` for encrypted mount logs and optional CloudWatch publishing.

### Step-by-Step Hands-On <a href="#step-by-step-hands-on" id="step-by-step-hands-on"></a>

### Step 1 — Create Security Groups

1. **Launch VPC Console ➜ Security Groups ➜ Create security group**
   * Name: **`sg-ec2`**
   * Inbound: `SSH 22` from your IP.
   * Outbound: allow **all traffic** (default).
2. **Create second group**
   * Name: **`sg-efs`**
   * Inbound rule: **NFS TCP 2049** with **Source = sg-ec2** (use the security group ID dropdown).
   * Outbound: default (all traffic).

This least-privilege design means only instances in `sg-ec2` can reach EFS.

### Step 2 — Create the EFS File System

1. Open **EFS Console ➜ Create file system**.
2. Choose **Quick create** or **Customize**:
   * **Performance mode:** _General Purpose_ (low latency).
   * **Throughput mode:** _Elastic_ (default; scales automatically).
   * **Encryption at rest:** enabled by default (AWS-managed KMS key).
   * **Lifecycle management:** keep default.
3. Click **Create**. Status shows **Creating** then **Available** within \~60 seconds.

> **Mount targets** are auto-provisioned—each picks an IP from the subnet you selected.

### Step 3 — Attach the EFS Security Group

1. In the EFS console **Network ➜ Manage**.
2. For every mount target, **replace** the default group with **`sg-efs`**.
3. Wait until the life-cycle state returns to **Available**.

### Step 4 — Launch Two Ubuntu 22.04 EC2 Instances

1. **EC2 Console ➜ Launch instance**.
2. AMI: _Ubuntu Server 22.04 LTS (HVM)_.
3. Instance type: _t3.micro_ (demo scale).
4. Network & subnet:
   * EC2-A ➜ Subnet-A (`us-east-1a`).
   * EC2-B ➜ Subnet-B (`us-east-1b`).
5. **IAM role:** `EC2EFSRole`.
6. **Security group:** attach **`sg-ec2`**.
7. Storage: default 8 GiB gp3.
8. Launch and note both public IPs for SSH.

### Step 5 — Install Required Packages on Ubuntu

SSH into **EC2-A** (repeat on EC2-B).

```
sudo apt-get update -y               # refresh repos
sudo apt-get install -y nfs-common   # native NFS client[11]
sudo apt-get install -y amazon-efs-utils  # EFS mount helper[42]
```

Verify version:

```
dpkg -l | grep amazon-efs-utils
```

`amazon-efs-utils` ≥ **2.3** supports IPv6 and improved TLS.

### Step 6 — Mount EFS

### 6-A. Mount with the EFS Mount Helper (Recommended)

```
FILE_SYSTEM_ID=fs-XYZ123          # replace
MOUNT_POINT=/mnt/efs
sudo mkdir -p $MOUNT_POINT
sudo mount -t efs $FILE_SYSTEM_ID:/ $MOUNT_POINT
```

The helper auto-injects optimal NFS options (rsize/wsize=1 MiB, hard, timeo=600, retrans=2).

### 6-B. Mount via Native NFS4 Client (Fallback)

```
REGION=us-east-1
AZ=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone)
DNS="$AZ.$FILE_SYSTEM_ID.efs.$REGION.amazonaws.com"
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport $DNS:/ $MOUNT_POINT
```

Use this when the mount helper package is unavailable.

### Step 7 — Verify Shared Storage

On **EC2-A**:

```
echo "Hello from EC2-A" | sudo tee /mnt/efs/hello.txt
```

On **EC2-B**:

```
cat /mnt/efs/hello.txt
# Output: Hello from EC2-A
```

Bidirectional visibility confirms the file system is shared.

### Step 8 — Enable Encryption in Transit (TLS)

Mount helper makes TLS frictionless:

```
sudo umount /mnt/efs
sudo mount -t efs -o tls $FILE_SYSTEM_ID:/ /mnt/efs
```

A local **stunnel** proxy (ports 20049–21049) now encrypts every NFS packet with TLS 1.2/AES-256. You can audit its log at `/var/log/amazon/efs/mount.log`.

### Step 9 — Auto-Mount on Reboot

Edit `/etc/fstab`:

```
sudo bash -c 'echo "$FILE_SYSTEM_ID:/ /mnt/efs efs _netdev,tls 0 0" >> /etc/fstab'
```

Test:

```
sudo umount /mnt/efs
sudo mount -a     # re-reads fstab
```

Reboot the instance and run `df -h` to confirm the mount persists.

### Step 10 — Optional Performance Benchmark

The `fio` utility provides quick insights:

```
sudo apt-get install -y fio
fio --name=seq-read --directory=/mnt/efs --rw=read --bs=4M --size=4G
fio --name=seq-write --directory=/mnt/efs --rw=write --bs=4M --size=4G --end_fsync=1
```

Observe throughput variations if you switch between:

* **Elastic throughput** (default; scales on demand).
* **Provisioned throughput** (manually set MiB/s).
* **Bursting throughput** (credits accrue at 50 KiB/s per GiB stored).

### Step 11 — Monitor with CloudWatch

EFS emits real-time metrics—add them to a dashboard:

| Metric               | Description                             | Typical Alert Threshold  |
| -------------------- | --------------------------------------- | ------------------------ |
| `BurstCreditBalance` | Remaining credits (Bursting mode)       | < 20% credits            |
| `PercentIOLimit`     | I/O utilization in General Purpose mode | > 80% for 15 min         |
| `DataReadIOBytes`    | Aggregate read throughput               | Abnormal spike detection |
| `DataWriteIOBytes`   | Aggregate write throughput              | Abnormal spike detection |

You can also install the **CloudWatch agent** on EC2 to capture per-instance throughput.

### Step 12 — Cleanup

1.  On each EC2 instance:

    ```
    sudo umount /mnt/efs
    ```
2. **EFS Console ➜ File system ➜ Delete** (deletes mount targets automatically).
3. Terminate EC2 instances.
4. Remove the security groups if unused.

### Troubleshooting Checklist <a href="#troubleshooting-checklist" id="troubleshooting-checklist"></a>

| Symptom                                | Likely Cause                                      | Fix                                                             |
| -------------------------------------- | ------------------------------------------------- | --------------------------------------------------------------- |
| `mount.nfs4: Connection timed out`     | Missing **NFS 2049** in either security group     | Reverify rules: inbound on `sg-efs`, outbound on `sg-ec2`       |
| `aws: error: EFS mount helper missing` | `amazon-efs-utils` not installed                  | `sudo apt-get install -y amazon-efs-utils`                      |
| `Access denied by server`              | POSIX UID/GID mismatch when using Access Points   | Align UID/GID or use EFS Access Point with root-squash disabled |
| Slow writes, high latency              | Exceeded `PercentIOLimit` in General Purpose mode | Switch to Elastic/Provisioned throughput or Max I/O             |
| Reboot loses mount                     | Fstab entry missing `_netdev` or wrong FS type    | Use `efs` type with `_netdev,tls`                               |

### Scenario-Based Quiz <a href="#scenario-based-quiz" id="scenario-based-quiz"></a>

Try these in your next workshop:

| Scenario                                                                              | Question                                                                                                |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| New compliance rule mandates **TLS for all data in transit**                          | Which single mount option enforces this and how do you verify it?                                       |
| Your workload spikes to 250 MiB/s for 10 minutes every hour                           | Which throughput mode minimizes cost without throttling?                                                |
| Server team reports intermittent `stale file handle` errors                           | List at least three investigation steps specific to EFS NFSv4.1 semantics.                              |
| You must restrict write access to a single application user while letting others read | Which combination of **POSIX permissions**, **EFS Access Point**, and **IAM** would you choose and why? |
| After moving to Max I/O mode, latency doubled                                         | Explain why this is expected and propose remediation.                                                   |

### Appendix B — Latency & Storage Class Table <a href="#appendix-b--latency--storage-class-table" id="appendix-b--latency--storage-class-table"></a>

| Storage Class             | Typical Read Latency | Typical Write Latency | Use Case Example             |
| ------------------------- | -------------------- | --------------------- | ---------------------------- |
| **EFS Standard**          | 250 µs               | 2.7 ms                | CMS, CI/CD, home directories |
| **EFS Infrequent Access** | \~10–50 ms           | \~10–50 ms            | Archive, disaster recovery   |
| **EFS Archive**           | \~tens of ms         | \~tens of ms          | Regulatory cold storage      |
