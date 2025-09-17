# Environments with Tf Workspaces

Terraform workspaces provide an efficient way to manage multiple environments (e.g., development, staging, production) using the same Terraform configuration. This article guides you through the basics of Terraform workspaces and demonstrates their usage with practical commands.

Press enter or click to view image in full size

### What are Terraform Workspaces? <a href="#d19b" id="d19b"></a>

Terraform workspaces are an organizational feature that allows you to maintain separate state files for different environments within a single configuration. This is useful for managing different stages of your infrastructure lifecycle, such as development, staging, and production, without needing to maintain separate directories or configurations for each environment.

### Setting Up and Managing Workspaces <a href="#id-31df" id="id-31df"></a>

Below are step-by-step commands to list, create, and switch between workspaces, as well as applying and destroying configurations.

#### List Existing Workspaces <a href="#f9f1" id="f9f1"></a>

To see the list of existing workspaces in your current Terraform configuration, use the following command:

```
terraform workspace list
```

This command displays all the workspaces, with an asterisk (\*) indicating the currently active workspace.

#### Create a New Workspace <a href="#e286" id="e286"></a>

To create a new workspace, use the following command:

```
terraform workspace new dev
```

Press enter or click to view image in full size

In this example, a new workspace named `dev` is created. This workspace will have its own state file, separate from other workspaces.

#### Verify the New Workspace <a href="#id-58c2" id="id-58c2"></a>

After creating a new workspace, you can list the workspaces again to verify its creation:

```
terraform workspace list
```

#### Using `terraform.workspace` <a href="#id-01cb" id="id-01cb"></a>

In Terraform, `terraform.workspace` is used to get the current workspace's name. This can be particularly useful for naming resources or for outputs to dynamically reflect the active workspace. For example:

```
output "current_workspace" {
  value = terraform.workspace
}
```

`terraform.workspace` is used within the `tags` block to dynamically name the instance based on the current workspace.

### Example Workflow <a href="#id-8827" id="id-8827"></a>

Here’s an example workflow that demonstrates creating, selecting, and managing workspaces:

```
provider "aws" {
  region = "ap-south-1"
}resource "aws_instance" "myinstance" {
  ami           = "ami-00fa32593b478ad6e"
  instance_type = "t2.micro"
  tags = {
    Name = "${terraform.workspace}-instance"
  }
}output "outputtest" {
  value = aws_instance.myinstance.public_ip
}
```

Instance Name varies based on the Terraform workspace, using `{terraform.workspace}` for dynamic naming.

**Create a new workspaces `dev` and `prod`:**

```
terraform workspace new dev
terraform workspace new prod
```

**Run the terraform script for `prod` workspace:**

```
terraform apply -auto-approve
```

The instance created will be tagged as `prod-instance`.

**Switch to `dev` workspace:**

```
terraform workspace select dev
```

**Run the terraform script for `dev` workspace:**

```
terraform apply -auto-approve
```

The instance created will be tagged as `dev-instance`.

Using `terraform.workspace` makes your Terraform configurations more flexible and environment-aware, allowing for smoother and more organized infrastructure management across multiple environments.

Press enter or click to view image in full size

### Conclusion <a href="#d155" id="d155"></a>

Terraform workspaces provide a powerful way to manage multiple environments within a single Terraform configuration. By using workspaces, you can isolate state files and ensure that changes in one environment do not affect another.

This article covered essential commands for listing, creating, switching, applying configurations, and destroying resources within Terraform workspaces. Leveraging these capabilities can greatly enhance your infrastructure management process, providing a clean and efficient way to handle different environments.

Happy managing!
