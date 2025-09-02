# Amazon EC2 Basics

It is very difficult to predict how much computing power one might require for an application which you might have just launched. There can be two scenarios, you may over-estimate the requirement, and buy stacks of servers which will not be of any use, or you may under-estimate the usage, which will lead to the crashing of your application. In this _**AWS EC2 Tutorial**_, we will understand all the key concepts using examples and also perform hands-on to launch an Ubuntu instance.

### What is AWS EC2? <a href="#id-30fb" id="id-30fb"></a>

Amazon Elastic Compute Cloud, EC2 is a web service from Amazon that provides **re-sizable** compute services in the cloud.

![](https://miro.medium.com/v2/resize:fit:300/1*_C1uZnHGbzUWkmblcqnQLA.png)

### **How are they re-sizable?** <a href="#id-6c30" id="id-6c30"></a>

They are re-sizable because you can quickly scale up or scale down the number of server instances you are using if your computing requirements change.

![](https://miro.medium.com/v2/resize:fit:600/1*yz_VwYdZ7n4o1ELS2kyhFQ.png)

_That brings us to our next question._

### **What is an Instance?** <a href="#id-6a38" id="id-6a38"></a>

An instance is a virtual server for running applications on Amazon’s EC2. It can also be understood like a tiny part of a larger computer, a tiny part which has its own Hard drive, network connection, OS etc. But it is actually all virtual. You can have multiple “tiny” computers on a single physical machine, and all these tiny machines are called Instances.

### **Difference between a service and an Instance?** <a href="#id-4bee" id="id-4bee"></a>

Let’s understand it this way:

* EC2 is a service along with other Amazon Web Services like S3 etc.
* When we use EC2 or any other service, we use it through an instance, e.g. t2.micro instance, in EC2 etc.

### Why AWS EC2? <a href="#f43f" id="f43f"></a>

Why not buy your own stack of servers and work independently? Because, suppose you are a developer, and since you want to work independently you buy some servers, you estimated the correct capacity, and the computing power is enough. Now, you have to look after the updation of security patches every day, you have to troubleshoot any problem which might occur at a back-end level in the servers and so on. These are all extra chores that you will be doing or maybe you will hire someone else to do these things for you.

But if you buy an EC2 instance, you don’t have to worry about any of these things as it will all be managed by Amazon; you just have to focus on your application. That too, at a fraction of a cost that you were incurring earlier! Isn’t that interesting?

_**Let’s understand Cost Savings using an example.**_

Suppose instead of taking AWS EC2, we consider taking a dedicated set of servers, so, what all we might have to face:

* Now for using these servers, we have to hire an IT team which can handle them.
* Also, having a fault in the system is unavoidable, therefore we have to bear the cost of getting it fixed, and if you don’t want to compromise on your up-time you have to keep your systems redundant to other servers, which might become more expensive.
* Your own purchased assets will depreciate over the period of time, however, as a matter of fact, the cost of an instance have dropped more than 50% over a 3-year period, while improving processor type and speed. So eventually, moving to Cloud is all more suggested.
* For scaling up we have to add more servers, and if your application is new and you experience sudden traffic, scaling up that quickly might become a problem.

These are just a few problems and there are many other scenarios which make the case for EC2 stronger!

_**Let’s understand the types of EC2 Computing Instances:**_

Computing is a very broad term, the nature of your task decides what kind of computing you need.

Therefore, AWS EC2 offers 5 types of instances which are as follows:

1. _**General Instances**_

For applications that require a balance of performance and cost.

E.g email responding systems, where you need a prompt response as well as it should be cost-effective since it doesn’t require much processing.

_**2. Compute Instances**_

For applications that require a lot of processing from the CPU.

E.g analysis of data from a stream of data, like a Twitter stream

_**3. Memory Instances**_

For applications that are heavy in nature, therefore, require a lot of RAM.

E.g when your system needs a lot of applications running in the background i.e multitasking.

_**4. Storage Instances**_

For applications that are huge in size or have a data set that occupies a lot of space.

E.g When your application is of huge size.

_**5. GPU Instances**_

For applications that require some heavy graphics rendering.

E.g 3D modeling etc.

_**Now, every instance type has a set of instances which are optimized for different workloads:**_

1. _General Instances_

* t2
* m4
* m3

_2. Compute Instances_

* c4
* c3

_3. Memory Instances_

* r3
* x1

_4. Storage Instances_

* i2
* d2

_5. GPU Instances_

* g2

_Now let’s understand the kind of work that each instance is optimized for, in this AWS EC2 Tutorial._

### **Burstable Performance Instances** <a href="#aed7" id="aed7"></a>

_T2 instances_ are burstable instances, meaning the CPU performs at a baseline, say 20% of its capability. When your application needs more than 20% of the performance of the CPU, the CPU enters into a burst mode giving the higher performance for a limited amount of time, therefore work happens faster.

* You get these credits when your CPU is idle.
* Each CPU credit gives a burst of 1 minute to the CPU.
* If your CPU credits are not used they are credited to your account and they stay there for 24 hours.
* Based on your credit balance, you can decide whether the t2 instance, should be scaled up or down.
* These bursts happen at a cost, every time a burst happens in a CPU, CPU credits are used.

### EBS-Optimized Instances <a href="#e6a9" id="e6a9"></a>

_C4, M4, and D2 instances_ are EBS optimized by default, EBS means Elastic Block Storage, which is a storage option provided by AWS in which the IOPS\* rate is quite high. Therefore, when an EBS volume is attached to an optimized instance, single-digit millisecond latencies can be achieved.

\*_IOPS (Input/Output Operations Per Second, pronounced eye-ops)_ is a performance measurement used to characterize computer storage devices.

### **Cluster Networking Instances** <a href="#id-83e1" id="id-83e1"></a>

_X1, M4, C4, C3, I2, G2 and D2 instances_ support cluster networking. Instances launched into a common placement group are put in a logical group that provides high-bandwidth, low latency between all the instances in the group.

* A placement group is basically a logical cluster where some select EC2 instances which are a part of that group can utilize up to 10Gbps for single flow and 20Gbps for multi-flow traffic in each direction.
* Instances which are not a part of that group are limited to 5 Gbps speed in multi-flow traffic. Cluster Networking is ideal for high-performance analytics system.

### **Dedicated Instances** <a href="#id-2201" id="id-2201"></a>

* They are the instances that run on single-tenant hardware dedicated to a single customer.
* They are perfect for workloads where a corporate policy or industry regulation requires that your instance should be isolated from any other customer’s instance, therefore they go for their own separate machines, and their instances are isolated at the hardware level.

_Let’s understand this through an example. Suppose in a company they have to follow the following tasks:_

1. _Analysis of customer’s data_

Customer’s website activity, etc. should all be monitored in real-time. There will be times when the traffic on the website will be minimum, therefore using a very powerful processor should not be considered, since it will become expensive for the company because it will not be used for every hour of the day. Hence, for this task, we might take **t2 instances** because they give **Burstable CPU performance** i.e when the traffic will be more the CPU performance will be increased accordingly to meet the requirements.

_2. Our auto-response emailing system_

It should be quick, therefore we would require systems, where the response time is as short as possible. This could be achieved by using **EBS optimized instances**, as they offer high IOPS and hence, low latencies.

_3. The search engine on our website_

It should be able to sort the keywords and return relevant results, therefore we might have 2 servers for this. One is the database and the other server for processing the keywords. Therefore, the communication between these servers should be at the maximum possible rate. To achieve this, we can put them in a placement group and for that, we have to use **Cluster Networking Instances.**

_4. Some processes in every organization are highly confidential_

Because these processes give us an edge over other companies, no matter how secure the servers, maybe, some policies are still made to be sure. Therefore, we might use **Dedicated Instances** for these kinds of processes.

_We now know about instances, let’s learn how to launch these instances?_

### How to run systems in EC2? <a href="#id-055e" id="id-055e"></a>

* Login to your AWS account and click on AWS EC2.
* Under create an instance, click on launch instance.

Now you have to select an **Amazon Machine Image (AMI),** AMIs are templates of OS and they provide the information needed to launch an instance.

When we want to launch an instance we have to specify which AMI we want to use. It could be Ubuntu, windows server etc.

1. The AMIs could be preconfigured or you can configure it on your own according to your requirements.

* For pre-configured AMIs, you have to select it from AWS marketplace.
* For setting up your own, go to quick-start and select one.

2\. While configuring you will reach a point where you have to select an **EBS** storage option.

**Elastic Block Storage (EBS)** is a persistent block level storage volumes which are used with EC2. Here each block acts as a hard drive.

_**But why do we need EBS with EC2?**_

Just like your computer needs a hard drive, AWS EC2 needs a storage volume to store the OS that your instance will be specifying. Options for EBS are:

1. **Provisioned IOPS**: This category is for workloads which are mission-critical, it provides high IOPS rates.

**2. General Purpose:** It is for workloads which need a performance and cost balance.

**3. Magnetic:** It is for data which is accessed less frequently, and also retrieval time is more.

* After selecting a suitable option in EBS, we give the instance a name and then we create a **security group**.
* A security group acts as a firewall to control inbound and outbound traffic. Each security group has rules according to which the traffic is governed.
* Each instance can be assigned up to 5 security groups.
* Finally, in the last step, the console shows all the settings that you have done, you can verify and launch it.

Now in this AWS EC2 Tutorial, when you launch the instance, it will need some authentication from you; otherwise, anyone can access your instance! To address that in this AWS EC2 Tutorial, let’s take a plunge into the security aspects of EC2,

### Security in AWS EC2 <a href="#a466" id="a466"></a>

![](https://miro.medium.com/v2/resize:fit:600/1*tWb4QucMNr3rfcREy_Hi9g.png)

To authenticate users to their instances, AWS employs a **key pair** method.

**What is key pair?**

Amazon EC2 uses public–key cryptography to encrypt and decrypt login information. Public–key cryptography uses a public key to encrypt a piece of data, such as a password, then the recipient uses the private key to decrypt the data. The public and private keys are known as a _key pair_.

**Additional Benefits:** Every service from Amazon is designed keeping in mind the customer. They claim to be the earth’s most customer-obsessed company. Having said that let’s understand some other benefits of EC2.

![](https://miro.medium.com/v2/resize:fit:600/1*y8SiaXenHJkYUH33eQK5Vw.png)

### Auto Scaling <a href="#id-3330" id="id-3330"></a>

_Auto Scaling_ is a service designed by AWS EC2, which automatically launch or terminate EC2’s instances based on user-defined policies, schedules, and health checks.

**Elastic Load Balancing**

_Elastic Load Balancing_ (ELB) automatically distributes incoming application traffic across multiple EC2 instances, in multiple Availability Zones.

Availability zones are basically places where Amazon has set up their servers. Since they have customers from the whole globe, they have set up multiple Availability zones to reduce the latency.

_Elastic IP Addresses_ are static IP addresses which are associated with your AWS account, they can be used to mask the failure of an instance by automatically remapping your address to another working instance in your account.

### AWS EC2 Pricing <a href="#id-4b96" id="id-4b96"></a>

In this AWS EC2 Tutorial, let’s start with the free things first!

AWS EC2 free tier allows 750 hrs of t2.micro instance usage per month!

The free tier for EC2 is valid for 1 year from SignUp of your AWS account.

There are basically 3 pricing options in EC2:

* Spot Instances
* On-Demand Instances
* Reserved Instances

_**Spot Instances**_ is a pricing option which enables you to bid on unused EC2 instances. The hourly price for a Spot Instance is set by AWS EC2, and it fluctuates according to the availability of the instances in a specific Availability Zone.

* Basically, you will set a price for an instance above which you do not wish to get charged for.
* The price that you set is for per hour basis, therefore the moment the price for that instance becomes greater than what you have set, the instance gets shut down automatically.

_**On-Demand Instances**_ are used when you want to pay for the hour, with no long-term commitments and upfront payments. They are useful for applications that may have unpredictable workloads or for test applications that are being deployed for the first time.

_**Reserved Instances**_ provide you with significant discounts as compared to On-Demand Instances. With Reserved Instances you reserve instances for a specific period of time with three payment options:

* No Upfront
* Partial Upfront
* Full Upfront

And two-term lengths:

* One Year Term
* Three Year Term

The higher the upfront payment is, the more you save money.

### AWS EC2 Use Case <a href="#b3c5" id="b3c5"></a>

Next, in this AWS EC2 Tutorial, let’s understand the whole EC2 instance creation process through a use case in which we’ll be creating an Ubuntu instance for a test environment.

1. **Login to AWS Management Console.**

![](https://miro.medium.com/v2/resize:fit:1056/1*IuoDDIE0oCPkPD4FHK69oA.png)

**2. Select your preferred Region.** Select a region from the drop-down, the selection of the region can be done on the basis of the criteria discussed earlier in the blog.

![](https://miro.medium.com/v2/resize:fit:870/1*hCaawoCpUlfjk7eIrplNpQ.png)

**3. Select EC2 Service** Click EC2 under Compute section. This will take you to the EC2 dashboard.

![](https://miro.medium.com/v2/resize:fit:630/1*kc5wmLKf6lo7KUcezww3OA.png)

**4**. Click **Launch Instance**.

**5. Select an AMI:** because you require a Linux instance, in the row for the basic 64-bit Ubuntu AMI, click Select.

![](https://miro.medium.com/v2/resize:fit:1200/1*1VRfDkDEwQURN5L_vbAwIw.png)

**6. Choose an Instance**

Select t2.micro instance, which is free tier eligible.

![](https://miro.medium.com/v2/resize:fit:1400/1*lY7NVYo4zSgXki6LlACu2A.png)

**7. Configure Instance Details.**\
Configure all the details and then click on add storage

![](https://miro.medium.com/v2/resize:fit:1400/1*VQO98pmDMXi43ceiEGUx-A.png)

**8. Add Storage**

![](https://miro.medium.com/v2/resize:fit:1400/1*wfN8Sm-pk0JfaFsOReyLtQ.png)

**9. Tag an Instance**

Type a name for your AWS EC2 instance in the value box. This name, more correctly known as a tag, will appear in the console when the instance launches. It makes it easy to keep track of running machines in a complex environment. Use a name that you can easily recognize and remember.

![](https://miro.medium.com/v2/resize:fit:1400/1*hh8buqIW5LMt0U_mJ5DDYw.png)

**10. Create a Security Group**

![](https://miro.medium.com/v2/resize:fit:1400/1*qFK5x1cUpRoUm4u1UvJp8g.png)

**11. Review and Launch an Instance**

![](https://miro.medium.com/v2/resize:fit:1400/1*lT8htUveGDa6e2PAi9m1OQ.png)

Verify the details that you have configured to launch an instance.

**12. Create a Key Pair & launch an Instance**

Next, in this AWS EC2 Tutorial, select the option ‘Create a new key pair’ and give a name of a key pair. After that, download it in your system and save it for future use.

![](https://miro.medium.com/v2/resize:fit:1240/1*XHokHF_68_RP9V0ngWCvcg.png)

**13. Check the details of a launched instance.**

![](https://miro.medium.com/v2/resize:fit:1400/1*OyMoGyuoFW5kSF9TOiJZzg.png)

**14. Converting Your Private Key Using PuTTYgen**

PuTTY does not natively support the private key format (.pem) generated by Amazon EC2. PuTTY has a tool called PuTTYgen, which can convert keys to the required PuTTY format (.ppk). You must convert your private key into this format (.ppk) before attempting to connect to your instance using PuTTY.

* Click Load. By default, PuTTYgen displays only files with the extension .ppk. To locate your .pem file, select the option to display files of all types.
* Select your .pem file for the key pair that you specified when you launch your instance, and then click Open. Click OK to dismiss the confirmation dialog box.
* Click Save private key to save the key in the format that PuTTY can use. PuTTYgen displays a warning about saving the key without a passphrase. Click Yes.
* Specify the same name for the key that you used for the key pair (for example, my-key-pair). PuTTY automatically adds the .ppk file extension.



**15. Connect to EC2 instance using SSH and PuTTY**

* Open PuTTY.exe
* In the Host Name box, enter Public IP of your instance.
* In the Category list, expand SSH.
* Click Auth (don’t expand it).
* In the Private Key file for authentication box, browse to the PPK file that you downloaded and double-click it.
* Click Open.
* Type in Ubuntu when prompted for login ID.

