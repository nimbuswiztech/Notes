# AWS CLI

In the fast-paced world of cloud computing, managing and interacting with your resources efficiently is key to success. Amazon Web Services (AWS) provides a powerful tool for this purpose — the AWS Command Line Interface (CLI). The AWS CLI empowers users to interact with AWS services directly from the command line, offering flexibility, speed, and automation capabilities.

Unlock the power of AWS CLI with our step-by-step guide on configuration and command execution. Delve into the heart of cloud computing as we walk you through the installation process, credential setup, and practical examples for running essential AWS CLI commands. Whether you’re a newcomer or an experienced user, this guide ensures you confidently navigate the AWS command-line landscape for efficient cloud resource management.

Below are step-by-step instructions to configure the AWS CLI on your local machine or on EC2 instance and run multiple commands:

## Step 1: Install AWS CLI <a href="#id-0f66" id="id-0f66"></a>

Ensure that you have the AWS CLI installed on your local machine. You can download and install it from the [official AWS CLI download page](https://aws.amazon.com/cli/).

## Step 2: Open a Terminal or Command Prompt <a href="#f9e1" id="f9e1"></a>

Open your terminal or command prompt on your local machine.

## Step 3: Run `aws configure` Command <a href="#id-2ace" id="id-2ace"></a>

In the terminal, run the following command:

```
aws configure
```

## Step 4: Enter AWS Access Key ID <a href="#id-3207" id="id-3207"></a>

Enter the AWS Access Key ID associated with your IAM user.

## Step 5: Enter AWS Secret Access Key <a href="#ae6b" id="ae6b"></a>

Enter the AWS Secret Access Key associated with your IAM user.

## Step 6: Set Default Region <a href="#f836" id="f836"></a>

Enter the default region you want to use.

## Step 7: Set Output Format <a href="#id-5af4" id="id-5af4"></a>

Choose the default output format.

Example Interaction:

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*eDGI0OTTNULL9NhPyJXOOg.png" alt="" height="125" width="700"><figcaption></figcaption></figure>

```
AWS Access Key ID [None]: YOUR_ACCESS_KEY_ID
AWS Secret Access Key [None]: YOUR_SECRET_ACCESS_KEY
Default region name [None]: us-central-1
Default output format [None]: json
```

## Step 8: Run AWS CLI Commands <a href="#id-4927" id="id-4927"></a>

### Example 1: List S3 Buckets <a href="#f40a" id="f40a"></a>

```
aws s3 ls
```

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*GmKZkrSsysOkEvwi6k5Bng.png" alt="" height="87" width="700"><figcaption></figcaption></figure>

This command will list the S3 buckets in your AWS account.

### Example 2: Describe EC2 Instances <a href="#id-7e51" id="id-7e51"></a>

```
aws ec2 describe-instances
```

This command will describe your EC2 instances in the default region.

### Example 3: Create an S3 Bucket <a href="#id-17c2" id="id-17c2"></a>

```
aws s3 mb s3://your-new-bucket-name
```

Replace `your-new-bucket-name` with a unique name for your new S3 bucket.

### Example 4: Upload a File to S3 <a href="#id-2799" id="id-2799"></a>

```
aws s3 cp local-file.txt s3://your-new-bucket-name/
```

Replace `local-file.txt` with the path to a file on your local machine and `your-new-bucket-name` with the S3 bucket you just created.

## Step 9: Verification <a href="#id-831a" id="id-831a"></a>

You can verify the results of your commands by checking the AWS Management Console or by running additional commands based on your needs.

### Note: <a href="#a2ad" id="a2ad"></a>

> Remember to replace placeholder values like `YOUR_ACCESS_KEY_ID`, `YOUR_SECRET_ACCESS_KEY`, `your-new-bucket-name`, and `local-file.txt` with your actual AWS credentials, bucket names, and file paths.

## You can store multiple AWS CLI profiles by configuring named profiles in your AWS CLI configuration file (`~/.aws/config`). This allows you to switch between different AWS accounts or IAM roles easily. <a href="#id-24e6" id="id-24e6"></a>

## a. Open or Create the AWS CLI Configuration File: <a href="#id-7bc7" id="id-7bc7"></a>

Open your terminal or command prompt and edit the AWS CLI configuration file. If the file doesn’t exist, it will be created:

```
nano ~/.aws/config
```

## b. Define AWS CLI Profiles: <a href="#id-094e" id="id-094e"></a>

In the configuration file, you can define multiple profiles using the following syntax:

```
[profile profile_name]
region = aws_region
output = output_format
```

* `profile_name`: Replace this with a name for your profile.
* `aws_region`: Set the AWS region you want to associate with the profile.
* `output_format`: Choose the default output format for the profile, e.g., `json`, `text`, or `table`.

Here’s an example of three profiles:

```
[profile customer1]
region = us-east-1
output = json

[profile customer2]
region = us-west-2
output = json

[profile customer3]
region = us-east-1
output = json
```

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*CsMRricH59yMAK4ZxAxEpg.png" alt="" height="213" width="700"><figcaption></figcaption></figure>

Save the changes to the configuration file and exit the text editor.

## d. Configure AWS CLI to Use a Specific Profile: <a href="#id-2340" id="id-2340"></a>

You can specify the profile you want to use for a particular AWS CLI command by using the `--profile` option:

```
aws s3 ls s3://name-of-the-s3bucket --profile customer1
```

This command lists S3 buckets using the `customer1` profile.

Press enter or click to view image in full size

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*f8YNNM-6KZ3-yzyknohXpA.png" alt="" height="418" width="700"><figcaption></figcaption></figure>

## e. Switching Profiles: <a href="#id-7516" id="id-7516"></a>

To switch between profiles, simply use the `--profile` option with different profile names:

```
aws ec2 describe-instances --profile work
```

This command describes EC2 instances using the `work` profile.

By using named profiles, you can manage and switch between different AWS configurations easily, making it convenient to work with multiple AWS accounts or roles from the AWS CLI.
