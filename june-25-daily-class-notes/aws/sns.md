# SNS

what if you want to send one message to many receivers? So we have the possibility to do a direct integration.

So we have our create service and needs to send email, talk to their XYZ service, talk to their shipping service, maybe talk to another SQS queue, so we could integrate all these things together, but it would be quite difficult..

The other approach is to do something called Pub / Sub, or publish- subscribe.

And so our create service publishes data to our SNS topic, and our SNS topic has many subscribers and all these subscribers get that data in real time as a notification. So it could be an email notification, text message, shipping service, SQS queue. You’re basically able to send your message once to an SNS topic and have many services receive it.

<figure><img src="https://miro.medium.com/v2/resize:fit:990/1*m27M5WgfLxejuJcv0GIoTA.png" alt="" height="204" width="495"><figcaption><p>AWS SNS</p></figcaption></figure>

**Basic Introduction:**

An Amazon SNS topic is a logical access point which acts as a communication channel. A topic lets you group multiple endpoints (such as AWS Lambda, Amazon SQS, HTTP/S, or an email address). To broadcast the messages of a message-producer system (for example, an e-commerce website) working with multiple other services that require its messages (for example, checkout and fulfillment systems), you can create a topic for your producer system. The first and most common Amazon SNS task is creating a topic

Each subscriber to the topic will get all the messages now we have new feature to filter messages. Up to 10,000,000 subscriptions per topic & 100,000 topics limit.

Subscribers can be:

* SQS
* HTTP/HTTPS (with delivery retries — how many times).
* Lambda
* Emails
* SMS messages
* Mobile Notification

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*vFFvknt95-onRKSNXzJW0Q.png" alt="" height="341" width="700"><figcaption></figcaption></figure>

Our SNS integrates with a lot of Amazon Services in AWS.

* With Amazon S3, we use it on bucket events.
* For our ASG Auto-Scaling Notifications.
* In CloudWatch for Alarms.
* In CloudFormation for state changes.

### **SNS + SQS : Fan Out** <a href="#id-14cb" id="id-14cb"></a>

The “fanout” scenario is when an Amazon SNS message is sent to a topic and then replicated and pushed to multiple Amazon SQS queues, HTTP endpoints, or email addresses. This allows for parallel asynchronous processing. For example, you could develop an application that sends an Amazon SNS message to a topic whenever an order is placed for a product. Then, the Amazon SQS queues that are subscribed to that topic would receive identical notifications for the new order. The Amazon EC2 server instance attached to one of the queues could handle the processing or fulfillment of the order while the other server instance could be attached to a data warehouse for analysis of all orders received.

It’s fully decoupled & there is no data loss. It has ability to add receivers of data later.

<figure><img src="https://miro.medium.com/v2/resize:fit:1162/1*iJMuVe6R3xbmJGhPKIdL1g.png" alt="" height="208" width="581"><figcaption><p>Fanout</p></figcaption></figure>

Let’s do Hands-On, we will go to SNS console & Give the topic name, I’m giving “MyTestTopic” & click on Next Step.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*YL3UCoTNEiliv1cNG8dinA.png" alt="" height="209" width="700"><figcaption></figcaption></figure>

We’re not going to apply any custom changes so will keep as default & click on Create Topic.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*lDbEjp-wr_n0CbK0_xSl-g.png" alt="" height="364" width="700"><figcaption></figcaption></figure>

Now as we see in below picture, there is no Subscription in Topic so we will create subscription, click in Create Subscription.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*K0yjy8z5QPohVC71BCWufw.png" alt="" height="200" width="700"><figcaption></figcaption></figure>

Now we will choose the protocol, there are many protocol available there like HTTP/HTTPS, Email, Lambda, SQS, SMS etc. I’ll choose Email & give the email id & Create subscription.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*3GPHw54o1TnTl8Ht8AMUSw.png" alt="" height="429" width="700"><figcaption></figcaption></figure>

Now you will see that status of subscription is in Pending Confirmation, Go to your email & click on received email from Amazon & Confirm it.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*7w0wMtx9oU-EsoDQU5cDsg.png" alt="" height="253" width="700"><figcaption></figcaption></figure>

Now you will see the status confirmed as I confirm the subscription in my email

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*FDbcd6hMV65DNmEGRD5_4w.png" alt="" height="154" width="700"><figcaption></figcaption></figure>

So this is my console, you can add more subscription. I have added 2 subscriptions. Let’s publish some message. Click on Publish Message button on top right hand side.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*oc_nP2FWETc-2Gy004bX0w.png" alt="" height="242" width="700"><figcaption></figcaption></figure>

I’m publishing my message, gave the details.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*vfkIllcTlg3F2M7vQK2Dbw.png" alt="" height="423" width="700"><figcaption></figcaption></figure>

Now go to SQS Queue & click on View/Delete Message in Actions(Make sure you subscribe the Topic in SQS)

> **Actions — Subscribe to SNS Topic (Do this work before publishing message from SNS.**

<figure><img src="https://miro.medium.com/v2/resize:fit:1228/1*E17D6awZ_lJbSmmGUNMoLw.png" alt="" height="268" width="614"><figcaption></figcaption></figure>

Now you can see you message in Queue as well as in your email.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*b9B2V0IXm-1MqZ8JLuefhA.png" alt="" height="257" width="700"><figcaption></figcaption></figure>

This is all about SNS

