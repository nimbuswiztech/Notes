# AWS Lamda overview

AWS Lambda is a compute service that allows you to run code for virtually any type of application or serverless service without the need for server administration. AWS Lambda handles all the administrative tasks for you, including server and operating system maintenance, resource allocation, automatic scaling, code monitoring, and logging. All you need to do is provide your code in one of the languages supported by AWS Lambda.

AWS Lambda is used for several key reasons:

1. Cost-Efficiency: You only pay for the compute time your service consumes. Since AWS Lambda functions are event-driven and stateless, you are not charged for idle resources, making it a cost-effective choice.
2. Speed: AWS Lambda functions start and execute quickly, which is ideal for applications requiring low latency and rapid response times.
3. Convenience: Lambda integrates seamlessly with various AWS services, allowing you to build complex workflows and applications easily. It also supports a wide range of event sources, including API Gateway, S3, DynamoDB, and more.
4. Scalability: Lambda can automatically scale to handle a large number of concurrent requests. Depending on the AWS region, you can run anywhere from 1000 to 3000 concurrent executions of your functions, and you can request an increase in this limit through AWS support if needed.

In summary, AWS Lambda offers a cost-effective, fast, and convenient way to build serverless applications that can scale to meet your performance requirements.

there are some limitations to the **AWS Lambda approach**:

1. **Lack of OS Control**: AWS Lambda abstracts away the underlying operating system, which means you cannot manage or customize the operating system itself. You are restricted to the runtime environment provided by AWS.
2. **Limited Control Over Resources**: AWS Lambda automatically manages CPU, memory, and other resources for you based on your function’s configuration. While this simplifies scaling, it also means you have limited control over resource allocation.
3. **Language Constraints**: You can only choose from the programming languages supported by AWS Lambda. This limits your language choices compared to traditional server environments.

Below is a brief list of the main functions of AWS Lambda. We will then discuss each of them in order.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*PhTTqY_ccWbus9Hk.png" alt="" height="675" width="700"><figcaption></figcaption></figure>

## Triggers <a href="#id-382d" id="id-382d"></a>

Triggers are Lambda’s “activators.” In a way, Lambda can be compared to PHP in the sense that it executes and terminates for us. We will delve into the mechanics of how it works in detail shortly. For now, it’s essential to understand that Lambda is a single function that executes in response to triggers.

### Below is a list of all possible triggers <a href="#d21e" id="d21e"></a>

1. API Gateway
2. AWS IoT
3. AWS Step Functions
4. CloudFront
5. CloudWatch Events
6. CloudWatch Logs
7. CodeCommit
8. Cognito Sync Trigger
9. DynamoDB
10. Kinesis
11. S3
12. SNS
13. SQS
14. Alexa Skills Kit
15. Lex
16. SES
17. Alexa Smart Home
18. CloudFormation
19. Custom Application
20. Direct Invocation
21. Mobile Application (e.g., Android, iOS)
22. AWS CloudTrail
23. AWS Config
24. AWS Glue
25. AWS Key Management Service (KMS)
26. AWS Secrets Manager
27. AWS Step Functions (Sync)
28. Aurora (RDS)
29. Elastic File System (EFS)
30. EventBridge
31. EventBridge Archive
32. EventBridge Default Bus
33. IoT Events
34. IoT Events — API Calls
35. IoT Events — API Calls (V2)
36. IoT Events — Events
37. IoT Events — Events (V2)
38. S3 Batch Operations
39. SageMaker
40. SQS FIFO
41. Web Application Firewall (WAF)

