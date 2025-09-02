# AWS EC2 Demo

## AWS EC2 Demonstration: Components & Architecture Explained <a href="#aws-ec2-demonstration-components--architecture-exp" id="aws-ec2-demonstration-components--architecture-exp"></a>

**Essential Takeaway:** Amazon EC2 (Elastic Compute Cloud) consists of interconnected components that work together to provide scalable virtual computing in the cloud. Understanding these components—from AMIs and instance types to security groups and storage options—is crucial for building robust cloud architectures.

### 1. Core EC2Components Overview <a href="#id-1-core-ec2components-overview" id="id-1-core-ec2components-overview"></a>

<figure><img src="https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/6e254eb6b626f96172738d8108367665/4c620b51-d14b-4c93-9032-46a90794f8d4/c42e1c3f.png" alt=""><figcaption></figcaption></figure>

EC2 Architecture Components and Their Relationships

Amazon EC2 operates through several integrated components that form a complete virtual computing environment. Each component serves a specific purpose in the overall architecture, and understanding their relationships is fundamental to effective cloud deployment.

### 2. Instance Types & Families (2025 Update) <a href="#id-2-instance-types--families-2025-update" id="id-2-instance-types--families-2025-update"></a>

**Instance Type Categories:**

| Instance Family                 | Key Features                                                  | Use Cases                                                 | Pricing Tier                  |
| ------------------------------- | ------------------------------------------------------------- | --------------------------------------------------------- | ----------------------------- |
| General Purpose (M8g, M7i, T4g) | Balanced CPU, memory, networking - AWS Graviton4/3 processors | Web servers, small databases, development environments    | Standard                      |
| Compute Optimized (C7g, C6i)    | High-performance processors for compute-heavy tasks           | HPC, batch processing, game servers, scientific computing | Higher than general purpose   |
| Memory Optimized (R6g, X2idn)   | High RAM capacity with AWS Graviton2/3 processors             | High-performance databases, in-memory analytics, SAP HANA | Premium for high memory       |
| Storage Optimized (I4i, D3)     | High-speed NVMe storage and disk throughput                   | Big data processing, data warehouses, log processing      | Varies by storage performance |
| Accelerated Computing (P4, G5)  | NVIDIA GPUs for AI/ML and graphics workloads                  | AI/ML training, deep learning, video transcoding          | Premium for GPU acceleration  |
| HPC Optimized (Hpc6id)          | High memory bandwidth for HPC applications                    | Computational chemistry, genomics, financial modeling     | Premium for specialized HPC   |

**2025 Instance Updates:**

* **M8g instances** now feature AWS Graviton4 processors with enhanced price-performance
* **C8 series** optimized for high-traffic e-commerce and real-time analytics
* **New R7 instances** with DDR5 memory offering 50% more bandwidth than DDR4

### Instance Naming Convention

EC2 instances follow a structured naming pattern: **Family + Generation + Size**

* Example: `m7g.large` = M-family, 7th generation, Graviton processor, large size

### 3. Storage Architecture Deep Dive <a href="#id-3-storage-architecture-deep-dive" id="id-3-storage-architecture-deep-dive"></a>

| Storage Type              | Persistence           | Performance         | Use Case                | Availability   | Cost                   |
| ------------------------- | --------------------- | ------------------- | ----------------------- | -------------- | ---------------------- |
| EBS (Elastic Block Store) | Persistent            | High IOPS available | Root volumes, databases | Single AZ      | Pay per GB/month       |
| Instance Store            | Ephemeral (Temporary) | Highest performance | Temporary data, caches  | Instance-local | Included with instance |
| EFS (Elastic File System) | Persistent            | Scalable            | Shared file storage     | Multi-AZ       | Pay per GB + access    |
| Amazon S3                 | Persistent            | Object storage      | Backup, static content  | Global         | Pay per GB stored      |

### EBS Volume Types (2025)

* **gp3 (General Purpose SSD)**: Balanced performance, default choice for most workloads
* **io2 (Provisioned IOPS SSD)**: High-performance for I/O intensive applications
* **st1 (Throughput Optimized HDD)**: Big data and sequential workloads
* **sc1 (Cold HDD)**: Infrequently accessed data

### 4. Security Components <a href="#id-4-security-components" id="id-4-security-components"></a>

### Security Groups

Security groups act as **virtual firewalls** controlling inbound and outbound traffic at the instance level. Key characteristics:

* **Stateful**: Return traffic is automatically allowed
* **Default-deny**: All inbound traffic blocked by default
* **Multiple groups**: Up to 5 security groups per network interface

**Real-world Demo Setup:**

1. **Web Server Security Group**: Allow HTTP (port 80) and HTTPS (port 443) from anywhere
2. **Database Security Group**: Allow MySQL (port 3306) only from web server security group
3. **SSH Access Group**: Allow SSH (port 22) only from specific IP ranges

### Key Pairs Authentication

EC2 uses **public-key cryptography** for secure access:

