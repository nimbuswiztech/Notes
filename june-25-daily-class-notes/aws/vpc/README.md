# VPC

## Building an AWS VPC, Step-by-Step Demonstration Guide <a href="#building-an-aws-vpc-step-by-step-demonstration-gui" id="building-an-aws-vpc-step-by-step-demonstration-gui"></a>

Amazon Virtual Private Cloud (VPC) is the networking backbone of every AWS workload. The walk-through below shows—click by click—how to create a VPC, Internet Gateway (IGW), NAT Gateway, public and private subnets, and the supporting route tables. Follow these steps in the AWS Management Console (or replicate them with AWS CLI/Terraform) to give students a hands-on understanding of cloud networking.

### Why This Lab Matters <a href="#why-this-lab-matters" id="why-this-lab-matters"></a>

By isolating back-end resources in private subnets while exposing only front-end tiers, architects reduce attack surface and control egress costs. The exercise also prepares learners for the AWS Solutions Architect exam objectives on VPC design.

### Lab Topology <a href="#lab-topology" id="lab-topology"></a>

A single-AZ VPC (10.0.0.0/16) hosting:

* Public subnet 10.0.1.0/24 for web/bastion hosts
* Private subnet 10.0.2.0/24 for databases/app servers
* IGW for inbound/outbound internet to public subnet
* NAT Gateway for secure outbound traffic from private subnet
* Two custom route tables controlling traffic paths

