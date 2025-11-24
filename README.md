# Terraformprojects
These are all notes for tutorials, workshops, projects I have completed using Terraform

# Importing existing infrastructue resources into terraform
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
