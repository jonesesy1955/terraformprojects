# Terraformprojects
These are all notes for tutorials, workshops, projects I have completed using Terraform

# TF Project_1: Creating an EC2 instance with Terraform (TF)

Using the tutorial: https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-create

## Why
I am learning how to use TF to create infrastructure. I understand it is a valuable IaC tool to be able to use, so I am working on developing this skill. 

## Things to remember 

- Verifying credentials `aws configure list`

- Data blocks are used to query your cloud provider for info about other resources. In this data block, it's fetching the latest AWS AMI matching the filter so hardcoding the AMI ID in the configuration isn't necessary.

- Data sources `data` have an ID used to reference data attributes within your configuration. Format `"block type" "name`

- When I configure the IAM credentials to authenticate the TF AWS provider, I have to set the `AWS_ACCESS_KEY` `AWS_SECRET_ACCESS_KEY` and `AWS_SESSION_TOKEN` environmental variables 

- resource block `resource` = defines components of the infrastructure. 
>> First line = `"resource type" "resource name"`
>> Resource address `"resource_type.resource_name

- To initialize your workspace `terraform init`

- To validate configuration `terraform validate`

- To create the infrastructure `terraform apply`, review, and then when prompted enter `yes`.

- After TF apply, there is file created called terraform.tfstate. To check and subsequently manage the resources and data sources stored about your infrastructure `terraform state list` To do list, use s3 for a remote state backend..

- To print the entire state `terraform show`

# Importing existing infrastructue resources into Terraform
Terraform CLI tools
  terraform import (the following are the arguments the tool uses)
    ADDR – the address of the resource in terraform (e.g. aws_instance.instance_name)
    ID – the resource ID in the cloud provider/k8s/database service/vcs service/etc

# Writing configuration 
I'm doing this on a Windows OS instead of macOS. So, since I'm installing terraform for a second time it's smoother due to prior troubleshooting. 
For ease, and probably the only way I could have executed on Windows, I installed windows subsystem for linux (WSL) and install Kali linus distro. I did this to follow the terraform tutorial because the commands were all in bash. 
I installed: 
  1) chocolately to install WSL
  2) putty - in case I need to SSH for any reason (but I still need to figure out how to set it up correctly) 
  3) installed Kali linux distro
  4) homebrew
  5) AWS cli
  6) terraform

Now, I'm following the Terraform tutorial to create infrastructure 
  remember that you need the keys and token in your path 

Successful. I was able to install, create and destroy infrastructure (3 of 5 for the tutorial)

# Importing EC2 (https://spacelift.io/blog/importing-exisiting-infrastructure-into-terraform)
I'm now working on importing resources to terraform. 
When I ran `terraform init` initially, I got an error message. I had to run `terraform init -upgrade` to allow selection of new versions
Remember to put keys in the path before running the command 
Questions to ask: how do I create multiple main.tf files? Or do I just have them in different folders?
Successful. I was able to import my ec2 instance to be managed by terraform 

# TF Project_1b Managing the Environment 

Now, on to the section on managing 

- Created a `variable.tf` file for my variables. 
- Variables allow me to update EC2 instance's name & type without modifying my configuration files each time 
- I can run `terraform plan -var instance_type=t2.large` to update my EC2 instance from the CLI


variable "instance_name" {
  description = "Value of the EC2 instance's Name tag."
  type        = string
  default     = "learn-terraform"
}

variable "instance_type" {
  description = "The EC2 instance's type."
  type        = string
  default     = "t2.micro"
}


- Updated my main.tf file with my input variables. 

- Created a outputs.tf file.
- Outputs allow access to attributes from TF configuration and consume their values with other automation tools/workflows.

output "instance_hostname" {
  description = "Private DNS name of the EC2 instance."
  value       = aws_instance.app_server.private_dns
}

Now, the section on Modules
- Modules are reusable sets of configuration used to manage complex infrastructure deployments 
- Added a module block in main.tf to create a VPC and networking resources for the EC2 instance
- Whenever adding new modules, you have to reinitialize the workspace by running `terraform init`. 
- TF had to destroy the EC2 instance and create another EC2 to put into the new VPC 



module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.19.0"

  name = "example-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["us-east-1a", "us-east-1b", "us-east-1c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.101.0/24"]

  enable_dns_hostnames    = true
}

- Updated the resource block to move the EC2 instance into the new VPC 
  vpc_security_group_ids = [module.vpc.default_security_group_id]
  subnet_id              = module.vpc.private.subnets[0]

- Troubleshooting

I made a typo with the code and forget to run this command and received an error message. I'll have to remember this. 

I had updated the AZs to us-east-1a, us-east-1b, us-east-1c to be consistent with my Region. I received error message and had to edit the code. Originally, it was us-west-2a, us-west-2b, us-west-2c. 

- Ran `terraform state list` to confirm resources (screenshot). Success!

- Module addresses must be unique within the configuration

- Resources destroyed `terraform destroy`