![Figure 1: Reference architecture—VPC with public and private subnets, Internet Gateway, NAT Gateway, and route tables](https://user-gen-media-assets.s3.amazonaws.com/gpt4o_images/07d3d6f3-dd76-403b-b20b-1701e1769648.png)Figure 1: Reference architecture—VPC with public and private subnets, Internet Gateway, NAT Gateway, and route tables

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* AWS account with console access (IAM user with **vpc\***, **ec2\*** permissions).
* Default limits allow five VPCs/region; ensure capacity.
* Region: **us-east-1** used in screenshots; substitute as needed.

### Create the VPC <a href="#create-the-vpc" id="create-the-vpc"></a>

1. Console → _VPC_ → _Your VPCs_ → **Create VPC**.
2. Name: `Demo-VPC`, IPv4 CIDR: **10.0.0.0/16**, Tenancy: default.
3. Click **Create**. The VPC now appears with ID like _vpc-0ab1c2d3_.

AWS automatically generates a main route table and a default NACL; both initially contain only a local route.

### Provision Subnets <a href="#provision-subnets" id="provision-subnets"></a>

### Public Subnet

1. _Subnets_ → **Create subnet**.
2. Select `Demo-VPC`; Availability Zone: `us-east-1a`.
3. Subnet CIDR: **10.0.1.0/24**; Name tag: `Public-SN`.
4. Create.

### Private Subnet

Repeat with **10.0.2.0/24**, Name `Private-SN`.

Subnet size rationale: /24 (256 IPs) is plenty for demo while leaving space for future /24 slices in 10.0.0.0/16.

### Attach an Internet Gateway <a href="#attach-an-internet-gateway" id="attach-an-internet-gateway"></a>

1. _Internet Gateways_ → **Create internet gateway**. Name: `Demo-IGW`.
2. Select the IGW → **Attach to VPC** → choose `Demo-VPC`.
3. IGW state changes to _Attached_.

IGW is free; only EC2 data transfer charges apply.

### Configure Route Table for Public Subnet <a href="#configure-route-table-for-public-subnet" id="configure-route-table-for-public-subnet"></a>

1. _Route Tables_ → **Create route table**. Name: `Public-RT`; VPC: `Demo-VPC`.
2. Select `Public-RT` → _Routes_ → **Edit routes** → **Add route**:
   * Destination **0.0.0.0/0**
   * Target **Internet Gateway** → `Demo-IGW`. Save.
3. _Subnet associations_ → **Edit subnet associations** → tick `Public-SN`.

Now any instance with a public IP in `Public-SN` can reach the internet and accept inbound traffic that security groups allow.

### Allocate Elastic IP & Create NAT Gateway <a href="#allocate-elastic-ip--create-nat-gateway" id="allocate-elastic-ip--create-nat-gateway"></a>

1. _Elastic IPs_ → **Allocate Elastic IP** → Allocate.
2. _NAT Gateways_ → **Create NAT gateway**.
   * Subnet: `Public-SN` (must be public).
   * Elastic IP: select the one just allocated.
   * Name: `Demo-NATGW`.
3. Create; status becomes _Available_ in \~1 minute.

Cost preview (us-east-1): $0.045/hour + $0.045/GB. Emphasize to students and remember to delete post-lab.

### Route Table for Private Subnet <a href="#route-table-for-private-subnet" id="route-table-for-private-subnet"></a>

1. _Route Tables_ → **Create route table**. Name: `Private-RT`; VPC: `Demo-VPC`.
2. Add route: Destination **0.0.0.0/0**, Target **NAT Gateway** → `Demo-NATGW`.
3. Associate `Private-SN` with `Private-RT`.

Instances in `Private-SN` now have outbound connectivity while remaining unreachable from the internet.

### Launch Test EC2 Instances <a href="#launch-test-ec2-instances" id="launch-test-ec2-instances"></a>

### Web Server in Public Subnet

* _EC2_ → **Launch instance** → Name `Web-Server`.
* AMI: Amazon Linux 2025, t2.micro (free tier).
* Network: `Demo-VPC`; Subnet: `Public-SN`; Auto-assign public IP: **Enable**.
* Security group: allow TCP 22 (SSH) & TCP 80 (HTTP) from _0.0.0.0/0_.
* Launch. Copy the public IPv4 address.

### DB Server in Private Subnet

* Launch another instance `DB-Server`.
* Subnet: `Private-SN`; Auto-assign public IP: **Disable**.
* SG: allow TCP 22 **from Web-Server’s security group ID**, TCP 3306 from Web-Server SG.

Explain that SSH to DB requires hopping via the public instance or Systems Manager Session Manager (preferred in production).

### Validate Connectivity <a href="#validate-connectivity" id="validate-connectivity"></a>

1. SSH into Web-Server (`ssh ec2-user@<public-ip>`).
2. From Web-Server, `curl amazon.com` succeeds (IGW path).
3. `ssh ec2-user@10.0.2.<DB-private-ip>` succeeds (within VPC).
4. From DB-Server (`ping 8.8.8.8`) → success via NAT Gateway.
5. Attempt inbound SSH from laptop directly to DB-Server → fails, proving private isolation.

### Real-Time Scenario: Two-Tier Web App <a href="#real-time-scenario-two-tier-web-app" id="real-time-scenario-two-tier-web-app"></a>

| Layer         | Location   | Traffic Path                                    | Reasoning                   |
| ------------- | ---------- | ----------------------------------------------- | --------------------------- |
| User Browser  | Internet   | HTTPS→IGW→ALB in Public-SN                      | Public entrypoint           |
| App EC2       | Public-SN  | ALB→instance; outbound software updates via IGW | Needs global reach          |
| MySQL RDS     | Private-SN | App-SG→3306                                     | No direct internet exposure |
| Patch Scripts | Private-SN | NATGW→Yum repos                                 | Secure outbound only        |

Students can deploy WordPress on Web-Server pointing to MySQL on DB-Server, then observe that breaking the NAT route blocks plugin updates while the site remains accessible.

### Clean-Up Checklist <a href="#clean-up-checklist" id="clean-up-checklist"></a>

* Terminate EC2 instances.
* Delete NAT Gateway (releases Elastic IP).
* Detach & delete IGW.
* Delete subnets and route tables.
* Finally delete the VPC.

Stress the importance of removing cost-incurring resources like NAT Gateways after class.

### Teaching Tips <a href="#teaching-tips" id="teaching-tips"></a>

### Whiteboard Bitmask Math

Tie back to the earlier CIDR lesson: show that /24 subnets 10.0.1.0/24 and 10.0.2.0/24 fit neatly inside the /16 VPC.

### CLI/Terraform Parallel

Demonstrate `aws ec2 create-vpc`, `create-subnet`, and `associate-route-table` commands or show equivalent Terraform code to instill IaC best practices.

### Cost-Awareness Exercise

Have students calculate monthly NAT costs for 100 GB egress and explore using VPC endpoints to avoid them.

### Key Takeaways <a href="#key-takeaways" id="key-takeaways"></a>

* **Public subnet = route to IGW; Private subnet = no IGW route.**
* **NAT Gateway** offers outbound-only internet for private resources but incurs hourly and per-GB fees.
* **Route tables** are the brain of the VPC; every subnet association explicitly defines traffic flow.
* Proper isolation protects databases, reduces attack surface, and can lower data-transfer spend.

With these building blocks, learners can design multi-AZ architectures, add VPN/Direct Connect, or integrate VPC endpoints for a production-ready network foundation.

explain the components of this website https://medium.com/@mrdevsecops/vpcs-components-213c4977f7daexplain the components of this website https://medium.com/@mrdevsecops/vpcs-components-213c4977f7da

## VPC Components Explained: Complete Breakdown for Students <a href="#vpc-components-explained-complete-breakdown-for-st" id="vpc-components-explained-complete-breakdown-for-st"></a>

This Medium article by MrDevSecOps provides a comprehensive overview of AWS Virtual Private Cloud (VPC) components, perfect for classroom demonstrations. The article organizes VPC architecture into three main categories: **Core Networking Components**, **Security Components**, and **IP Address Management**. Here's how you can use this resource to teach your students about VPC fundamentals.

### Core Networking Components <a href="#core-networking-components" id="core-networking-components"></a>

### Internet Gateway (IGW)

The article explains that an **Internet Gateway is a logical connection between an Amazon VPC and the Internet**. Key teaching points include:

* **Not a physical device** - it's a software construct that enables internet connectivity
* **One-to-one relationship** - only one IGW can be associated with each VPC
* **Manual attachment required** - creating an IGW doesn't automatically attach it to the VPC
* **No bandwidth limitations** - the only bandwidth restriction comes from EC2 instance sizes, not the IGW itself

**Demonstration Tip:** Show students that without an IGW, VPC resources cannot be accessed from the internet, making this the critical component for public connectivity.

### AWS Router and Route Tables

The article describes the **VPC router as the traffic director** that uses route tables to determine where network packets should go. This connects perfectly with your previous CIDR lesson:

**Route Table Fundamentals:**

* **Set of rules (routes)** that determine network traffic direction
* **Destination and Target pairs** - where packets are trying to go and where they should be sent
* **One-to-one subnet association** - each subnet associates with exactly one route table, but multiple subnets can share the same route table
* **Default local route** - every route table includes automatic local routing within the VPC

**Two Types of Route Tables:**

| Route Table Type       | Association                      | Key Route                       | Purpose                     |
| ---------------------- | -------------------------------- | ------------------------------- | --------------------------- |
| **Main Route Table**   | Default for unassociated subnets | Local + NAT Gateway routes      | Private subnet connectivity |
| **Custom Route Table** | Explicitly associated subnets    | Local + Internet Gateway routes | Public subnet connectivity  |

**Public Route Table Configuration:**

* **Destination:** 0.0.0.0/0 (all internet traffic)
* **Target:** Internet Gateway ID (e.g., igw-1a2b3c4d)

**Private Route Table Configuration:**

* **Destination:** 0.0.0.0/0 (all internet traffic)
* **Target:** NAT Gateway ID (e.g., nat-1a2b3c4d)

### NAT Gateway

The article emphasizes that **NAT Gateway enables outbound internet connectivity for private instances while preventing inbound internet connections**. Essential characteristics include:

* **Resides in public subnets** - must be placed in a subnet with internet access
* **Requires Elastic IP** - must be attached during creation
* **IPv4 only** - doesn't support IPv6 traffic (use egress-only IGW instead)
* **Unidirectional connectivity** - allows outbound but blocks inbound internet connections

### Subnets

The article defines subnets as **logical subdivisions of the VPC network with specific IP address ranges**:

**Public Subnets:**

* **Associated with route tables that route to Internet Gateway**
* **Resources can access the internet directly**
* **Can receive inbound internet traffic** (when security groups allow)

**Private Subnets:**

* **Associated with route tables that route to NAT Gateway**
* **Outbound internet access only** - cannot receive direct inbound connections
* **Database and application server placement** - ideal for backend resources
* **Access methods:** VPN or bastion host in public subnet

### Security Components <a href="#security-components" id="security-components"></a>

### Security Groups

The article describes Security Groups as **instance-level firewalls** with these characteristics:

* **Instance-level protection** - not applied at subnet level
* **Stateful operation** - responses to allowed requests are automatically permitted
* **Allow rules only** - cannot specify deny rules
* **Default behavior** - allows all outbound traffic
* **Flexible assignment** - each instance can have different security group combinations

### Network Access Control Lists (NACLs)

**Subnet-level firewalls** that complement security groups:

* **Stateless operation** - must explicitly allow both inbound and outbound traffic
* **Subnet-level application** - applies to all instances in associated subnets
* **Default NACL** - created automatically with VPC, allows all IPv4 traffic
* **Custom NACLs** - deny all traffic by default until rules are added
* **Additional security layer** - works alongside security groups for defense in depth

### VPC Flow Logs

**Network monitoring and troubleshooting tool**:

* **Traffic capture** - records IP traffic information for network interfaces
* **CloudWatch integration** - stores log data in Amazon CloudWatch Logs
* **Multiple scopes** - can monitor entire VPC, specific subnets, or individual network interfaces
* **Troubleshooting support** - helps diagnose connectivity issues
* **Not real-time** - doesn't provide live traffic streams

### IP Address Management <a href="#ip-address-management" id="ip-address-management"></a>

### Private IP Addresses

**Internal VPC communication addresses**:

* **Internet isolation** - not reachable from the internet
* **Automatic assignment** - all instances receive private IPs within subnet range
* **Persistent during lifecycle** - retained through stops/starts, released on termination
* **Secondary IPs** - additional private IPs can be assigned and transferred between interfaces

### Public IP Addresses

**Internet-accessible addresses with important limitations**:

* **Internet reachability** - enables direct internet communication
* **Subnet dependency** - assignment depends on subnet's public IP addressing setting
* **Instance-level override** - can be enabled during instance creation
* **AWS pool assignment** - comes from AWS's IP address pool
* **Non-persistent** - released when instance stops, restarts, or terminates

### Elastic IP Addresses

**Static, persistent public IP addresses**:

* **Account ownership** - allocated to VPC and owned by account until released
* **Transferable** - can be moved between instances within the same account
* **Mutual exclusivity** - instances can have either public IP or Elastic IP, not both
* **Cross-VPC mobility** - can be transferred between VPCs in the same account
* **Cost implications** - charged for non-usage (unassociated or associated with stopped instances)

### Teaching Applications <a href="#teaching-applications" id="teaching-applications"></a>

### Connecting to Your VPC Lab

This article perfectly complements your hands-on VPC demonstration by providing the theoretical foundation. Use it to:

1. **Pre-lab reading assignment** - have students read this before the practical session
2. **Reference during demonstration** - refer back to specific concepts while creating resources
3. **Post-lab reinforcement** - use for review and deeper understanding of what was built

### Real-World Architecture Context

The article's explanations help students understand **why** each component exists:

* **Internet Gateway** enables the public-facing web tier
* **NAT Gateway** secures the private application/database tiers
* **Route Tables** control traffic flow and enforce security boundaries
* **Security Groups + NACLs** provide layered defense
* **IP addressing** supports both internal communication and external access

### Assessment Opportunities

Use the article's component descriptions to create practical questions:

* "Explain why a NAT Gateway must reside in a public subnet"
* "Compare stateful Security Groups with stateless NACLs"
* "Describe the difference between Main and Custom Route Tables"
* "When would you choose Elastic IP over Public IP?"

This resource provides the perfect theoretical foundation for your practical VPC demonstration, ensuring students understand both the "how" and "why" of AWS networking architecture.
