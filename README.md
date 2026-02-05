# Terraformprojects
Tutorials and projects using Terraform.

## Project_1: Creating an EC2 instance with Terraform (TF)

Tutorial: https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-create

### Description
I am learning how to use TF to create infrastructure. I understand it is a valuable IaC tool to be able to use, so I am working on developing this skill. 

### CLI tools:    
`aws configure`  
`terraform init`  
`terraform validate`  
`terraform apply`  
`terraform state list`  
`terraform show`  

### Configurations: 
1. Resource block, data block
2. main.tf, variable.tf, outputs.tf

### Additional notes
- Verifying credentials `aws configure list`

- Data blocks are used to query your cloud provider for info about other resources. In this data block, it's fetching the latest AWS AMI matching the filter so hardcoding the AMI ID in the configuration isn't necessary.

- Data sources `data` have an ID used to reference data attributes within your configuration. Format `"block type" "name`

- When I configure the IAM credentials to authenticate the TF AWS provider, I have to set the `AWS_ACCESS_KEY` `AWS_SECRET_ACCESS_KEY` and `AWS_SESSION_TOKEN` environmental variables 

- Resource block `resource` = defines components of the infrastructure. 
>> First line = `"resource type" "resource name"`
>> Resource address `"resource_type.resource_name

- To initialize your workspace `terraform init`

- To validate configuration `terraform validate`

- To create the infrastructure `terraform apply`, review, and then when prompted enter `yes`.

- After TF apply, there is file created called terraform.tfstate. To check and subsequently manage the resources and data sources stored about your infrastructure `terraform state list` To do list, use s3 for a remote state backend..

- To print the entire state `terraform show`

#### Managing infrastructure

- Created a `variable.tf` file for my variables. 
- Variables allow me to update EC2 instance's name & type without modifying my configuration files each time 
- I can run `terraform plan -var instance_type=t2.large` to update my EC2 instance from the CLI
- Updated my main.tf file with my input variables. 
- Created a outputs.tf file.
- Outputs allow access to attributes from TF configuration and consume their values with other automation tools/workflows.

#### Modules
- Modules are reusable sets of configuration used to manage complex infrastructure deployments 
- Added a module block in main.tf to create a VPC and networking resources for the EC2 instance
- Whenever adding new modules, you have to reinitialize the workspace by running `terraform init`. 
- TF had to destroy the EC2 instance and create another EC2 to put into the new VPC 

### Troubleshooting

I made a typo with the code and forget to run this command and received an error message. I'll have to remember this. 

I updated the AZs to us-east-1a, us-east-1b, us-east-1c to be consistent with my Region. I received an error message and had to edit the code. Originally, it was us-west-2a, us-west-2b, us-west-2c. 

- Ran `terraform state list` to confirm resources (screenshot). 

- Module addresses must be unique within the configuration

- Resources destroyed `terraform destroy`


## Project_2: Importing existing infrastructue resources into TF

Tutorial: (https://spacelift.io/blog/importing-exisiting-infrastructure-into-terraform)  

### Description 
I'm now working on importing resources to TF. 

### CLI tools    
`terraform import`  

### Additional notes
`terraform import` (the following are the arguments the tool uses)

ADDR – the address of the resource in terraform (e.g. aws_instance.instance_name)

ID – the resource ID in the cloud provider/k8s/database service/vcs service/etc [my_role]

Example: `terraform import aws_instance.instance_name my_role`

### Troubleshooting
When I ran `terraform init` initially, I got an error message. I had to run `terraform init -upgrade` to allow selection of new versions. Remember to put keys in the path before running the command. 