Please note that Lambda supports a wide range of triggers, enabling it to respond to various events and integrate with [many AWS services](https://gartsolutions.com/services/devops/aws-devops-services/) and external systems.

For each of these triggers, you can configure unique parameters that are available for these triggers. Additionally, you can set up multiple triggers for a single Lambda function. Whether Lambda executes synchronously or asynchronously depends on the trigger type.

Please note that you can also invoke Lambda manually using the AWS CLI or AWS SDK by passing all the necessary parameters, including specifying whether it should run synchronously or not.

Let’s break it down with examples:

1. API Gateway: This trigger allows you to invoke a Lambda function via an HTTP request and requires a response to be returned to the user. Such an operation cannot be performed asynchronously since it requires a response. For synchronous operations, some features may not be available.
2. SQS (Simple Queue Service): For instance, if your Lambda function processes messages from SQS, there’s no need to return a result anywhere, and it can be executed asynchronously. When executed asynchronously, several new possibilities arise. For example, you can configure retries in case of errors or forward such requests to a “dead letter” SQS queue.

## Permissions to AWS Services <a href="#id-14ad" id="id-14ad"></a>

These are the AWS services that Lambda has default access to. What this means is that in the Lambda function you write, you can always include the AWS SDK, and without keys or any authentication parameters, you can use the available AWS services. You define all the accessible services in the IAM Role you use for that Lambda.

For each programming language used, there is a corresponding SDK that can communicate with core AWS services.

Note: For each Lambda function, you configure an IAM Role that specifies the permissions and privileges under which the Lambda function will operate.

## VPC (Virtual Private Cloud) <a href="#ff21" id="ff21"></a>

You can configure a virtual network for your Lambda function, for example, to establish a secure connection to Amazon RDS (Relational Database Service) or other resources within your Virtual Private Cloud. This allows you to isolate your Lambda functions within your own network, enhancing security and control over network traffic and access to resources.

## Online Editor <a href="#f711" id="f711"></a>

AWS Lambda also provides the option to edit your function’s code directly from a web-based interface in your browser. This feature allows you to make code changes and updates to your Lambda function without the need to download, edit, and re-upload code files manually. It offers a convenient way to make quick code modifications and iterations within the AWS Lambda console.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*_W_7a9IYNLDcm0Q6.png" alt="" height="316" width="700"><figcaption></figcaption></figure>

## Logging <a href="#a4b2" id="a4b2"></a>

All invocations of a Lambda function are logged in CloudWatch, and it also records information about execution time and memory usage. These logs can be instrumental in setting resource limits effectively. Additionally, within your code, you have the capability to log custom data (for example, using `console.log` in Node.js).

Furthermore, you can always view usage statistics for your Lambda function on the Monitoring tab, providing insights into its performance and utilization.

## Environment Variables <a href="#e2e5" id="e2e5"></a>

You have the capability to securely pass environment variables to your code, allowing you to configure critical parts of your system without having to modify the code itself. There’s also an option to encrypt these [environment variables using keys](https://docs.aws.amazon.com/lambda/latest/dg/env_variables.html).

Note: There is a predefined [list of environment variables](https://docs.aws.amazon.com/lambda/latest/dg/lambda-environment-variables.html) available for use as well.

## Code <a href="#id-8c26" id="id-8c26"></a>

Now, let’s dive into the most interesting part. A Lambda function itself consists of several components:

**Layers** — The bottom layer. While not mandatory, if you need to include additional libraries to use your Lambda function, it’s recommended to separate them from the main code. This approach significantly reduces the size of the main code and improves the performance of the function.

Layers in AWS Lambda are somewhat similar to layers in Docker in the sense that they are separate from the function itself and must be managed independently. Moreover, you can reuse layers across different Lambda functions.

**Function Environment** — Within your code, there must be a function that is the entry point for execution (known as the handler). We’ll discuss this in more detail below. Preceding the handler function is its environment, which we configure. The management of resources occurs in such a way that this environment is preserved separately from the function for some time after its execution. When the function is invoked again, this environment is resumed without incurring the overhead of initialization, saving time and resources. Therefore, it’s advisable to initialize everything that is possible before the function itself, such as configurations and library connections.

**Handler** — This is the actual executable code of the Lambda function and varies depending on the programming language you’re using. For example, let’s consider Node.js. To get your code to execute, you typically need:

* One JavaScript file (e.g., index.js)
* An export statement that defines a function (e.g., `exports.yourFunction = () => {//Your code}`)

By default, Lambda looks for an `index.js` file and searches for a function named "handler" within it. It then executes this function. Your function can be asynchronous in your code, and this does not affect the synchronous execution of the Lambda.

Here’s an example of code, and I’ll describe what’s happening within it:

```
// index.js

// Export a function named "handler"
exports.handler = async (event) => {
  try {
    // Your code logic here
    const result = await someAsyncOperation();
    
    // Return a response if needed
    return {
      statusCode: 200,
      body: JSON.stringify(result),
    };
  } catch (error) {
    // Handle errors and return an error response
    return {
      statusCode: 500,
      body: JSON.stringify({ error: 'An error occurred' }),
    };
  }
};
```

## Versioning <a href="#fe2f" id="fe2f"></a>

AWS Lambda offers convenient versioning capabilities. In brief, you can assign a version to each uploaded copy of your function code. Additionally, you can create aliases that point to specific versions. How does this work? Let’s take a look at the diagram below:

Zoom image will be displayed

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/0*8JHMVu8Dlji42FNb.png" alt="" height="663" width="700"><figcaption></figcaption></figure>

**Initial State**

Create the first version of your Lambda function. When you do this, an automatic pointer to the version labeled “$LATEST” is created. This pointer always points to the latest version of your function.

Creating Aliases

1. Add an alias named “Dev.” When creating this alias, you have several options for where to attach it. In this case, you can choose to attach it to a specific version number (e.g., “1”), attach it to “$LATEST,” or attach it to another alias. For this scenario, let’s attach it to “$LATEST.” This means that your “Dev” alias will always point to the latest version of your Lambda function. This allows you to continuously test your application with the latest Lambda version in your development environment. If you ever need to test how it works with an older version, you can simply switch the alias in your trigger or modify the alias reference without affecting your application.
2. Add an alias named “Stage” and attach it to the first version of your Lambda function. This alias will represent the version of your Lambda function that you consider ready for the staging environment.
3. Similarly, add an alias named “Prod” and repeat the steps you took for the “Stage” alias. This alias will represent the version of your Lambda function that you consider ready for the production environment.

**Second State**

1. Create the second version of your Lambda function. You can add new functionality or perhaps modify it to include an additional “Hello world” output. At this point, “$LATEST” will automatically point to the second version you created. Since “Dev” is attached to “$LATEST,” it will also start pointing to the second version.
2. Next, you want “Stage” to also point to the second version. Currently, it’s linked to version “1.” You will need to manually update the version that “Stage” points to, changing it to the second version.

At this stage, you have achieved what you see in the second state on the diagram. “Prod” points to the first version, “Dev” and “Stage” point to the second version.

**Third State**

To get to the third state, you only need to add a few lines of code to your function, creating a third version of your Lambda function. Once you do this, “Dev” will start pointing to this new third version.

By following these steps, you can transition between different versions and aliases of your Lambda function, allowing you to control which version is used in each environment (development, staging, production) easily. This provides flexibility and safety when deploying updates and changes to your serverless application.
