# terraformprojects
These are all notes for tutorials, workshops, projects I have completed using Terraform

# Importing existing infrastructue resources into terraform
terraform CLI tools
  terraform import (the following are the arguments the tool uses)
    ADDR – the address of the resource in terraform (e.g. aws_instance.instance_name)
    ID – the resource ID in the cloud provider/k8s/database service/vcs service/etc
