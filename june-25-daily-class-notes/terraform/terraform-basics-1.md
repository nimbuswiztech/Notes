# Terraform Basics

**Introduction:**

Terraform is an open-source infrastructure provisioning tool created by HashiCorp, it allows you to define and manage your infrastructure using a declarative language which is written in GO language.

Terraform allows you to create, modify, and destroy infrastructure resources across different cloud providers, such as AWS, Azure, GCP, etc. It describes the desire state of the infrastructure on the premises environment.

### Benefits of Using Terraform <a href="#c188" id="c188"></a>

🔥 It supports multiple cloud providers such as AWS, Azure, Oracle, GCP, and more.

🔥Terraform configuration is written in easy-to-understand language.

🔥 It simplifies the management and orchestration of multi-tier infrastructure.

🔥Terraform modules allow you to spate resources in dedicated and reusable templates.

### **Terraform Lab setup:** <a href="#id-0639" id="id-0639"></a>

To make a terraform lab setup we have to install the following pre-requisites:

1. Download & install [terraform.](https://developer.hashicorp.com/terraform/downloads)
2. IDE has used [VS code editor](https://code.visualstudio.com/download) can be downloaded.
3. Terraform Extensions for VS Code Editor:

👉Terraform Autocomplete

👉 AWS Tool Kit

👉Simple Terraform Snippets

👉 AWS CLI

4\. Configure AWS Credentials:

👉AWS Account

👉 Generate Security Credentials

👉Configure in AWS CLI

### Terraform Configuration <a href="#id-233b" id="id-233b"></a>

🫱Terraform script/code are stored in plain text file .tf extension

🫱The file that contains terraform code is called a configuration file or terraform manifest.

🫱You should know you're working directory before executing the code, whenever you execute the terraform code you should be in the working directory where the configuration file is available.

### Terraform Configuration Syntax <a href="#id-4701" id="id-4701"></a>

```
<BLOCK TYPE> "<BLOCK LABEL>" "<BLOCK LABEL>"   {
  # Block body
  <IDENTIFIER> = <EXPRESSION> # Argument
}
```

### Blocks and Arguments: <a href="#f07e" id="f07e"></a>

* Blocks are like a container to define other contents of your infrastructure.
* An argument assigns a value to a particular name: argument name and argument value.
* Argument names, block type names, and the names of most Terraform-specific constructs like resources, input variables, etc. are all identifiers.

Press enter or click to view image in full size

> **Let’s starts with terraform basic concepts in terraform registry, providers and resources and terraform workflow.**

**Registry** has all the reusable components for providers and modules. For more information check the website [registry](https://registry.terraform.io/). It provides official, verified, and community tier providers for free in terraform configuration.

**Providers** are plugins that implement resource types. It’s a logical abstraction of an upstream API. They are responsible for understanding API interactions and exposing resources.

**Modules** make your work easy, as we can just plug in the common modules to our terraform code. Modules are self-contained packages of Terraform configurations that are managed as a group.

### **Terraform Workflow** <a href="#id-0ee6" id="id-0ee6"></a>

The workflow consists of lifecycle stages which start with the init, plan, apply, and destroy. All these have been executed as a command for the written code to update or changes in the infrastructure built.

Press enter or click to view image in full size

**Terraform Init**

🎯 Terraform initializes the working directory containing terraform configuration files.

🎯 Terraform initializes downloads and installs the provider’s plugin so that it can later be executed.

🎯Terraform has created a lock file .terraform.lock.hcl to record the provider selections it made above. And also, it initializes the backend configuration.

**Terraform fmt**

🎯Terraform format command is used to rewrite Terraform configuration files to a canonical format and style.

**Terraform Validate**

🎯 The terraform validate command validates the configuration files in a directory.

🎯 Validate command to verify whether a configuration is syntactically valid and thus primarily useful for general verification of reusable modules, including the correctness of attribute names and value types.

🎯It can run before the terraform plan. Validation requires an initialized working directory with any referenced plugins and modules installed.

**Terraform Plan**\
\
🎯 Terraform plan command is used to create an execution plan. It will not modify things in infrastructure. It compares the desired terraform state with the current state in the cloud. It doesn’t change the deployment.

🎯This command is a convenient way to check whether the execution plan for a set of changes matches your expectations without making any changes to the state

**Terraform Apply**

🎯It applies the changes required to reach the desired state of the configuration. This command executes the plan that is already created. It compares the correct state with desire state.

🎯 It will write data into terraform.tfstate file. Once the application is completed, resources are immediately available.

**Terraform Destroy**

🎯The Terraform destroy command is used to delete the created resources in the Terraform-managed infrastructure.

_**In the terraform configuration file, most of the terraform features are controlled by top-level blocks. We need to have the following 3 blocks for creating a terraform configuration.**_

**Terraform Block —** Here is the required terraform version, list of providers required, and terraform backend.

```
terraform {
required_providers {
aws = {
source = "hashicorp/aws"
version = "5.0.1"
}
```

**Provider Block —** It declares providers for Terraform to install the providers which is going to be used. Terraform relies on the providers to interact with remote systems. Provider configuration belongs to the root module.

```
provider "aws" {
region = "us-east-1"
profile = "default"
}
```

**Resource Block —** Each resource block describes one or more infrastructure objects. Each resource block is an infrastructure object, like compute instances, virtual networks, or other components.

```
resource "aws_instance" "webserver" {
ami = "ami_id"
instance_type = "t2.micro"
}
```

**Let us apply the above terraform blocks in configuration and execute commands.**

### **Creating an EC2 Instance on AWS using Terraform** <a href="#a9a7" id="a9a7"></a>

Press enter or click to view image in full size

Press enter or click to view image in full size

Press enter or click to view image in full size

Press enter or click to view image in full size

Press enter or click to view image in full size

Press enter or click to view image in full size

Press enter or click to view image in full size

**Now let us discuss the important files created during execution:**

### **1. Dependency Lock File:** <a href="#id-94f7" id="id-94f7"></a>

❇️ The dependency lock file belongs to the configuration as a whole, rather than to each separate module in the configuration

❇️ The lock file is named as **.terraform.lock.hcl and** available in the subdirectory of .terraform of your working directory.

❇️ It automatically gets created when we execute the _terraform init_ command to record the provider so that terraform can make the same selections by default when you run terraform init in the future.

Press enter or click to view image in full size

### **2. Terraform State File:** <a href="#a5e1" id="a5e1"></a>

❇️ Terraform state stores the infrastructure information and its configuration. The state file maintains a record of the resources created by your configuration and maps them to real-world resources.

❇️ The state file is stored in JSON format in the name of **terraform.tfstate** by default in our local workspace. Also, terraform maintains a backup file for terraform state which is named **terraform.tfstate.backup**

❇️ Whenever we execute _terraform apply_ a new terraform.tfstate file will be created and its backup will be written in terraform.tfstate.backup file.

❇️ The state file can be stored/maintained remotely in various methods like using the version control tool Git, Amazon S3 as remote store and also in terraform cloud, enterprise, and consul.

Press enter or click to view image in full size

### **Terraform State commands:** <a href="#e292" id="e292"></a>

These are the list of terraform state commands used frequently.

📢**terraform state list:** It lists the resources within the state file.

📢**terraform state mv :** Move items within terraform state. This will be used for resource renaming without destruction.

📢**terraform state pull:** It manually downloads and outputs the state from the state file.

📢**terraform state push:** Manually upload a local state file to the remote state

📢**terraform state rm:** It removes items from the state & no longer managed by Terraform. The resources removed from the state are not physically destroyed.

📢**terraform state show:** Show attributes of a single resource in the state.

### **What Is the Current State and Desire State in Terraform** <a href="#ddf4" id="ddf4"></a>

### **Current state** <a href="#cf9e" id="cf9e"></a>

📍Current state is the present state of the resources deployed, stored locally as terraform.tfstate file and not in remote infrastructure.

📍After creating the infrastructure, any manual changes made in the configuration will not reflect in the current state file.

📍In the next execution of the terraform apply command, it will make all the infrastructure changes manually and keep the infrastructure the same in its current state.

### **Desire state** <a href="#id-9886" id="id-9886"></a>

📍Desire state is the new state expected to apply from your terraform configuration.

📍It compares with the current state every time to update the infrastructure.

📍It will detect the changes between the desired state and the current state and try to ensure that the deployed infra is based on the desired state.

📍If there is a difference between the two, the _terraform plan_ will show the necessary changes required to achieve the desired state.

### **Types of Terraform state store:** <a href="#b805" id="b805"></a>

There are two types of stores for Terraform state: local and remote.

**Local state** refers to the Terraform state stored on the local filesystem, i.e. on your laptop or whatever system you are running the Terraform command from.

**Remote state** is Terraform state stored remotely, such as in an S3 bucket or a database like PostgreSQL.

### **Terraform Variables:** <a href="#id-3851" id="id-3851"></a>

Terraform variables store the values which can be reusable in terraform configuration. The functions of variables are as follows:

**Input Variables** as function arguments, so users can customize behavior without editing the source.

**Output Values** are like return values for a Terraform module.

**Local Values** are a convenience feature for assigning a short name to an expression.

**Input Variables**

Input variables are declared in the variable block. It has the following arguments declared in the variable block such as name, type, default, and description.

```
Variable <variable name> {
description = "updating instance type" - - - Give a meaningful description
type = string - - - - - - - - - - - - - ex: string, number, bool, list, map
default = "t2.micro" - - - - - - - - - - variable default value
```

**Let’s create a few simple variable files, the type of value of the variables like string, number, or bool.**

* **String** — A sequence of Unicode characters representing some text
* **Number** — A numeric value that includes either a whole number or a fractional value.
* **Bool** — It's either TRUE or FALSE. A Boolean value.

Next check on the **complex variable** types of variables like **Map** and **list** are collection type.

### Maps <a href="#id-0a69" id="id-0a69"></a>

Maps are associative arrays used for situations where you want to use one value to look up another value. We can take the example, AMI for creating EC2 instances should be taken as region-specific.

To change the region, we need to look up a new AMI using the input variable Map of strings.

### **List** <a href="#id-8bda" id="id-8bda"></a>

Each value can be called by its corresponding index in the list. Here is an example of a list variable definition. Also in the list, we need to denote the index value that you are looking for.

### **Assigning Values to Input Variables:** <a href="#eb0f" id="eb0f"></a>

There are multiple ways to assign values for input variables as listed below.

💡In command-line option

💡In variable definitions files

💡As an environment variable

**Command-line Option:**

To mention the specific individual variables on the command line, we can use the **–var** option while running the terraform plan and terraform apply commands. Ex: We will specify a region on the command line.

```
$ terraform plan -var 'aws_region=us-west-1'
```

Same as can do an authentication of the access key and secret key in the command line as given below:

```
$ terraform apply –var="AWS_ACCESS_KEY=<Access key>"
$ terraform apply –var="AWS_SECRET_KEY=<Secret key>"
```

### **CLI Variables:** <a href="#id-65cb" id="id-65cb"></a>

We can mention the same file in the command line option using **–var-file** in “terraform apply” as follows:

```
$ terraform apply -var="image_id=ami-xyz234"
```

**Variable Definition Files Option:**

### **Tfvars file:** <a href="#bc9e" id="bc9e"></a>

To set many variables by specifying their values in a single variable file will be convenient that we can use the variable definition file ending in **.tfvars.** The file created is named **terraform.tfvars** or **terraform.tfvars.json** as shown below.

```
$ terraform apply –var-file="sample.tfvars"
```

**auto tfvars files:**

Another file name is given as **.auto.tfvars** or **.auto.tfvars.json .** The file .auto we add will be loaded automatically without specifying the –var-file flag.

### **Environment Variables:** <a href="#fc9d" id="fc9d"></a>

Terraform by default searches environment variables named **TF\_VAR\_** followed by the name of the declared variable. The environment variable names are case-sensitive, they match the variable name exactly as given in the configuration.

```
Export TF_VAR_AWS_REGION=us-east-1
```

### **Output Values:** <a href="#bb90" id="bb90"></a>

Output values make information about your infrastructure available on the command line and can expose information for other Terraform configurations to use. Output values are similar to return values in a programming language.

### **Local Values:** <a href="#be5a" id="be5a"></a>

Terraform local values are like temporary local variables. It’s an expression name assigned by local values so that we can use it multiple times within the same modules. It helps to avoid repeating the same values in a module configuration.

Press enter or click to view image in full size

### **Meta Arguments in Terraform** <a href="#id-178a" id="id-178a"></a>

Meta arguments are used to control how resources are created, updated, and destroyed. It controls the behavior of resources.

🎯count

🎯count.index

🎯for\_each

🎯depends\_on

🎯provider

🎯lifecycle

**Count** argument with its value as a whole number, terraform creates that many numbers of resources on the execution of the configuration. The argument can be used with each type of resource block.

```
resource "aws_instance" "sample" {
Count = 4
Ami = "ami-ID"
Instance_type = "t2.micro"
}
```

**Count.Index** To modify each of the instances or any other resource, here we can use count.index ${count.index} in our configuration in all blocks where the count argument is declared. The index numbers always start from 0.

```
resource "aws_instance" "linux_server" {
count = 4 - - - - - - - creates four similar EC2 instances
ami = "ami-ID"
instance_type = "t2.micro"
tags = {
Name = "linux_server ${count.index}"
}
}
```

This will create four instances with the names “linux\_server\_0” , “linux\_server\_1”, up to 4.

**For\_Each** looping can be used when the value is either as a map or a set of strings. It allows you to create n number of resources of the same type but different properties. In the below example, the resource block takes the inputs from variable to for\_each argument to create EC2 instances from the values declared in vars.tf configuration.

```
resource "aws_instance" "webserver" {
for_each =var.webservers
ami = "ami-ID"
instance_type ="t2.micro"
tags = {
name = each.value["name"]
}
}
```

**Depends\_on is** used to specify that the resource depends on another resource. This can be useful when creating resources that require other resources to exist first, such as a load balancer that depends on an auto-scaling group.

**Lifecycle** it is used to manage a complete life cycle of the resources in the configuration file. It handles when to create, destroy, or change. Examples of these sub-arguments include _create\_before\_destroy, prevent\_destroy, ignore\_changes_.

```
Resource "aws_instance" "example" {
Lifecycle {
Create_before_destroy = true
}
}
```

Here _create\_before\_destroy_ meta-argument changes this behavior so that the new replacement object is created first, and the prior object is destroyed after the replacement is created.

### **Terraform Provisioners** <a href="#bf70" id="bf70"></a>

Provisioners are used to create specific actions on the local machine or on a remote machine in order to prepare servers or other infrastructure objects for service. There are two types of provisioners generic (listed below) and vendor (chef, puppet)

**· file**

**· local-exec**

**· remote-exec**

**file provisioner** helps to transfer files or directories from one source to the destination machine like we can transfer the file from your local machine to an ec2 instance.

```
provisioner "file" {
source = "specify the path from where we need to copy"
destination = "destination path where you want your file to be"
}
```

**Local-exec provisioner** used to perform a task on to our local machine where you have been installed terraform. The provisioner executes the command locally after the resource gets created.

**Remote-exec provisioner** executes the script on the resource created by the terraform script. It can be done by connecting with remote resources using ssh.

### **Terraform Modules** <a href="#id-8070" id="id-8070"></a>

The Terraform module allows you to create a logical abstraction on some resource sets. It allows you to group resources and reuse them for later use. We can customize our module by using already-built Terraform modules hosted in the Terraform Registry.

### **Root Module** <a href="#d8cf" id="d8cf"></a>

🔥Each terraform configuration has at least one module known as the root module, which consists of the resources defined in the .tf files in the main working directory.

### **Child Modules** <a href="#id-5e94" id="id-5e94"></a>

🔥A root module can call other modules to include their resources into the configuration is referred to as a child module.

🔥Child modules can be called multiple times within the same configuration, and multiple configurations can use the same child module.

### **Published Modules** <a href="#a458" id="a458"></a>

🔥Terraform Registry hosts a broad collection of publicly available Terraform modules for configuring many kinds of common infrastructure.

🔥Terraform can download them automatically if you specify the appropriate source and version in a module call block.

🔥Terraform can load modules from a public or private registry. These modules are free to use.

### **How to Use Modules** <a href="#c5c1" id="c5c1"></a>

🔥Module Blocks document the syntax for calling a child module from a parent module, including meta-arguments like for\_each.

🔥Module Sources document what kinds of paths, addresses, and URIs can be used in the source argument of a module block.

🔥The Meta-Arguments section documents special arguments that can be used with every module, including providers, depends\_on, count, and for\_each.

**Let’s do it with Terraform Modules which can create a re-usable module for hosting a static website.**

Have created a terraform module structure that contains a directory called modules with respective .tf files and inside the module, we will be creating another directory S3, within this file we’re going to define an aws\_S3\_bucket resource.

**Terraform**\
|\
**Modules**\
\| — -[main.tf](http://main.tf/)\
\| — -[variables.tf](http://variables.tf/)\
\| — -[outputs.tf](http://outputs.tf/)\
&#xNAN;**| — — — — S3**\
\| — — [main.tf](http://main.tf/)\
\| — — [variables.tf](http://variables.tf/)\
\| — — [output.tf](http://output.tf/)\
\| — — — — — — www\
\| — — -index.html\
\| — — -error.html

Apply the structure, and syntax for creating S3 bucket and attach the policy.

```
resource "aws_s3_bucket" "staticsite" {
 bucket = var.bucket_name
 acl = "public-read"
 policy = <<EOF # here paste the aws_iam_policy_document
 EOF 
 website {
 index_document = "index.html"
 error_document = "error.html"
 }
 tags = var.tags
 force_destroy = true
 }
```

Step up module within the root terraform.tf file. Within here we’ll specify a Provider, variable, and initiate our static site module.

```
module "website_s3_bucket" {
 source = "../modules/s3" # relative path
bucket_name = var.my_bucket  tags = var.my_tags
 }
```

Press enter or click to view image in full size

Press enter or click to view image in full size

Using the terraform module in this configuration we have hosted the static website.

_Execution Result — Static webpage created screenshot given below_

Press enter or click to view image in full size

### **Terraform workspace** <a href="#id-391d" id="id-391d"></a>

It allows you to create different and independent states on the same configuration. Terraform workspace reduces the usage of code. As it’s compatible with the remote backend this workspace is shared with your team.

Press enter or click to view image in full size

**How to use Workspace?**

_**$ terraform -help workspace**_

Usage: It lists the terraform workspace commands given below

💥**new** Create a new workspace

💥**list** Lists Workspaces

💥**show** It shows the name of the current workspace

💥**select** Select a workspace

💥**delete** To delete a workspace

### **How does it work?** <a href="#id-2580" id="id-2580"></a>

Multiple workspaces are supported by backends like _S3, AzureRM, Remote, Consul_, etc.,

This example will be deployed locally on 3 different stages, one state for each environment which are development, staging, and production.

If deployed to all 3-environment workspace will produce three different state files.

├── website.sh

├── instance.tf

├── provider.tf

├── terraform.tfstate.d

**│ └── dev**

│ └── terraform.tfstate

**│ └── Stag**

│ └── terraform.tfstate

**│ └── Prod**

│ └── terraform.tfstate

├── terraform.tfvars

└── vars.tf

### **Workspace Setup — S3 as Remote Backend:** <a href="#id-19d2" id="id-19d2"></a>

We can also backup the remote state of each workspace to S3, here the terraform script is given below.

Press enter or click to view image in full size

Press enter or click to view image in full size

**Created a new workspace for developing an environment.**

Press enter or click to view image in full size

Press enter or click to view image in full size

Press enter or click to view image in full size

Press enter or click to view image in full size

We require ideally similar infrastructure for development, production, staging, and QA. Terraform workspace is the perfect solution for this kind of problem.

Finally, workspace cleanup was performed.

Press enter or click to view image in full size

In conclusion, we have delved into the fundamentals of Terraform configuration, equipping you with the necessary knowledge to get started on your infrastructure-as-code journey.

Remember, continuous learning and staying up to date with the latest Terraform releases and best practices will enhance your proficiency and lead to even more successful infrastructure deployments.

Happy Terraforming🙂!!
