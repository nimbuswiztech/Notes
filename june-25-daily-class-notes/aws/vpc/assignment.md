# Assignment

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

## AWS VPC Lab Session: Step-by-Step Practice Guide <a href="#aws-vpc-lab-session-step-by-step-practice-guide" id="aws-vpc-lab-session-step-by-step-practice-guide"></a>

Follow these detailed instructions to replicate the AWS VPC class lab, building a functional multi-subnet VPC with routing, gateways, and EC2 instances.

### 1. Preparation <a href="#id-1-preparation" id="id-1-preparation"></a>

* **Log into AWS Console** with your credentials.
* Ensure your AWS region is set (preferably the default from your organization or "N. Virginia" for practice).

### 2. Create the VPC <a href="#id-2-create-the-vpc" id="id-2-create-the-vpc"></a>

1. **Go to Services** > **VPC**.
2. **Click "Create VPC"**.
   * _Name_: `svm-vpc-01`
   * _IPv4 CIDR block_: `10.0.0.0/27` (provides 32 IPs: 10.0.0.0–10.0.0.31)
   * Leave IPv6 CIDR empty (unless needed)
   * _Tenancy_: Default
   * Click **Create VPC**.

### 3. Add Subnets <a href="#id-3-add-subnets" id="id-3-add-subnets"></a>

Create two subnets within your VPC:

1. **Navigate to "Subnets"** > **Create subnet**.
2. _VPC_: Select `svm-vpc-01`.

**For Public Subnet:**

* _Subnet name_: `Nimbus-public`
* _Availability Zone_: Pick one (e.g., us-east-1a)
* _IPv4 CIDR block_: `10.0.0.0/28` (IPs 10.0.0.0–10.0.0.15)
* Click **Add subnet**.

**For Private Subnet:**

* _Subnet name_: `Nimbus-private`
* _Availability Zone_: Pick another or same
* _IPv4 CIDR block_: `10.0.0.16/28` (IPs 10.0.0.16–10.0.0.31)
* Click **Create subnet**.

### 4. Create Route Tables <a href="#id-4-create-route-tables" id="id-4-create-route-tables"></a>

1. **Go to "Route Tables"** > **Create route table**.
2. _Name_: `rd-public`
   * _VPC_: `svm-vpc-01`
   * Click **Create route table**.
3. Repeat for private subnet:
   * _Name_: `rd-private`
   * _VPC_: `svm-vpc-01`
   * Click **Create route table**.

You should now have two custom route tables.

### 5. Set Up Internet Gateway (IGW) <a href="#id-5-set-up-internet-gateway-igw" id="id-5-set-up-internet-gateway-igw"></a>

1. **Navigate to "Internet Gateways"** > **Create internet gateway**.
   * _Name_: `Nimbus-icw`
   * Click **Create internet gateway**.
2. Select the gateway and click **Actions > Attach to VPC**.
   * Select `svm-vpc-01`.
   * Click **Attach internet gateway**.

### 6. Allocate Elastic IP (EIP) <a href="#id-6-allocate-elastic-ip-eip" id="id-6-allocate-elastic-ip-eip"></a>

1. **Go to "Elastic IPs"** > **Allocate Elastic IP address**.
   * Click **Allocate** (leave settings at default).
2. You now have a static EIP for NAT Gateway use.

### 7. Create NAT Gateway <a href="#id-7-create-nat-gateway" id="id-7-create-nat-gateway"></a>

1. **Navigate to "NAT Gateways"** > **Create NAT gateway**.
   * _Subnet_: Select `Nimbus-public`.
   * _Elastic IP_: Choose the one just allocated.
   * _Name_: e.g., `windows-nat`
   * Click **Create NAT gateway**.
2. Wait for the NAT Gateway status to become "Available".

### 8. Associate Subnets to Route Tables <a href="#id-8-associate-subnets-to-route-tables" id="id-8-associate-subnets-to-route-tables"></a>

1. **Go to "Route Tables"**.
2. Select `rd-public`:
   * Click **Subnet associations > Edit subnet associations**.
   * Check `Nimbus-public`.
   * Save.
3. Select `rd-private`:
   * Click **Subnet associations > Edit subnet associations**.
   * Check `Nimbus-private`.
   * Save.

### 9. Configure Route Table Routes <a href="#id-9-configure-route-table-routes" id="id-9-configure-route-table-routes"></a>

**Public Route Table (`rd-public`):**

* Select `rd-public`.
* Under **Routes > Edit routes > Add route**:
  * _Destination_: `0.0.0.0/0`
  * _Target_: `Internet Gateway` > Select `Nimbus-icw`
  * Save changes.

**Private Route Table (`rd-private`):**

* Select `rd-private`.
* Under **Routes > Edit routes > Add route**:
  * _Destination_: `0.0.0.0/0`
  * _Target_: `NAT Gateway` > Select your NAT
  * Save changes.

### 10. (Optional) Review Security Groups <a href="#id-10-optional-review-security-groups" id="id-10-optional-review-security-groups"></a>

* Default security group allows all outbound, but restricts inbound. For practice, you can adjust or create a new group as needed:
  * Allow SSH (port 22) or RDP (port 3389) from your own IP to connect to EC2.

### 11. Launch EC2 Instances <a href="#id-11-launch-ec2-instances" id="id-11-launch-ec2-instances"></a>

1. **Go to "EC2 > Instances"** > **Launch Instance**.
2. _Name_: `public-instance`
   * _AMI_: Amazon Linux 2 or Windows (as preferred)
   * _Subnet_: `Nimbus-public`
   * _Auto-assign Public IP_: Enable (for public instance)
   * _Security Group_: Select one allowing SSH/RDP from your IP
   * Click **Launch instance**
3. Repeat for the private subnet:
   * _Name_: `private-instance`
   * _Subnet_: `Nimbus-private`
   * _Auto-assign Public IP_: Disable (will not be internet-accessible directly)
   * _Security Group_: As above

### 12. Test Connectivity <a href="#id-12-test-connectivity" id="id-12-test-connectivity"></a>

* SSH/RDP into your public instance using the public IP.
* From the public instance, attempt to access the private instance (private IP).
* For outbound internet:
  * From the private instance, use CLI (`ping`, `curl`, etc.) to verify outbound connectivity (internet works via NAT Gateway).
* Private instance is not reachable directly from the internet (secured).

### 13. Clean Up <a href="#id-13-clean-up" id="id-13-clean-up"></a>

* Delete Elastic IP if no longer needed (otherwise, charges may apply).
* Terminate EC2 instances and remove resources after practice.

### Best Practices <a href="#best-practices" id="best-practices"></a>

* Always associate NAT Gateway with a public subnet.
* Never keep unused Elastic IP addresses.
* Use least privilege on security group rules.
* Map subnets deliberately to correct route tables for proper traffic flow.

\
