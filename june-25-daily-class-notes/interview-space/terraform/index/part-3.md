# Part 3

**Q1: Suppose you created an ec2 instance with terraform and after creation, you have removed the entry from the state file now, when you run terraform apply what will happen?**

* As we have removed the entry from that state file so terraform will no longer manage that resource so on the next apply it will create a new resource.

**Q2: What is a state file in Terraform?**

* A state file is a file in which Terraform keeps track of all the infrastructure that is deployed by it.

**Q3: What is the best way to store the terraform state file?**

* The best way to store the state file is to keep it in the remote backend like S3 or GitLab-managed terraform state so, that whenever multiple people are working on the same code resource duplication won’t happen.

**Q4: What is terraform state locking?**

* Whenever we are working on any terraform code and do terraform plan, apply or destroy terraform will lock the state file in order to prevent the destructive action.

**Q5: What is Terraform backend?**

* A backend defines where Terraform stores its state data files. Terraform uses persisted state data to keep track of the resources it manages.

**Q6: What is a plugin in Terraform?**

* The plugin is responsible for converting the HCL code into API calls and sends the request to the appropriate provider (AWS, GCP)

**Q7: What is a null resource?**

* As in the name you see a prefix null which means this resource will not exist on your Cloud Infrastructure
* Terraform null\_resource can be used in the following scenario -
* Run shell command
* You can use it along with local provisioner and remote provisioner
* It can also be used with Terraform Module, Terraform count, Terraform Data source, Local variables
* It can even be used for output block

**Q8: What are the types of provisioners?**

* **Remote exec:** Run commands using Terraform on a remote server
* **Local exec:** Run commands using Terraform on the local system

**Q9: What is the use of Terraform module?**

* We can create the terraform modules one time and reuse them whenever needed
* To make to code standardized
* To reduce the code duplication
* The module can be versioned

**Q10: If I have created EC2 and VPC using Terraform and unfortunately tfstate file got deleted, can you recover it? (File is only on the local machine not on s3 or dynamo DB)**

* You can import the resources that are created by Terraform using terraform import command and then it will come to the state file

**Q11: If we have created different-different modules like VPC, EC2, security group, access key, and subnet so how terraform will get an idea of which resource should deploy first?**

* Terraform automatically figures out the dependency graph based on the resource references in your code. It understands the relationships between resources, and it uses this information to determine the order in which the resources should be created or modified.
* You can define the explicit dependency with the depends\_on keyword

**Q12: How I can delete/destroy specific resources without changing logic?**

* Using taint and destroy command
* We need to taint that resource using terraform taint RESOURCE\_TYPE.RESOURCE\_NAME command
* After tainting the resource, you can run the “destroy” command to remove the tainted resources using terraform destroy -target=RESOURCE\_TYPE.RESOURCE\_NAME command

**Q13: How can we rename a resource in Terraform without deleting it?**

* We can rename a resource without deleting it using terraform mv command

**Q14: Let’s say you have created an EC2 instance using Terraform and someone does the manual change on it next time you run Terraform plan what will happen?**

* Terraform state will be mismatched and terraform will modify the EC2 instance to the desired state i.e. whatever we have defined in the .tf file

**Q15: What is the difference between locals & variables in terraform?**

* The variables are defined in the variables.tf file or using variables keyword that can be overridden but the locals can not be overridden.
* So if you want to restrict the overriding the variables at that time you need to use the locals.
