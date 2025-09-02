# Automate Start-Stop EC2 Instances Using Lambda



<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*nkY4YzPBymWnvig5fFnNwg.png" alt="" height="270" width="700"><figcaption></figcaption></figure>

In today’s fast-paced world of cloud computing, optimizing resources and minimizing costs are essential tasks for any organization. Amazon Web Services (AWS) offers a plethora of tools and services to help achieve this goal, and one such tool is AWS Lambda. In this step-by-step guide, we’ll walk you through the process of automating the start-stop cycle of your EC2 instances using AWS Lambda. This automation can lead to significant cost savings by only running instances when they are needed.

## Introduction to EC2 Instance Automation <a href="#id-942d" id="id-942d"></a>

Amazon Elastic Compute Cloud (EC2) provides scalable computing capacity in the AWS cloud. However, it’s common to have instances running 24/7 even when they’re not actively being used. This leads to unnecessary costs. By automating the start-stop process of EC2 instances using AWS Lambda, you can ensure that instances are only running when needed, thus optimizing your expenses.

## Prerequisites <a href="#id-78f5" id="id-78f5"></a>

Before you begin, make sure you have the following:

* An AWS account with appropriate permissions.
* EC2 instances that you want to automate.
* Basic knowledge of AWS services like EC2, IAM, Lambda, and CloudWatch.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*iBdp-VWpwm-3EZOmfusJ4A.png" alt="" height="382" width="700"><figcaption></figcaption></figure>

## Setting up IAM Role and Policy <a href="#id-3072" id="id-3072"></a>

The first thing you need to do is create an IAM policy and an IAM role.\
In order for our Lambda Function to work properly, it should possess an IAM role that gives the permission to read and list EC2 Instances and also describes EC2 Instance’s tags.

1. Go to the AWS Management Console.
2. Navigate to the IAM dashboard.
3. Go to **Policies** and create a policy.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*_G93ithegduPDk5qZn8zuQ.png" alt="" height="151" width="700"><figcaption><p>create a policy.</p></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*17kwHJfajtY86-HxFBYA1w.png" alt="" height="273" width="700"><figcaption><p>select an ec2</p></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*it2t1y2BDjTQPiEzunf6aA.png" alt="" height="294" width="700"><figcaption><p>go to Write and select stop instances and start instances and click on Add Arn</p></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*xS58yPYwXKa-kQ8vPq12sg.png" alt="" height="442" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*5BfDspHb8MZG29bDdbDvFw.png" alt="" height="279" width="700"><figcaption></figcaption></figure>

1. Click on “Roles” and then “Create role.”
2. Choose “Lambda” as the trusted entity type.
3. Attach policies like `AmazonEC2FullAccess` and `CloudWatchEventsFullAccess` to the role.
4. Name the role and create it.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*v7i6WIadCwwvaKjtJonORw.png" alt="" height="238" width="700"><figcaption><p>Click on “Roles” and then “Create role.”</p></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*MYKzPcsOMnPu-D5bNkdcAg.png" alt="" height="284" width="700"><figcaption><p>select an AWS service Choose “Lambda” and click on the next</p></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*0p0Ovj0G8t5rXrfdeUQLWQ.png" alt="" height="249" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*cfrsIMuoDdABmyJCwY0PAw.png" alt="" height="370" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*XNPtUCWjPz9FmszvTrxOag.png" alt="" height="269" width="700"><figcaption></figcaption></figure>

## Creating a Lambda Function <a href="#id-3fce" id="id-3fce"></a>

Next, we are going to create an AWS Lambda Function using Python3.9. The Lambda Function will be used for starting and stopping the instances.

1. Go to AWS Services → **Lambda** and click on ‘**Create function**’.
2. Choose an **Author from scratch**
3. Under **Basic information**, enter the following information:\
   For **Function name**, enter a name that identifies it as the function that’s used to stop your EC2 instances. For example, “StopEC2Instances”.
4. For **Runtime,** choose **Python 3.9**
5. **Under Permissions,** expand Change default execution role
6. **Under the Execution role**, choose to **Use an existing role**
7. **Under Existing role**, choose the **IAM role** that you created
8. Choose **Create function**
9. After creating the Lambda Function, go to → Under Code, Code source, copy and paste the below Python code.

```
#Lambda code reference to stop EC2

import boto3
region = 'us-west-1'
instances = ['i-12345cb6de4f78g9h', 'i-08ce9b2d7eccf6d26']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('stopped your instances: ' + str(instances))

#Lambda code reference to start EC2

import boto3
region = 'us-west-1'
instances = ['i-12345cb6de4f78g9h', 'i-08ce9b2d7eccf6d26']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.start_instances(InstanceIds=instances)
    print('started your instances: ' + str(instances))
```

Replace `instance_id_1`, etc., with your actual instance IDs.

10\. Choose **Deploy**

11\. Complete the function creation process.

* Repeat steps 1–11 to create a **function** to start your EC2 instances.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*QAL9w0n5jiBuMyQQZ-WogQ.png" alt="" height="286" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*bfQK2vixOp9syiEcyKjS8g.png" alt="" height="147" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*1ONAd-zdNM6COhOe-ssVIA.png" alt="" height="702" width="700"><figcaption></figcaption></figure>

## Setting Up CloudWatch Events <a href="#d75f" id="d75f"></a>

To trigger the Lambda function on a schedule, you can use CloudWatch Events:

1. Go to the AWS Management Console.
2. Navigate to the CloudWatch dashboard.
3. Click on “Rules” under “Events” and then “Create rule.”
4. Enter a **Name** for your rule, such as “StopEC2Instances”. (Optional) Enter a description for the rule in **Description**.
5. For **Rule type**, choose **Schedule**, and then choose **Continue in EventBridge Scheduler**.
6. For Schedule pattern, choose **Recurring schedule**.
7. Under **Schedule pattern**, for **Occurrence**, choose **Recurring schedule**.
8. For **Schedule type**, choose the type that’s right for your need and complete the following steps:\
   When **Schedule type** is **Rate-based schedule**, for **Rate expression**, enter a rate value and choose an interval of time in minutes, hours, or days.\
   -or-\
   When **Schedule type** is **Cron-based schedule**, for **Cron expression**, enter an expression that tells Lambda when to stop your instance. For information on expression syntax, see [Schedule expressions for rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html).\
   **Note:** Cron expressions are evaluated in UTC. Make sure that you adjust the expression for your preferred time zone.
9. In **Select targets**, choose **Lambda function** from the **Target** dropdown list.
10. For **Function**, choose the function that stops your EC2 instances.
11. Choose **Skip to review and create**, and then choose **Create**.

* Repeat steps 1–11 to create a rule to start your EC2 instances. Complete the following steps differently:\
  Enter a name for your rule, such as “StartEC2Instances”.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*mY2_DiZhaoASRKknOuyqaQ.png" alt="" height="388" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*xk2oNdpiu75zxwuWLKh3Mg.png" alt="" height="331" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*4qDYPwmu8hc9VAagEc_7ew.png" alt="" height="459" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*MSt3Vr6uS1SLyA0ezIZ_zw.png" alt="" height="285" width="700"><figcaption></figcaption></figure>

## Testing the Automation <a href="#faff" id="faff"></a>

After setting up the CloudWatch rule, your Lambda function will automatically start and stop instances as per the schedule. Monitor the CloudWatch logs and the EC2 console to ensure everything is working as expected.