* **Public key**: Stored on EC2 instance in `~/.ssh/authorized_keys`
* **Private key**: Kept securely by user for SSH access
* **Windows instances**: Private key used to decrypt administrator password

### 5. Amazon Machine Images (AMIs) <a href="#id-5-amazon-machine-images-amis" id="id-5-amazon-machine-images-amis"></a>

AMIs serve as **templates** containing everything needed to launch an instance:

**AMI Components:**

* **Root volume template**: Operating system and pre-installed software
* **Launch permissions**: Controls who can use the AMI
* **Block device mapping**: Specifies storage volumes to attach

**AMI Types (2025):**

* **Public AMIs**: AWS-provided, community, or marketplace AMIs
* **Private AMIs**: Custom AMIs created from your instances
* **Shared AMIs**: AMIs shared with specific AWS accounts

### 6. Advanced Features <a href="#id-6-advanced-features" id="id-6-advanced-features"></a>

### Placement Groups

Control instance placement for **performance optimization**:

* **Cluster**: Low-latency networking within single AZ for HPC
* **Partition**: Spread across logical partitions for large distributed systems
* **Spread**: Separate instances across distinct hardware to reduce failures

### Launch Templates

**Next-generation** configuration templates replacing Launch Configurations:

* **Versioning support**: Create multiple template versions
* **Mixed instance types**: Support for multiple instance families
* **Advanced networking**: Enhanced network configuration options

### 7. Monitoring with CloudWatch <a href="#id-7-monitoring-with-cloudwatch" id="id-7-monitoring-with-cloudwatch"></a>

EC2 integrates with **CloudWatch** for comprehensive monitoring:

**Basic Metrics (5-minute intervals)**:

* CPU Utilization, Network In/Out, Disk Read/Write Operations
* Memory metrics require CloudWatch Agent installation

**Enhanced Monitoring (1-minute intervals)**:

* Additional cost but provides faster alerting capabilities
* Custom metrics through CloudWatch Agent

### 8. Pricing Models Comparison <a href="#id-8-pricing-models-comparison" id="id-8-pricing-models-comparison"></a>

![EC2 Pricing Models - Potential Cost Savings Comparison](https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/6e254eb6b626f96172738d8108367665/e2afcec5-94ab-44b3-b915-b768f296d66c/658b6cf9.png)EC2 Pricing Models - Potential Cost Savings Comparison

| Pricing Model      | Payment                | Cost Savings          | Best For                      | Availability             |
| ------------------ | ---------------------- | --------------------- | ----------------------------- | ------------------------ |
| On-Demand          | Pay per hour/second    | No savings (baseline) | Variable workloads, testing   | Always available         |
| Reserved Instances | 1-3 year commitment    | Up to 72% savings     | Steady-state usage            | Capacity reservation     |
| Spot Instances     | Bid-based pricing      | Up to 90% savings     | Fault-tolerant, flexible apps | Can be terminated by AWS |
| Dedicated Hosts    | Physical server rental | Up to 70% savings     | Compliance, licensing         | Dedicated hardware       |

### 9. Real-World Demo Scenarios <a href="#id-9-real-world-demo-scenarios" id="id-9-real-world-demo-scenarios"></a>

### Scenario 1: Web Application Deployment

1. **Choose AMI**: Amazon Linux 2023 with pre-installed web server
2. **Instance Type**: t3.medium for balanced performance
3. **Storage**: 20GB gp3 EBS root volume + 100GB data volume
4. **Security**: Web security group (ports 80, 443) + SSH group (port 22)
5. **Key Pair**: Generate new keypair for secure access
6. **Monitoring**: Enable detailed CloudWatch monitoring

### Scenario 2: High-Performance Computing Cluster

1. **Placement Group**: Cluster type for low-latency communication
2. **Instance Type**: C7g.xlarge compute-optimized instances
3. **Storage**: Instance store for temporary high-speed processing
4. **Networking**: Enhanced networking with SR-IOV enabled
5. **Launch Template**: Automate deployment of identical configurations

### Scenario 3: Database Server with High Availability

1. **Multi-AZ Deployment**: Primary and standby in different AZs
2. **Instance Type**: R7g.large memory-optimized for database workloads
3. **Storage**: io2 EBS volumes with high IOPS for database performance
4. **Security**: Database security group allowing access only from application servers
5. **Backup**: Automated EBS snapshots for data protection

### 10. Best Practices <a href="#id-10-best-practices-for-students" id="id-10-best-practices-for-students"></a>

**Cost Optimization:**

* Start with t3.micro instances (Free Tier eligible) for learning
* Use Spot instances for non-critical experiments (up to 90% savings)
* Always stop instances when not in use to avoid charges

**Security Hardening:**

* Never use default security groups in production
* Implement least privilege principle in security group rules
* Rotate key pairs regularly and use different keys for different environments

**Performance Optimization:**

* Right-size instances based on actual usage patterns
* Use appropriate storage types for specific workload requirements
* Monitor with CloudWatch and set up alerts for resource utilization

