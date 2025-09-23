# NOTES AWS

<p align="center">AMAZON WEB SERVICE</p>

What is AWS?

Amazon web service is an online platform that provides scalable and cost-effective cloud computing solutions.

AWS is a broadly adopted cloud platform that offers several on-demand operations like compute power, database storage, content delivery

What is Cloud Computing?

Cloud Computing is the delivery of online services (such as servers, databases, software) to users. With the help of cloud computing, storing data on local machines is not required. It helps you access data from a remote server. Moreover, it is also used to store and access data from anywhere across the world.

Applications of AWS

1\. Storage and Backup

2\. Websites

3\. Gaming

4\. Mobile, Web and Social Applications

5\. Big Data Management and Analytics (Application)

7\. Messages and Notifications

9\. Game Development

AWS Services

·       Compute service

·       Storage

·       Database

·       Networking and delivery of content

·       Security tools

·       Developer tools

·       Management tools

AWS EC2

It is a web service that allows developers to rent virtual machines and automatically scales the compute capacity when required.

It offers various instance types to developers so that they can choose required resources such as CPU, memory, storage, and networking capacity based on their application requirements.

VPC

It helps a developer to deploy AWS resources, such as Amazon EC2 instances into a [private virtual cloud.](https://www.simplilearn.com/tutorials/aws-tutorial/aws-vpc)

It gives you control over the complete cloud network environment, including the section of your [IP address](https://www.simplilearn.com/tutorials/cyber-security-tutorial/what-is-an-ip-address) range, subnets, route table configuration, and network gateways.

With this, developers can both IPv4 and IPv6 at a time for your resources in a highly secure environment.

There are two types of VPCs in AWS, namely:

Default VPC :

Each Amazon account comes with a default VPC that is pre-configured for you to start using immediately.

User-defined VPC:

Created by users as per their requirements. creating a custom VPC allows you to:

·       Make things more secure

·       Customize your virtual network, as you can define your own our IP address range&#x20;

·       Create your subnets that are both private and public

·       Tighten security settings

&#x20;

Architecture of VPC

![VPC](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.jpg)

Inside the region, we have VPC, and outside the VPC, we have internet gateway and virtual private gateway. Internet Gateway and Virtual Private Gateway are the ways of connecting to the VPC. Both these connections go to the router in a VPC and then router directs the traffic to the route table. Route table will then direct the traffic to Network ACL. Network ACL is the firewall or much like security groups. Network ACL are statelist which allows as well as deny the roles. You can also block the IP address on your Network ACL. Now, move over to the security group that accesses another line against the EC2 instance. It has two subnets, i.e., Public and Private subnet. In public subnet, the internet is accessible by an EC2 instance, but in private subnet, an EC2 instance cannot access the internet on their own. We can connect the instances. To connect an instance, move over to the public subnet and then it SSH to the private subnet. This is known as jump boxes. In this way, we can connect an instance in public subnet to an instance in private subnet.

CIDR

Classless Inter-Domain Routing – a method for allocating IP addresses

Used in Security groups and AWS networking in general

Help define IP ranges

&#x20;

CIDR has Two components

·       Base IP – XXX.XXX.XXX.XXX

Represent an IP contained

·       Subnet mask - ./32

Define how many can change in IP

192.168.0.0/32  -           allow for 1 IP                            -           192.168.0.0

192.168.0.0/24  -           allow for 256 IP                        -           192.168.0.255

192.168.0.0/16  -           last two octets can change          -        192.168.255.255

192.168.0.0/8    -           last three octets can change        -        192.255.255.255

192.168.0.0/0    -           all octets can change                  -        255.255.255.255

&#x20;

[https://www.ipaddressguide.com/cidr.aspx](https://www.ipaddressguide.com/cidr.aspx)

&#x20;

Private, Public, and Elastic IP Addresses

Private IP addresses : IP addresses not reachable over the internet are defined as private. Private IPs enable communication between instances in the same network.

Public IP addresses : Public IP addresses are used for communication between other instances on the internet. Public IP addresses linked to your instances are from Amazon's list of public IPs. On stopping or terminating your instance, the public IP address gets released, and a new one is linked to the instance when it restarts.

Elastic IP Addresses : Elastic IP addresses are static or persistent public IPs that come with your account. If any of your software or instances fail, they can be remapped to another instance quickly with the elastic IP address. An elastic IP address remains in your account until you choose to release it.

&#x20;

&#x20;

&#x20;

&#x20;

Subnet

<p align="center"><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image004.jpg" alt=""></p>

A subnet is a subdivision of a network. When a network is broken down into smaller sub networks or subnets, that process is called subnetting.

AWS reserves 5 IP addresses in each subnet (first 4 & last 1)

Eg: 10.0.0.0/24

&#x20;           10.0.0.0  -         Network address

10.0.0.1  -         reserved by AWS for VPC router

10.0.0.2  -         reserved by AWS for mapping to Amazon-provider DNS

10.0.0.3  -         reserved by AWS for future use

10.0.0.255  -     Network Broadcast Address

&#x20;

_Public subnets:_ A public subnet is used for resources that must be connected to the internet; web servers are an example. A public subnet is made public because the main route table sends the subnets traffic that is destined for the internet to the internet gateway.

_Private subnets :_ Private subnets are for resources that don't need an internet connection, or that you want to protect from the internet; database instances are an example.

Networking

Internet Gateway

An internet gateway enables communication between instances in your VPC and the internet. To give your VPC the ability to connect to the internet, you need to attach an internet gateway. Only one internet gateway can be attached per VPC. Attaching an internet gateway is the first stage in permitting internet access to instances in your VPC.

·       Attach an Internet gateway to your VPC&#x20;

·       Ensure that your instances have either a public IP address or an elastic IP address&#x20;

·       Point your subnet’s route table to the internet gateway&#x20;

·       Make sure that your security group and network access control rules allow relevant traffic to flow in and out of your instance

<p align="center"><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image006.jpg" alt=""></p>

Route Table

Route table as a set of rules, called routes, which are used to determine where network traffic is directed.

Each subnet has to be linked to a route table, and a subnet can only be linked to one route table. On the other hand, one route table can have associations with multiple subnets. Route table to customize the network traffic routes associated with your VPC. The route table diagram is as shown:

<p align="center"><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image007.jpg" alt="custom-route-table.jpg"></p>

Bastion Hosts

<p align="center"><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image009.jpg" alt=""></p>

We can use Bastion Host to SSH into our Private EC2 instances

The Bastion is in the public subnet which is then connected to all private subnets

Bastion hosts security must be Max

&#x20;

NAT Devices

<p align="center"><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image011.jpg" alt=""></p>

A Network Address Translation (NAT) device can be used to enable instances in a private subnet to connect to the internet or the AWS services, but this prevents the internet from initiating connections with the instances in a private subnet.

As mentioned earlier, public and private subnets protect your assets from being directly connected to the internet. For example, your web server would sit in the public subnet and database in the private subnet, which has no internet connectivity. However, your private subnet database instance might still need internet access or the ability to connect to other AWS resources. You can use a NAT device to do so.&#x20;

The NAT device directs traffic from your private subnet to either the internet or other AWS services. It then sends the response back to your instances. When traffic is directed to the internet, the source IP address of your instance is replaced with the NAT device address, and when the internet traffic returns, the NAT device translates the address to your instance’s private IP address.

NAT Gateway

<p align="center"><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image013.jpg" alt=""></p>

<p align="center"><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image015.jpg" alt=""></p>

To Create NAT Gateway

AWS Console – NAT Gateway – Name – Public subnet – ElasticIP – Route Table – Allow all 0.0.0.0/0 from NAT Gateway

Security Groups

Security group as a virtual [firewall](https://www.simplilearn.com/tutorials/cyber-security-tutorial/what-is-firewall) that controls the traffic for one or more instances. Rules are added to each security group, which allows traffic to or from its associated instances. Basically, a security group controls inbound and outbound traffic for one or more EC2 instances.

<p align="center"><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image017.jpg" alt=""></p>

&#x20;

&#x20;

&#x20;

&#x20;

Network ACL ( NACL )

The Network Access Control List (ACL) is an optional security layer for your VPC. It acts as a firewall for controlling traffic flow to and from one or more subnets. Network ACLs can be set up with rules similar to your security groups

<p align="center"><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image019.jpg" alt=""></p>

&#x20;

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Security Group</td><td valign="top">NACL (Network Access Control List)</td></tr><tr><td valign="top">It supports only allow rules, and by default, all the rules are denied. You cannot deny the rule for establishing a connection.</td><td valign="top">It supports both allow and deny rules, and by default, all the rules are denied. You need to add the rule which you can either allow or deny it.</td></tr><tr><td valign="top">It is a stateful means that any changes made in the inbound rule will be automatically reflected in the outbound rule. For example, If you are allowing an incoming port 80, then you also have to add the outbound rule explicitly.</td><td valign="top">It is a stateless means that any changes made in the inbound rule will not reflect the outbound rule, i.e., you need to add the outbound rule separately. For example, if you add an inbound rule port number 80, then you also have to explicitly add the outbound rule.</td></tr><tr><td valign="top">It is associated with an EC2 instance.</td><td valign="top">It is associated with a subnet.</td></tr><tr><td valign="top">All the rules are evaluated before deciding whether to allow the traffic.</td><td valign="top">Rules are evaluated in order, starting from the lowest number.</td></tr><tr><td valign="top">Security Group is applied to an instance only when you specify a security group while launching an instance.</td><td valign="top">NACL has applied automatically to all the instances which are associated with an instance.</td></tr><tr><td valign="top">It is the first layer of defense.</td><td valign="top">It is the second layer of defense.</td></tr></tbody></table>

VPC Peering

<figure><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image021.jpg" alt=""><figcaption></figcaption></figure>

A peering connection can be made between your own VPCs or with a VPC in another AWS account, as long as it is in the same region.

AWS VPC Peering is a functionality that enables two private networks to communicate with each other by building fast and reliable connections. AWS VPC peering connections can be used to route traffic from one VPC to another VPC network or to provide access to resources of one network to another. Peering actually allows traffic between two VPCs based on a specific resource’s network address.

If you have instances in VPC A, they wouldn't be able to communicate with instances in VPC B or C unless you set up a peering connection. Peering is a one-to-one relationship; a VPC can have multiple peering connections to other VPCs, but transitive peering is not supported. In other words, VPC A can connect to B and C in the above diagram, but C cannot communicate with B unless directly paired.

<figure><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image023.jpg" alt=""><figcaption></figcaption></figure>

VPC Endpoints

[VPC endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) enables creation of a private connection between VPC to supported AWS services and VPC endpoint services powered by PrivateLink using its private IP address. Traffic between VPC and AWS service does not leave the Amazon network.

VPC Endpoints Key points

1\.     VPC endpoint enables users to privately connect their VPC to supported AWS services.

2\.     VPC Endpoint does not require a public IP address, access over the Internet, NAT device, a VPN connection or AWS Direct Connect to communicate with resources in the service.

<figure><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image025.jpg" alt=""><figcaption></figcaption></figure>

<figure><img src="file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image027.jpg" alt=""><figcaption></figcaption></figure>

EBS

·       EBS stands for Elastic Block Store

·       Amazon EBS allows you to create storage volumes and attach them to the EC2 instances.

·       EBS volumes are specific to availability zones and can only be attached to instances within the same availability zone.&#x20;

·       Amazon EBS provides persistent block storage for our instance

EBS Volume Types

_SSD_ : SSD stands for solid-state Drives. It is a general purpose storage SSD storage is very high performing, but it is quite expensive as compared to HDD It supports up to 4000 IOPS which is quite very high.

_HDD_ : It stands for Hard Disk Drive. The size of the HDD based storage could be between 1 GB to 1TB. It can support up to 100 IOPS which is very low.

_Throughput Optimized HDD (st1)_ : It is used for Big data, Data warehouses, Log processing, etc. It supports up to 500 IOPS

_Cold HDD (sc1)_ : It is useful when data is rarely accessed. The size of the Cold Hard disk can be 500 GiB to 16 TiB. It supports up to 250 IOPS

#### _Magnetic Volume_ : It is the lowest cost storage per gigabyte of all EBS volume types

&#x20;

_ubuntu@ubuntu:\~$_ sudo df -hT

![https://linuxhint.com/wp-content/uploads/2021/08/13-16.jpg](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image028.jpg)

_ubuntu@ubuntu:\~$_ sudo lsblk

![https://linuxhint.com/wp-content/uploads/2021/08/14-15.jpg](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image029.jpg)

_ubuntu@ubuntu:\~$_ sudo growpart /dev/xvda 1

![https://linuxhint.com/wp-content/uploads/2021/08/19-8.jpg](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image030.jpg)

_ubuntu@ubuntu:\~$_ sudo lsblk

![https://linuxhint.com/wp-content/uploads/2021/08/16-13.jpg](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image031.jpg)

### Extent filesystem

_ubuntu@ubuntu:\~$_ sudo df -hT

![https://linuxhint.com/wp-content/uploads/2021/08/17-9.jpg](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image032.jpg)

### Extend ext4 filesystem

_ubuntu@ubuntu:\~$_ sudo resize2fs /dev/xvda1

![https://linuxhint.com/wp-content/uploads/2021/08/18-9.jpg](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image033.jpg)

_ubuntu@ubuntu:\~$_ sudo df -hT

![https://linuxhint.com/wp-content/uploads/2021/08/19-9.jpg](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image030.jpg)

AMI

·       An AMI stands for **Amazon Machine Images**.

·       An AMI is a virtual image used to create a virtual machine within an EC2 instance.

·       You can also create multiple instances using single AMI when you need instances with the same configuration.

·       You can also create multiple instances using different AMI when you need instances with a different configuration.

·       It also provides a template for the root volume of an instance.

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image035.jpg)

&#x20;

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image037.jpg)

&#x20;

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image039.jpg)

&#x20;

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image041.jpg)

&#x20;

&#x20;

&#x20;

EBS Snapshot

·       An EBS snapshot is a point-in-time copy of your Amazon EBS volume.

·       copied to Amazon Simple Storage Service (Amazon S3).

·       EBS snapshots are incremental copies of data. This means that only unique blocks of EBS volume data that have changed since the last EBS snapshot are stored in the next EBS snapshot.

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image043.jpg)

&#x20;

### Diffrence between Snapshots and AMI

| Snapshots                                                                  | AMI                                                  |
| -------------------------------------------------------------------------- | ---------------------------------------------------- |
| It is used as a backup of a single EBS volume attached to the EC2 instance | It is used as a backup of an EC2 instance            |
| Opt for this when the instance contains multiple static EBS volumes        | This is widely used to replace a failed EC2 instance |
| Here, pay only for the storage of the modified data                        | Here, pay only for the storage that you use          |
| It is a non-bootable image on EBS volume                                   | It is a bootable image on an EC2 instance            |

&#x20;

What is S3?

·       S3 stands for Simple Storage Service.

·       S3 provides developers and IT teams with secure, durable, highly scalable object storage.

·       It is easy to use with a simple web services interface to store and retrieve any amount of data from anywhere on the web.

·       S3 is a safe place to store the files.

·       It is Object-based storage, i.e., you can store the images, word files, pdf files, etc.

·       The files which are stored in S3 can be from 0 Bytes to 5 TB.

·       If you create a bucket, URL look like:

![AWS S3](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image045.jpg)

&#x20;

### Types of S3 Storage Classes

These provide us the storage for data that is rarely used, doesn’t require instant access, long-term archive, digital preservation, and many more.

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image047.jpg)

1\.     S3 Standard – General Purpose

S3 Standard is the default storage class if none of the storage class is specified during upload. It is ideal for frequently accessed data because it provides low latency and high availability.

2\.     S3 Standard-Infrequent Access

S3 Standard-Infrequent Access is optimized for long-lived and less frequently accessed data but requires rapid access whenever required. Similar to S3 Standard,

3\.     S3 One Zone- Infrequent Access

S3 One Zone- Infrequent Access is for the data that is accessed less frequently but available for millisecond access

4\.     S3 Glacier Instant Retrieval

S3 Glacier Instant Retrieval delivers the fastest access to archive storage.

5\.     S3 Glacier Flexible Retrieval

It is a suitable solution for backing up the data so that it can be recovered easily a few times in a year.

6\.     S3 Glacier Deep Archive

That may be accessed only once or twice in a year. It is ideal for those industries which store data for 5-10 years

7\.     S3 Intelligent-Tiering

It moves objects that have not been accessed for 30 consecutive days to the infrequent access tier

AWS S3 Versioning

Versioning automatically keeps up with different versions of the same object.

Turn it on in a bucket and all objects in that bucket automatically use versioning

Any new object uploaded to that bucket will receive a Version ID.

If we try to delete the object, all versions will stay in the bucket, but S3 will insert a delete marker at the latest version of object

AWS S3 Encryption

Data encryption is a process for securing data by encoding information. Data is encoded using a password or an encryption (cypher) key and special encryption algorithms.

If an unauthorized person gets access to the encrypted data, the data is unreadable without the key or password.

S3 Encryption Types

Server-side encryption

Encryption operations are performed on the server side in the AWS cloud. You send unencrypted data to AWS and then data is encrypted on the AWS side when recorded on the cloud storage.

**SSE-S3 :** The keys are managed and handled by AWS to encrypt the data AES-256 is used as the encryption algorithm

**SSE-KMS :** AWS Key Management Service (KMS) is used to encrypt S3 data on the Amazon server side. The data key is managed by AWS but a user manages the customer master key (CMK) in AWS KMS.

**SSE-C** : keys are provided by a customer and AWS doesn’t store the encryption keys. A user must ensure the safety of the keys.

Client-side encryption

The client is responsible for all encryption operations. Data is not encrypted by AWS.

Two options are provided for S3 client-side encryption

A master key can be stored on the client side or on the server side.

If a master key is stored on the client side, the client takes full responsibility for encryption.

&#x20;AWS S3 Security

AWS S3 is the only object storage service that allows us to block public access to all of our objects at the bucket level or the account level with S3 Block Public Access.

User based security :

IAM Policies : IAM users have IAM policies, and they authorize which API calls should be allowed and if user is authorized through IAM policy to access our Amazon S3 bucket then they able to access

Resource based :

Bucket policies :

·       They're bucket-wise rules that we can set in the S3 console

·       they will say what principals can and cannot do on our S3 bucket.

Object Access control list (ACL) : we set at the object level access rule.

Bucket ACL : we set at the Bucket level access rule

AWS S3 Terminology:

·       Bucket: Data, in S3, is stored in containers called _buckets_.

* Each bucket will have its own set of policies and configuration. This enables users to have more control over their data.
* Bucket Names must be unique.
* Can be thought of as a parent folder of data.
* There is a limit of 100 buckets per AWS accounts. But it can be increased if requested from AWS support.

·       Bucket Owner: The person or organization that owns a particular bucket is its _bucket owner_.

·       Import/Export Station:  A machine that uploads or downloads data to/from S3.

·       Key: Key, in S3, is a unique identifier for an object in a bucket. For example in a bucket ‘ABC’ your _GFG.java_ file is stored at _javaPrograms/GFG.java_ then _‘javaPrograms/GFG.java’_ is your object key for _GFG.java_.

* It is important to note that ‘bucketName+key’ is unique for all objects.
* This also means that there can be only one object for a key in a bucket. If you upload 2 files with the same key. The file uploaded latest will overwrite the previously contained file.

·       Versioning:  Versioning means to always keep a record of previously uploaded files in S3. Points to note:

* Versioning is not enabled by default. Once enabled, it is enabled for all objects in a bucket.
* Versioning keeps all the copies of your file, so, it adds cost for storing multiple copies of your data. For example, 10 copies of a file of size 1GB will have you charged for using 10GBs for S3 space.
* Versioning is helpful to prevent unintended overwrites and deletions.
* Note that objects with the same key can be stored in a bucket if versioning is enabled (since they have a unique version ID).

·       null Object: Version ID for objects in a bucket where versioning is suspended is null. Such objects may be referred to as null objects.

* For buckets with versioning enabled, each version of a file has a specific version ID.

·       Object: Fundamental entity type stored in AWS S3.

·       Access Control Lists (ACL): A document for verifying the access to S3 buckets from outside your AWS account. Each bucket has its own ACL.

·       Bucket Policies: A document for verifying the access to S3 buckets from within your AWS account, this controls which services and users have what kind of access to your S3 bucket. Each bucket has its own Bucket Policies.

·       Lifecycle Rules: This is a cost-saving practice that can move your files to AWS Glacier (The AWS Data Archive Service) or to some other S3 storage class for cheaper storage of old data or completely delete the data after the specified time.

&#x20;

AWS S3 Replication

Replication helps you to copy data from one S3 bucket automatically without blocking operations. AWS S3 Replication can Replicate data across the different source and destination buckets irrespective of the account or region they belong to.

AWS S3 Replication Rules

Replication rules define the source bucket objects to replicate and the destination bucket or buckets where the replicated objects are stored.

IAM

·       IAM stands for Identity Access Management.

·       AWS Identity and Access Management (IAM) is a web service for securely controlling access to AWS resources. It enables you to create and control services for user authentication or limit access to a certain set of people who use our AWS resources.

How Does IAM Work?

·       A principal is an entity that can perform actions on an AWS resource. A user, a role or an application can be a principal.

·       Authentication is the process of confirming the identity of the principal trying to access an AWS product. The principal must provide its credentials or required keys for authentication.

·       Request: A principal sends a request to AWS specifying the action and which resource should perform it.

·       Authorization: By default, all resources are denied. IAM authorizes a request only if all parts of the request are allowed by a matching policy. After authenticating and authorizing the request, AWS approves the action.

·       Actions are used to view, create, edit or delete a resource.

·       Resources: A set of actions can be performed on a resource related to your AWS account.

&#x20;

Components of IAM

Users

An IAM user is an identity with an associated credential and permissions attached to it. This could be an actual person who is a user, or it could be an application that is a user. With IAM, you can securely manage access to AWS services by creating an IAM user name for each employee in your organization. Each IAM user is associated with only one AWS account. The advantage of having one-to-one user specification is that you can individually assign permissions to each user.

Groups

A collection of IAM users is an IAM group. You can use IAM groups to specify permissions for multiple users so that any permissions applied to the group are applied to the individual users in that group as well. Managing groups is quite easy. You set permissions for the group, and those permissions are automatically applied to all the users in the group. If you add another user to the group, the new user will automatically inherit all the policies and the permissions already assigned to that group

Policies

An IAM policy sets permission and controls access to AWS resources. Policies are stored in AWS as JSON documents. Permissions specify who has access to the resources and what actions they can perform.

For example, a policy could allow an IAM user to access one of the buckets in [Amazon S3](https://www.simplilearn.com/tutorials/aws-tutorial/aws-s3).

The policy would contain the following information:

1\.     Who can access it

2\.     What actions that user can take

3\.     Which AWS resources that user can access

4\.     When they can be accessed

There are 3 different types of  IAM policies available:

1\.     Managed Policies

2\.     Customer Managed Policies

3\.     Inline Policies

&#x20;

&#x20;

&#x20;

Managed Policy

A Managed Policy is an IAM policy which is created and administered by AWS. AWS provided Mnaged Policies for common use cases

Assign appropriate permissions to your users, groups, and roles without having to write the policy yourself.

A single Managed Policy can be attached to multiple users, groups, or roles within the same AWS account and across different accounts.

You cannot change the permissions defined in an AWS Managed Policy.

Customer Managed Policy

A Customer Managed Policy is a standalone policy that you create and administer inside your own AWS account. You can attach this policy to multiple users, groups, and roles; but only within your own account.

In order to create a Customer Managed Policy, you can copy an existing AWS Managed Policy and customize it to fit the requirements of your organization.

Inline Policy

An Inline Policy is an IAM policy which is actually embedded within the user, group, or role to which it applies. There is a strict 1:1 relationship between the entity and the policy.

When you delete the user, group, or role in which the Inline policy is embedded, the policy will also be deleted.

Roles

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image049.jpg)

An IAM role is a set of permissions that define what actions are allowed and denied by an entity in the AWS console. It is similar to a user in that it can be accessed by any type of entity (an individual or AWS service). Role permissions are temporary credentials.

For example, you might want to allow a mobile app to use AWS resources, but you do not want it to save the key, credential or password. Or you might want to give access to resources to a user who already has an identity defined outside of AWS, such as a user who already has Google or Facebook authentication. If you want to provide someone with a service or let someone access resources in your account, you can use roles for that purpose too. You also might want to grant temporary access to your account to a third party, such as a consultant or an auditor. They’re not permanent users, just users with temporary access to your environment.

AWS Auto Scaling

AWS Auto Scaling is a service that helps the user to automatically monitors applications and adjusts compute resources to maintain steady, predictable performance for applications hosted in the Amazon Web Services ([AWS](https://www.techtarget.com/searchaws/definition/Amazon-Web-Services)) public cloud.

AWS Auto Scaling automatically discovers and tracks the performance of all the scalable resources -- which can span various cloud services -- that support a user's application. These resources include Elastic Compute Cloud (EC2) Auto Scaling groups, Amazon Elastic Container Service ([ECS](https://www.techtarget.com/searchaws/definition/Amazon-EC2-Container-Service)) components              As demand spikes, the AWS Auto Scaling service can automatically scale those resources, and, as demand drops, scale them back down.

Benefits of Auto Scaling

·       Better fault tolerance

·       High availability of resources

·       Better cost management

·       High reliability of resources

·       The high flexibility of resources

How Does AWS Auto Scaling work?

·       Configure a single unified scaling policy per application source.

·       Explore the application and create a system that adds and removes EC2 instances in response to the requirement.

·       Choose the service that you want to scale up or down.

·       Select what to optimize. Based on a schedule, scale your application in response to predictable load changes.&#x20;

·       Keep tracing the scaling load and maintain a steady count of instances.

&#x20;

&#x20;

Load Balancer

A load balancer acts as the “traffic police” sitting in front of your servers and routing client requests across all servers capable of fulfilling those requests in a manner that maximizes speed and capacity utilization and ensures that no one server is overworked, which could degrade performance.

Elastic Load Balancing (ELB) is a [load-balancing](https://www.techtarget.com/searchnetworking/definition/load-balancing) service for [Amazon Web Services](https://www.techtarget.com/searchaws/definition/Amazon-Web-Services) (AWS) deployments. ELB automatically distributes incoming [application](https://www.techtarget.com/searchsoftwarequality/definition/application) traffic and scales resources to meet traffic demands.

ELB helps adjust capacity according to incoming application and network traffic. Users enable ELB within a single availability zone or across multiple availability zones to maintain consistent application performance.

Types of Load Balancers

·       Classic load balancer

·       Application load balancer

·       Network load balancer

Classic Load Balancer

·       It is widely used in EC2 instances and is a basic type of load balancer.

·       Based on IP address and TCP port, the classic balancer routes the traffic between backend servers and users.

·       This balancer does not support host-based routing and results in low efficiency of resources.

#### Application Load Balancer

·       It is responsible for performing the tasks in the application layer of the OSI model and is the advanced form of load balancing.

·       It is used when there are HTTP and HTTPS traffic routing.

·       It supports host-based and path-based routing.

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image051.jpg)

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image053.jpg)

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image055.jpg)

![](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image057.jpg)

&#x20;

#### Network Load Balancer

·       It performs the task at layer 4 of the connection level in the OSI model.

·       Its primary purpose is to route TCP traffic.

·       It can manage a massive amount of traffic and is suitable for managing low latencies.

## Differences ALB - NLB

·       Application Load Balancer works at the Application Layer (Layer 7 of the OSI model, Request level).  Supported protocol like HTTP

·       Network Load Balancer works at Transport layer (Layer 4 of the OSI model, Connection level). Supported protocol like TCP UDP

·       NLB just forward requests whereas

·       ALB examines the contents of the HTTP request header to determine where to route the request. So, application load balancer is performing content based routing.

·       NLB cannot assure availability of the application. This is because it bases its decisions solely on network and TCP-layer variables and has no awareness of the application at all. Generally a NLB determines availability based on the ability of a server to respond to ICMP ping, or to correctly complete the three-way TCP handshake. ALB goes much deeper, and is capable of determining availability based on not only a successful HTTP GET of a particular page but also the verification that the content is as was expected based on the input parameters.

·       When considering the deployment of multiple applications on the same host sharing IP addresses (virtual hosts in old school speak), NLB will not differentiate between Application A and Application B when checking availability (indeed it cannot unless ports are different) but ALB will differentiate between the two applications by examining the application layer data available to it. This difference means that NLB may end up sending requests to an application that has crashed or is offline, but ALB will never make that same mistake.

#### AWS Cloudtrail

It is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account. It continuously logs and monitors the activities and actions across your AWS account. It also provides the event history of your AWS account including information about who is accessing your AWS services.

#### AWS Cloudwatch

It is a monitoring tool used for real-time monitoring of AWS resources and applications. CloudWatch also detect irregular behavior in your environments. It also sets the alarm. It monitors various AWS resources like Amazon EC2, Amazon RDS, Amazon S3, Elastic Load Balancer, etc.&#x20;

#### AWS Cloudwatch & AWS CloudTrial

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">S.No.</td><td valign="top">AWS Cloudwatch</td><td valign="top">AWS Cloudtrail</td></tr><tr><td valign="top">1.</td><td valign="top">It is mainly concerned with happenings on AWS resources.</td><td valign="top">It is mainly concerned with what is done on AWS and by whom.</td></tr><tr><td valign="top">2.</td><td valign="top">It is a monitoring service for AWS resources and applications.</td><td valign="top">It records API activity in the AWS account.</td></tr><tr><td valign="top">3.</td><td valign="top">Using Cloudwatch you can track metrics and monitor log files. You can also set alarm for various events.</td><td valign="top">CloudTrail provides greater visibility into user activity by tracking AWS console actions including who made the call, from which IP address and when.</td></tr><tr><td valign="top">4.</td><td valign="top">It specifically records the application logs.</td><td valign="top">It provides information about what occurred in your AWS account.</td></tr><tr><td valign="top">5.</td><td valign="top">It delivers metric data in 1 minute period for detailed monitoring and 5 minute periods for basic monitoring.</td><td valign="top">It delivers an event within 15 minutes of the API call.</td></tr><tr><td valign="top">6.</td><td valign="top">It stores data in its own dashboard in the form of metrics and logs.</td><td valign="top">It can centralize all the logs across regions and even across many accounts and store them on S3 bucket.</td></tr></tbody></table>

#### Amazon Route 53

Route 53 is a web service that is a highly available and scalable Domain Name System (DNS.)![Route 53](file:///Users/apple/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image059.jpg)

AWS Route 53 lets developers and organizations route end users to their web applications in a very reliable and cost-effective manner. It is a Domain Name System (DNS) that translates domain names into IP addresses to direct traffic to your website. In simple terms, it converts World Wide Web addresses like www.example.com to IP addresses like 10.20.30.40.

#### AWS Lambda

#### What is Serverless?

Serverless is a term that generally refers to serverless applications. Serverless applications are ones that don’t need any server provision and do not require to manage servers.

#### What is AWS Lambda?

AWS Lambda is an event-driven, serverless computing platform provided by Amazon as a part of Amazon Web Services. Therefore you don’t need to worry about which AWS resources to launch, or how will you manage them. Instead, you need to put the code on Lambda, and it runs.

In AWS Lambda the code is executed based on the response of events in AWS services such as add/delete files in S3 bucket, HTTP request from Amazon API gateway, etc. However, Amazon Lambda can only be used to execute background tasks.

#### How AWS Lambda Works

1. Deploy your code to Lambda
2. Make the code ready to trigger an event
3. The code only runs when triggered
4. Pay only when your code is running

#### IAM policy for lamda function:

{

&#x20; "Version": "2012-10-17",

&#x20; "Statement": \[

&#x20;   {

&#x20;     "Effect": "Allow",

&#x20;     "Action": \[

&#x20;       "logs:CreateLogGroup",

&#x20;       "logs:CreateLogStream",

&#x20;       "logs:PutLogEvents"

&#x20;     ],

&#x20;     "Resource": "arn:aws:logs:\*:\*:\*"

&#x20;   },

&#x20;   {

&#x20;     "Effect": "Allow",

&#x20;     "Action": \[

&#x20;       "ec2:Start\*",

&#x20;       "ec2:Stop\*"

&#x20;     ],

&#x20;     "Resource": "\*"

&#x20;   }

&#x20; ]

}

&#x20;

\=========================================================

&#x20;

&#x20;

&#x20;

&#x20;

Stops the EC2 instances:

&#x20;

import boto3

region = 'ap-south-1'

instances = \['i-12345', 'i-2345']

ec2 = boto3.client('ec2', region\_name=region)

&#x20;

def lambda\_handler(event, context):

&#x20;   ec2.stop\_instances(InstanceIds=instances)

&#x20;   print('stopped your instances: ' + str(instances))

&#x20;

Starts the EC2 instances:

&#x20;

import boto3

region = 'ap-south-1'

instances = \['i-12345', 'i-2345']

ec2 = boto3.client('ec2', region\_name=region)

&#x20;

def lambda\_handler(event, context):

&#x20;   ec2.start\_instances(InstanceIds=instances)

&#x20;   print('started your instances: ' + str(instances))

&#x20;

&#x20;

#### AWS CLI

AWS Command Line Interface(AWS CLI) is a unified tool using which, you can manage and monitor all your AWS services from a terminal session on your client.

&#x20;

#### Installing the AWS CLI

Step 1: Install pip (on Ubuntu OS)

$ sudo apt install python3-pip

&#x20;

&#x20;

Step 2: Install CLI

$ pip install awscli --upgrade –user

&#x20;

&#x20;

Step 3: Check installation

$ aws --version

&#x20;

#### Configure AWS CLI

Step 4: Use the below command to configure AWS CLI

$ aws configure

AWS Access Key ID \[None]: AKI\*\*\*\*\*\*\*\*\*\*\*\*

AWS Secret Access Key \[None]: wJalr\*\*\*\*\*\*\*\*

Default region name \[None]: us-west-2

Default output format \[None]: json

#### Commands

aws ec2 describe-instances

&#x20;

&#x20;

aws ec2 describe-instances --query "Reservations\[\*].Instances\[\*].{PublicIP:PublicIpAddress}" --output text

&#x20;

&#x20;

aws ec2 describe-instances --query "Reservations\[\*].Instances\[\*].{PublicIP:PublicIpAddress,Name:Tags\[?Key=='Name']|\[0].Value,Status:State.Name,Type:InstanceType,VpcId:VpcId}" --output table

&#x20;

aws s3 ls

&#x20;

&#x20;

aws s3 cp files s3://bucket\_name

&#x20;

&#x20;

aws s3 sync directory s3://bucket\_name

&#x20;

&#x20;
