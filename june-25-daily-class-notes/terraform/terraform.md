# 🔥🚀Terraform

Terraform is an infrastructure as code (Iac) tool that lets you build, change, and version cloud and on-prem resources safely and efficiently. So, what is Infrastructure as code mean. It means, instead of manually creating resources, we declared resource needed as code. And this code will handle resource creation/update and deletion.

Press enter or click to view image in full size

**Benefits?**

•Open source, platform agnostic (multiple cloud platforms)

•Terraform is Declarative

•Terraform is Agentless

•Collaboration

•The human-readable configuration language

**How does Terraform work?**

Terraform creates and manages resources on cloud platforms and other services through their application programming interfaces (APIs). Providers enable Terraform to work with virtually any platform or service with an accessible API. HashiCorp and the Terraform community have already written **thousands of providers** to manage many different types of resources and services.

Press enter or click to view image in full size

**Provider**

Providers are the plugins that Terraform uses to manage those resources. Every supported service or infrastructure platform has a provider that defines which resources are available and performs API calls to manage those resources. So, provider understand API interactions and expose resources.

Press enter or click to view image in full size

•Registry …… directory of providers [https://registry.terraform.io](https://registry.terraform.io/)

•Provider …… understand API interactions and expose resources

•terraform init …… download provider

**The core Terraform workflow**

Press enter or click to view image in full size

Press enter or click to view image in full size

**Authentication-AWS**

Press enter or click to view image in full size

**Write configuration**

•HashiCorp Configuration Language (HCL)

•Declarative (desired state = actual state)

•easy to read and write

•a JSON-like syntax with .tf extension

•e.g main.tf

•Configuration file contains:

Press enter or click to view image in full size

**Resource block {}**

Press enter or click to view image in full size

Press enter or click to view image in full size

Press enter or click to view image in full size

**Authentication-Azure**

Press enter or click to view image in full size

**Variables**

A variable in Terraform is similar to a variable in programming language. A variable is a container that holds a value. It can be used to represent different values at different times during the execution of a program. Variables in [Terraform](https://upcloud.com/blog/upcloud-verified-terraform-provider/) are a great way to define centrally controlled reusable values. Terraform supports 3 different types of variables input variables, output variables and local variables.

* Input variables = like function argument
* Output variables = like function return value
* Local variables = like a function’s temporary local variable

Variable is declared using _variable_ key word, used using _var.variableName_

Type of value that will be accepted as value of variable are string, number, bool, list, set, map, object, tuple.

ASSIGNING VALUE TO VARIABLES

_Command line argument_

•terraform plan –var rgname=“demo123“ –var rglocation=“East US”

_Using file (.ftvars)_

terraform apply -var-file prod.tfvars

_File auto load by terraform_

terraform.tfvars or terraform.tfvars.json

.auto.tfvars or .auto.tfvars.json

_Environment Variables_

TF\_VAR\_variableName

Press enter or click to view image in full size

**Local Variable**

A local value assigns a name to an [expression](https://developer.hashicorp.com/terraform/language/expressions), so you can reference local multiple times, reducing duplication in your code. Unlike input variable, locals are not set directly by users of your configuration. Local variable is declared using key word locals. Once a local value is declared, u can reference it as local.name of local.

Press enter or click to view image in full size

**Output Variable**

Outputs variables can be used to print certain values in the CLI output after running terraform apply. A child module can use output variable to expose a subset of its resource attributes to a root module.

Press enter or click to view image in full size

**Data Source**

Data sources let you dynamically fetch data that is present outside terraform context. Example is AWS account number, AMI ID, list of availability zones, secrets. You can use data sources to make your configuration file more dynamic. You can fetch aws availability zone data from data sources instead of hardcoding on config file.

It’s a two-step process: 1) declare data source 2) access data source

Each infrastructure in terraform has its resources block and data sources block. Just like resources, data sources also have argument reference and attribute reference.

Press enter or click to view image in full size

Press enter or click to view image in full size

Press enter or click to view image in full size

**Meta-arguments of resource {}**

[Meta-arguments](https://developer.hashicorp.com/terraform/language/meta-arguments/depends_on) in Terraform are special arguments that can be used with resource blocks to control their behavior or influence the infrastructure provisioning process. They provide additional configuration options beyond the regular resource-specific arguments.

•depends\_on

•count

•for\_each

•provider

•lifecycle

**State**

Terraform state is a vital mechanism that records metadata about your infrastructure. This information includes not just the configurations you’ve written but also identifiers, dependencies, and other relevant details of the resources that Terraform manages. Essentially, the state is a snapshot of your infrastructure at a given point in time, helping Terraform predict and execute changes efficiently. So, mapping of your desired state i.e config file to real world resources is done through state file. It is the single source of truth for terraform. Terraform uses state to determine which changes to make to your infrastructure. Prior to any operation, Terraform does a [refresh](https://developer.hashicorp.com/terraform/cli/commands/refresh) to update the state with the real infrastructure.

This state file is stored by default in a local file named “terraform.tfstate“. It stores the state in a local JSON file on disk. Terraform recommend storing it in cloud to version, encrypt and securely share it with team. Backends block on the configuration determine where terraform stores its state file. Backends are responsible for supporting [state locking](https://developer.hashicorp.com/terraform/language/state/locking) if possible. Not all backends support locking. What actually is state locking. With state locking, Terraform will lock your state for all operations that could write state. This prevents others from acquiring the lock and potentially corrupting your state. [state locking](https://developer.hashicorp.com/terraform/language/state/locking) prevent concurrent runs of Terraform against the same state. Some of the example of backend that support state locking are: aws s3, postgres database, google cloud storage, azurerm. Please follow documentation for the details.

Press enter or click to view image in full size

Terraform state can contain sensitive data. The state contains resource IDs and all resource attributes, database passwords, private keys etc. When using local state, state is stored in plain-text JSON files. Storing state remotely can provide better security. Terraform recommend storing state remotely with encrypting state data at rest. For example, The S3 backend supports encryption at rest when the encrypt option is enabled. IAM policies and logging can be used to identify any invalid access. Another option is using terraform cloud which always encrypt state at rest.

**Workspaces**

Terraform workspaces are associated with a specific working directory and isolate multiple state files in the same working directory, letting you manage multiple groups of resources with a single configuration file. Terraform starts with a single workspace named default that you cannot delete. When you run terraform plan in a new workspace, Terraform does not access existing resources in other workspaces. These resources still physically exist, but you must switch workspaces to manage them. A common use for multiple workspaces is to create a parallel, distinct copy of infrastructure to test a set of changes before modifying production infrastructure.

Press enter or click to view image in full size

**workspaces cli commands**

$ terraform workspace \[list, show, new, delete, select]

Press enter or click to view image in full size

**Terraform Module**

Modules in terraform is the same concept that you have learned in any programming language. Modules are the key ingredient to write reusable, maintainable, and testable Terraform code. Modules are containers for multiple resources that are used together. A module consists of a collection of .tf files kept together in a directory. A terraform module (usually root module) can call other modules to include their resources into configuration. A module that has been called by another module is called child module. Child module can be defined within root module using module block, which specifies the source of module and any input variables need to be set. Root module is top level module of terraform config. It serves as entry point of your entire terraform config and typically includes provider config and any variables that are needed to define your infrastructure resources. So, root module is responsible for initializing terraform, configure provider and declaring resources.

Press enter or click to view image in full size

When you go to terraform registry, you will see publicly available modules. Almost 16 thousand modules. You can use that module and create your resources, or you can create your own module from scratch and create resources using your own module.

Press enter or click to view image in full size

Directly from documentation

Press enter or click to view image in full size

Press enter or click to view image in full size

Press enter or click to view image in full size

**Terraform import**

Terraform import command is used to bring your manually created existing infrastructure under Terraform management. Importing existing resources into Terraform ensures that your infrastructure’s state is centralized and managed through a single source of truth, enhancing visibility and control. Once imported, your resources seamlessly integrate with the broader Terraform ecosystem, allowing you to leverage modules, variables, and other advanced features. Terraform import only available in Terraform v1.5.0 and later.

Press enter or click to view image in full size

OLD WAY: The old way to import resources under terraform management is using this command. Terrafrom import resource type dot resource name followed by resource id. This only update state file. You need to manually update config file.

Press enter or click to view image in full size

NEW WAY: The new way is to use import block. This block accept two argument to and id. We need to add resource block manuall&#x79;**. After you do terraform init, plan, apply, you comment out import block.**

Press enter or click to view image in full size

This another new way which will auto generate config file. You declare import block without resource block. Then you run terraform plan with generate config flag. This will auto generate config file. Review the generated file and run terraform plan and apply. After apply command, comment out import block import.

Press enter or click to view image in full size

Please follow me on YouTube for the detailed explanation as well as for hands on demo !!!

HAPPY LEARNING HAPPY CODING
