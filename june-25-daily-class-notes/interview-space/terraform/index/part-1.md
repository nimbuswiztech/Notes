# Part 1

### **Basic Level (15 Questions)**

1. **What is Terraform, and how does it differ from tools like Ansible or CloudFormation?**
   * **Answer:** Terraform is an Infrastructure as Code (IaC) tool that allows declarative provisioning of infrastructure. Unlike Ansible, which focuses on configuration management, Terraform primarily provisions infrastructure. CloudFormation is AWS-specific, while Terraform supports multiple cloud providers.
2. **Explain the purpose of the `terraform init` command.**
   * **Answer:** Initializes a Terraform working directory by downloading the required provider plugins, setting up the backend, and preparing the workspace.
3. **What happens when you run `terraform apply`?**
   * **Answer:** Terraform applies the configuration changes defined in `.tf` files by creating, modifying, or destroying resources to match the desired state.
4. **How do you store and manage Terraform state files securely?**
   * **Answer:** Use remote state backends like AWS S3 with state locking enabled using DynamoDB to avoid conflicts in team environments.
5. **What is the difference between `terraform plan` and `terraform apply`?**
   * **Answer:** `terraform plan` shows what changes will be applied without making any modifications. `terraform apply` actually executes the changes.
6. **What is a Terraform provider?**
   * **Answer:** A provider is a plugin that allows Terraform to interact with APIs of cloud platforms like AWS, Azure, and GCP.
7. **What are Terraform modules, and why use them?**
   * **Answer:** A module is a reusable set of Terraform configurations. It promotes code reusability, modularization, and maintainability.
8. **What is Terraform state, and why is it required?**
   * **Answer:** Terraform state tracks the current state of managed infrastructure. It helps Terraform determine what changes need to be made.
9. **What is the purpose of the `.terraform.lock.hcl` file?**
   * **Answer:** It ensures consistency by locking specific versions of providers, preventing accidental upgrades.
10. **What is the difference between `terraform taint` and `terraform destroy`?**

* **Answer:** `terraform taint` marks a resource for recreation in the next apply. `terraform destroy` deletes all resources.

11. **What are input variables in Terraform, and how do you define them?**

* **Answer:** Input variables allow parameterization of Terraform configurations. Defined using the `variable` block.

12. **What is an output variable in Terraform?**

* **Answer:** Output variables display useful information after Terraform applies changes.

13. **How can you enforce Terraform to use a specific version of a provider?**

* **Answer:** Use the `required_providers` block in the `terraform` block.

14. **How do you handle secrets in Terraform?**

* **Answer:** Use HashiCorp Vault, AWS Secrets Manager, or environment variables to store sensitive data instead of hardcoding them in `.tf` files.

15. **How do you upgrade Terraform to the latest version?**

* **Answer:** Use `terraform version` to check the current version and `terraform init -upgrade` to upgrade providers.

***

### **Intermediate Level (15 Questions)**

16. **Your team is using Terraform in a collaborative environment. How do you prevent multiple engineers from applying changes simultaneously?**

* **Answer:** Use remote state locking with AWS S3 + DynamoDB or Terraform Cloud.

17. **You mistakenly applied a configuration that removed a resource. How do you recover it?**

* **Answer:** Retrieve the last state backup file (`terraform.tfstate.backup`) and manually restore the resource or import it using `terraform import`.

18. **How do you import an existing AWS EC2 instance into Terraform state?**

* **Answer:** Use `terraform import aws_instance.example i-1234567890abcdef0` to import an EC2 instance.

19. **What is the difference between `terraform workspace` and separate backend configurations?**

* **Answer:** `terraform workspace` allows multiple environments (e.g., dev, prod) within the same backend. Separate backends use different state files.

20. **How do you manage different environments (dev, staging, production) in Terraform?**

* **Answer:** Use workspaces, variable files (`terraform.tfvars`), or separate state backends.

21. **How do you roll back changes in Terraform?**

* **Answer:** Use version control (`git revert`) or restore a previous state file from backup.

22. **What are `lifecycle` rules in Terraform?**

* **Answer:** Used to control the behavior of resources (`prevent_destroy`, `ignore_changes`, `create_before_destroy`).

23. **How do you manage dependencies between resources in Terraform?**

* **Answer:** Terraform automatically detects dependencies but can be explicitly managed using the `depends_on` argument.

24. **Explain `count` vs. `for_each` in Terraform.**

* **Answer:**
  * `count` is used for creating a list of resources dynamically based on an integer.
  * `for_each` is used for creating resources from a map or set.

25. **What is the difference between local and remote execution modes in Terraform?**

* **Answer:** Local execution runs Terraform on a developer's machine, while remote execution (Terraform Cloud/Enterprise) runs in a managed environment.

26. **How do you handle failed Terraform apply operations?**

* **Answer:** Use `terraform state list` and `terraform state rm` to remove failed resources before re-applying.

27. **What is the use of `terraform graph`?**

* **Answer:** Generates a visual representation of dependencies between Terraform resources.

28. **How do you integrate Terraform with CI/CD pipelines?**

* **Answer:** Use tools like GitHub Actions, Jenkins, or GitLab CI to automate `terraform plan` and `terraform apply`.

29. **How do you handle breaking changes in Terraform modules?**

* **Answer:** Use semantic versioning and test changes in a separate environment before applying.

30. **Can Terraform work without internet access?**

* **Answer:** Yes, by using pre-downloaded provider binaries and modules.

***

### **Advanced Level (15 Questions)**

31. **How do you use Terraform with Kubernetes (EKS/GKE/AKS)?**

* **Answer:** Use Terraform providers (`kubernetes`, `helm`) to manage cluster resources.

32. **How can you manage multiple Terraform projects efficiently?**

* **Answer:** Use Terragrunt for managing multiple repositories and environments.

33. **You need to deploy infrastructure in multiple regions. How do you handle it?**

* **Answer:** Use multiple provider configurations with aliasing.

34. **What happens if Terraform state is accidentally deleted?**

* **Answer:** If using remote state (e.g., S3), restore from versioned backups.

35. **How do you execute only specific resources in Terraform?**

* **Answer:** Use `terraform apply -target=<resource_name>`.

36. **Explain the difference between `terraform refresh` and `terraform apply`.**

* **Answer:**
  * `terraform refresh` updates the state file without making changes.
  * `terraform apply` modifies the infrastructure.

37. **How do you refactor Terraform code while preserving the state?**

* **Answer:** Use `terraform mv` to rename resources in state.

38. **How do you enforce security policies in Terraform?**

* **Answer:** Use Sentinel policies, Open Policy Agent (OPA), or Terraform Cloud.

39. **How do you prevent accidental deletion of critical resources?**

* **Answer:** Use `prevent_destroy` in the `lifecycle` block.

40. **What are dynamic blocks in Terraform, and when do you use them?**

* **Answer:** Used for generating nested configuration dynamically within resources.

41. **How do you debug Terraform issues?**

* **Answer:** Use `TF_LOG=DEBUG terraform apply`.

42. **How do you ensure Terraform module versioning?**

* **Answer:** Use `source = "registry/module"` with `version` constraints.

43. **What is Terraform Cloud, and how does it differ from Terraform CLI?**

* **Answer:** Terraform Cloud provides remote state management, runs, and policies.

44. **How do you handle secrets in Terraform state?**

* **Answer:** Use `sensitive = true` and store secrets in Vault.

45. **How can you integrate Terraform with monitoring tools?**

* **Answer:** Use Terraform to provision monitoring resources like CloudWatch, Datadog, or Prometheus.
