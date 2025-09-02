# ELB Lab

## AWS Load Balancer Practical Lab <a href="#aws-load-balancer-practical-lab-step-by-step-guide" id="aws-load-balancer-practical-lab-step-by-step-guide"></a>



This hands-on lab guides students through deploying an Application Load Balancer (ALB) in AWS, EC2 web-server instances, and validating health checks and fault tolerance.

### 1. Prerequisites and Setup <a href="#id-1-prerequisites-and-setup" id="id-1-prerequisites-and-setup"></a>

1. **AWS Account & Permissions**
   * Ensure each student has an AWS account with “AdministratorAccess” or equivalent privileges.
2. **VPC and Subnet**
   * Use the default VPC in a single AWS Region (e.g., us-east-1).
   * Confirm at least two public subnets exist.
3. **Key Pair**
   * Create or select an existing EC2 key pair for SSH access.

### 2. EC2 Instance Deployment <a href="#id-2-ec2-instance-deployment" id="id-2-ec2-instance-deployment"></a>

1. **Launch Two EC2 Instances**
   * AMI: Amazon Linux 2
   * Instance Type: t2.micro (Free Tier)
   * Network: Default VPC, two different public subnets
   * Security Group “web-sg”: allow inbound TCP 80 (HTTP) and 22 (SSH) from your IP
   * Key Pair: your chosen key
2. **Install Web Server**
   *   SSH into each instance:

       ```
       ssh -i "your-key.pem" ec2-user@<EC2_Public_IP>
       ```
   *   Install Apache:

       ```
       sudo yum update -y
       sudo yum install httpd -y
       sudo systemctl start httpd
       sudo systemctl enable httpd
       ```
   *   Create distinct homepages:

       ```
       echo "Welcome to Server 1" | sudo tee /var/www/html/index.html   # on instance #1
       echo "Welcome to Server 2" | sudo tee /var/www/html/index.html   # on instance #2
       ```

### 3. Configure Health Checks <a href="#id-3-configure-health-checks" id="id-3-configure-health-checks"></a>

1. **Modify Security Group for Health Checks**
   * In “web-sg”, ensure inbound TCP 80 from the ALB’s security group (to be created) is allowed.
2. **Verify Web Servers**
   * In browser, visit each EC2’s public IP; confirm distinct messages.

### 4. Create Application Load Balancer <a href="#id-4-create-application-load-balancer" id="id-4-create-application-load-balancer"></a>

1. **Navigate to EC2 > Load Balancers > Create Load Balancer**
2. **Select “Application Load Balancer”**
   * Name: `lab-alb`
   * Scheme: internet-facing
   * Listeners: HTTP:80
   * Availability Zones: Select both subnets you used for EC2 instances
3. **Configure Security Group for ALB**
   * Create “alb-sg”: allow inbound TCP 80 from 0.0.0.0/0
4. **Configure Target Group**
   * Target group name: `lab-tg`
   * Target type: instance
   * Protocol: HTTP, Port: 80
   * Health check path: `/`
5. **Register Targets**
   * Add both EC2 instances to `lab-tg`
6. **Review & Create**
   * Confirm settings and click “Create load balancer”

### 5. Test Load Balancer Behavior <a href="#id-5-test-load-balancer-behavior" id="id-5-test-load-balancer-behavior"></a>

1. **Obtain ALB DNS Name**
   * In Load Balancers list, copy the DNS (e.g., `lab-alb-123456.elb.amazonaws.com`).
2. **Validate Round-Robin**
   * In browser, visit the DNS URL repeatedly; pages should alternate between “Server 1” and “Server 2.”
3. **Simulate Failure**
   *   SSH into one EC2 instance and stop Apache:

       ```
       sudo systemctl stop httpd
       ```
   * Refresh the ALB URL; only the healthy instance’s page appears.
   *   Restart Apache on stopped instance:

       ```
       sudo systemctl start httpd
       ```

### 6. Optional Enhancements <a href="#id-6-optional-enhancements" id="id-6-optional-enhancements"></a>

1. **Enable Sticky Sessions**
   * Edit `lab-tg` attributes → Enable “Stickiness” (duration e.g. 300 seconds)
2. **Path-Based Routing**
   * In ALB listener rules, add rule:
     * If path is `/app1*`, forward to a second target group (you may launch additional instances)
3. **Auto Scaling**
   * Create an Auto Scaling group behind `lab-tg` with min=2, max=4, scale-out on CPU > 50%

### 7. Cleanup <a href="#id-7-cleanup" id="id-7-cleanup"></a>

* Terminate EC2 instances
* Delete Load Balancer, Target Group, Security Groups

### Scenario-Based Questions <a href="#scenario-based-questions" id="scenario-based-questions"></a>

1. If one EC2 instance fails its health check, how does the ALB respond?
2. After adding two more instances, what steps register them with the ALB?
3. How would you restrict direct traffic to EC2, allowing only the ALB to reach them?
4. If the health-check path returns HTTP 500, what happens to that target?
5. Design a multi-region ALB architecture for cross-region redundancy.
6. When are sticky sessions useful, and what drawbacks do they introduce?
7. Which AWS metrics would you monitor to detect ALB performance issues?
