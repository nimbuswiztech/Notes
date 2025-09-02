# SNS Deep dive

Introduction

As a continuation of our previous post where we had a deep dive into [SQS](https://medium.com/@joudwawad/aws-sqs-deep-dive-dfed03c8b640), we continue to explore more AWS services that are widely used in today's architecture and integrate together.

In today’s blog post, we’ll take an in-depth look at AWS Simple Notification Service (SNS), exploring its core components, functionality, implementation details, and how it integrates with other AWS services.

### What is AWS SNS? <a href="#id-7ff2" id="id-7ff2"></a>

Amazon Simple Notification Service (SNS) operates as a managed service designed to facilitate message delivery between _**publishers**_ (producers) and _**subscribers**_ (consumers).

In this system, publishers can send messages to a “topic,” which acts as a logical access point and communication channel. Subscribers, who wish to receive these messages, can subscribe to the SNS topic using various supported endpoint types. These endpoints include Amazon Data Firehose, Amazon SQS, AWS Lambda, HTTP, email, mobile push notifications, and SMS. Through this _**asynchronous**_ communication model, publishers and subscribers can effectively exchange information.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*m4g0wju6GxgvbLSp.png" alt="" height="700" width="700"><figcaption><p>SNS Service high-level overview ~ <a href="https://docs.aws.amazon.com/sns/latest/dg/welcome.html">Source AWS SNS Documentation</a></p></figcaption></figure>

SNS works on different levels and with different destinations (protocols), we will talk about these destinations in detail in this blog post but for now, we need to categorize these destinations into two categories:

1. **Application 2 Application (A2A):** Application-to-application messaging supports subscribers such as Amazon Data Firehose delivery streams, Lambda functions, Amazon SQS queues, and HTTP/S endpoints.
2. **Application 2 Person (A2P):** Application-to-person notifications provide user notifications to subscribers such as mobile applications, mobile phone numbers, and email addresses.

SNS uses what is called “Topic” to publish a message to _**multiple**_ subscribers, thus it acts like a pub/sub, duo to AWS SNS pub/sub nature it is mainly used when we need to use a fanout pattern in our architecture.

## What is a Fanout Pattern? <a href="#a181" id="a181"></a>

A **Fanout Pattern** refers to a system design approach for efficiently distributing data to multiple consumers in a scalable manner.

Fan-out is a messaging pattern where messages are broadcast in a _**one-to-many**_ arrangement. A basic example of this pattern can be seen in the functionality of a Publish/Subscribe messaging system, as Pub/Sub implies the ability to route messages from a single sender/multiple senders to multiple receivers.

Zoom image will be displayed

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*nFbrHli2OP3Qiy8C-gRWmA.png" alt="" height="180" width="700"><figcaption><p>Pub/Sub Architecture</p></figcaption></figure>

AWS SNS operates within a similar architecture, this architecture underpins how SNS functions at a high level, providing a clear understanding of its role in message delivery. Now that we’ve established the foundational concept, it’s time to delve into the implementation details and explore how SNS integrates with other AWS services.

> When integrated with other AWS services, SNS becomes a powerful tool for building scalable, event-driven architectures. For instance, SNS can work alongside Amazon SQS (Simple Queue Service) to ensure that messages are queued and processed reliably. By subscribing an SQS queue to an SNS topic, messages published to the topic are automatically enqueued for further processing.

## SNS Core Components <a href="#id-5a82" id="id-5a82"></a>

Let us now start looking into the inner architecture components of SNS, as you have seen in the pub/sub architecture, we do have the same components in SNS:

1. _**Publisher**_
2. _**Topic**_
3. _**Subscriber**_

let us examine each of them in detail

### SNS Publisher <a href="#fca0" id="fca0"></a>

Publishers are the ones who generate data that will be sent to a “topic”, AWS integrates SNS with lots of sources that we can divide into two categories:

1. **AWS Publishers Through AWS SNS SDK:** AWS provides an SDK for SNS that we can use in order to publish messages to an SNS topic, this can be achieved through multiple programming languages.
2. **AWS Services:** AWS has built internal integration between SNS and other AWS services, for example, SNS can receive notifications when changes occur to an Amazon S3 bucket when a file is uploaded for example, the following diagram shows an architecture of AWS integrated services with SNS:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*4Yfnx0i2Dm7UIt4J5ICzOw.png" alt="" height="239" width="700"><figcaption><p>AWS SNS Services Integration</p></figcaption></figure>

There is a big list of services that can be integrated and we recommend reading the full list of them on the [official SNS documentation](https://docs.aws.amazon.com/sns/latest/dg/sns-event-sources.html).

### Publishing a message through the AWS SDK <a href="#id-2568" id="id-2568"></a>

AWS SNS SDK provides two APIs to publish messages to a topic:

* `Publish` which is used to send a single message to a topic.
* `PublishBatch` API is used to publish up to 10 messages in a single API request. Sending messages in batches can help you reduce the costs associated with connecting distributed applications.

> Publishing messages with the `PublishBatch` API is similar to publishing messages with the `Publish` API. The main difference is that each message within a `PublishBatch` API request needs to be assigned a **unique batch ID** (up to 80 characters). This way, Amazon SNS can return individual API responses for every message within a batch to confirm that each message was either published or that a failure occurred. For messages being published to FIFO topics, in addition to including assigning a unique batch ID, you still need to include a `MessageDeduplicationID` and `MessageGroupId` for each individual message.

### Message Attribute <a href="#a412" id="a412"></a>

Amazon SNS supports the delivery of message attributes, which let you provide structured metadata items (such as timestamps, versioning, signatures, and identifiers) about the message.

> Message attributes are sent only when the message structure is String, not JSON. we discuess this later.

There are multiple reasons why you would use message attributes:

1. For once you can use these attributes to add additional information to your payload without adding it to the body of the message, this can be information such as Version number which can be used by consumers to validate the schema.
2. You can also use message attributes to make your messages filterable using subscription filter policies (mentioned in the later section). You can apply filter policies to the topic subscription level.
3. You can also use message attributes to help structure the push notification message for mobile endpoints. In this scenario, the message attributes are used only to help structure the push notification message. The attributes are _**not delivered to the endpoint**_ as they are when sending messages with message attributes to Amazon SQS endpoints.

The following code shows how to publish an SNS message through the javascript SDK V3

```
import { SNSClient, PublishCommand } from "@aws-sdk/client-sns";
const client = new SNSClient(config);

const input = {
  TopicArn: "STRING_VALUE",
  TargetArn: "STRING_VALUE", // optional
  PhoneNumber: "STRING_VALUE", // optional
  Message: "STRING_VALUE",
  MessageAttributes: {
    "Version": {
      DataType: "Number",
      StringValue: "1"
    },
  },
  MessageDeduplicationId: "STRING_VALUE", // required for FIFO topics
  MessageGroupId: "STRING_VALUE", // required for FIFO topics
  Subject: "STRING_VALUE" // optional
};
const command = new PublishCommand(input);
const response = await client.send(command);
```

Let us examine these options on a high level:

* **TopicArn:** The topic you want to publish at. If you don’t specify a value for the `TopicArn` parameter, you must specify a value for the `PhoneNumber` or `TargetArn` parameters. This option basically specifies who you want to publish the message to, for TopicArn you send the message to all participants in the Topic, for PhoneNumber you send a message to a specific phone number while for TargetArn you specify the value for a specific subscriber.
* **Message:** The message you want to send.
* **MessageAttributes:** A key/value pairs, these attributes are used for Publish action.
* **MessageDeduplicationId:** This parameter applies only to FIFO (first-in-first-out) topics. Every message must have a unique `MessageDeduplicationId`, which is a token used for deduplication of sent messages. If a message with a particular `MessageDeduplicationId` is sent successfully, any message sent with the same `MessageDeduplicationId` during the 5-minute deduplication interval is treated as a duplicate.
* **MessageGroupId:** This parameter applies only to FIFO (first-in-first-out) topics. The `MessageGroupId` is a tag that specifies that a message belongs to a specific message group. Messages that belong to the same message group are processed in a FIFO manner (however, messages in different message groups might be processed out of order). Every message must include a `MessageGroupId`
* **Subject:** Optional parameter to be used as the “Subject” line when the message is delivered to email endpoints.
* **MessageStructure:** By default, this value is not required, however, it accepts a value of `json`Setting the value to `json` if you want to send a different message for each protocol (Different subscriber services). For example, using one `publish API`action, you can send a short message to your SMS subscribers and a longer message to your email subscribers. If you set `MessageStructure` to `json`, the value of the `Message` parameter must:\
  \- be a syntactically valid JSON object;\
  \- It contains at least a top-level JSON key of “default” with a value that is a string.

You can define other top-level keys that define the message you want to send to a specific transport protocol (e.g., “http”), let us take an example of this in code:

```
{
 "default": "Sample fallback message",
 "email": "Sample message for email endpoints",
 "sqs": "Sample message for Amazon SQS endpoints",
 "lambda": "Sample message for AWS Lambda endpoints",
 "http": "Sample message for HTTP endpoints",
 "https": "Sample message for HTTPS endpoints"
}
```

to make this even more clear to you as for me it was challenging to understand it at first, take this diagram as an example with the above JSON I provided assuming the `MessageStructure` is set to `JSON`

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*9_iNsXFVRu7UR-I-isZTtg.png" alt="" height="600" width="700"><figcaption><p>SNS fan-out to multiple protocols</p></figcaption></figure>

with the above JSON, the SNS will send a different message for each subscription as it represents a different protocol, however for SMS protocol we see there is no field that represent it in the JSON we provided thus it will fall to the `default` field.

> Keep in mind that for the message attributes there are some reserved attributes that is used specially for push notification, please refer to the [AWS documentation](https://docs.aws.amazon.com/sns/latest/dg/sns-message-attributes.html#sns-attrib-mobile-reserved) for these values

### Publishing large messages with Amazon SNS <a href="#id-6d26" id="id-6d26"></a>

To publish large Amazon SNS messages, you can use the [Amazon SNS Extended Client Library for Java](https://github.com/awslabs/amazon-sns-java-extended-client-lib/), or the [Amazon SNS Extended Client Library for Python](https://github.com/awslabs/amazon-sns-python-extended-client-lib). These libraries are useful for messages that are larger than the current maximum of 256 KB, with a maximum of 2 GB.

However there is still a better solution that you can leverage which is programming language agnostic and leverage the event-driven nature of AWS, the following diagram shows how this approach can work

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*U5rz-5LHvvC0m_W0PL5mTA.png" alt="" height="393" width="700"><figcaption><p>Large files Flow with SNS</p></figcaption></figure>

SNS can be used as a destination for S3 bucket events, so the client can upload the large payload to the S3 and then use the event nature of S3 to trigger an SNS fanout which will then send it to the other consumers but it will only send a reference to the file in the S3 bucket instead of the full file.

## SNS Topics <a href="#e34d" id="e34d"></a>

Topics are the glue of SNS, An Amazon SNS topic is a logical access point that acts as a _communication channel_. A topic lets you group multiple _endpoints_ (such as AWS Lambda, Amazon SQS, HTTP/S, or an email address).

To broadcast the messages of a message-producer system (for example, a notification system) working with multiple other services that require its messages (for example, email service, SMS service, and notification analytic service), you can create a topic for your producer system.

When creating a topic you will see that you have a choice between a Standard topic and a FIFO Topic as the following figure shows

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*9cugcfODrT4_ZMOJ2Gltvg.png" alt="" height="364" width="700"><figcaption><p>SNS Topic Types</p></figcaption></figure>

the way AWS designed SNS topics is different and each one of these types has its own use cases and limitations let us go over each of them in detail to explain which one you should choose for your architecture, the main topics we want to compare are:

1. _**Message Ordering**_
2. _**Message Duplication**_
3. _**Throughput & Performance**_
4. _**Supported Subscription Protocols**_

Despite the extensive list of features we plan to cover, we will also address a few additional aspects specific to SNS FIFO topics. After completing these comparisons, we will delve deeper into certain topics that are relevant to both Standard and FIFO topics.

### System Architecture as an Example <a href="#id-27ef" id="id-27ef"></a>

Before diving into the details of Standard and FIFO topics, I want to introduce a basic system architecture that we’ll use as a reference. This will help us clearly understand the key differences between these two types of topics. Our system includes a notification flow where SNS serves as a fanout mechanism, publishing messages to various consumers. These consumers, primarily SQS queues, will hold the messages until they are processed. The following diagram provides a high-level overview of this design.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*cOXoRI_OzJFWmv_KnLJkcQ.png" alt="" height="348" width="700"><figcaption><p>Notification System Flow</p></figcaption></figure>

In our notification system, a publisher (producer) sends notifications to inform users about events in the system.

We have different flows for each notification type: _email notifications_, _SMS notifications_, and a third flow that uses _Firehose_ to dump data into S3 for further analytics. As shown, SNS effectively acts as a fanout pattern, distributing a single notification to multiple interested subscribers (AWS services).

## Standard Topic <a href="#c458" id="c458"></a>

AWS defined a “Standard Topic” as:

* **Best-effort message ordering:** SNS will try it is best to deliver messages in the same order they were produced, however, it is not guaranteed that the ordering will be preserved.
* **At-least once message delivery:** SNS will do multiple retries until the message is delivered and processed.
* **Highest throughput in publishes/second**

### Message Ordering <a href="#id-4897" id="id-4897"></a>

as mentioned before ordering is best effort, with that being said SNS does not guarantee that your messages will be delivered and processed in the same order they were published, your consumers should be aware of that and they should implement logic to handle this kind of behavior, as SNS standard topic support destinations such as Lambda and SQS _**you will need to know that you can not use SNS standard topic with SQS FIFO, and for lambda invocation, SNS invoke lambda in an asynchronous manner.**_

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*QaJZ1GPbNNci4zuUqw9A8g.png" alt="" height="371" width="700"><figcaption><p>Message Delivery in SNS Standard Topic</p></figcaption></figure>

As the diagram illustrates, a publisher sends two messages (M1 and M2) sequentially to an SNS Standard Topic. However, once these messages are broadcast to their consumers, they may be received in a different order than they were sent. We intentionally highlight two cases of potential message disorder: one for SQS (SMS) and another for Firehose. For some consumers, like Firehose, the order may not be crucial since data is aggregated before being dumped into S3. However, for others, such as SQS, maintaining the order might be critical. It’s important to account for these considerations in your implementation.

### Message Duplication <a href="#id-66bb" id="id-66bb"></a>

SNS Standard Topic uses an at-least-once message delivery model, meaning your message can be delivered multiple times to consumers. Duplications can occur at the publisher level, as illustrated in the diagram:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*GLSN5geXMN5TokWXa9wO0A.png" alt="" height="371" width="700"><figcaption><p>SNS Standard Topic Duplication</p></figcaption></figure>

For instance, a publisher may attempt to send a message to SNS but fail to receive an acknowledgment due to network issues. Consequently, the publisher retries, and the message is successfully delivered. However, the SNS Standard Topic cannot detect and filter out duplicate messages, resulting in both messages being sent to subscribers. It’s crucial for subscribers to manage these duplications.

> To mitigate this, ensure that your consumers are designed to be idempotent. One way to support idempotency is by assigning a UUID to your message within its attributes, allowing subscribers to identify and handle duplicate messages effectively.

### Throughput & Performance <a href="#a926" id="a926"></a>

AWS does not have a unified number of the `Publish` API, as it differs from one region to another, thus we recommend checking the [AWS documentation](https://docs.aws.amazon.com/general/latest/gr/sns.html#limits_sns) for the limit per the region you are operating at.

Keeping in mind the general rule of thumb that a Standard Topic has a higher API request per second than a FIFO topic.

### Supported Subscription Protocols <a href="#id-6fbe" id="id-6fbe"></a>

In the context of AWS SNS, “protocols” refer to the destination services or integration options available for delivering messages published to an SNS standard topic. These protocols determine how and where the messages are routed once they are sent to a topic. In this section we only cover the _Application 2 Application (A2A)_ integration, let’s explore the various supported A2A destinations for the standard topic:

1. **AWS Firehose:** Deliver events to delivery streams for archiving and analysis purposes. Through delivery streams, you can deliver events to AWS destinations like Amazon Simple Storage Service (Amazon S3), Amazon Redshift, and Amazon OpenSearch Service (OpenSearch Service), or to third-party destinations.
2. **AWS Lambda:** Deliver events to functions to trigger the execution of custom business logic.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*mIxPKZsNv7zRPePVVamloA.png" alt="" height="314" width="700"><figcaption><p>Lambda Invocation with SNS</p></figcaption></figure>

When discussing the integration between AWS SNS and AWS Lambda, it’s important to clarify a common misconception regarding the nature of their interaction. Many users mistakenly believe that SNS invokes Lambda functions in a “_**Synchronous”**_ manner. Consequently, when they use the SNS `Publish` API to send a message to Lambda and receive a 200 status code, they often assume that the Lambda function has been executed successfully. However, this assumption is _incorrect_.

The invocation of Lambda functions by SNS actually occurs in an “_**Asynchronous”**_ manner. When SNS publishes a message to a Lambda function, the message is placed into what is known as the “Event Queue,” an internal component within the Lambda architecture. The 200 status code returned by the `Publish` API simply indicates that the message has been successfully placed in this queue, not that the Lambda function has completed execution.

Once the message is in the Event Queue, Lambda begins processing the invocation _**Asynchronously**_. If the Lambda function fails to execute successfully, the retry policy configured in SNS does not apply. Instead, Lambda employs its own retry mechanism, which attempts to execute the function up to three times with an exponential backoff.

3\. **AWS SQS:** Deliver events to functions for triggering the execution of custom business logic, keep in mind that the SNS standard topic can only be connected to a “Standard SQS”.

4\. **AWS Event Fork Pipelines:** Deliver events to event backup and storage, event search and analytics, or event replay pipelines.

5\. **HTTP/S:** Deliver events to external webhooks. Let us examine this in more depth as it is an important topic to know about, You can use Amazon SNS to send notification messages to one or more HTTP or HTTPS endpoints. When you subscribe an endpoint to a topic, you can publish a notification to the topic and Amazon SNS sends an HTTP POST request delivering the contents of the notification to the subscribed endpoint. When you subscribe to the endpoint, you choose whether Amazon SNS uses HTTP or HTTPS to send the POST request to the endpoint. If you use HTTPS, then you can take advantage of the support in Amazon SNS for the following:

* **Server Name Indication (SNI)** — This allows Amazon SNS to support HTTPS endpoints that require SNI, such as a server requiring multiple certificates for hosting multiple domains.
* **Basic and Digest Access Authentication** — This allows you to specify a username and password in the HTTPS URL for the HTTP POST request, such as `https://user:password@domain.com` or `https://user@domain.com` The username and password are encrypted over the SSL connection established when using HTTPS. Only the domain name is sent in plaintext.

> Amazon SNS does not currently support private HTTP(S) endpoints.
>
> HTTPS URLs are only retrievable from the Amazon SNS `GetSubscriptionAttributes` API action, for principals to which you have granted API access.
>
> The client service must be able to support the `HTTP/1.1 401 Unauthorized` header response

before adding an endpoint as a subscription to a topic make sure that your endpoint follows these rules from the [documentation from the AWS](https://docs.aws.amazon.com/sns/latest/dg/sns-subscribe-https-s-endpoints-to-topic.html).

## FIFO Topic <a href="#id-5e28" id="id-5e28"></a>

FIFO topics (First-In First-Out) can be defined as:

* **Strictly-preserved message ordering:** SNS guarantees that the messages will be processed in the same order they have been published.
* **Exactly-once message delivery:** SNS will make sure to deliver only one copy of the message when the right conditions are met.
* **High throughput but less than Standard Topics**

### Message Ordering <a href="#c42e" id="c42e"></a>

SNS FIFO topics make sure that it delivers messages to subscribed Amazon SQS queues in the exact order in which the messages are published to the topic, and only once.

> SQS is the only supported protocol in SNS FIFO topics, thus when mentioning a subscriber in this section we refer to SQS, however SNS Support both Standard SQS and FIFO SQS.

With an Amazon SQS FIFO queue subscribed, the consumer of the _**queue**_ receives the messages in the _**exact order**_ in which the messages are delivered to the _**queue**_, and no duplicates.

With an Amazon _**SQS standard queue**_ subscribed, however, the consumer of the queue may receive messages _**out of order**_, and _**more than once**_. This enables further decoupling of subscribers from publishers, giving subscribers more flexibility in terms of message consumption and cost optimization, as shown in the following diagram:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*yt2XKGHyNGj56sLwNeOqGQ.png" alt="" height="231" width="700"><figcaption><p>SNS FIFO Topic with SQS Standard and FIFO</p></figcaption></figure>

let us analyze the flows to see how SNS FIFO interacted with each of the queues:

1. _**SQS FIFO Email Queue:**_ SNS FIFO published the messages in the same orders they were produced by the publisher and made sure that they were delivered exactly one time, furthermore, the SQS FIFO did more work on its side and ensured that messages will be also consumed by workers (lambda) in the same order they were published to it from the SNS, so the end result is strictly ordered and no-duplication occurs.
2. **SQS Standard SMS Queue:** SNS FIFO published the messages in the same orders they were produced by the publisher and made sure that they were delivered exactly one time, however on the SQS Standard side things got a little bit different this time, “SQS Standard” due to its nature and design does not guarantee the ordering will be preserved and due to how it works, it also does not guarantee exactly-once delivery, for that the end result **MAY** have duplication and **MAY** be out of order.

### **Message Ordering With Parallel Publishers** <a href="#ba7e" id="ba7e"></a>

You can have multiple applications (or multiple threads within the same application) publishing messages to an SNS FIFO topic in parallel. When you do this, you effectively delegate message sequencing to the Amazon SNS service. To determine the established sequence of messages, you can check the sequence number.

The sequence number is a large, non-consecutive number that Amazon SNS assigns to each message. The length of the sequence number is 128-bits, and continues to increase for each Message Group (we talk in detail about it later). The sequence number is passed to the subscribed Amazon SQS queues as part of the message body. However, if you enable raw message delivery (mentioned later), the message that’s delivered to the Amazon SQS queue doesn’t include the sequence number or any other Amazon SNS message metadata.

The Following Diagram explains this in detail:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*0idL8GUNZe-bfSSp2EACMQ.png" alt="" height="330" width="700"><figcaption><p>Sequence Numbers in SNS</p></figcaption></figure>

Amazon SNS FIFO topics define ordering in the context of a _**message group**_, a feature which we discuss next.

### Message Grouping In FIFO Topics <a href="#id-6c95" id="id-6c95"></a>

I wanted to address “Message Grouping” to clarify a few key concepts about SNS FIFO Topics before we dive into more technical details. Understanding this aspect is crucial, as it underpins how FIFO topics provide their delivery and ordering guarantees.

So what is _**Message Grouping?**_

Message grouping is a mechanism that ensures messages with the same key (same ID) are routed to the same partition. This is achieved by assigning a specific value to each message, which determines its placement within a particular partition, ensuring related messages are processed together.

SNS FIFO provides a field called _**“MessageGroupID”**_ which is used when using the `Publish` API and that field is used as a routing key for messages.\
The Following Diagram shows a high-overview of how MessageGrouping works:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*I4ZTEqtsUEeM44C90Jm86Q.png" alt="" height="414" width="700"><figcaption><p>MessageGroupID Routing Mechanism</p></figcaption></figure>

Messages that belong to the same group (partition) are processed one by one, in a strict order relative to the group.

When you publish messages to an Amazon SNS FIFO topic, you set the message group ID. The group ID is a mandatory token that specifies that a message belongs to a specific message group. _**The SNS FIFO topic passes the group ID to the subscribed Amazon SQS FIFO queues.**_ There is no limit to the number of group IDs in SNS FIFO topics or SQS FIFO queues.\
&#xNAN;_**Message group ID is not passed to Amazon SQS “standard” queues.**_

There’s no affinity between a message group and a subscription. Therefore, messages that are published to any message group are delivered to all subscribed queues, subject to any filter policies attached to subscriptions.

Let us go back to the notification system design that we had before, and let us examine how the message grouping works on both FIFO and Standard Topics

<figure><img src="https://miro.medium.com/v2/resize:fit:2000/1*WDurLVFaTfj6PWg1nnHifA.png" alt="" height="299" width="1000"><figcaption><p>FIFO Topic Message Grouping Behaviour</p></figcaption></figure>

notice the following, there’s a dedicated message group for each userId sold on the platform. The same Amazon SNS FIFO topic is used for processing all notifications. The sequence of notification updates is preserved within the context of a single user (userId), but not across multiple users. The diagram shows how this works. Notice that, for the user whose message group ID is **user-1**, message **m1** is processed before message **m3**. This sequence is preserved throughout the workflows that use Amazon SNS FIFO to Amazon SQS FIFO.

Likewise, for the user whose message group ID is **user-2**, message **m2** is processed before message **m4**, as long as the workflows use _Amazon SNS FIFO and Amazon SQS FIFO_. However, when using Amazon SQS standard queues, the message order is no longer guaranteed and message groups do not exist. The **user-1** and **user-2** message groups are independent of each other, so there is no relationship between how their messages are sequenced.

### Improve Performance By Distributing messages through GroupID <a href="#a070" id="a070"></a>

To optimize delivery throughput, Amazon SNS FIFO topics deliver messages from different message groups in parallel, while message order is strictly maintained within each message group. Each individual message group can deliver a maximum of 300 messages per second. Therefore, to achieve high throughput for a single topic, use a large number of distinct message group IDs. By utilizing a diverse set of message groups, Amazon SNS FIFO topics automatically distribute messages across a larger number of parallel partitions.

When publishing to your Amazon SNS FIFO topic with high throughput and one or more Amazon SQS FIFO queues are subscribed, it is recommended that you enable “high throughput” on your _**queues**_.

> Amazon SNS FIFO topics are optimized for uniform distribution of messages across message group IDs, regardless of the number of groups. AWS recommends that you use a large number of distinct message group IDs for optimized performance.

### Message Duplication <a href="#id-35d6" id="id-35d6"></a>

Amazon SNS FIFO (first in, first out) topics support delivery to both Amazon SQS standard and FIFO queues to provide customers with flexibility and control when integrating distributed applications that require data consistency in near real-time.

Amazon _**SNS FIFO**_ topics and Amazon _**SQS FIFO**_ queues support message deduplication, which provides exactly-once message delivery and processing as long as the following conditions are met:

* The subscribed Amazon SQS FIFO queue exists and has permissions that allow the Amazon SNS service principal to deliver messages to the queue.
* The Amazon SQS FIFO queue consumer processes the message and deletes it from the queue before the visibility timeout expires.
* The Amazon SNS subscription topic has no message filtering. When you configure message filtering, Amazon SNS FIFO topics support at-most-once delivery, as messages can be filtered out based on your subscription filter policies.
* There are no network disruptions that prevent acknowledgment of the message delivery.

When you publish a message to an Amazon SNS FIFO topic, the message must include a deduplication ID. This ID is included in the message that the Amazon SNS FIFO topic delivers to the subscribed Amazon SQS FIFO queues.

If a message with a particular deduplication ID is successfully published to an Amazon SNS FIFO topic, any message published with the same deduplication ID, within the _**five-minute deduplication interval**_, is accepted but not delivered. The Amazon SNS FIFO topic continues to track the message deduplication ID, even after the message is delivered to subscribed endpoints.

If the message body is guaranteed to be unique for each published message, you can enable content-based deduplication for an Amazon SNS FIFO topic and the subscribed Amazon SQS FIFO queues. Amazon SNS uses the message body to generate a unique hash value to use as the deduplication ID for each message, so you don’t need to set a deduplication ID when you send each message.

When content-based deduplication is enabled for an Amazon SNS FIFO topic, and a message is published with a deduplication ID, the published deduplication ID overrides the generated content-based deduplication ID.

> Message deduplication applies to an entire Amazon SNS FIFO topic, not to an individual message group.

In the notification system that we have, the system must set a universally unique deduplication ID for each notification update. This is because the message body can be identical even when the message attribute is different

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*x6vTd5Xj_PLSXRASukfF4Q.png" alt="" height="231" width="700"><figcaption><p>Understanding Duplication in FIFO Topics</p></figcaption></figure>

### Throughput & Performance <a href="#id-2a33" id="id-2a33"></a>

Unlike SNS Standard Topics, FIFO topics have a unified throughput in all regions, AWS mentions that for a FIFO topic, you can send 3,000 messages per second or 20 MB per second, per topic, whichever comes first.

For cross-region delivery cases, FIFO topics support 1,000 messages per second or 6 MB per second, whichever comes first.

### Supported Subscription Protocols <a href="#fd3d" id="fd3d"></a>

AWS FIFO only supports one protocol which is AWS SQS, to sum it up on which SQS type you should choose with your FIFO Topic:

* For workloads that need to preserve strict message ordering or de-duplication, the combination of Amazon SNS FIFO topics with [Amazon SQS FIFO queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues.html) subscribed as the delivery endpoint provides enhanced messaging between applications when the order of operations and events is critical, or where duplicates can’t be tolerated.
* For workloads that tolerate best-effort ordering and at-least-once delivery, subscribing [Amazon SQS standard queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues.html) to Amazon SNS FIFO topics provides the ability to lower costs, in addition to sharing queues across workloads that don’t utilize FIFO.

> SNS FIFO topics can’t deliver messages to customer managed endpoints, such as email addresses, mobile apps, phone numbers for text messaging (SMS), or HTTP(S) endpoints. These endpoint types _**aren’t**_ guaranteed to preserve strict message ordering. Attempts to subscribe customer managed endpoints to SNS FIFO topics result in errors.

## Message Delivery <a href="#id-6a7f" id="id-6a7f"></a>

In this section we talk about what are the available options to deliver messages to SNS protocol, this delivery applies to both types of SNS, then in the next section, we discuss the durability aspect of SNS and introduce a few patterns to how you will make sure that your messages will be preserved.

### Raw Message Delivery <a href="#id-165f" id="id-165f"></a>

We start this by talking about available options to deliver messages in SNS, SNS provides two types of message formats for delivery:

1. **Raw message delivery Disabled**
2. **Raw message delivery Enabled**

let us examine when to use each one of them, basically, an SNS message is a JSON object that contains more information than just a body of the message in it, a normal SNS message will look like this at the consumer level:

```
{
  "Type": "Notification",
  "MessageId": "dc1e94d9-56c5-5e96-808d-cc7f68faa162",
  "TopicArn": "arn:aws:sns:us-east-2:111122223333:ExampleTopic1",
  "Subject": "TestSubject",
  "Message": "This is a test message.",
  "Timestamp": "2021-02-16T21:41:19.978Z",
  "SignatureVersion": "1",
  "Signature": "FMG5tlZhJNHLHUXvZgtZzlk24FzVa7oX0T4P03neeXw8ZEXZx6z35j2FOTuNYShn2h0bKNC/zLTnMyIxEzmi2X1shOBWsJHkrW2xkR58ABZF+4uWHEE73yDVR4SyYAikP9jstZzDRm+bcVs8+T0yaLiEGLrIIIL4esi1llhIkgErCuy5btPcWXBdio2fpCRD5x9oR6gmE/rd5O7lX1c1uvnv4r1Lkk4pqP2/iUfxFZva1xLSRvgyfm6D9hNklVyPfy+7TalMD0lzmJuOrExtnSIbZew3foxgx8GT+lbZkLd0ZdtdRJlIyPRP44eyq78sU0Eo/LsDr0Iak4ZDpg8dXg==",
  "SigningCertURL": "https://sns.us-east-2.amazonaws.com/SimpleNotificationService-010a507c1833636cd94bdb98bd93083a.pem",
  "UnsubscribeURL": "https://sns.us-east-2.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-east-2:111122223333:ExampleTopic1:e1039402-24e7-40a3-a0d4-797da162b297"
}
```

However, we need to keep in mind that there are few services that does not need to work with all this information, which may mean that if we used the full JSON we will incur additional charges due to storage and network.

For Standard Topic, for example, to avoid having Amazon Data Firehose, Amazon SQS, and HTTP/S endpoints process the JSON formatting of messages, Amazon SNS allows raw message delivery:

* When you enable raw message delivery for Amazon Data Firehose or Amazon SQS endpoints, any Amazon SNS metadata is stripped from the published message, and the message is sent as is.
* When you enable raw message delivery for HTTP/S endpoints, the HTTP header `x-amz-sns-rawdelivery` with its value set to `true` is added to the message, indicating that the message has been published without JSON formatting.
* When you enable raw message delivery for HTTP/S endpoints, the message body, client IP, and the required headers are delivered. When you specify message attributes, it won’t be sent.
* When you enable raw message delivery for Firehose endpoints, the message body is delivered. When you specify message attributes, it won’t be sent.

To enable Raw Message Delivery you have two options, either doing that from the AWS Console or using AWS SDK, you must use the `SetSubscriptionAttribute` API action and set the value of the `RawMessageDelivery` attribute to `true`.

If we enabled the Raw Delivery of the message, our above JSON example will be delivered in the following format to protocols

```
This is a test message.
```

### Message attributes and raw message delivery for Amazon SQS subscriptions <a href="#id-3faf" id="id-3faf"></a>

Amazon SNS supports the delivery of message attributes, which allow you to provide structured metadata items, such as timestamps, signatures, version, and identifiers, about the message. For Amazon SQS subscriptions with **Raw Message Delivery** enabled, a maximum of 10 message attributes can be sent. To send more than 10 message attributes, you must disable Raw Message Delivery. However, Amazon SNS discards messages with more than 10 message attributes directed towards Amazon SQS subscriptions with Raw Message Delivery enabled, treating them as client-side errors.

### Message delivery status <a href="#id-3365" id="id-3365"></a>

Amazon SNS provides support to log the delivery status of notification messages sent to topics with the following Amazon SNS endpoints:

* HTTP
* Amazon Data Firehose
* AWS Lambda
* Platform application endpoint
* Amazon Simple Queue Service (Only option for FIFO Topics)

After you configure the message delivery status attributes, log entries are sent to CloudWatch Logs for messages sent to topic subscribers. Logging message delivery status helps provide better operational insight, such as the following:

* Knowing whether a message was delivered to the Amazon SNS endpoint.
* Identifying the response sent from the Amazon SNS endpoint to Amazon SNS.
* Determining the message dwell time (the time between the publish timestamp and just before handing off to an Amazon SNS endpoint).

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*7pR34UFL3JdNN_1Tm12YMQ.png" alt="" height="437" width="700"><figcaption><p>Delivery Status</p></figcaption></figure>

invoking a lambda function from an SNS while the delivery status logging is enabled will log a similar record in the Cloudwatch logs

```
{
    "notification": {
        "messageMD5Sum": "ed5359919d1600abfdb43cb90b01ba9c",
        "messageId": "3fcf1d02-fe87-5fb0-9358-424556de759d",
        "topicArn": "arn:aws:sns:us-east-2:<account_id>:testSNStopic",
        "timestamp": "2024-08-27 10:10:33.576"
    },
    "delivery": {
        "deliveryId": "72f4e6e9-dd5e-58c8-828e-b9ae7507af70",
        "destination": "arn:aws:lambda:us-east-2:<account_id>:function:SNSLambdaTester",
        "providerResponse": "{\"lambdaRequestId\":\"a2f654bc-9d46-483c-b62a-870e4c4bbee9\"}",
        "dwellTimeMs": 47,
        "statusCode": 202
    },
    "status": "SUCCESS"
}js
```

## SNS Message Durability <a href="#id-4526" id="id-4526"></a>

We have been talking so far on the happy path of SNS where all messages are delivered to their destination and no errors or network issues happen, in this section, we talk about the path of failing to deliver a message to it is destination, what could happen and how to avoid message lost.

### Why do message deliveries fail? <a href="#id-5ddb" id="id-5ddb"></a>

In general, message delivery fails when Amazon SNS can’t access a subscribed endpoint due to a _client-side_ or _server-side error_. When Amazon SNS receives a client-side error, or continues to receive a server-side error for a message beyond the number of retries specified by the corresponding retry policy, _**Amazon SNS discards the message**_ — unless a dead-letter queue is attached to the subscription. Failed deliveries don’t change the status of your subscriptions.

* _**Client-side errors:**_ Client-side errors can happen when Amazon SNS has stale subscription metadata. These errors commonly occur when an owner deletes the endpoint (for example, a Lambda function subscribed to an Amazon SNS topic) or when an owner changes the policy attached to the subscribed endpoint in a way that prevents Amazon SNS from delivering messages to the endpoint. Amazon SNS doesn’t retry the message delivery that fails as a result of a client-side error.
* _**Server-side errors:**_ Server-side errors can happen when the system responsible for the subscribed endpoint becomes unavailable or returns an exception that indicates that it can’t process a valid request from Amazon SNS. When server-side errors occur, Amazon SNS retries the failed deliveries using either a linear or exponential backoff function. For server-side errors caused by AWS managed endpoints backed by Amazon SQS or AWS Lambda, Amazon SNS retries delivery up to 100,015 times, over 23 days.\
  Customer managed endpoints (such as HTTP, SMTP, SMS, or mobile push) can also cause server-side errors. Amazon SNS retries delivery to these types of endpoints as well. While HTTP endpoints support customer-defined retry policies, Amazon SNS sets an internal delivery retry policy to 50 times over 6 hours, for SMTP, SMS, and mobile push endpoints.

To avoid any message delivery fails we need to save messages in a durable place until they are re-processed or audited.

Let us explore Delivery Retries and then the DLQ pattern.

### SNS message delivery retries <a href="#id-1a0f" id="id-1a0f"></a>

Amazon SNS defines a _delivery policy_ for each delivery protocol. The delivery policy defines how Amazon SNS retries the delivery of messages when server-side errors occur (when the system that hosts the subscribed endpoint becomes unavailable). When the delivery policy is exhausted, Amazon SNS stops retrying the delivery and discards the message — unless a dead-letter queue is attached to the subscription.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*biUGMSZJyclH7X2hNWYVgw.png" alt="" height="309" width="700"><figcaption><p>SNS Message Delivery Retries ~ <a href="https://docs.aws.amazon.com/sns/latest/dg/sns-message-delivery-retries.html">Source AWS Documentation</a></p></figcaption></figure>

> With the exception of HTTP/S, you can’t change Amazon SNS-defined delivery policies. Only HTTP/S supports custom policies.
>
> **The total policy retry time for an HTTP/S endpoint cannot be greater than 3,600 seconds. This is a hard limit and cannot be increased**.

_**Delivery policy stages**_

The following diagram shows the phases of a delivery policy.

<figure><img src="https://miro.medium.com/v2/resize:fit:1342/0*zcBdJR2_9gwhKyU3.png" alt="" height="225" width="671"><figcaption><p>Delivery Retries ~ <a href="https://docs.aws.amazon.com/sns/latest/dg/sns-message-delivery-retries.html">Source AWS Documentation</a></p></figcaption></figure>

Each delivery policy is comprised of four phases:

1. **Immediate Retry Phase (No Delay)** — This phase occurs immediately after the initial delivery attempt. There is no delay between retries in this phase.
2. **Pre-Backoff Phase** — This phase follows the Immediate Retry Phase. Amazon SNS uses this phase to attempt a set of retries before applying a backoff function. This phase specifies the number of retries and the amount of delay between them.
3. **Backoff Phase** — This phase controls the delay between retries by using the retry-backoff function. This phase sets a minimum delay, a maximum delay, and a retry-backoff function that defines how quickly the delay increases from the minimum to the maximum delay. The backoff function can be arithmetic, exponential, geometric, or linear.
4. **Post-Backoff Phase** — This phase follows the backoff phase. It specifies a number of retries and the amount of delay between them. This is the final phase.

### How do dead-letter queues work? <a href="#c153" id="c153"></a>

A dead-letter queue is attached to an Amazon SNS subscription (rather than a topic) because message deliveries happen at the subscription level. This lets you identify the original target endpoint for each message more easily.

A dead-letter queue associated with an Amazon SNS subscription is an ordinary Amazon SQS queue. You can change the message retention period using the Amazon SQS `SetQueueAttributes` API action. To make your applications more resilient, it is recommend setting the maximum retention period for dead-letter queues to 14 days.

### DLQ with Notification System <a href="#b5d5" id="b5d5"></a>

Going back to our notification system and how it is implemented, we want to add an additional layer of durability, to do that we can assign an Amazon SQS dead-letter queue (DLQ) to each Amazon SNS topic _**subscription**_, as well as to each subscribed Amazon SQS queue. This protects us from any notification update loss.

The Following Diagram shows our updated architecture where we add a DLQ on each subscriber level and on the SQS level as well:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*ph-7oEUwiUWDF3qwj5UB-A.png" alt="" height="525" width="700"><figcaption><p>DLQ pattern with SNS</p></figcaption></figure>

The dead-letter queue associated with an Amazon SNS subscription must be an Amazon SQS queue of the same type as the subscribing queue. For example, the Amazon SNS FIFO subscription for an Amazon SQS FIFO queue must have an Amazon SQS FIFO queue as the dead-letter queue. Similarly, the Amazon SNS FIFO subscription for an Amazon SQS standard queue must have an Amazon SQS standard queue as its dead-letter queue.

For extended durability to assist in recovery from downstream failures, topic owners can also use FIFO topics to archive messages up to 365 days. Topic subscribers can then replay those messages to a subscribed endpoint to recover messages lost due to a failure in a downstream application, or to replicate the state of an existing application. (We Discuss Archiving and Replay in the next section).

For More Information About SQS and DLQ please refer to this [blog post](https://medium.com/metalab-tech/aws-sqs-deep-dive-dfed03c8b640).

## Message Archiving and Replay (FIFO Topics Only) <a href="#id-72ca" id="id-72ca"></a>

Amazon SNS message archiving and replay is a no-code, in-place message archive that lets topic owners store (or _archive_) messages within their topic. Topic subscribers can then retrieve (or _replay_) the archived messages back to a subscribed endpoint, which can be used to:

* Recover messages that may have been lost due to a failure in a downstream application.
* Replicate the state of an existing application to a new application by subscribing to the new endpoint, and selecting the desired timestamp to replicate from.

> Amazon SNS message archiving and replay is only available for application-to-application (A2A) FIFO topics.

Message archiving and replay consists of two main components:

1. **Message archiving** — The topic owner enables the archiving and replay feature on a topic, and sets a message retention period (up to 365 days). The topic owner can also monitor archived messages using Amazon CloudWatch metrics.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*IfHY3OIPfQXT7xoUYrFflw.png" alt="" height="334" width="700"><figcaption><p>Archiving Policy in FIFO Topic</p></figcaption></figure>

**2. Message replay** — The topic subscriber initiates a replay for a set of messages from the topic to their subscribed endpoint.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*PPVx8EgEQu6mGsBMH_jvkQ.png" alt="" height="423" width="700"><figcaption><p>Replay Messages For A Subscriber</p></figcaption></figure>

## SNS Subscriber “Message Filtering” <a href="#id-15d2" id="id-15d2"></a>

Going back to our notification system design example, our system assumes that each notification we want to send is an SMS and Email notification, so until now there is no way for us to send a specific kind of notification for SMS only or Email only, well let us not worry about that because SNS will take care of this filtering for us, SNS introduced what is know as “Message Filtering”.

By default, an Amazon SNS topic subscriber receives every message that’s published to the topic. To receive only a subset of the messages, a subscriber must assign a _filter policy_ to the topic subscription.

A filter policy is a JSON object containing properties that define which messages the subscriber receives. Amazon SNS supports policies that act on the message attributes or on the message body, according to the filter policy scope that you set for the subscription. Filter policies for the message body assume that the message payload is a well-formed JSON object.

If a subscription doesn’t have a filter policy, the subscriber receives every message published to its topic. When you publish a message to a topic with a filter policy in place, Amazon SNS compares the message attributes or the message body to the properties in the filter policy for each of the topic’s subscriptions. If any of the message attributes or message body properties match, Amazon SNS sends the message to the subscriber. Otherwise, Amazon SNS doesn’t send the message to that subscriber.

> The `FilterPolicyScope` subscription attribute lets you choose the filtering scope by setting one of the following values:
>
> `MessageAttributes` – The filter policy is applied to the message attributes. This is the default.
>
> `MessageBody` – The filter policy is applied to the message body.

for a full list of available filter operations available, we would recommend reading the [official documentation](https://docs.aws.amazon.com/sns/latest/dg/sns-subscription-filter-policies.html).

### Applying “Message Filtering” to our notification system <a href="#id-3600" id="id-3600"></a>

Let us now apply message filtering to our application flow and see what we can achieve with it

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*kADDlMRBoE717nR-mlf6-g.png" alt="" height="462" width="700"><figcaption><p>Notification System with “Message Filtering”</p></figcaption></figure>

let us explore how our publisher now sends messages and how the policies are applied to the subscribers, first, we have two subscribers for each one of them we will have a “Message Attribute” policy that looks like this:

```
# Email FIFO Queue Policy
{
  "NotificationServiceTarget": ["EMAIL"]
}
```

```
# SMS FIFO Queue Policy
{
  "NotificationServiceTarget": ["SMS"]
}
```

now when our publisher service that uses the SNS SDK publishes a message, the code used will look something like this

```
import { SNSClient, PublishCommand } from "@aws-sdk/client-sns";
const client = new SNSClient(config);

const input = {
  TopicArn: "topic_arn_value",
  Message: "This is a notification message body...",
  MessageAttributes: {
    "Version": {
      DataType: "Number",
      StringValue: "1"
    },
    // Determines the target subscriber for this message
    "NotificationServiceTarget": {
      DataType: "String",
      StringValue: "EMAIL"
    }
  },
  MessageDeduplicationId: "fec3f001-1e0c-4fd2-b26d-e975bbe9021c",
  MessageGroupId: "user_id_1234",
  Subject: "Random Notification"
};
const command = new PublishCommand(input);
const response = await client.send(command);
```

The command above publishes a message to SNS, which is then distributed to all subscribers. At this stage, SNS checks for any filtering policies attached to the subscribers. If a filtering policy exists, SNS compares it with the message attributes or body to determine whether the message should be delivered to that subscriber.

In this example, the notification is processed only by the SQS Email subscriber, as the `NotificationServiceTarget` attribute is set to `EMAIL`. The SMS subscriber does not receive the message because its filtering policy requires the value to be `SMS`.

With this, we can conclude this section, having demonstrated how to apply message filtering in notification systems and ensure it is implemented correctly.

## Message data protection (Standard Topic Only) <a href="#b054" id="b054"></a>

One last feature we want to talk about in SNS is data protection, SNS provides out-of-the-box features for protecting data that flows into SNS, while SNS provides the ability to encrypt data through the usage of AWS KMS service, there are additional ways that allow you to protect/audit sensitive data passed through your SNS topic.

### What is message data protection? <a href="#d6fe" id="d6fe"></a>

**Message data protection** safeguards the data that’s published to your Amazon SNS topics by using data protection policies to audit, mask, redact, or block the sensitive information that moves between applications or AWS services.

Message data protection scans data in motion for personally identifiable information (PII) and protected health information (PHI) using _data identifiers_. You can choose to use predefined (or Amazon SNS managed) data identifiers (for example, names, addresses, credit card numbers, and prescription drug codes), or you can create your own custom data identifiers, specific to your business use case. Using the scanned information, message data protection provides detailed audit logs, and allows you to take action to protect that data.

Message data protection supports the following actions to help protect sensitive customer information:

* **Audit** — Audit up to 99% of the data that are published to an Amazon SNS topic. You can then choose to send the findings to _Amazon CloudWatch, Amazon S3, or Amazon Data Firehose._
* **De-identify** — Mask or redact sensitive data without interrupting message publishing or delivery.
* **Deny** — Block the transmission of data between applications and AWS resources if sensitive data is present within the payload.

> Amazon SNS supports message data protection for Amazon **SNS standard topics only.**

By introducing message data protection into your governance, risk management, and compliance programs, you can implement data protection policies that help you to identify and prevent data leakage. This provides your teams with tools that can help to reduce financial, legal, and regulatory risks by complying with privacy regulations such as HIPAA, GDPR, PCI, and FedRAMP.

### Data protection policies <a href="#e72e" id="e72e"></a>

Amazon SNS uses **data protection policies** to select the sensitive data for which you want to scan, and the actions that you want to take to protect that data from being exchanged by your Amazon SNS topics. To select the sensitive data of interest, you use _**data identifiers**_. Amazon SNS message data protection then detects the sensitive data by using machine learning and pattern matching.

To act upon data identifiers that are found, you can define an **audit**, **de-identify**, or **deny** operation. These operations let you log the sensitive data that is found (or not found), mask or redact sensitive data, or deny message delivery.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*sYNtNOCxeq-_dHM_X70PLQ.png" alt="" height="520" width="700"><figcaption><p>Data Protection Policies In Action</p></figcaption></figure>

Policies are defined on the SNS Topic level and can be used for various kind of actions

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*_Vo26DHZzS9g0nNqviMjJA.png" alt="" height="575" width="700"><figcaption><p>SNS Data Protection Settings</p></figcaption></figure>

Two approaches worth highlighting include directing all audit findings into an S3 bucket for end-of-day analytics to identify potential issues. Another option is data masking, which is particularly useful when dealing with sensitive information. If your data may contain sensitive elements that you prefer not to expose to downstream systems, especially when interacting with third-party services outside of AWS, you can implement masking to obscure these values. This ensures that consumers have no visibility into the sensitive data.

## Conclusion <a href="#id-90e6" id="id-90e6"></a>

At the end of the day, SNS can be thought of as the go-to tool when you consider one of these scenarios:

* Sending email, SMS, and push notifications to end-users in realtime. Note that you can send push notifications to Apple, Google, Fire OS, and Windows devices.
* Broadcasting messages to multiple subscribers (for example, fanning out the same push notifications to all users of your app).
* Workflow systems. For example, you can use SNS to pass events between distributed apps, or to update records in various business systems (e.g., inventory changes).
* Realtime alerts and monitoring applications.

SNS stands as one of the great services that AWS offers to build a resilient, scalable application in the cloud, the integration details differ from one application to another, but at the high-level, it is important to know when to pick this service, and hopefully our blog post contains all the information you need to start using SNS in your system.

\
