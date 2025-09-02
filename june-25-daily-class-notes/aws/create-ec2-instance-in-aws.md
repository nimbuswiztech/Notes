# Create EC2 Instance in AWS

Amazon EC2 (Elastic Compute Cloud) is a cloud computing service provided by AWS that allows users to rent virtual machines (VMs) to run applications on-demand. EC2 Offers a scalable, cost-efficient, and flexible computing environment without the need for users to manage physical hardware. ]

Users can customize their instances by configuring CPU, memory, and storage according to specific requirements. Additionally, EC2 supports secure configurations using Virtual Private Clouds (VPCs), subnets, and security groups. With auto-scaling capabilities, EC2 instances can automatically scale up or down based on application traffic, ensuring optimal resource usage.

### How to Create EC2 Instance in AWS (Amazon) <a href="#create-ec2-instance-in-aws-amazon" id="create-ec2-instance-in-aws-amazon"></a>

Follow the below steps to create an EC2 instance in AWS (Amazon):

#### **Step 1: Login and Navigate to EC2 Dashboard** <a href="#step-1-login-and-navigate-to-ec2-dashboard" id="step-1-login-and-navigate-to-ec2-dashboard"></a>

First, log into your AWS account and click on **"services"** present on the left of the AWS management console, i.e. the primary screen. From the drop-down menu of options, tap on **"EC2"**. To create an AWS free tier account refer to Amazon Web Services (AWS) – Free Tier Account Set up.![AWS Console](https://media.geeksforgeeks.org/wp-content/uploads/20210527183025/awsec2navigation.png)

Under Resources >> Click on "Instances running" -- It will show if any EC2 instances are running or not.

#### **Step 2: Launch a New Instance** <a href="#step-2-launch-a-new-instance" id="step-2-launch-a-new-instance"></a>

* Click on the launch instance, and you will be redirected to a launch page where we can create an instance.
* Enter a name for the instance and select required configurations.

<figure><img src="https://media.geeksforgeeks.org/wp-content/uploads/20230215120003/Screenshot_5.png" alt="Launch Instance"><figcaption><p>Naming instance</p></figcaption></figure>

#### **Step 3: Choose Amazon Machine Image** <a href="#step-3-choose-amazon-machine-image" id="step-3-choose-amazon-machine-image"></a>

[Select AMI](https://www.geeksforgeeks.org/what-is-amazon-machine-image-ami/) - Required operating system from the available. There are different types of OS available select the OS as per your requirement.

<figure><img src="https://media.geeksforgeeks.org/wp-content/uploads/20230219122158/Screenshot_1.png" alt="Select the OS"><figcaption><p>Selecting AMI</p></figcaption></figure>

#### **Step 4: Select Instance Type** <a href="#step-4-select-instance-type" id="step-4-select-instance-type"></a>

By default, it selects a free tier of storage. (IF YOU ARE ELIGIBLE FOR THE FREE TIER). From the available storage specifications, select a free tier-eligible storage service. The instance type includes the no.of CPUs required and the Memory required for your application. By default, the instance type is "t2.micro" which is a free tier-eligible service. Do not select any other which leads to the billing amount. To know more about instance types refer to Amazon EC2 – Instance Types.

<figure><img src="https://media.geeksforgeeks.org/wp-content/uploads/20230219122654/Screenshot_3.png" alt="Instance type"><figcaption><p>Select instance type</p></figcaption></figure>

#### **Step 5: Configure Key Pair** <a href="#step-5-configure-key-pair" id="step-5-configure-key-pair"></a>

Now, create a key-value pair, by clicking on "Create new key pair". A window will pop up for creating key pair as shown below. The key value pair plays a major role while connecting to the EC2-Instance it will act as an SSH-Key to connect to the instance. Create Key-Pair Enter name>>Select ".pem" and create. Automatically key pair which was created will be downloaded. Select the created key pair.

<figure><img src="https://media.geeksforgeeks.org/wp-content/uploads/20230219123030/Screenshot_4.png" alt="Key-Pair"><figcaption><p>Creating key pair</p></figcaption></figure>

#### **Step 6: Network and Storage Configuration** <a href="#step-6-network-and-storage-configuration" id="step-6-network-and-storage-configuration"></a>

Keep the network settings as default settings and make changes if required. Storage As mentioned in the picture, Free tier eligible can get up to 30 GB of [EBS Storage.](https://www.geeksforgeeks.org/introduction-to-aws-elastic-block-storeebs/) Keep it as default.

![Configuring Storage](https://media.geeksforgeeks.org/wp-content/uploads/20230219125752/Screenshot-01-11-2023-204625.png)

#### **Step 7: Review and Launch** <a href="#step-7-review-and-launch" id="step-7-review-and-launch"></a>

Launching Instance At last, Check if all the selected are eligible for a free tier or not and click on "Launch instance".That's it, an instance will be created.

![Launching instance](https://media.geeksforgeeks.org/wp-content/uploads/20230215120529/Screenshot_8.png)

### How To Connect Terminal Using SSH-Key <a href="#steps-to-connect-terminal-using-sshkey" id="steps-to-connect-terminal-using-sshkey"></a>

Once your instance is launched, secure access is essential. Follow the below steps to know how to connect using a terminal and your key pair.

#### **Step 1: Locate Connection Details** <a href="#step-1-locate-connection-details" id="step-1-locate-connection-details"></a>

Select the server to which you want to connect and click on the connect button at the top of that instance as shown in the image below.

![EC2-Instance.](https://media.geeksforgeeks.org/wp-content/uploads/20230802121544/00890203-5650-4b8d-9163-ccd1c9b688eb.webp)

#### **Step 2: Copy the SSH Command** <a href="#step-2-use-terminal" id="step-2-use-terminal"></a>

Copy the SSH key which is right following the example it will acct as a [key-pair](https://www.geeksforgeeks.org/how-to-create-a-public-private-key-pair/) to connect to EC2-Instance.

![Select the SSH key](https://media.geeksforgeeks.org/wp-content/uploads/20230802121725/eae88ebf-fea2-46a8-b637-8a36693af993.webp)

#### **Step 3: Use Terminal** <a href="#step-3-use-terminal" id="step-3-use-terminal"></a>

Open the terminal and go to the folder where your .pem file is located and paste the key that you have copied in AWS and paste it in the terminal.

![TERMINAL](https://media.geeksforgeeks.org/wp-content/uploads/20230802122211/17221a11-89b3-4d44-a060-18f3ff7c6edf.webp)

To know whether you connected to EC2-Instance perfectly or not you can check the[ IP-Address ](https://www.geeksforgeeks.org/what-is-an-ip-address/)of the instance if the IP is displaying then you have connected successfully

### EC2 Instance All-State in AWS <a href="#ec2-instance-allstate-in-aws" id="ec2-instance-allstate-in-aws"></a>

The common EC2 instance states are Pending, Running, Stopping, Stopped, Terminated, Shutting Down, and Rebooted. It is important to keep track of the state of your EC2 instances so that you can manage them properly. You can view the state of your instances in the EC2 Console, AWS CLI, or AWS SDKs. In AWS, EC2 (Elastic Compute Cloud) instances can have different states, which indicate what operations can be performed on them. Here are some of the common EC2 instance states:

1. **Pending:** When you launch an EC2 instance, it enters the pending state. This means that AWS is in the process of creating the instance and initializing all of the necessary components, such as the virtual machine and the associated networking resources. During this time, you won't be able to access the instance, as it is not yet ready to be used.
2. **Running:** Once an EC2 instance has finished initializing, it enters the running state. This means that the instance is up and running and is ready to be used. In this state, you can log in to the instance and start using it to run your applications and services.
3. **Stopping:** If you manually stop an EC2 instance, or if it is part of an auto-scaling group and is being terminated, it enters the stopping state. During this state, AWS prepares the instance for shutdown by stopping any processes or applications running on the instance and disconnecting it from the network. However, the instance's configuration and data are preserved, so you can start the instance again later if you need to.
4. **Stopped:** Once an EC2 instance has been stopped, it enters the stopped state. In this state, the instance is not running and is not available for use. However, the instance's configuration and data are preserved, so you can start the instance again later if you need to. You might stop an instance if you don't need it for a period of time but don't want to terminate it entirely.
5. **Terminated:** If you manually terminate an EC2 instance, or if it is part of an auto-scaling group and is being terminated, it enters the terminated state. In this state, the instance is permanently deleted, and all of its configuration and data are lost. You might terminate an instance if you no longer need it, or if you want to replace it with a new instance.
6. **Shutting-down:** If AWS is retiring an instance, it goes into the "Shutting-down" state for a brief period before the instance is terminated. During this time, the instance is no longer available for use, and the data and configuration are preserved. This state is similar to the stopping state but with an added step of preparing the instance for retirement.
7. **Rebooting:** If you choose to reboot an EC2 instance, it enters the rebooting state. During this state, the instance's operating system is shut down and then restarted, but the instance's configuration and data are preserved. You might reboot an instance if you need to apply updates or make changes to the instance's configuration.

\
