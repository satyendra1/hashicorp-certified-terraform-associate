***********read the file ./01-Infrastructure-as-Code-IaC-Basics/README.md ************
# Infrastructure as Code Basics

## Step-01: Understand Problems with Traditional way of Managing Infrastructure
- Time it takes for building multiple environments
- Issues we face with different environments
- Scale-Up and Scale-Down On-Demand

## Step-02: Discuss how IaC with Terraform Solves them
- Visibility
- Stability
- Scalability
- Security
- Audit======================================================================
***********read the file ./02-Terraform-Basics/02-01-Install-Tools-TerraformCLI-AWSCLI-VSCodeIDE/README.md ************
# Terraform & AWS CLI Installation

## Step-01: Introduction
- Install Terraform CLI
- Install AWS CLI
- Install VS Code Editor
- Install HashiCorp Terraform plugin for VS Code


## Step-02: MACOS: Terraform Install
- [Download Terraform MAC](https://www.terraform.io/downloads.html)
- [Install CLI](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- Unzip the package
```
# Copy binary zip file to a folder
mkdir /Users/<YOUR-USER>/Documents/terraform-install
COPY Package to "terraform-install" folder

# Unzip
unzip <PACKAGE-NAME>
unzip terraform_0.14.3_darwin_amd64.zip

# Copy terraform binary to /usr/local/bin
echo $PATH
mv terraform /usr/local/bin

# Verify Version
terraform version

# To Uninstall Terraform (NOT REQUIRED)
rm -rf /usr/local/bin/terraform
``` 

## Step-03: MACOS: IDE for Terraform - VS Code Editor
- [Microsoft Visual Studio Code Editor](https://code.visualstudio.com/download)
- [Hashicorp Terraform Plugin for VS Code](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)


### Step-04: MACOS: Install AWS CLI
- [AWS CLI Install](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
- [Install AWS CLI - MAC](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-mac.html#cliv2-mac-install-cmd)

```
# Install AWS CLI V2
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
which aws
aws --version

# Uninstall AWS CLI V2 (NOT REQUIRED)
which aws
ls -l /usr/local/bin/aws
sudo rm /usr/local/bin/aws
sudo rm /usr/local/bin/aws_completer
sudo rm -rf /usr/local/aws-cli
```


## Step-05: MACOS: Configure AWS Credentials 
- **Pre-requisite:** Should have AWS Account.
  - [Create an AWS Account](https://portal.aws.amazon.com/billing/signup?nc2=h_ct&src=header_signup&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start)
- Generate Security Credentials using AWS Management Console
  - Go to Services -> IAM -> Users -> "Your-Admin-User" -> Security Credentials -> Create Access Key
- Configure AWS credentials using SSH Terminal on your local desktop
```
# Configure AWS Credentials in command line
$ aws configure
AWS Access Key ID [None]: AKIASUF7DEFKSIAWMZ7K
AWS Secret Access Key [None]: WL9G9Tl8lGm7w9t7B3NEDny1+w3N/K5F3HWtdFH/
Default region name [None]: us-east-1
Default output format [None]: json

# Verify if we are able list S3 buckets
aws s3 ls
```
- Verify the AWS Credentials Profile
```
cat $HOME/.aws/credentials 
```

## Step-06: WindowsOS: Terraform & AWS CLI Install
- [Download Terraform](https://www.terraform.io/downloads.html)
- [Install CLI](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- Unzip the package
- Create new folder `terraform-bins`
- Copy the `terraform.exe` to a `terraform-bins`
- Set PATH in windows 
- Install [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)

## Step-07: LinuxOS: Terraform & AWS CLI Install
- [Download Terraform](https://www.terraform.io/downloads.html)
- [Linux OS - Terraform Install](https://learn.hashicorp.com/tutorials/terraform/install-cli)======================================================================
***********read the file ./02-Terraform-Basics/02-02-Terraform-Command-Basics/README.md ************
# Terraform Command Basics

## Step-01: Introduction
- Understand basic Terraform Commands
  - terraform init
  - terraform validate
  - terraform plan
  - terraform apply
  - terraform destroy      

## Step-02: Review terraform manifest for EC2 Instance
- **Pre-Conditions-1:** Ensure you have **default-vpc** in that respective region
- **Pre-Conditions-2:** Ensure AMI you are provisioning exists in that region if not update AMI ID 
- **Pre-Conditions-3:** Verify your AWS Credentials in **$HOME/.aws/credentials**
```t
# Terraform Settings Block
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      #version = "~> 3.21" # Optional but recommended in production
    }
  }
}

# Provider Block
provider "aws" {
  profile = "default" # AWS Credentials Profile configured on your local desktop terminal  $HOME/.aws/credentials
  region  = "us-east-1"
}

# Resource Block
resource "aws_instance" "ec2demo" {
  ami           = "ami-04d29b6f966df1537" # Amazon Linux in us-east-1, update as per your region
  instance_type = "t2.micro"
}
```

## Step-03: Terraform Core Commands
```t
# Initialize Terraform
terraform init

# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
```

## Step-04: Verify the EC2 Instance in AWS Management Console
- Go to AWS Management Console -> Services -> EC2
- Verify newly created EC2 instance



## Step-05: Destroy Infrastructure
```t
# Destroy EC2 Instance
terraform destroy

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-08: Conclusion
- Re-iterate what we have learned in this section
- Learned about Important Terraform Commands
  - terraform init
  - terraform validate
  - terraform plan
  - terraform apply
  - terraform destroy     



======================================================================
***********read the file ./02-Terraform-Basics/02-03-Terraform-Language-Syntax/README.md ************
# Terraform Configuration Language Syntax

## Step-01: Introduction
- Understand Terraform Language Basics
  - Understand Blocks
  - Understand Arguments, Attributes & Meta-Arguments
  - Understand Identifiers
  - Understand Comments
  


## Step-02: Terraform Configuration Language Syntax
- Understand Blocks
- Understand Arguments
- Understand Identifiers
- Understand Comments
- [Terraform Configuration](https://www.terraform.io/docs/configuration/index.html)
- [Terraform Configuration Syntax](https://www.terraform.io/docs/configuration/syntax.html)
```t
# Template
<BLOCK TYPE> "<BLOCK LABEL>" "<BLOCK LABEL>"   {
  # Block body
  <IDENTIFIER> = <EXPRESSION> # Argument
}

# AWS Example
resource "aws_instance" "ec2demo" { # BLOCK
  ami           = "ami-04d29b6f966df1537" # Argument
  instance_type = var.instance_type # Argument with value as expression (Variable value replaced from varibales.tf
}
```

## Step-03: Understand about Arguments, Attributes and Meta-Arguments.
- Arguments can be `required` or `optional`
- Attribues format looks like `resource_type.resource_name.attribute_name`
- Meta-Arguments change a resource type's behavior (Example: count, for_each)
- [Additional Reference](https://learn.hashicorp.com/tutorials/terraform/resource?in=terraform/configuration-language) 
- [Resource: AWS Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
- [Resource: AWS Instance Argument Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#argument-reference)
- [Resource: AWS Instance Attribute Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#attributes-reference)
- [Resource: Meta-Arguments](https://www.terraform.io/docs/language/meta-arguments/depends_on.html)

## Step-04: Understand about Terraform Top-Level Blocks
- Discuss about Terraform Top-Level blocks
  - Terraform Settings Block
  - Provider Block
  - Resource Block
  - Input Variables Block
  - Output Values Block
  - Local Values Block
  - Data Sources Block
  - Modules Block

======================================================================
***********read the file ./03-Terraform-Fundamental-Blocks/03-01-Terraform-Block/README.md ************
# Terraform Block 

## Step-01: Introduction
- Understand about Terraform Block and its importance
- Understand how to handle version constraints for Terraform Version and Provider Version in Terraform Block

### Step-02: Understand about Terraform Settings Block
- Required Terraform Version
- Provider Requirements
- Terraform backends
- Experimental Language Features
- Passing Metadata to Providers
- Review the file named **sample-terraform-settings.tf** for more understading

## Step-03: Create a simple terraform block and play with required_version
- `required_version` focuses on underlying Terraform CLI installed on your desktop
- If the running version of Terraform on your local desktop doesn't match the constraints specified in your terraform block, Terraform will produce an error and exit without taking any further actions.
- By changing the versions try `terraform init` and observe whats happening
```
Play with Terraform Version
  required_version = "~> 0.14.3" 
  required_version = "= 0.14.3"    
  required_version = "= 0.14.4"  
  required_version = ">= 0.13"   
  required_version = "= 0.13"    
  required_version = "~> 0.13"   
 

# Terraform Block
terraform {
  required_version = "~> 0.14"
}

# To view my Terraform CLI Version installed on my desktop
terraform version

# Initialize Terraform
terraform init
```
## Step-04: Add Provider and play with Provider version
- `required_providers` block specifies all of the providers required by the current module, mapping each local provider name to a source address and a version constraint.

```
Play with Provider Version
      version = "~> 3.0"            
      version = ">= 3.0.0, < 3.1.0"
      version = ">= 3.0.0, <= 3.1.0"
      version = "~> 2.0"
      version = "~> 3.0"   
```

```
# Terraform Init with upgrade option to change provider version
terraform init -upgrade
```


## Step-05: Clean-Up
```
# Delete Terraform Folders & Files
rm -rf .terraform*
```

## References
- [Terraform Version Constraints](https://www.terraform.io/docs/configuration/version-constraints.html)
- [Terraform Versions - Best Practices](https://www.terraform.io/docs/configuration/version-constraints.html#best-practices)

======================================================================
***********read the file ./03-Terraform-Fundamental-Blocks/03-02-Provider-Block/README.md ************
# Terraform Provider Block

## Step-01: Introduction
- What are Terraform Providers?
- What Providers Do?
- Where do providers reside (Terraform Registry)?
- How to use Providers?
- What are Provider Badges?


## Step-02: Terraform Providers
- What are Terraform Providers?
- What Providers Do?
- Where do providers reside (Terraform Registry)?


## Step-03: Provider Requirements
- Define required providers in Terraform Block
- Understand about three things about defining `required_providers` in terraform block
  - local names
  - source
  - version
```t
# Terraform Block
terraform {
  required_version = "~> 0.14.4"
  required_providers {
    aws = { 
      source = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
}
```


## Step-04: Provider Block  
- Create a Provider Block for AWS
```t
# Provider Block
provider "aws" {
  region = "us-east-1"
  profile = "default"  # defining it is optional for default profile
}
```
- Discuss about [Authentication Types](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication) 
- Static Credentials - NOT A RECOMMENDED Option
- Environment variables
- Shared credentials/configuration file ( We are going to use this)
  - Verify your `cat $HOME/.aws/credentials`
  - If not, use `aws configure` to configure the aws credentials

```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration files
terraform validate

# Execute Terraform Plan
terraform plan
```  

## Step-05: Add a Resource Block to create a AWS VPC
- [AWS VPC Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)
```t
resource "aws_vpc" "myvpc" {
  cidr_block = "10.0.0.0/16"
  tags = {
    "Name" = "myvpc"
  }
}
```

## Step-06: Execute Terraform commands to create AWS VPC
```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration files
terraform validate

# Execute Terraform Plan
terraform plan

# Create Resources using Terraform Apply
terraform apply -auto-approve
```  

## Step-07: Clean-Up 
```t
# Destroy Terraform Resources
terraform destroy -auto-approve

# Delete Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```


## References
- [Terraform Providers](https://www.terraform.io/docs/configuration/providers.html)
- [AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS VPC](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)======================================================================
***********read the file ./03-Terraform-Fundamental-Blocks/03-03-Multiple-Provider-Configurations/README.md ************
# Multiple Provider Configurations

## Step-01: Introduction
- Understand and Implement Multiple Provider Configurations

## Step-02: How to define multiple provider configuration of same Provider?
- Understand about default provider
- Understand and define multiple provider configurations of same provider
```t
# Provider-1 for us-east-1 (Default Provider)
provider "aws" {
  region = "us-east-1"
  profile = "default"
}

# Provider-2 for us-west-1
provider "aws" {
  region = "us-west-1"
  profile = "default"
  alias = "aws-west-1"
}
```

## Step-03: How to reference the non-default provider configuration in a resource?
```t
# Resource Block to Create VPC in us-west-1
resource "aws_vpc" "vpc-us-west-1" {
  cidr_block = "10.2.0.0/16"
  #<PROVIDER NAME>.<ALIAS>
  provider = aws.aws-west-1
  tags = {
    "Name" = "vpc-us-west-1"
  }
}
```

## Step-04: Execute Terraform Commands
```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Generate Terraform Plan
terraform plan

# Create Resources
terraform apply -auto-approve

# Verify the same
1. Verify the VPC created in us-east-1
2. Verify the VPC created in us-west-2
```

## Step-05: Clean-Up 
```t
# Destroy Terraform Resources
terraform destroy -auto-approve

# Delete Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```



## References
- [Provider Meta Argument](https://www.terraform.io/docs/configuration/meta-arguments/resource-provider.html)======================================================================
***********read the file ./03-Terraform-Fundamental-Blocks/03-04-Providers-Dependency-Lock-File/README.md ************
# Providers - Dependency Lock File

## Step-01: Introduction
- Understand the importance of Dependency Lock File which is introduced in Terraform v0.14

## Step-02: Review the Terraform Manifests
- c1-versions.tf
  - Discuss about Terraform, AWS and Random Pet Provider Versions
- c2-s3bucket.tf
  - Discuss about Random Pet Resource
  - Discuss about S3 Bucket Resource
- .terraform.lock.hcl
  - Discuss about Provider Version, Version Constraints and Hashes

## Step-03: Initialize and apply the configuration
```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration files
terraform validate

# Execute Terraform Plan
terraform plan

# Create Resources using Terraform Apply
terraform apply
```
- Review **.terraform.lock.hcl**
  - Discuss about versions
  - Compare `.terraform.lock.hcl-ORIGINAL` & `.terraform.lock.hcl`
  - Backup `.terraform.lock.hcl` as `.terraform.lock.hcl-FIRST-INIT` 
```
# Backup
cp .terraform.lock.hcl .terraform.lock.hcl-FIRST-INIT
```

## Step-04: Upgrade the AWS provider version
- For AWS Provider, with version constraint `version = ">= 2.0.0"`, it is going to upgrade to latest `3.x.x` version with command `terraform init -upgrade` 
```t
# Upgrade Provider Version
terraform init -upgrade
```
- Review **.terraform.lock.hcl**
  - Discuss about AWS Versions
  - Compare `.terraform.lock.hcl-FIRST-INIT` & `.terraform.lock.hcl`

## Step-05: Apply Terraform configuration with latest AWS Provider
- Should fail due to S3 related latest changes came in AWS v3.x provider when compared to AWS v2.x provider
```
# Terraform Apply
terraform apply
```

## Step-06: Comment region in S3 Resource and try Terraform Apply
- It should work. 
- When we do a major version upgrade to providers, it might break few features. 
- So with `.terraform.lock.hcl`, we can avoid this type of issues.
```
# Comment Region Attribute
# Resource Block: Create AWS S3 Bucket
resource "aws_s3_bucket" "sample" {
  bucket = random_pet.petname.id
  acl    = "public-read"

  #region = "us-west-2"
}

# Terraform Apply
terraform apply
```

## Step-07: Clean-Up
```
# Destroy Resources
terraform destroy

# Delete Terraform Files
rm -rf .terraform    # We are not removing files named ".terraform.lock.hcl, .terraform.lock.hcl-ORIGINAL" which are needed for this demo for you.
rm -rf terraform.tfstate*
```

## References
- [Random Pet Provider](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/pet)
- [Dependency Lock File](https://www.terraform.io/docs/configuration/dependency-lock.html)
- [Terraform New Features in v0.14](https://learn.hashicorp.com/tutorials/terraform/provider-versioning?in=terraform/0-14)
- [AWS S3 Bucket Region - Read Only in AWS Provider V3.x](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/guides/version-3-upgrade#region-attribute-is-now-read-only)======================================================================
***********read the file ./04-Terraform-Resources/04-01-Resource-Syntax-and-Behavior/README.md ************
# Terraform Resource Syntax & Behavior

## Step-01: Introduction
- Understand Resource Syntax
- Understand Resource Behavior
- Understanding Terraform State File
  - terraform.tfstate
- Understanding Desired and Current States (High Level Only)

## Step-02: Understand Resource Syntax
- We are going to understand about below concepts from Resource Syntax perspective
- Resource Block
- Resource Type
- Resource Local Name
- Resource Arguments
- Resource Meta-Arguments

## Step-03: Understand Resource Behavior
- We are going to understand resource behavior in combination with Terraform State
  - Create Resource
  - Update in-place Resources
  - Destroy and Re-create Resources
  - Destroy Resource  


## Step-04: Resource: Create Resource: Create EC2 Instance
```
# Initialize Terraform
terraform init

Observation: 
1) Successfully downloaded providers in .terraform folder
2) Created lock file named ".terraform.lock.hcl"

# Validate Terraform configuration files
terraform validate
Observation: No files changed / added in current working directory

# Format Terraform configuration files
terraform fmt
Observations: *.tf files will change to format them if any format changes exists

# Review the terraform plan
terraform plan 
Observation-1: Nothing happens during the first run from terraform state perspective
Observation-2: From Resource Behavior perspective you can see "+ create", we are creating 

# Create Resources 
terraform apply -auto-approve
Observation: 
1) Creates terraform.tfstate file in local working directory
2) Creates actual resource in AWS Cloud
```
- **Important Note:** Here we have seen example for **Create Resource**


## Step-05: Understanding Terraform State File
- What is Terraform State ? 
  - It is the primary core thing for terraform to function
  - In a short way, its the underlying database containing the resources information which are provisioning using Terraform
  - **Primary Purpose:** To store bindings between objects in a remote system and resource instances declared in your configuration. 
  - When Terraform creates a remote object in response to a change of configuration, it will record the identity of that remote object against a particular resource instance, and then potentially update or delete that object in response to future configuration changes.
- Terraform state file created when we first run the `terraform apply`
- Terraform state file is created locally in working directory.
- If required, we can confiure the `backend block` in `terraform block` which will allow us to store state file remotely.  Storing remotely is recommended option which we will see in the next section of the course. 

## Step-06: Review terraform.tfstate file
- Terraform State files are JSON based
- Manual editing of Terraform state files is highly not recommended
- Review `terraform.tfstate` file step by step


## Step-07: Resource: Update In-Place: Make changes by adding new tag to EC2 Instance 
- Add a new tag in `c2-ec2-instance.tf`
```
# Add this for EC2 Instance tags
    "tag1" = "Update-test-1"
```
- Review Terraform Plan
```
# Review the terraform plan
terraform plan 
Observation: You should see "~ update in-place" 
"Plan: 0 to add, 1 to change, 0 to destroy."

# Create / Update Resources 
terraform apply -auto-approve
Observation: "Apply complete! Resources: 0 added, 1 changed, 0 destroyed."
```
- **Important Note:** Here we have seen example for **update in-place**


## Step-08: Resource: Destroy and Re-create Resources: Update Availability Zone
- This will destroy the EC2 Instance in 1 AZ and re-creates in other AZ
```
# Before
  availability_zone = "us-east-1a"

# After
  availability_zone = "us-east-1b"  
```
```
# Review the terraform plan
terraform plan 
Observation: 
1) -/+ destroy and then create replacement
2) # aws_instance.my-ec2-vm must be "replaced"
3) # aws_instance.my-ec2-vm must be "replaced" - This parameter forces replacement
4) "Plan: 1 to add, 0 to change, 1 to destroy."

# Create / Update Resources 
terraform apply -auto-approve
Observation: "Apply complete! Resources: 1 added, 0 changed, 1 destroyed."
```


## Step-09: Resource: Destroy Resource
```
# Destroy Resource
terraform destroy 
Observation: 
1) - destroy
2) # aws_instance.my-ec2-vm will be destroyed
3) Plan: 0 to add, 0 to change, 1 to destroy
4) Destroy complete! Resources: 1 destroyed
```

## Step-10: Understand Desired and Current States (High-Level Only)
- **Desired State:** Local Terraform Manifest (All *.tf files)
- **Current State:**  Real Resources present in your cloud

## Step-11: Clean-Up
```
# Destroy Resource
terraform destroy -auto-approve 

# Remove Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## References
- [Terraform State](https://www.terraform.io/docs/language/state/index.html)
- [Manipulating Terraform State](https://www.terraform.io/docs/cli/state/index.html)======================================================================
***********read the file ./04-Terraform-Resources/04-02-Meta-Argument-depends_on/README.md ************
# Terraform Resource Meta-Argument depends_on

## Step-01: Introduction
- Create 9 aws resources in a step by step manner
- Create Terraform Block
- Create Provider Block
- Create 9 Resource Blocks
  - Create VPC
  - Create Subnet
  - Create Internet Gateway
  - Create Route Table
  - Create Route in Route Table for Internet Access
  - Associate Route Table with Subnet
  - Create Security Group in the VPC with port 80, 22 as inbound open
  - Create EC2 Instance in respective new vpc, new subnet created above with a static key pair, associate Security group created earlier
  - Create Elastic IP Address and Associate to EC2 Instance
  - Use `depends_on` Resource Meta-Argument attribute when creating Elastic IP  

## Step-02: Pre-requisite - Create a EC2 Key Pair
- Create EC2 Key pair `terraform-key` and download pem file and put ready for SSH login

## Step-03: c1-versions.tf - Create Terraform & Provider Blocks 
- Create Terraform Block
- Create Provider Block
```
# Terraform Block
terraform {
  required_version = ">= 1.4" 
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}

# Provider Block
provider "aws" {
  region = "us-east-1"
  profile = "default"
}
```
## Step-04: c2-vpc.tf - Create VPC Resources 
### Step-04-01: Create VPC using AWS Management Console
- Create VPC Manually and understand all the resources we are going to create. Delete that VPC and start writing the VPC template using terraform
### Step-04-02: Create VPC using Terraform
- Create VPC Resources listed below  
  - Create VPC
  - Create Subnet
  - Create Internet Gateway
  - Create Route Table
  - Create Route in Route Table for Internet Access
  - Associate Route Table with Subnet
  - Create Security Group in the VPC with port 80, 22 as inbound open
```
# Resource Block
# Resource-1: Create VPC
resource "aws_vpc" "vpc-dev" {
  cidr_block = "10.0.0.0/16"

  tags = {
    "name" = "vpc-dev"
  }
}

# Resource-2: Create Subnets
resource "aws_subnet" "vpc-dev-public-subnet-1" {
  vpc_id = aws_vpc.vpc-dev.id
  cidr_block = "10.0.1.0/24"
  availability_zone = "us-east-1a"
  map_public_ip_on_launch = true
}


# Resource-3: Internet Gateway
resource "aws_internet_gateway" "vpc-dev-igw" {
  vpc_id = aws_vpc.vpc-dev.id
}

# Resource-4: Create Route Table
resource "aws_route_table" "vpc-dev-public-route-table" {
  vpc_id = aws_vpc.vpc-dev.id
}

# Resource-5: Create Route in Route Table for Internet Access
resource "aws_route" "vpc-dev-public-route" {
  route_table_id = aws_route_table.vpc-dev-public-route-table.id 
  destination_cidr_block = "0.0.0.0/0"
  gateway_id = aws_internet_gateway.vpc-dev-igw.id 
}


# Resource-6: Associate the Route Table with the Subnet
resource "aws_route_table_association" "vpc-dev-public-route-table-associate" {
  route_table_id = aws_route_table.vpc-dev-public-route-table.id 
  subnet_id = aws_subnet.vpc-dev-public-subnet-1.id
}

# Resource-7: Create Security Group
resource "aws_security_group" "dev-vpc-sg" {
  name = "dev-vpc-default-sg"
  vpc_id = aws_vpc.vpc-dev.id
  description = "Dev VPC Default Security Group"

  ingress {
    description = "Allow Port 22"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "Allow Port 80"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    description = "Allow all ip and ports outboun"
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

}
```

## Step-05: c3-ec2-instance.tf - Create EC2 Instance Resource
- Review `apache-install.sh`
```sh
#! /bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo service httpd start  
sudo systemctl enable httpd
echo "<h1>Welcome to StackSimplify ! AWS Infra created using Terraform in us-east-1 Region</h1>" > /var/www/html/index.html
```
- Create EC2 Instance Resource
```
# Resource-8: Create EC2 Instance
resource "aws_instance" "my-ec2-vm" {
  ami = "ami-0be2609ba883822ec" # Amazon Linux
  instance_type = "t2.micro"
  subnet_id = aws_subnet.vpc-dev-public-subnet-1.id
  key_name = "terraform-key"
	#user_data = file("apache-install.sh")
  user_data = <<-EOF
    #!/bin/bash
    sudo yum update -y
    sudo yum install httpd -y
    sudo systemctl enable httpd
    sudo systemctl start httpd
    echo "<h1>Welcome to StackSimplify ! AWS Infra created using Terraform in us-east-1 Region</h1>" > /var/www/html/index.html
    EOF  
  vpc_security_group_ids = [ aws_security_group.dev-vpc-sg.id ]
}
```

## Step-06: c4-elastic-ip.tf - Create Elastic IP Resource
- Create Elastic IP Resource
- Add a Resource Meta-Argument `depends_on` ensuring Elastic IP gets created only after AWS Internet Gateway in a VPC is present or created
```
# Resource-9: Create Elastic IP
resource "aws_eip" "my-eip" {
  instance = aws_instance.my-ec2-vm.id
  vpc = true
  depends_on = [ aws_internet_gateway.vpc-dev-igw ]
}
```

## Step-07: Execute Terraform commands to Create Resources using Terraform
```
# Initialize Terraform
terraform init

# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
```

## Step-08: Verify the Resources
- Verify VPC
- Verify EC2 Instance
- Verify Elastic IP
- Review the `terraform.tfstate` file
- Access Apache Webserver Static page using Elastic IP
```
# Access Application
http://<AWS-ELASTIC-IP>
```

## Step-09: Destroy Terraform Resources
```
# Destroy Terraform Resources
terraform destroy

# Remove Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## References 
- [Elastic IP creation depends on VPC Internet Gateway](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eip)
- [Resource: aws_vpc](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)
- [Resource: aws_subnet](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet)
- [Resource: aws_internet_gateway](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/internet_gateway)
- [Resource: aws_route_table](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route_table)
- [Resource: aws_route](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route)
- [Resource: aws_route_table_association](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route_table_association)
- [Resource: aws_security_group](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group)
- [Resource: aws_instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
- [Resource: aws_eip](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eip)======================================================================
***********read the file ./04-Terraform-Resources/04-03-Meta-Argument-count/README.md ************
# Terraform Resource Meta-Argument count

## Step-01: Introduction
- Understand Resource Meta-Argument `count`
- Also implement count and count index practically 

## Step-02: Create 5 EC2 Instances using Terraform
- In general, 1 EC2 Instance Resource in Terraform equals to 1 EC2 Instance in Real AWS Cloud
- 5 EC2 Instance Resources = 5 EC2 Instances in AWS Cloud
- With `Meta-Argument count` this is going to become super simple. 
- Lets see how. 
```t
# Create EC2 Instance
resource "aws_instance" "web" {
  ami = "ami-047a51fa27710816e" # Amazon Linux
  instance_type = "t2.micro"
  count = 5
  tags = {
    "Name" = "web"
  }
}
```
- **Execute Terraform Commands**
```t
# Initialize Terraform
terraform init

# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
```
- Verify EC2 Instances and its Name


## Step-03: Understand about count index
- If we currently see all our EC2 Instances has the same name `web`
- Lets name them by using count index `web-0, web-1, web-2, web-3, web-4`
```t
# Create EC2 Instance
resource "aws_instance" "web" {
  ami = "ami-047a51fa27710816e" # Amazon Linux
  instance_type = "t2.micro"
  count = 5
  tags = {
    #"Name" = "web"
    "Name" = "web-${count.index}"
  }
}
```
- **Execute Terraform Commands**
```t
# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
```
- Verify EC2 Instances


## Step-04: Destroy Terraform Resources
```t
# Destroy Terraform Resources
terraform destroy

# Remove Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## References
- [Resources: Count Meta-Argument](https://www.terraform.io/docs/language/meta-arguments/count.html)======================================================================
***********read the file ./04-Terraform-Resources/04-04-Meta-Argument-for_each/README.md ************
# Terraform Resource Meta-Argument for_each

## Step-01: Introduction
- Understand about Meta-Argument `for_each`
- Implement `for_each` with **Maps**
- Implement `for_each` with **Set of Strings**

## Step-02: Implement for_each with Maps
- **Reference Folder:** v1-for_each-maps
- **Use case:** Create four S3 buckets using for_each maps 
- **c2-s3bucket.tf**
```t
# Create S3 Bucket per environment with for_each and maps
# https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket

resource "aws_s3_bucket" "mys3bucket" {

  # for_each Meta-Argument
  for_each = {
    dev  = "my-dapp-bucket"
    qa   = "my-qapp-bucket"
    stag = "my-sapp-bucket"
    prod = "my-papp-bucket"
  }

  bucket = "${each.key}-${each.value}"
  #acl    = "private" # This argument is deprecated, so commenting it. 
  

  tags = {
    Environment = each.key
    bucketname  = "${each.key}-${each.value}"
    eachvalue   = each.value
  }
}

```

## Step-03: Execute Terraform Commands
```t
# Switch to Working Directory
cd v1-for_each-maps

# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Format Terraform Configuration Files
terraform fmt

# Generate Terraform Plan
terraform plan
Observation: 
1) Four buckets creation will be generated in plan
2) Review Resource Names ResourceType.ResourceLocalName[each.key]
2) Review bucket name (each.key+each.value)
3) Review bucket tags

# Create Resources
terraform apply
Observation: 
1) 4 S3 buckets should be created
2) Review bucket names and tags in AWS Management console

# Destroy Resources
terraform destroy

# Clean-Up 
rm -rf .terraform*
rm -rf terraform.tfstate*
```


## Step-04: Implement for_each with toset "Strings"
- **Reference Folder:** v2-for_each-toset
- **Use case:** Create four IAM Users using for_each toset strings 
- **c2-iamuser.tf**
```t
# Create 4 IAM Users
resource "aws_iam_user" "myuser" {
  for_each = toset( ["Jack", "James", "Madhu", "Dave"] )
  name     = each.key
}
```

## Step-05: Execute Terraform Commands
```t
# Switch to Working Directory
cd v2-for_each-toset

# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Format Terraform Configuration Files
terraform fmt

# Generate Terraform Plan
terraform plan
Observation: 
1) Four IAM users creation will be generated in plan
2) Review Resource Names ResourceType.ResourceLocalName[each.key]
2) Review IAM User name (each.key)

# Create Resources
terraform apply
Observation: 
1) 4 IAM users should be created
2) Review IAM users in AWS Management console

# Destroy Resources
terraform destroy

# Clean-Up 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Reference
- [Resource Meta-Argument: for_each](https://www.terraform.io/docs/language/meta-arguments/for_each.html)
- [Resource: AWS S3 Bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket)
- [Resource: AWS IAM User](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_user)======================================================================
***********read the file ./04-Terraform-Resources/04-05-Meta-Argument-lifecycle/README.md ************
# Terraform Resource Meta-Argument lifecycle

## Step-01: Introduction
- lifecyle Meta-Argument block contains 3 arguments
  - create_before_destroy
  - prevent_destroy
  - ignore_changes
- Understand and implement each of these 3 things practically in a step by step manner  

## Step-02: lifecyle - create_before_destroy
- Default Behavior of a Resource: Destroy Resource & re-create Resource
- With Lifecycle Block we can change that using `create_before_destroy=true`
  - First new resource will get created
  - Second old resource will get destroyed
- **Add Lifecycle Block inside Resource Block to alter behavior**  
```t
  lifecycle {
    create_before_destroy = true
  }
```  
### Step-02-01: Observe without Lifecycle Block
```t
# Switch to Working Directory
cd v1-create_before_destroy

# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Format Terraform Configuration Files
terraform fmt

# Generate Terraform Plan
terraform plan

# Create Resources
terraform apply -auto-approve

# Modify Resource Configuration
Change Availability Zone from us-east-1a to us-east-1b

# Apply Changes
terraform apply -auto-approve
Observation: 
1) First us-east-1a resource will be destroyed
2) Second us-east-1b resource will get
```
### Step-02-02: With Lifecycle Block
- Add Lifecycle block in the resource (Uncomment lifecycle block)
```t
# Generate Terraform Plan
terraform plan

# Apply Changes
terraform apply -auto-approve

# Modify Resource Configuration
Change Availability Zone from us-east-1b to us-east-1a

# Apply Changes
terraform apply -auto-approve
Observation: 
1) First us-east-1a resource will get created
2) Second us-east-1b resource will get deleted
```
### Step-02-03: Clean-Up Resources
```t
# Destroy Resources
terraform destroy -auto-approve

# Clean-Up 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-03: lifecycle - prevent_destroy
### Step-03-01: Introduction
- This meta-argument, when set to true, will cause Terraform to reject with an error any plan that would destroy the infrastructure object associated with the resource, as long as the argument remains present in the configuration.
- This can be used as a measure of safety against the accidental replacement of objects that may be costly to reproduce, such as database instances
- However, it will make certain configuration changes impossible to apply, and will prevent the use of the `terraform destroy` command once such objects are created, and so this option should be used `sparingly`.
- Since this argument must be present in configuration for the protection to apply, note that this setting does not prevent the remote object from being destroyed if the resource block were removed from configuration entirely: in that case, the `prevent_destroy` setting is removed along with it, and so Terraform will allow the destroy operation to succeed.
```t
  lifecycle {
    prevent_destroy = true # Default is false
  }
```
### Step-03-02: Execute Terraform Commands
```t
# Switch to Working Directory
cd v2-prevent_destroy

# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Format Terraform Configuration Files
terraform fmt

# Generate Terraform Plan
terraform plan

# Create Resources
terraform apply -auto-approve

# Destroy Resource
terraform destroy 
Observation: 
Kalyans-MacBook-Pro:v2-prevent_destroy kdaida$ terraform destroy -auto-approve
Error: Instance cannot be destroyed
  on c2-ec2-instance.tf line 2:
   2: resource "aws_instance" "web" {
Resource aws_instance.web has lifecycle.prevent_destroy set, but the plan
calls for this resource to be destroyed. To avoid this error and continue with
the plan, either disable lifecycle.prevent_destroy or reduce the scope of the
plan using the -target flag.


# Remove/Comment Lifecycle block
- Remove or Comment lifecycle block and clean-up

# Destroy Resource after removing lifecycle block
terraform destroy

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*
```


## Step-04: lifecyle - ignore_changes
### Step-04-01: Create an EC2 Changes, make manual changes and understand the behavior
- Create EC2 Instance
```t
# Switch to Working Directory
cd v3-ignore_changes

# Initialize Terraform
terraform init

# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
```
### Step-04-02: Update the tag by going to AWS management console
- Add a new tag manually to EC2 Instance
- Try `terraform apply` now
- Terraform will find the difference in configuration on remote side when compare to local and tries to remove the manual change when we execute `terraform apply`
```t
# Add new tag manually
WebServer = Apache

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
Observation: 
1) It will remove the changes which we manually added using AWS Management console
```

### Step-04-03: Add the lifecyle - ignore_changes block
- Enable the block in `c2-ec2-instance.tf`

```t
   lifecycle {
    ignore_changes = [
      # Ignore changes to tags, e.g. because a management agent
      # updates these based on some ruleset managed elsewhere.
      tags,
    ]
  }
```
- Add new tags manually using AWS mgmt console for the EC2 Instance
```t
# Add new tag manually
WebServer = Apache2
ignorechanges = test1

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
Observation: 
1) Manual changes should not be touched. They should be ignored by terraform
2) Verify the same on AWS management console

# Destroy Resource
terraform destroy -auto-approve

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## References
- [Resource Meat-Argument: Lifecycle](https://www.terraform.io/docs/language/meta-arguments/lifecycle.html)======================================================================
***********read the file ./04-Terraform-Resources/04-06-Provisioners/README.md ************
# Provisioners
-  We are going to learn Terraform Provisioners in detail in [Section-09](https://github.com/stacksimplify/hashicorp-certified-terraform-associate/tree/master/09-Terraform-Provisioners) of this course. 
======================================================================
***********read the file ./05-Terraform-Variables/05-01-Terraform-Input-Variables/README.md ************
# Terraform Input Variables

## Step-00: Introduction
- **v1:** Input Variables - Basics
- **v2:** Provide Input Variables when prompted during terraform plan or apply
- **v3:** Override default variable values using CLI argument `-var` 
- **v4:** Override default variable values using Environment Variables
- **v5:** Provide Input Variables using `terraform.tfvars` files
- **v6:** Provide Input Variables using `<any-name>.tfvars` file with CLI 
argument `-var-file`
- **v7:** Provide Input Variables using `auto.tfvars` files
- **v8-01:** Implement complex type constructors like `list` 
- **v8-02:** Implement complex type constructors like `maps`
- **v9:** Implement Custom Validation Rules in Variables
- **v10:** Protect Sensitive Input Variables
- **v11:** Understand about `File` function

## Pre-requisite
- Create a new EC2 Key pair with name as `terraform-key`
- In all the templates listed below V1 to V12, we will be using `key_name      = "terraform-key"` incase if you want to login to EC2 Instance you can use this key


## Step-01: Input Variables Basics 
- **Reference Sub folder:** v1-Input-Variables-Basic
- Create / Review the terraform manifests
  - c1-versions.tf
  - c2-variables.tf
  - c3-security-groups.tf
  - c4-ec2-instance.tf
- We are going to define `c3-variables.tf` and define the below listed variables
  - aws_region is a variable of type `string`
  - ec2_ami_id is a variable of type `string`
  - ec2_instance_count is a variable of type `number`
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan

# Create Resources
terraform apply

# Access Application
http://<Public-IP-Address>

# Clean-Up
terraform destroy -auto-approve
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-02: Input Variables Assign When Prompted
- **Reference Sub folder:** v2-Input-Variables-Assign-when-prompted
- Add a new variable in `variables.tf` named `ec2_instance_type` without any default value. 
- As the variable doesn't have any default value when you execute `terraform plan` or `terraform apply` it will prompt for the variable. 

```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan
```

## Step-03: Input Variables Override default value with cli argument `-var`
- **Reference Sub folder:** v3-Input-Variables-Override-default-with-cli
- We are going to override the default values defined in `variables.tf` by providing new values using the `-var` argument using CLI
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Option-1 (Always provide -var for both plan and apply)
# Review the terraform plan
terraform plan -var="ec2_instance_type=t3.large" -var="ec2_instance_count=1"

# Create Resources (optional)
terraform apply -var="ec2_instance_type=t3.large" -var="ec2_instance_count=1"

# Option-2 (Generate plan file with -var and use that with apply)
# Generate Terraform plan file
terraform plan -var="ec2_instance_type=t3.large" -var="ec2_instance_count=1" -out v3out.plan

# Create / Deploy Terraform Resources using Plan file
terraform apply v3out.plan 
```

## Step-04: Input Variables Override with Environment Variables
- **Reference Sub folder:** v4-Input-Variables-Override-with-Environment-Variables
- Set environment variables and execute `terraform plan` to see if it overrides default values 
```t
# Sample
export TF_VAR_variable_name=value

# SET Environment Variables
export TF_VAR_ec2_instance_count=1
export TF_VAR_ec2_instance_type=t3.large
echo $TF_VAR_ec2_instance_count, $TF_VAR_ec2_instance_type

# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan

# UNSET Environment Variables after demo
unset TF_VAR_ec2_instance_count
unset TF_VAR_ec2_instance_type
echo $TF_VAR_ec2_instance_count, $TF_VAR_ec2_instance_type
```

## Step-05: Assign Input Variables from terraform.tfvars
- **Reference Sub folder:** v5-Input-Variables-Assign-with-terraform-tfvars
- Create a file named `terraform.tfvars` and define variables
- If the file name is `terraform.tfvars`, terraform will auto-load the variables present in this file by overriding the `default` values in `variables.tf`
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan

# Create Resources
terraform apply

# Access Application
http://<Elastic-IP-Address>
```

## Step-06: Assign Input Variables with -var-file argument
- **Reference Sub folder:** v6-Input-Variables-Assign-with-tfvars-var-file
- If we plan to use different names for  `.tfvars` files, then we need to explicitly provide the argument `-var-file` during the `terraform plan or apply`
- We will use following things in this example
  - **c2-variables.tf:** aws_region variable will be picked with default value
  - **terraform.tfvars:** ec2_instance_count variable will be picked from this file
  - **web.tfvars:** ec2_instance_type variable will be picked from this file
  - **app.tfvars:** ec2_instance_type variable will be picked from this file
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan -var-file="web.tfvars"
terraform plan -var-file="app.tfvars"
```

## Step-07: Auto load input variables with .auto.tfvars files
- **Reference Sub folder:** v7-Input-Variables-Assign-with-auto-tfvars
- We will create a file with extension as `.auto.tfvars`. 
- With this extension, whatever may be the file name, the variables inside these files will be auto loaded during `terraform plan or apply`
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 
```
## Step-08: Implement complex type cosntructors like `list` and `maps`
- **Reference Sub folder:** v8-Input-Variables-Lists-Maps
- [Type Constraints](https://www.terraform.io/docs/language/expressions/types.html)
### Step-08-01: Implement Vairable Type as List
- **list (or tuple):** a sequence of values, like ["us-west-1a", "us-west-1c"]. Elements in a list or tuple are identified by consecutive whole numbers, starting with zero.
- Implement List function for variable `ec2_instance_type`
```t
# Implement List Function in variables.tf
variable "ec2_instance_type" {
  description = "EC2 Instance Type"
  type = list(string)
  default = ["t3.micro", "t3.small", "t3.medium"]
}

# Reference Values from List in ec2-instance.tf
instance_type = var.ec2_instance_type[0] --> t3.micro
instance_type = var.ec2_instance_type[1] --> t3.small
instance_type = var.ec2_instance_type[2] --> t3.medium

# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 
```

### Step-08-02: Implement Vairable Type as Map
- **map (or object):** a group of values identified by named labels, like {name = "Mabel", age = 52}.
- Implement Map function for variable `ec2_instance_tags`
```t
# Implement Map Function for tags
variable "ec2_instance_tags" {
  description = "EC2 Instance Tags"
  type = map(string)
  default = {
    "Name" = "ec2-web"
    "Tier" = "Web"
  }

# Reference Values from Map in ec2-instance.tf
tags = var.ec2_instance_tags  

# Implement Map Function for Instance Type
# Important Note: comment "ec2_instance_type" variable with list function
variable "ec2_instance_type_map" {
  description = "EC2 Instance Type using maps"
  type = map(string)
  default = {
    "small-apps" = "t3.micro"
    "medium-apps" = "t3.medium" 
    "big-apps" = "t3.large"
  }

# Reference Instance Type from Maps Variables
instance_type = var.ec2_instance_type_map["small-apps"]
instance_type = var.ec2_instance_type_map["medium-apps"]
instance_type = var.ec2_instance_type_map["big-apps"]

# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 
```



## Step-09: Implement Custom Validation Rules in Variables
- **Reference Sub folder:** v9-Input-Variables-Validation-Rules
- Understand and implement custom validation rules in variables
- [Terraform Console](https://www.terraform.io/docs/cli/commands/console.html)
- The `terraform console` command provides an interactive console for evaluating expressions.
### Step-09-01: Learn Terraform Length Function
- [Terraform Length Function](https://www.terraform.io/docs/language/functions/length.html)
```t
# Go to Terraform Console
terraform console

# Test length function
Template: length()
length("hi")
length("hello")
length(["a", "b", "c"]) # List
length({"key" = "value"}) # Map
length({"key1" = "value1", "key2" = "value2" }) #Map
```

### Step-09-02: Learn Terraform SubString Function
- [Terraform Sub String Function](https://www.terraform.io/docs/language/functions/substr.html)
```t
# Go to Terraform Console
terraform console

# Test substr function
Template: substr(string, offset, length)
substr("stack simplify", 1, 4)
substr("stack simplify", 0, 6)
substr("stack simplify", 0, 1)
substr("stack simplify", 0, 0)
substr("stack simplify", 0, 10)
```

### Step-09-03: Implement Validation Rule for ec2_ami_id variable
```t
variable "ec2_ami_id" {
  description = "AMI ID"
  type = string  
  default = "ami-0be2609ba883822ec"
  validation {
    condition = length(var.ec2_ami_id) > 4 && substr(var.ec2_ami_id, 0, 4) == "ami-"
    error_message = "The ec2_ami_id value must be a valid AMI id, starting with \"ami-\"."
  }
}
```
- **Run Terraform commands**
```
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan
```

## Step-10: Protect Sensitive Input Variables
- **Reference Sub folder:** v10-Sensitive-Input-Variables
- [AWS RDS DB Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_instance)
- [Vault Provider](https://learn.hashicorp.com/tutorials/terraform/secrets-vault?in=terraform/secrets)
- When using environment variables to set sensitive values, keep in mind that those values will be in your environment and command-line history
`Example: export TF_VAR_db_username=admin TF_VAR_db_password=adifferentpassword`
- When you use sensitive variables in your Terraform configuration, you can use them as you would any other variable. 
- Terraform will `redact` these values in command output and log files, and raise an error when it detects that they will be exposed in other ways.
- **Important Note-1:** Never check-in `secrets.tfvars` to git repositories
- **Important Note-2:** Terraform state file contains values for these sensitive variables `terraform.tfstate`. You must keep your state file secure to avoid exposing this data.
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan -var-file="secrets.tfvars"

# Create Resources
terraform apply -var-file="secrets.tfvars"

# Verify Terraform State files
grep password terraform.tfstate
grep username terraform.tfstate 

# Destroy Resources
terraform destroy var-file="secrets.tfvars"

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*
```

### Variable Definition Precedence
- [Terraform Variable Definition Precedence](https://www.terraform.io/docs/language/values/variables.html#variable-definition-precedence)


## Step-11: Understand about `File` function
- **Reference Sub folder:** v11-File-Function
- Understand about [Terraform File Function](https://www.terraform.io/docs/language/functions/file.html)

```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 

# Create Resources
terraform apply 

# Access Application
http://<Public-IP>

# Destroy Resources
terraform destroy -auto-approve
```


## References
- [Terraform Input Variables](https://www.terraform.io/docs/language/values/variables.html)
- [Resource: AWS EC2 Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
- [Resource: AWS Security Group](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group)
- [Sensitive Variables - Additional Reference](https://learn.hashicorp.com/tutorials/terraform/sensitive-variables)



======================================================================
***********read the file ./05-Terraform-Variables/05-02-Terraform-Output-Values/README.md ************
# Terraform Output Values

## Step-01: Introduction
- Understand about Output Values and implement them
- Query outputs using `terraform output`
- Understand about redacting secure attributes in output values
- Generate machine-readable output

## Step-02: Basics of Output Values
- **Reference Sub folder:** terraform-manifests
- Understand Output Values
- You can export both Argument & Attribute References
- [Terraform AWS EC2 Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 

# Create Resources
terraform apply -auto-approve

# Access Application
http://<Public-IP>
http://<Public-DNS>
```

## Step-03: Query Terraform Outputs
- Terraform will load the project state in state file, so that using `terraform output` command we can query the state file. 
```t
# Terraform Output Commands
terraform output
terraform output ec2_publicdns
```


## Step-04: Output Values - Suppressing Sensitive Values in Output
- We can redact the sensitive outputs using `sensitve = true` in output block
- This will only redact the cli output for terraform plan and apply
- When you query using `terraform output`, you will be able to fetch exact values from `terraform.tfstate` files
- Add `sensitve = true` for output `ec2_publicdns`
```t
# Attribute Reference - Create Public DNS URL with http:// appended
output "ec2_publicdns" {
  description = "Public DNS URL of an EC2 Instance"
  value = "http://${aws_instance.my-ec2-vm.public_dns}"
  sensitive = true
}
```
- Test the flow
```t
# Terraform Apply
terraform apply -auto-approve
Observation: You should see the value as sensitive

# Query using terraform output
terraform output ec2_publicdns
Observation: You should get non-redacted original value from terraform.tfstate file
```

## Step-05: Generate machine-readable output
```t
# Generate machine-readable output
terraform output -json
```

## Step-06: Destroy Resources
```t
# Destroy Resources
terraform destroy -auto-approve

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*
```


## References
- [Terraform Output Values](https://www.terraform.io/docs/language/values/outputs.html)======================================================================
***********read the file ./05-Terraform-Variables/05-03-Terraform-Local-Values/README.md ************
# Terraform Local Values

## Step-01: Introduction
- Understand DRY Principle
- What is local value in terraform?
- When To Use Local Values?
- What is the problem locals are solving ?

```
What is DRY Principle ?
Don't repeat yourself

What is local value in terraform?
The local block defines one or more local variables within a module. 
A local value assigns a name to an terraform expression, allowing it to be used multiple times within a module without repeating it.

When To Use Local Values?
Local values can be helpful to avoid repeating the same values or expressions multiple times in a configuration
If overused they can also make a configuration hard to read by future maintainers by hiding the actual values used.
Use local values only in moderation, in situations where a single value or result is used in many places and that value is likely to be changed in future. The ability to easily change the value in a central place is the key advantage of local values.

What is the problem locals are solving ?
Currently terraform doesn’t allow variable substitution within variables. The terraform way of doing this is by using local values or locals where you can somehow keep your code DRY.

Another use case (at least for me) for locals is to shorten references on upstream terraform projects as seen below. This will make your terraform templates/modules more readable.
```

## Step-02: Create / Review Terraform configuration files
- c1-versions.tf
- c2-variables.tf
- c3-s3-bucket.tf


## Step-03: Test the Terraform configuration using commands
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 

# Create Resources (Optional)
terraform apply -auto-approve
```

## References
- [Terraform Local values](https://www.terraform.io/docs/language/values/locals.html)======================================================================
***********read the file ./06-Terraform-Datasources/README.md ************
# Terraform Datasources

## Step-01: Introduction
- Understand about Datasources in Terraform
- Implement a sample usecase with Datasources.
- Get the latest Amazon Linux 2 AMI ID using datasources and reference that value when creating EC2 Instance resource `ami = data.aws_ami.amzlinux.id`

## Step-02: Create a Datasource to fetch latest AMI ID
- Create or review manifest `c6-ami-datasource.tf`
- Go to AWS Mgmt Console -> Services -> EC2 -> Images -> AMI 
- Search for "Public Images" -> Provide AMI ID
  - We can get AMI Name format
  - We can get Owner Name
  - Visibility
  - Platform
  - Root Device Type
  - and many more info here. 
- Accordingly using this information build your filters in datasource

## Step-03: Reference the datasource in ec2-instance.tf
```t
  ami           = data.aws_ami.amzlinux.id 
```

## Step-04: Test using Terraform commands
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 

# Create Resources (Optional)
terraform apply -auto-approve

# Access Application
http://<Public-DNS>

# Destroy Resources
terraform destroy -auto-approve
```


## References
- [AWS EC2 AMI Datasource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/ami)
======================================================================
***********read the file ./07-Terraform-State/07-01-Terraform-Remote-State-Storage-and-Locking/README.md ************
# Terraform Remote State Storage & Locking

## Step-01: Introduction
- Understand Terraform Backends
- Understand about Remote State Storage and its advantages
- This state is stored by default in a local file named "terraform.tfstate", but it can also be stored remotely, which works better in a team environment.
- Create AWS S3 bucket to store `terraform.tfstate` file and enable backend configurations in terraform settings block
- Understand about **State Locking** and its advantages
- Create DynamoDB Table and  implement State Locking by enabling the same in Terraform backend configuration

## Step-02: Create S3 Bucket
- Go to Services -> S3 -> Create Bucket
- **Bucket name:** terraform-stacksimplify
- **Region:** US-East (N.Virginia)
- **Bucket settings for Block Public Access:** leave to defaults
- **Bucket Versioning:** Enable
- Rest all leave to **defaults**
- Click on **Create Bucket**
- **Create Folder**
  - **Folder Name:** dev
  - Click on **Create Folder**


## Step-03: Terraform Backend Configuration
- **Reference Sub-folder:** terraform-manifests
- [Terraform Backend as S3](https://www.terraform.io/docs/language/settings/backends/s3.html)
- Add the below listed Terraform backend block in `Terrafrom Settings` block in `main.tf`
```t
# Terraform Backend Block
  backend "s3" {
    bucket = "terraform-stacksimplify"
    key    = "dev/terraform.tfstate"
    region = "us-east-1"    
  }
```

## Step-04: Test with Remote State Storage Backend
```t
# Initialize Terraform
terraform init

Observation: 
Successfully configured the backend "s3"! Terraform will automatically
use this backend unless the backend configuration changes.

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate

# Validate Terraform configuration files
terraform validate

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate

# Format Terraform configuration files
terraform fmt

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate

# Review the terraform plan
terraform plan 

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate

# Create Resources 
terraform apply -auto-approve

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate
Observation: Finally at this point you should see the terraform.tfstate file in s3 bucket

# Access Application
http://<Public-DNS>
```

## Step-05: Bucket Versioning - Test
- Update in `c2-variables.tf` 
```t
variable "instance_type" {
  description = "EC2 Instance Type - Instance Sizing"
  type = string
  #default = "t2.micro"
  default = "t2.small"
}
```
- Execute Terraform Commands
```t
# Review the terraform plan
terraform plan 

# Create Resources 
terraform apply -auto-approve

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate
Observation: New version of terraform.tfstate file will be created
```


## Step-06: Destroy Resources
- Destroy Resources and Verify Bucket Versioning
```t
# Destroy Resources
terraform destroy -auto-approve
```
## Step-07: Terraform State Locking Introduction
- Understand about Terraform State Locking Advantages

## Step-08: Add State Locking Feature using DynamoDB Table
- Create Dynamo DB Table
  - **Table Name:** terraform-dev-state-table
  - **Partition key (Primary Key):** LockID (Type as String)
  - **Table settings:** Use default settings (checked)
  - Click on **Create**

## Step-09: Update DynamoDB Table entry in backend in c1-versions.tf
- Enable State Locking in `c1-versions.tf`
```t
  # Adding Backend as S3 for Remote State Storage with State Locking
  backend "s3" {
    bucket = "terraform-stacksimplify"
    key    = "dev2/terraform.tfstate"
    region = "us-east-1"  

    # For State Locking
    dynamodb_table = "terraform-dev-state-table"
  }
```

## Step-10: Execute Terraform Commands
```t
# Initialize Terraform 
terraform init

# Review the terraform plan
terraform plan 
Observation: 
1) Below messages displayed at start and end of command
Acquiring state lock. This may take a few moments...
Releasing state lock. This may take a few moments...
2) Verify DynamoDB Table -> Items tab

# Create Resources 
terraform apply -auto-approve

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev2/terraform.tfstate
Observation: New version of terraform.tfstate file will be created in dev2 folder
```

## Step-11: Destroy Resources
- Destroy Resources and Verify Bucket Versioning
```t
# Destroy Resources
terraform destroy -auto-approve

# Clean-Up Files
rm -rf .terraform*
rm -rf terraform.tfstate*  # This step not needed as e are using remote state storage here
```

## References 
- [AWS S3 Backend](https://www.terraform.io/docs/language/settings/backends/s3.html)
- [Terraform Backends](https://www.terraform.io/docs/language/settings/backends/index.html)
- [Terraform State Storage](https://www.terraform.io/docs/language/state/backends.html)
- [Terraform State Locking](https://www.terraform.io/docs/language/state/locking.html)
- [Remote Backends - Enhanced](https://www.terraform.io/docs/language/settings/backends/remote.html)======================================================================
***********read the file ./07-Terraform-State/07-02-Terraform-State-Commands/README.md ************
# Terraform State Commands

## Step-01: Introduction
- Terraform Commands
  - terraform show
  - terraform refresh
  - terraform plan (internally calls refresh)
  - terraform state
  - terraform force-unlock   
  - terraform taint
  - terraform untaint
  - terraform apply target command  

## Pre-requisite: c1-versions.tf
- Update your backend block with your bucket name, key and region
- Also update your dynamodb table name
```t
# Terraform Block
terraform {
  required_version = "~> 0.14" # which means any version equal & above 0.14 like 0.15, 0.16 etc and < 1.xx
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
  # Adding Backend as S3 for Remote State Storage
  backend "s3" {
    bucket = "terraform-stacksimplify"
    key    = "statecommands/terraform.tfstate"
    region = "us-east-1" 

    # Enable during Step-09     
    # For State Locking
    dynamodb_table = "terraform-dev-state-table"    
    
  }
}
```  

## Step-02: Terraform Show Command to review Terraform plan files
- The `terraform show` command is used to provide human-readable output from a state or plan file. 
- This can be used to inspect a plan to ensure that the planned operations are expected, or to inspect the current state as
- Terraform plan output files are binary files. We can read them using `terraform show` command
```t
# Initialize Terraform
terraform init

# Create Plan 
terraform plan -out=v1plan.out

# Read the plan 
terraform show v1plan.out

# Read the plan in json format
terraform show -json v1plan.out
```

## Step-03: Terraform Show command to Read State files
- By default, in the working directory if we have `terraform.tfstate` file, when we provide the command `terraform show` it will read the state file automatically and display output.  
```t
# Terraform Show
terraform show
Observation: It should say "No State" because we will still didn't create any resources yet and no state file in current working directory

# Create Resources
terraform apply -auto-approve

# Terraform Show 
terraform show
Observation: It should display the state file
```

## Step-04: Understand terraform refresh in detail
- This commands comes under **Terraform Inspecting State**
- Understanding `terraform refresh` clears a lot of doubts in our mind and terraform state file and state feature
- The terraform refresh command is used to reconcile the state Terraform knows about (via its state file) with the real-world infrastructure. 
- This can be used to detect any drift from the last-known state, and to update the state file.
- This does not modify infrastructure, but does modify the state file. If the state is changed, this may cause changes to occur during the next plan or apply.
- **terraform refresh:** Update local state file against real resources in cloud
- **Desired State:** Local Terraform Manifest (All *.tf files)
- **Current State:**  Real Resources present in your cloud
- **Command Order of Execution:** refresh, plan, make a decision, apply
- Why? Lets understand that in detail about this order of execution
### Step-04-01: Add a new tag to EC2 Instance using AWS Management Console
```t
"demotag" = "refreshtest"
```

### Step-04-02: Execute terraform plan  
- You should observe no changes to local state file because plan does the comparison in memory 
- Verify S3 Bucket - no update to tfstate file about the change 
```t
# Execute Terraform plan
terraform plan 
```
### Step-04-03: Execute terraform refresh 
- You should see terraform state file updated with new demo tag
```
# Execute terraform refresh
terraform refresh

# Review terraform state file
1) terraform show
2) Also verify S3 bucket, new version of terraform.tfstate file will be created
```
### Step-04-04: Why you need to the execution in this order (refresh, plan, make a decision, apply) ?
- There are changes happened in your infra manually and not via terraform.
- Now decision to be made if you want those changes or not.
- **Choice-1:** If you dont want those changes proceed with terraform apply so manual changes we have done on our cloud EC2 Instance will be removed.
- **Choice-2:** If you want those changes, refer `terraform.tfstate` file about changes and embed them in your terraform manifests (example: c4-ec2-instance.tf) and proceed with flow (referesh, plan, review execution plan and apply)

### Step-04-05: I picked choice-2, so i will update the tags in c4-ec2-instance.tf
- Update in c4-ec2-instance.tf
```t
  tags = {
    "Name" = "amz-linux-vm"
    "demotag" = "refreshtest"
  }
```
### Step-04-06: Execute the commands to make our manual change official in terraform manifests and tfstate files perspective  
```t
# Terraform Refresh
terraform refresh

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve
```


## Step-05: Terraform State Command
### Step-05-01: Terraform State List and Show commands
- These two commands comes under **Terraform Inspecting State**
- **terraform state list:**  This command is used to list resources within a Terraform state.
- **terraform  state show:** This command is used to show the attributes of a single resource in the Terraform state.
```t
# List Resources from Terraform State
terraform state list

# Show the attributes of a single resource from Terraform State
terraform  state show data.aws_ami.amzlinux
terraform  state show aws_instance.my-ec2-vm
```
### Step-05-02: Terraform State mv command
- This commands comes under **Terraform Moving Resources**
- This command will move an item matched by the address given to the
 destination address. 
- This command can also move to a destination address
 in a completely different state file
- Very dangerous command
- Very advanced usage command
- Results will be unpredictable if concept is not clear about terraform state files mainly  desired state and current state.  
- Try this in production environments, only  when everything worked well in lower environments. 
```t
# Terraform List Resources
terraform state list

# Terraform State Move Resources to different name
terraform state mv -dry-run aws_instance.my-ec2-vm aws_instance.my-ec2-vm-new 
terraform state mv aws_instance.my-ec2-vm aws_instance.my-ec2-vm-new 
ls -lrta

Observation: 
1) It should create a backup file of terraform.tfstate as something like this "terraform.tfstate.1611929587.backup"
1) It renamed the name of "my-ec2-vm" in state file to "my-ec2-vm-new". 
2) Run terraform plan and observe what happens in next run of terraform plan and apply
-----------------------------
# WRONG APPROACH 
-----------------------------
# WRONG APPROACH OF MOVING TO TERRAFORM PLAN AND APPLY AFTER ABOVE CHANGE terraform state mv CHANGE
# WE NEED TO UPDATE EQUIVALENT RESOURCE in terraform manifests to match the same new name. 

# Terraform Plan
terraform plan
Observation: It will show "Plan: 1 to add, 0 to change, 1 to destroy."
1 to add: New EC2 Instance will be added
1 to destroy: Old EC2 instance will be destroyed

DON'T DO TERRAFORM APPLY because it shows make changes. Nothing changed other than state file local naming of a resource. Ideally nothing on current state (real cloud environment should not change due to this)
-----------------------------

-----------------------------
# RIGHT APPROACH
-----------------------------
Update "c3-ec2-instance.tf"
Before: resource "aws_instance" "my-ec2-vm" {
After: resource "aws_instance" "my-ec2-vm-new" {  

Update all references of this resources in your terraform manifests
Update c5-outputs.tf
Before: value = aws_instance.my-ec2-vm.public_ip
After: value = aws_instance.my-ec2-vm-new.public_ip

Before: value = aws_instance.my-ec2-vm.public_dns
After: value = aws_instance.my-ec2-vm-new.public_dns

Now run terraform plan and you should see no changes to Infra

# Terraform Plan
terraform plan
Observation: 
1) Message-1: No changes. Infrastructure is up-to-date
2) Message-2: This means that Terraform did not detect any differences between your
configuration and real physical resources that exist. As a result, no
actions need to be performed.
 
```
### Step-05-03: Terraform State rm command
- This commands comes under **Terraform Moving Resources**
- The `terraform state rm` command is used to remove items from the Terraform state. 
- This command can remove single resources, single instances of a resource, entire modules, and more.
```t
# Terraform List Resources
terraform state list

# Remove Resources from Terraform State
terraform state rm -dry-run aws_instance.my-ec2-vm-new
terraform state rm aws_instance.my-ec2-vm-new
Observation: 
1) Firstly takes backup of current state file before removing (example: terraform.tfstate.1611930284.backup)
2) Removes it from terraform.tfstate file

# Terraform Plan
terraform plan
Observation: It will tell you that resource is not in state file but same is present in your terraform manifests (03-ec2-instace.tf - DESIRED STATE). Do you want to re-create it?
This will re-create new EC2 instance excluding one created earlier and running

Make a  Choice
-------------
Choice-1: You want this resource to be running on cloud but should not be managed by terraform. Then remove its references in terraform manifests(DESIRED STATE). So that the one running in AWS cloud (current infra) this instance will be independent of terraform. 
Choice-2: You want a new resource to be created without deleting other one (non-terraform managed resource now in current state). Run terraform plan and apply
LIKE THIS WE NEED TO MAKE DECISIONS ON WHAT WOULD BE OUR OUTCOME OF REMOVING A RESOURCE FROM STATE.

PRIMARY REASON for this is command is that respective resource need to be removed from as terraform managed. 

# Run Terraform Plan
terraform plan

# Run Terraform Apply
terraform apply 
```

# Step-05-04: Terraform State replace-provider command
- This commands comes under **Terraform Moving Resources**
- [Terraform State Replace Provider](https://www.terraform.io/docs/cli/commands/state/replace-provider.html)


### Step-05-05: Terraform State pull / push command
- This command comes under **Terraform Disaster Recovery Concept**
- **terraform state pull:** 
  - The `terraform state pull` command is used to manually download and output the state from remote state.
  - This command also works with local state.
  - This command will download the state from its current location and output the raw format to stdout.
- **terraform state push:** The `terraform state push` command is used to manually upload a local state file to remote state. 

```t
# Other State Commands (Pull / Push)
terraform state pull
terraform state push
```

## Step-06: Terraform force-unlock command
- This command comes under **Terraform Disaster Recovery Concept**
- Manually unlock the state for the defined configuration.
- This will not modify your infrastructure. 
- This command removes the lock on the state for the current configuration. 
- The behavior of this lock is dependent on the backend (aws s3 with dynamodb for state locking etc) being used. 
- **Important Note:** Local state files cannot be unlocked by another process.
```t
# Manually Unlock the State
terraform force-unlock LOCK_ID
```

## Step-07: Terraform taint & untaint commands
-  These commands comes under **Terraform Forcing Re-creation of Resources**
- When a resource declaration is modified, Terraform usually attempts to update the existing resource in place (although some changes can require destruction and re-creation, usually due to upstream API limitations).
- **Example:** A virtual machine that configures itself with cloud-init on startup might no longer meet your needs if the cloud-init configuration changes.
- **terraform taint:** The `terraform taint` command manually marks a Terraform-managed resource as tainted, forcing it to be destroyed and recreated on the next apply.
- **terraform untaint:** 
  - The terraform untaint command manually unmarks a Terraform-managed resource as tainted, restoring it as the primary instance in the state. 
  - This reverses either a manual terraform taint or the result of provisioners failing on a resource.
  - This command will not modify infrastructure, but does modify the state file in order to unmark a resource as tainted.
```t
# List Resources from state
terraform state list

# Taint a Resource
terraform taint <RESOURCE_NAME_IN_TERRAFORM_LOCALLY>
terraform taint aws_instance.my-ec2-vm-new

# Terraform Plan
terraform plan
Observation: 
Message: "-/+ destroy and then create replacement"

# Untaint a Resource
terraform untaint <RESOURCE_NAME_IN_TERRAFORM_LOCALLY>
terraform untaint aws_instance.my-ec2-vm-new

# Terraform Plan
terraform plan
Observation: 
Message: "No changes. Infrastructure is up-to-date."
```


## Step-08: Terraform Resource Targeting - Plan, Apply (-target) Option
- The `-target` option can be used to focus Terraform's attention on only a subset of resources. 
- [Terraform Resource Targeting](https://www.terraform.io/docs/cli/commands/plan.html#resource-targeting)
- This targeting capability is provided for exceptional circumstances, such as recovering from mistakes or working around Terraform limitations.
-  It is not recommended to use `-target` for routine operations, since this can lead to undetected configuration drift and confusion about how the true state of resources relates to configuration.
- Instead of using `-target` as a means to operate on isolated portions of very large configurations, prefer instead to break large configurations into several smaller configurations that can each be independently applied.
```t
# Lets make two changes
Change-1: Add new tag in c4-ec2-instance.tf
    "target" = "Target-Test-1"
Change-2: Add additional inbound rule in "vpc-web" security group for port 8080
  ingress {
    description = "Allow Port 8080"
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
Change-3: Add new EC2 Resource  
# New VM
resource "aws_instance" "my-demo-vm" {
  ami           = data.aws_ami.amzlinux.id 
  instance_type = var.instance_type
  tags = {
    "Name" = "demo-vm1"
  }
}

# List Resources from state
terraform state list

# Terraform plan
terraform plan -target=aws_instance.my-ec2-vm-new
Observation:
0) Message: "Plan: 0 to add, 2 to change, 0 to destroy."
1) It is updating Change-1 because we are targeting that resource "aws_instance.my-ec2-vm-new"
2) It is updating change-2 "vpc-web" because its a dependent resource for "aws_instance.my-ec2-vm-new"
3) It is not touching the new resource which we are creating now. It will be in terraform configuration but not getting provisioned when we are using -target

# Terraform Apply
terraform apply -target=aws_instance.my-ec2-vm-new

```

## Step-09: Terraform Destroy & Clean-Up
```t
# Destory Resources
terraform destroy -auto-approve

# Clean-Up Files
rm -rf .terraform*
rm -rf v1plan.out

# Kalyan - Not to forgot to change these things after Recording
# Ensure below two lines are in commented state in c4-ec2-instance.tf
1) This is required for students demo for this entire section to implement in a step by step manner from beginning
    #"demotag" = "refreshtest"  # Enable during Step-04-05
    # "target" = "Target-Test-1" # Enable during step-08
2) Rename the local name for EC2 Instance from "my-ec2-vm-new" to "my-ec2-vm"
Changed during step 05-02: aws_instance.my-ec2-vm-new
Roll back for the students to have a seamless demo

3) Roll back changes you have made in step-08
3.1: New tag in EC2 Instance
3.2: New rule in vpc-web security group
3.3: New EC2 Instance resource in c4-ec2-instance.tf
```

## References
- [Terraform State Command](https://www.terraform.io/docs/cli/commands/state/index.html)
- [Terraform Inspect State](https://www.terraform.io/docs/cli/state/inspect.html)
- [Terraform Moving Resources](https://www.terraform.io/docs/cli/state/move.html)
- [Terraform Disaster Recovery](https://www.terraform.io/docs/cli/state/recover.html)
- [Terraform Taint](https://www.terraform.io/docs/cli/state/taint.html)
- [Terraform State](https://www.terraform.io/docs/language/state/index.html)
- [Manipulating Terraform State](https://www.terraform.io/docs/cli/state/index.html)
- [Additional Reference](https://www.hashicorp.com/blog/detecting-and-managing-drift-with-terraform)
======================================================================
***********read the file ./08-Terraform-Workspaces/README.md ************
# Terraform Workspaces

## Step-01: Introduction
- We are going to create 2 more workspaces (dev,qa) in addition to default workspace
- Update our terraform manifests to support `terraform workspace` 
  - Primarily for security group name to be unique for each workspace
  - In the same way for EC2 VM Instance Name tag. 
- Master the below listed `terraform workspace` commands
  - terraform workspace show
  - terraform workspace list
  - terraform workspace new
  - terraform workspace select
  - terraform workspace delete


## Step-02: Update terraform manifests to support multiple workspaces
- **Sub-folder we are working on:** v1-local-backend
- Ideally, AWS don't allow to create a security group with same name twice. 
- With that said, we need to change our security group names in our `c2-security-groups.tf`
- At the same time, just for reading convenience we can also have our EC2 Instance `Name tag` also updated inline with workspace name. 
- What is **${terraform.workspace}**? - It will get the workspace name 
- **Popular Usage-1:** Using the workspace name as part of naming or tagging behavior
- **Popular Usage-2:** Referencing the current workspace is useful for changing behavior based on the workspace. For example, for non-default workspaces, it may be useful to spin up smaller cluster sizes.
```t
# Change-1: Security Group Names
  name        = "vpc-ssh-${terraform.workspace}"
  name        = "vpc-web-${terraform.workspace}"  

# Change-2: For non-default workspaces, it may be useful to spin up smaller cluster sizes.
  count = terraform.workspace == "default" ? 2 : 1  
This will create 2 instances if we are in default workspace and in any other workspaces it will create 1 instance

# Change-3: EC2 Instance Name tag
    "Name" = "vm-${terraform.workspace}-${count.index}"

# Change-4: Outputs
  value = aws_instance.my-ec2-vm.*.public_ip
  value = aws_instance.my-ec2-vm.*.public_dns    
You can create a list of all of the values of a given attribute for the items in the collection with a star. For instance, aws_instance.my-ec2-vm.*.id will be a list of all of the Public IP of the instances.  
```

## Step-03: Create resources in default workspaces
- Default Workspace: Every initialized working directory has at least one workspace. 
- If you haven't created other workspaces, it is a workspace named **default**
- For a given working directory, only one workspace can be selected at a time.
- Most Terraform commands (including provisioning and state manipulation commands) only interact with the currently selected workspace.
```t
# Terraform Init
terraform init 

# List Workspaces
terraform workspace list

# Output Current Workspace using show
terraform workspace show

# Terraform Plan
terraform plan
Observation: This should show us two instances based on the statement in EC2 Instance Resource "count = terraform.workspace == "default" ? 2 : 1" because we are creating this in default workspace

# Terraform Apply
terraform apply -auto-approve

# Verify
Verify the same in AWS Management console
Observation: 
1) Two instances should be created with name as "vm-default-0, vm-default-1")
2) Security Groups should be created with names as "vpc-ssh-default, vpc-web-default)
3) Observe the outputs on CLI, you should see list of Public IP and Public DNS 
```

## Step-04: Create New Workspace and Provision Infra 
```t
# Create New Workspace
terraform workspace new dev

# Verify the folder
cd terraform.tfstate.d 
cd dev
ls
cd ../../

# Terraform Plan
terraform plan
Observation:  This should show us creating only 1 instance based on statement "count = terraform.workspace == "default" ? 2 : 1" as we are creating this in non-default workspace named dev

# Terraform Apply
terraform apply -auto-approve

# Verify Dev Workspace statefile
cd terraform.tfstate.d/dev
ls
cd ../../
Observation: You should fine "terraform.tfstate" in "current-working-directory/terraform.tfstate.d/dev" folder

# Verify EC2 Instance in AWS mgmt console
Observation:
1) Name should be with "vm-dev-0"
2) Security Group names should be as "vpc-ssh-dev, vpc-web-dev"
```

## Step-05: Switch workspace and destroy resources
- Switch workspace from dev to default and destroy resources in default workspace
```t
# Show current workspace
terraform workspace show

# List Worksapces
terraform workspace list

# Workspace select
terraform workspace select default

# Delete Resources from default workspace
terraform destroy 

# Verify
1) Verify in AWS Mgmt Console (both instances and security groups should be deleted)
```

## Step-06: Delete dev workspace
- We cannot delete "default" workspace
- We can delete workspaces which we created (dev, qa etc)
```t
# Delete Dev Workspace
terraform workspace delete dev
Observation: Workspace "dev" is not empty.
Deleting "dev" can result in dangling resources: resources that
exist but are no longer manageable by Terraform. Please destroy
these resources first.  If you want to delete this workspace
anyway and risk dangling resources, use the '-force' flag.

# Switch to Dev Workspace
terraform workspace select dev

# Destroy Resources
terraform destroy -auto-approve

# Delete Dev Workspace
terraform workspace delete dev
Observation:
Workspace "dev" is your active workspace.
You cannot delete the currently active workspace. Please switch
to another workspace and try again.

# Switch Workspace to default
terraform workspace select default

# Delete Dev Workspace
terraform workspace delete dev
Observation: Successfully delete workspace dev

# Verify 
In AWS mgmt console, all EC2 instances should be deleted
```

## Step-07: Clean-Up Local folder
```t
# Clean-Up local folder
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-08: Terraform Workspaces in combination with Terraform Backend (Remote State Storage)
### Step-08-01: Review terraform manifest (primarily c1-versions.tf)
- **Reference Sub-Folder:** v2-remote-backend
- Only change in the template is in **c1-versions.tf**, we will have the remote backend block which we learned during section **07-01-Terraform-Remote-State-Storage-and-Locking**
```t
  # Adding Backend as S3 for Remote State Storage
  backend "s3" {
    bucket = "terraform-stacksimplify"
    key    = "workspaces/terraform.tfstate"
    region = "us-east-1"  
  # For State Locking
    dynamodb_table = "terraform-dev-state-table"           
  }
```
### Step-08-02: Provision infra using default workspace
```t
# Initialize Terraform
terraform init

# List Terraform Workspaces
terraform workspace list

# Show current Terraform workspace
terraform workspace show

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Review State file in S3 Bucket for default workspace
Go to AWS Mgmt Console -> Services -> S3 -> terraform-stacksimplify -> workspaces -> terraform.tfstate
```
### Step-08-03: Create new workspace dev and provison infra using that workspace
```t
# List Terraform Workspaces
terraform workspace list

# Create New Terraform Workspace
terraform workspace new dev

# List Terraform Workspaces
terraform workspace list

# Show current Terraform workspace
terraform workspace show

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Review State file in S3 Bucket for dev workspace
Go to AWS Mgmt Console -> Services -> S3 -> terraform-stacksimplify -> env:/ -> dev -> workspaces -> terraform.tfstate
```

### Step-08-04: Destroy resources in both workspaces (default, dev)

```t
# Show current Terraform workspace
terraform workspace show

# Destroy Resources in Dev Workspace
terraform destroy -auto-approve

# Show current Terraform workspace
terraform workspace show

# Select other workspace
terraform workspace select default

# Show current Terraform workspace
terraform workspace show

# Destroy Resources in default Workspace
terraform destroy -auto-approve

# Delete Dev Workspace
terraform workspace delete dev
```
### Step-08-05: Try deleting default workspace and understand what happens

```t
# Try deleting default workspace
terraform workspace delete default
Observation: 
1) Workspace "default" is your active workspace.
2) You cannot delete the currently active workspace. Please switch
to another workspace and try again.

# Create new workspace
terraform workspace new test1

# Show current Terraform workspace
terraform workspace show

# Try deleting default workspace now
terraform workspace delete default
Observation:
1) can't delete default state
```

### Step-08-06: Clean-Up
```t
# Switch workspace to default
terraform workspace select default

# Delete test1 workspace
terraform workspace delete test1

# Clean-Up Terraform local folders
rm -rf .terraform*

# Clean-Up State files in S3 bucket 
Go to S3 Bucket and delete files
```


## References
- [Terraform Workspaces](https://www.terraform.io/docs/language/state/workspaces.html)
- [Managing Workspaces](https://www.terraform.io/docs/cli/workspaces/index.html)======================================================================
***********read the file ./09-Terraform-Provisioners/09-01-File-Provisioner/README.md ************
# Terraform Provisioners

## Step-00: Provisioner Concepts
- Generic Provisioners
  - file
  - local-exec
  - remote-exec
- Provisioner Timings
  - Creation-Time Provisioners (by default)
  - Destroy-Time Provisioners  
- Provisioner Failure Behavior
  - continue
  - fail
- Provisioner Connections
- Provisioner Without a Resource  (Null Resource)

## Pre-requisites
- Create a EC2 Key pain with name `terraform-key` and copy the `terraform-key.pem` file in the folder `private-key` in `terraform-manifest` folder
- Connection Block for provisioners uses this to connect to newly created EC2 instance to copy files using `file provisioner`, execute scripts using `remote-exec provisioner`

## Step-01: Introduction
- Understand about File Provisioners
- Create Provisioner Connections block required for File Provisioners
- We will also discuss about **Creation-Time Provisioners (by default)**
- Understand about Provisioner Failure Behavior
  - continue
  - fail
- Discuss about Destroy-Time Provisioners    


## Step-02: File Provisioner & Connection Block
- **Referencec Sub-Folder:** terraform-manifests
- Understand about file provisioner & Connection Block
- **Connection Block**
  - We can have connection block inside resource block for all provisioners 
  -[or] We can have connection block inside a provisioner block for that respective provisioner
- **Self Object**
  - **Important Technical Note:** Resource references are restricted here because references create dependencies. Referring to a resource by name within its own block would create a dependency cycle.
  - Expressions in provisioner blocks cannot refer to their parent resource by name. Instead, they can use the special **self object.**
  - The **self object** represents the provisioner's parent resource, and has all of that resource's attributes. 
```t
  # Connection Block for Provisioners to connect to EC2 Instance
  connection {
    type = "ssh"
    host = self.public_ip # Understand what is "self"
    user = "ec2-user"
    password = ""
    private_key = file("private-key/terraform-key.pem")
  }  
```

## Step-03: Create multiple provisioners of various types
- **Creation-Time Provisioners:** 
- By default, provisioners run when the resource they are defined within is created. 
- Creation-time provisioners are only run during creation, not during updating or any other lifecycle. 
- They are meant as a means to perform bootstrapping of a system.
- If a creation-time provisioner fails, the resource is marked as tainted. 
- A tainted resource will be planned for destruction and recreation upon the next terraform apply.
- Terraform does this because a failed provisioner can leave a resource in a semi-configured state. 
- Because Terraform cannot reason about what the provisioner does, the only way to ensure proper creation of a resource is to recreate it. This is tainting.
- You can change this behavior by setting the on_failure attribute, which is covered in detail below.

```t

 # Copies the file-copy.html file to /tmp/file-copy.html
  provisioner "file" {
    source      = "apps/file-copy.html"
    destination = "/tmp/file-copy.html"
  }

  # Copies the string in content into /tmp/file.log
  provisioner "file" {
    content     = "ami used: ${self.ami}" # Understand what is "self"
    destination = "/tmp/file.log"
  }

  # Copies the app1 folder to /tmp - FOLDER COPY
  provisioner "file" {
    source      = "apps/app1"
    destination = "/tmp"
  }

  # Copies all files and folders in apps/app2 to /tmp - CONTENTS of FOLDER WILL BE COPIED
  provisioner "file" {
    source      = "apps/app2/" # when "/" at the end is added - CONTENTS of FOLDER WILL BE COPIED
    destination = "/tmp"
  }
 # Copies the file-copy.html file to /tmp/file-copy.html
  provisioner "file" {
    source      = "apps/file-copy.html"
    destination = "/tmp/file-copy.html"
  }

  # Copies the string in content into /tmp/file.log
  provisioner "file" {
    content     = "ami used: ${self.ami}" # Understand what is "self"
    destination = "/tmp/file.log"
  }

  # Copies the app1 folder to /tmp - FOLDER COPY
  provisioner "file" {
    source      = "apps/app1"
    destination = "/tmp"
  }

  # Copies all files and folders in apps/app2 to /tmp - CONTENTS of FOLDER WILL BE COPIED
  provisioner "file" {
    source      = "apps/app2/" # when "/" at the end is added - CONTENTS of FOLDER WILL BE COPIED
    destination = "/tmp"
  }
```

## Step-04: Create Resources using Terraform commands

```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify - Login to EC2 Instance
chmod 400 private-key/terraform-key.pem 
ssh -i private-key/terraform-key.pem ec2-user@IP_ADDRESSS_OF_YOUR_VM
ssh -i private-key/terraform-key.pem ec2-user@54.197.54.126
Verify /tmp for all files copied
cd /tmp
ls -lrta /tmp

# Clean-up
terraform destroy -auto-approve
rm -rf terraform.tfsate*
```

## Step-05: Failure Behavior: Understand Decision making when provisioner fails (continue / fail)
- By default, provisioners that fail will also cause the Terraform apply itself to fail. The on_failure setting can be used to change this. The allowed values are:
  - **continue:** Ignore the error and continue with creation or destruction.
  - **fail:** (Default Behavior) Raise an error and stop applying (the default behavior). If this is a creation provisioner, taint the resource.  
- Try copying a file to Apache static content folder "/var/www/html" using file-provisioner
- This will fail because, the user you are using to copy these files is "ec2-user" for amazon linux vm. This user don't have access to folder "/var/www/html/" top copy files. 
- We need to use sudo to do that. 
- All we know is we cannot copy it directly, but we know we have already copied this file in "/tmp" using file provisioner
- **Try two scenarios**
  - No `on_failure` attribute (Same as `on_failure = fail`) - default what happens It will Raise an error and stop applying. If this is a creation provisioner, it will taint the resource.
  - When `on_failure = continue`, will continue creating resources
  - **Verify:**  Verify `terraform.tfstate` for  `"status": "tainted"`
```t
# Test-1: Without on_failure attribute which will fail terraform apply
 # Copies the file-copy.html file to /var/www/html/file-copy.html
  provisioner "file" {
    source      = "apps/file-copy.html"
    destination = "/var/www/html/file-copy.html"
   }
###### Verify:  Verify terraform.tfstate for  "status": "tainted"

# Test-2: With on_failure = continue
 # Copies the file-copy.html file to /var/www/html/file-copy.html
  provisioner "file" {
    source      = "apps/file-copy.html"
    destination = "/var/www/html/file-copy.html"
    on_failure  = continue 
   }
###### Verify:  Verify terraform.tfstate for  "status": "tainted"  
```
```t

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify
Login to EC2 Instance
Verify /tmp, /var/www/html for all files copied
```

## Step-06: Clean-Up Resources & local working directory
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-07: Destroy Time Provisioners
- Discuss about this concept
- [Destroy Time Provisioners](https://www.terraform.io/docs/language/resources/provisioners/syntax.html#destroy-time-provisioners)
- Inside a provisioner when you add this statement `when    = destroy` it will provision this during the resource destroy time
```t
resource "aws_instance" "web" {
  # ...

  provisioner "local-exec" {
    when    = destroy 
    command = "echo 'Destroy-time provisioner'"
  }
}
```======================================================================
***********read the file ./09-Terraform-Provisioners/09-02-remote-exec-provisioner/README.md ************
# Terraform remote-exec Provisioner

## Step-00: Pre-requisites
- Create a EC2 Key pain with name `terraform-key` and copy the `terraform-key.pem` file in the folder `private-key` in `terraform-manifest` folder
- Connection Block for provisioners uses this to connect to newly created EC2 instance to copy files using `file provisioner`, execute scripts using `remote-exec provisioner`

## Step-01: Introduction
- Understand about **remote-exec** Provisioner
- The `remote-exec` provisioner invokes a script on a remote resource after it is created. 
- This can be used to run a configuration management tool, bootstrap into a cluster, etc. 

## Step-02: Create / Review Provisioner configuration
- **Usecase:** 
1. We will copy a file named `file-copy.html` using `File Provisioner` to "/tmp" directory
2. Using `remote-exec provisioner`, using linux commands we will in-turn copy the file to Apache Webserver static content directory `/var/www/html` and access it via browser once it is provisioned
```t
 # Copies the file-copy.html file to /tmp/file-copy.html
  provisioner "file" {
    source      = "apps/file-copy.html"
    destination = "/tmp/file-copy.html"
  }

# Copies the file to Apache Webserver /var/www/html directory
  provisioner "remote-exec" {
    inline = [
      "sleep 120",  # Will sleep for 120 seconds to ensure Apache webserver is provisioned using user_data
      "sudo cp /tmp/file-copy.html /var/www/html"
    ]
  }
```

## Step-03: Review Terraform manifests & Execute Terraform Commands
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify
1) Login to EC2 Instance
chmod 400 private-key/terraform-key.pem 
ssh -i private-key/terraform-key.pem ec2-user@IP_ADDRESSS_OF_YOUR_VM
ssh -i private-key/terraform-key.pem ec2-user@54.197.54.126

2) Verify /tmp for file named file-copy.html all files copied (ls -lrt /tmp/file-copy.html)
3) Verify /var/www/html for a file named file-copy.html (ls -lrt /var/www/html/file-copy.html)
4) Access via browser http://<Public-IP>/file-copy.html
```
## Step-04: Clean-Up Resources & local working directory
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

======================================================================
***********read the file ./09-Terraform-Provisioners/09-03-local-exec-provisioner/README.md ************
# Terraform local-exec Provisioner

## Step-00: Pre-requisites
- Create a EC2 Key pain with name `terraform-key` and copy the `terraform-key.pem` file in the folder `private-key` in `terraform-manifest` folder
- Connection Block for provisioners uses this to connect to newly created EC2 instance to copy files using `file provisioner`, execute scripts using `remote-exec provisioner`

## Step-01: Introduction
- Understand about **local-exec** Provisioner
- The `local-exec` provisioner invokes a local executable after a resource is created. 
- This invokes a process on the machine running Terraform, not on the resource. 

## Step-02: Review local-exec provisioner code
- We will create one provisioner during creation-time. It will output private ip of the instance in to a file named `creation-time-private-ip.txt`
- We will create one more provisioner during destroy time. It will output destroy time with date in to a file named `destroy-time.txt`
- **C3-ec2-instance.tf**
```t
  # local-exec provisioner (Creation-Time Provisioner - Triggered during Create Resource)
  provisioner "local-exec" {
    command = "echo ${aws_instance.my-ec2-vm.private_ip} >> creation-time-private-ip.txt"
    working_dir = "local-exec-output-files/"
    #on_failure = continue
  }

  # local-exec provisioner - (Destroy-Time Provisioner - Triggered during Destroy Resource)
  provisioner "local-exec" {
    when    = destroy
    command = "echo Destroy-time provisioner Instanace Destroyed at `date` >> destroy-time.txt"
    working_dir = "local-exec-output-files/"
  }  
```


## Step-03: Review Terraform manifests & Execute Terraform Commands
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify
Verify the file in folder "local-exe-output-files/creation-time-private-ip.txt"

```
## Step-04: Clean-Up Resources & local working directory
```t
# Terraform Destroy
terraform destroy -auto-approve

# Verify
Verify the file in folder "local-exe-output-files/destroy-time.txt"

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

======================================================================
***********read the file ./09-Terraform-Provisioners/09-04-Null-Resource/README.md ************
# Terraform Null Resource

## Step-01: Introduction
- Understand about [Null Provider](https://registry.terraform.io/providers/hashicorp/null/latest/docs)
- Understand about [Null Resource](https://www.terraform.io/docs/language/resources/provisioners/null_resource.html)
- Understand about [Time Provider](https://registry.terraform.io/providers/hashicorp/time/latest/docs)
- **Usecase:** Force a resource to update based on a changed null_resource
- Create `time_sleep` resource to wait for 90 seconds after EC2 Instance creation
- Create Null resource with required provisioners
  - File Provisioner: Copy apps/app1 folder to /tmp
  - Remote Exec Provisioner: Copy app1 folder from /tmp to /var/www/htnl
- Over the process we will learn about
  - null_resource
  - time_sleep resource
  - We will also learn how to Force a resource to update based on a changed null_resource using `timestamp function` and `triggers` in `null_resource`


## Step-02: Define null provider in Terraform Settings Block
- Update null provider info listed below in **c1-versions.tf**
```t
    null = {
      source = "hashicorp/null"
      version = "~> 3.0.0"
    }
```

## Step-03: Define Time Provider in Terraform Settings Block
- Update time provider info listed below in **c1-versions.tf**
```t
    time = {
      source = "hashicorp/time"
      version = "~> 0.6.0"
    }  
```

## Step-04: Create / Review the c4-ec2-instance.tf terraform configuration
### Step-04-01: Create Time Sleep Resource
- This resource will wait for 90 seconds after EC2 Instance creation.
- This wait time will give EC2 Instance to provision the Apache Webserver and create all its relevant folders
- Primarily if we want to copy static content we need Apache webserver static folder `/var/www/html`
```t
# Wait for 90 seconds after creating the above EC2 Instance 
resource "time_sleep" "wait_90_seconds" {
  depends_on = [aws_instance.my-ec2-vm]
  create_duration = "90s"
}
```
### Step-04-02: Create Null Resource
- Create Null resource with `triggers` with `timestamp()` function which will trigger for every `terraform apply`
- This `Null Resource` will help us to sync the static content from our local folder to EC2 Instnace as and when required.
- Also only changes will applied using only `null_resource` when `terraform apply` is run. In other words, when static content changes, how will we sync those changes to EC2 Instance using terraform - This is one simple solution.
- Primarily the focus here is to learn the following
  - null_resource
  - null_resource trigger
  - How trigger works based on timestamp() function ?
  - Provisioners in Null Resource
```t
# Sync App1 Static Content to Webserver using Provisioners
resource "null_resource" "sync_app1_static" {
  depends_on = [ time_sleep.wait_90_seconds ]
  triggers = {
    always-update =  timestamp()
  }

  # Connection Block for Provisioners to connect to EC2 Instance
  connection {
    type = "ssh"
    host = aws_instance.my-ec2-vm.public_ip 
    user = "ec2-user"
    password = ""
    private_key = file("private-key/terraform-key.pem")
  }  

 # Copies the app1 folder to /tmp
  provisioner "file" {
    source      = "apps/app1"
    destination = "/tmp"
  }

# Copies the /tmp/app1 folder to Apache Webserver /var/www/html directory
  provisioner "remote-exec" {
    inline = [
      "sudo cp -r /tmp/app1 /var/www/html"
    ]
  }
}
```

## Step-05: Execute Terraform Commands
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify
ssh -i private-key/terraform-key.pem ec2-user@<PUBLIC-IP>
ls -lrt /tmp
ls -lrt /tmp/app1
ls -lrt /var/www/html
ls -lrt /var/www/html/app1
http://<public-ip>/app1/file1.html
http://<public-ip>/app1/file2.html
```

## Step-06: Create new file locally in app1 folder
- Create a new file named `file3.html`
- Also updated `file1.html` with some additional info
- **file3.html**
```html
<h1>>App1 File3</h1
```
- **file1.html**
```html
<h1>>App1 File1 - Updated</h1
```

## Step-07: Execute Terraform plan and apply commands
```t
# Terraform Plan
terraform plan
Observation: You should see changes for "null_resource.sync_app1_static" because trigger will have new timestamp when you fired the terraform plan command

# Terraform Apply
terraform apply -auto-approve

# Verify
chmod 400 private-key/terraform-key.pem
ssh -i private-key/terraform-key.pem ec2-user@<PUBLIC-IP>
ls -lrt /tmp
ls -lrt /tmp/app1
ls -lrt /var/www/html
ls -lrt /var/www/html/app1
http://<public-ip>/app1/file1.html
http://<public-ip>/app1/file3.html
```

## Step-08: Clean-Up Resources & local working directory
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```



## References
- [Resource: time_sleep](https://registry.terraform.io/providers/hashicorp/time/latest/docs/resources/sleep)
- [Time Provider](https://registry.terraform.io/providers/hashicorp/time/latest/docs)
======================================================================
***********read the file ./10-Terraform-Modules/10-01-Terraform-Modules-Basics/README.md ************
# Terraform Module Basics

## Step-01: Introduction
1. Introduction - Module Basics  
  - Root Module
  - Child Module
  - Published Modules (Terraform Registry)

2. Module Basics 
  - Defining a Child Module
    - Source (Mandatory)
    - Version
    - Meta-arguments (count, for_each, providers, depends_on, )
    - Accessing Module Output Values
    - Tainting resources within a module

## Step-02: Defining a Child Module
- We need to understand about the following
  - Module Source (Mandatory): To start with we will use Terraform Registry
  - Module Version (Optional): Recommended to use module version
- Create a simple EC2 Instance module
  - c1-versions.tf: standard
  - c2-variables.tf: standard
  - c3-ami-datasource.tf: standard
  - c4-ec2instance-module.tf: We will focus on building this template  
```t
# AWS EC2 Instance Module

module "ec2_cluster" {
  source                 = "terraform-aws-modules/ec2-instance/aws"
  version                = "~> 2.0"

  name                   = "my-modules-demo"
  instance_count         = 2

  ami                    = data.aws_ami.amzlinux.id
  instance_type          = "t2.micro"
  key_name               = "terraform-key"
  monitoring             = true
  vpc_security_group_ids = ["sg-08b25c5a5bf489ffa"]  # Get Default VPC Security Group ID and replace
  subnet_id              = "subnet-4ee95470" # Get one public subnet id from default vpc and replace
  user_data               = file("apache-install.sh")

  tags = {
    Name        = "Modules-Demo"
    Terraform   = "true"
    Environment = "dev"
  }
}
```

## Step-03: Define Outputs from a EC2 Instance Module
- c5-outputs.tf: We will output the EC2 Instance Module attributes (Public DNS and Public IP)
```t
# Output variable definitions

output "ec2_instance_public_ip" {
  description = "Public IP addresses of EC2 instances"
  value       = module.ec2_cluster.*.public_ip
}

output "ec2_instance_public_dns" {
  description = "Public IP addresses of EC2 instances"
  value       = module.ec2_cluster.*.public_dns
}
```

## Step-04: Execute Terraform Commands
```t
# Terraform Init
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-apporve

# Verify 
1) Verify in AWS management console , if EC2 Instances got created.
2) Update default security group of a VPC to allow port 80 from internet
3) Access Apache Webserver
http://<Public-IP-VM1>
http://<Public-IP-VM2>
```

## Step-05: Tainting Resources in a Module
- The **taint command** can be used to taint specific resources within a module
- **Very Very Important Note:** It is not possible to taint an entire module. Instead, each resource within the module must be tainted separately.
```t
# List Resources from State
terraform state list

# Taint a Resource
terraform taint <RESOURCE-NAME>
terraform taint module.ec2_cluster.aws_instance.this[0]

# Terraform Plan
terraform plan
Observation: First VM will be destroyed and re-created

# Terraform Apply
terraform apply -auto-approve
```

## Step-06: Clean-Up Resources & local working directory
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-07: Meta-Arguments for Modules
- Meta-Argument concepts are going to be same as how we learned during Resources section.
  - count
  - for_each
  - providers
  - depends_on
- [Meta-Arguments for Modules](https://www.terraform.io/docs/language/modules/syntax.html#meta-arguments)


## Step-08: Upgrade Terraform Module from v2.x to v5.x
- **Terraform Manifests Folder:** terraform-manifests-upgraded
- **File:** c4-ec2instance-module.tf
```t
# AWS EC2 Instance Module
module "ec2_cluster" {
  source                 = "terraform-aws-modules/ec2-instance/aws"
  version                = "~> 5.0"

  name                   = "my-modules-demo"

  ami                    = data.aws_ami.amzlinux.id 
  instance_type          = "t2.micro"
  key_name               = "terraform-key"
  monitoring             = true
  vpc_security_group_ids = ["sg-b8406afc"] # Get Default VPC Security Group ID and replace
  subnet_id              = "subnet-4ee95470" # Get one public subnet id from default vpc and replace
  user_data              = file("apache-install.sh") 

# Module Upgrade from v2.x to v5.x 
## In v2.x module, Meta-argument count is used
## In v5.x module, Meta-argument for_each is used
  #instance_count         = 2
  for_each = toset(["one", "two", "three"])

  tags = {
    Name        = "Modules-Demo-${each.key}"
    Terraform   = "true"
    Environment = "dev"
  }
}
```

## Step-09: Update Terraform EC2 Instance module outputs to support latest version upgrade changes
- **Terraform Manifests Folder:** terraform-manifests-upgraded
- **File:** c5-outputs.tf
```t
# Output variable definitions

output "ec2_instance_public_ip" {
  description = "Public IP Addressess of EC2 Instances"
  #value = module.ec2_cluster.*.public_ip
  value = [for ec2_cluster in module.ec2_cluster: ec2_cluster.id ]   
}

output "ec2_instance_public_dns" {
  description = "Public DNS for EC2 Instances"
  #value = module.ec2_cluster.*.public_dns
  value = [for ec2_cluster in module.ec2_cluster: ec2_cluster.public_dns ] 
}

output "ec2_instance_private_ip" {
  description = "Private IP Addresses for EC2 Instances"
  #value = module.ec2_cluster.*.private_ip
  value = [for ec2_cluster in module.ec2_cluster: ec2_cluster.private_ip ] 
}
```

## Step-10: Execute Terraform Commands and Verify
- **Terraform Manifests Folder:** terraform-manifests-upgraded
```t
# Terraform Initialize
cd terraform-manifests-upgraded
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify Application
1. Verify EC2 Instances
2. Verify Application installed (Access using browser with EC2 Instance Public IP)
http://<EC2-Instance-Public-IP>
```


## References
- [Terraform EC2 Instance Module](https://registry.terraform.io/modules/terraform-aws-modules/ec2-instance/aws/latest)======================================================================
***********read the file ./10-Terraform-Modules/10-02-Terraform-Build-a-Module/Oldv1-backup/v3-build-a-module-to-host-static-website-on-aws-s3/modules/aws-s3-static-website-bucket/README.md ************
# AWS S3 static website bucket
- This module provisions AWS S3 buckets configured for static website hosting.
- This will be a demo S3 module

======================================================================
***********read the file ./10-Terraform-Modules/10-02-Terraform-Build-a-Module/Oldv2-2024-Backup/README.md ************
# Build a Terraform Module

## Step-01: Introduction
- Build a Terraform Module
    - Create a Terraform module
    - Use local Terraform modules in your configuration
    - Configure modules with variables
    - Use module outputs
    - We are going to write a local re-usable module for the following usecase.
- **Usecase: Hosting a static website with AWS S3 buckets**
1. Create an S3 Bucket
2. Create Public Read policy for the bucket
3. Once above two are ready, we can deploy Static Content 
4. For steps, 1 and 2 we are going to create a re-usable module in Terraform
- **How are we going to do this?**
- We are going to do this in 3 sections
- **Section-1 - Full Manual:** Create Static Website on S3 using AWS Management Consoleand host static content and test 
- **Section-2 - Terraform Resources:** Automate section-1 using Terraform Resources
- **Section-3 - Terraform Modules:** Create a re-usable module for hosting static website by referencing section-2 terraform configuration files. 

## Step-02: Hosting a Static Website with AWS S3 using AWS Management Console
- **Reference Sub-folder:** v1-create-static-website-on-s3-using-aws-mgmt-console
- We are going to host a static website with AWS S3 using AWS Management console
### Step-02-01: Create AWS S3 Bucket
- Go to AWS Services -> S3 -> Create Bucket 
- **Bucket Name:** mybucket-1045 (Note: Bucket name should be unique across AWS)
- **Region:** US.East (N.Virginia)
- Rest all leave to defaults
- Click on **Create Bucket**

### Step-02-02: Enable Static website hosting
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Properties Tab -> At the end
- Edit to enable **Static website hosting**
- **Static website hosting:** enable
- **Index document:** index.html
- Click on **Save Changes**

### Step-02-03: Remove Block public access (bucket settings)
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Permissions Tab 
- Edit **Block public access (bucket settings)** 
- Uncheck **Block all public access**
- Click on **Save Changes**
- Provide text `confirm` and Click on **Confirm**

### Step-02-04: Add Bucket policy for public read by bucket owners
- Update your bucket name in the below listed policy
- **Location:** v1-create-static-website-on-s3-using-aws-mgmt-console/policy-public-read-access-for-website.json
```json
{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Sid": "PublicReadGetObject",
          "Effect": "Allow",
          "Principal": "*",
          "Action": [
              "s3:GetObject"
          ],
          "Resource": [
              "arn:aws:s3:::mybucket-1045/*"
          ]
      }
  ]
}
```
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Permissions Tab 
- Edit -> **Bucket policy** -> Copy paste the policy above with your bucket name
- Click on **Save Changes**

### Step-02-05: Upload index.html
- **Location:** v1-create-static-website-on-s3-using-aws-mgmt-console/index.html
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Objects Tab 
- Upload **index.html**

### Step-02-06: Access Static Website using S3 Website Endpoint
- Access the newly uploaded index.html to S3 bucket using browser
```
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1045.s3-website.us-east-1.amazonaws.com/
```

### Step-02-07: Conclusion
- We have used multiple manual steps to host a static website on AWS
- Now all the above manual steps automate using Terraform in next step

## Step-03: Create Terraform Configuration to Host a Static Website on AWS S3
- **Reference Sub-folder:** v2-host-static-website-on-s3-using-terraform-manifests
- We are going to host a static website on AWS S3 using general terraform configuration files
### Step-03-01: Create Terraform Configuration Files step by step
1. versions.tf
2. main.tf
3. variables.tf
4. outputs.tf
5. terraform.tfvars

### Step-03-02: Execute Terraform Commands & Verify the bucket
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify 
1. Bucket has static website hosting enabled
2. Bucket has public read access enabled using policy
3. Bucket has "Block all public access" unchecked
```

### Step-03-03: Upload index.html and test
```
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1046.s3-website.us-east-1.amazonaws.com/
```
### Step-03-04: Destroy and Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```


### Step-03-05: Conclusion
- Using above terraform configurations we have hosted a static website in AWS S3 in seconds. 
- In next step, we will convert these **terraform configuration files** to a Module which will be re-usable just by calling it.


## Step-04: Build a Terraform Module to Host a Static Website on AWS S3
- **Reference Sub-folder:** v3-build-a-module-to-host-static-website-on-aws-s3
- We will build a Terraform module to host a static website on AWS S3

### Step-04-01: Create Module Folder Structure
- We are going to create `modules` folder and in that we are going to create a module named `aws-s3-static-website-bucket`
- We will copy required files from previous section for this respective module.
- Terraform Working Directory: v3-build-a-module-to-host-static-website-on-aws-s3
    - modules
        - Module-1: aws-s3-static-website-bucket
            - main.tf
            - variables.tf
            - outputs.tf
            - README.md
            - LICENSE
- Inside `modules/aws-s3-static-website-bucket`, copy below listed three files from `v2-host-static-website-on-s3-using-terraform-manifests`
    - main.tf
    - variables.tf
    - outputs.tf


### Step-04-02: Call Module from Terraform Work Directory (Root Module)
- Create Terraform Configuration in Root Module by calling the newly created module
- c1-versions.tf
- c2-variables.tf
- c3-s3bucket.tf
- c4-outputs.tf
```t
module "website_s3_bucket" {
  source = "./modules/aws-s3-static-website-bucket"
  bucket_name = var.my_s3_bucket
  tags = var.my_s3_tags
}
```
### Step-04-03: Execute Terraform Commands
```
# Terraform Initialize
terraform init
Observation: 
1. Verify ".terraform", you will find "modules" folder in addition to "providers" folder
2. Verify inside ".terraform/modules" folder too.

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify 
1. Bucket has static website hosting enabled
2. Bucket has public read access enabled using policy
3. Bucket has "Block all public access" unchecked
```

### Step-04-04: Upload index.html and test
```
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1047.s3-website.us-east-1.amazonaws.com/
```

### Step-04-05: Destroy and Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

### Step-04-06: Understand terraform get command
- We have used `terraform init` to download providers from terraform registry and at the same time to download `modules` present in local modules folder in terraform working directory. 
- Assuming we already have initialized using `terraform init` and later we have created `module` configs, we can `terraform get` to download the same.
- Whenever you add a new module to a configuration, Terraform must install the module before it can be used. 
- Both the `terraform get` and `terraform init` commands will install and update modules. 
- The `terraform init` command will also initialize backends and install plugins.
```
# Delete modules in .terraform folder
ls -lrt .terraform/modules
rm -rf .terraform/modules
ls -lrt .terraform/modules

# Terraform Get
terraform get
ls -lrt .terraform/modules
```
### Step-04-07: Major difference between Local and Remote Module
- When installing a remote module, Terraform will download it into the .terraform directory in your configuration's root directory. 
- When installing a local module, Terraform will instead refer directly to the source directory. 
- Because of this, Terraform will automatically notice changes to local modules without having to re-run terraform init or terraform get.

## Step-05: Conclusion
- Created a Terraform module
- Used local Terraform modules in your configuration
- Configured modules with variables
- Used module outputs



















======================================================================
***********read the file ./10-Terraform-Modules/10-02-Terraform-Build-a-Module/Oldv2-2024-Backup/v3-build-a-module-to-host-static-website-on-aws-s3/modules/aws-s3-static-website-bucket/README.md ************
# AWS S3 static website bucket
- This module provisions AWS S3 buckets configured for static website hosting.
- This will be a demo S3 module

======================================================================
***********read the file ./10-Terraform-Modules/10-02-Terraform-Build-a-Module/README.md ************
---
title: Build Terraform Module from Scratch
description: Create Terraform Modules locally
---
# Build a Terraform Module

## Step-01: Introduction
- Build a Terraform Module
    - Create a Terraform module
    - Use local Terraform modules in your configuration
    - Configure modules with variables
    - Use module outputs
    - We are going to write a local re-usable module for the following usecase.
- **Usecase: Hosting a static website with AWS S3 buckets**
1. Create an S3 Bucket
2. Create Public Read policy for the bucket
3. Once above two are ready, we can deploy Static Content 
4. For steps, 1 and 2 we are going to create a re-usable module in Terraform
- **How are we going to do this?**
- We are going to do this in 3 sections
- **Section-1 - Full Manual:** Create Static Website on S3 using AWS Management Consoleand host static content and test 
- **Section-2 - Terraform Resources:** Automate section-1 using Terraform Resources
- **Section-3 - Terraform Modules:** Create a re-usable module for hosting static website by referencing section-2 terraform configuration files. 

## Step-02: Hosting a Static Website with AWS S3 using AWS Management Console
- **Reference Sub-folder:** v1-create-static-website-on-s3-using-aws-mgmt-console
- We are going to host a static website with AWS S3 using AWS Management console
### Step-02-01: Create AWS S3 Bucket
- Go to AWS Services -> S3 -> Create Bucket 
- **Bucket Name:** mybucket-1045 (Note: Bucket name should be unique across AWS)
- **Region:** US.East (N.Virginia)
- Rest all leave to defaults
- Click on **Create Bucket**

### Step-02-02: Enable Static website hosting
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Properties Tab -> At the end
- Edit to enable **Static website hosting**
- **Static website hosting:** enable
- **Index document:** index.html
- Click on **Save Changes**

### Step-02-03: Remove Block public access (bucket settings)
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Permissions Tab 
- Edit **Block public access (bucket settings)** 
- Uncheck **Block all public access**
- Click on **Save Changes**
- Provide text `confirm` and Click on **Confirm**

### Step-02-04: Add Bucket policy for public read by bucket owners
- Update your bucket name in the below listed policy
- **Location:** v1-create-static-website-on-s3-using-aws-mgmt-console/policy-public-read-access-for-website.json
```json
{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Sid": "PublicReadGetObject",
          "Effect": "Allow",
          "Principal": "*",
          "Action": [
              "s3:GetObject"
          ],
          "Resource": [
              "arn:aws:s3:::mybucket-1045/*"
          ]
      }
  ]
}
```
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Permissions Tab 
- Edit -> **Bucket policy** -> Copy paste the policy above with your bucket name
- Click on **Save Changes**

### Step-02-05: Upload index.html
- **Location:** v1-create-static-website-on-s3-using-aws-mgmt-console/index.html
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Objects Tab 
- Upload **index.html**

### Step-02-06: Access Static Website using S3 Website Endpoint
- Access the newly uploaded index.html to S3 bucket using browser
```
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1045.s3-website.us-east-1.amazonaws.com/
```

### Step-02-07: Conclusion
- We have used multiple manual steps to host a static website on AWS
- Now all the above manual steps automate using Terraform in next step

## Step-03: Create Terraform Configuration to Host a Static Website on AWS S3
- **Reference Sub-folder:** v2-host-static-website-on-s3-using-terraform-manifests
- We are going to host a static website on AWS S3 using general terraform configuration files
### Step-03-01: Create Terraform Configuration Files step by step
1. versions.tf
2. main.tf
3. variables.tf
4. outputs.tf
5. terraform.tfvars

### Step-03-02: Execute Terraform Commands & Verify the bucket
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify 
1. Bucket has static website hosting enabled
2. Bucket has public read access enabled using policy
3. Bucket has "Block all public access" unchecked
```

### Step-03-03: Upload index.html and test
```
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1046.s3-website.us-east-1.amazonaws.com/
```
### Step-03-04: Destroy and Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```


### Step-03-05: Conclusion
- Using above terraform configurations we have hosted a static website in AWS S3 in seconds. 
- In next step, we will convert these **terraform configuration files** to a Module which will be re-usable just by calling it.


## Step-04: Build a Terraform Module to Host a Static Website on AWS S3
- **Reference Sub-folder:** v3-build-a-module-to-host-static-website-on-aws-s3
- We will build a Terraform module to host a static website on AWS S3

### Step-04-01: Create Module Folder Structure
- We are going to create `modules` folder and in that we are going to create a module named `aws-s3-static-website-bucket`
- We will copy required files from previous section for this respective module.
- Terraform Working Directory: v3-build-a-module-to-host-static-website-on-aws-s3
    - modules
        - Module-1: aws-s3-static-website-bucket
            - main.tf
            - variables.tf
            - outputs.tf
            - README.md
            - LICENSE
- Inside `modules/aws-s3-static-website-bucket`, copy below listed three files from `v2-host-static-website-on-s3-using-terraform-manifests`
    - main.tf
    - variables.tf
    - outputs.tf


### Step-04-02: Call Module from Terraform Work Directory (Root Module)
- Create Terraform Configuration in Root Module by calling the newly created module
- c1-versions.tf
- c2-variables.tf
- c3-s3bucket.tf
- c4-outputs.tf
```t
module "website_s3_bucket" {
  source = "./modules/aws-s3-static-website-bucket"
  bucket_name = var.my_s3_bucket
  tags = var.my_s3_tags
}
```
### Step-04-03: Execute Terraform Commands
```
# Terraform Initialize
terraform init
Observation: 
1. Verify ".terraform", you will find "modules" folder in addition to "providers" folder
2. Verify inside ".terraform/modules" folder too.

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify 
1. Bucket has static website hosting enabled
2. Bucket has public read access enabled using policy
3. Bucket has "Block all public access" unchecked
```

### Step-04-04: Upload index.html and test
```
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1047.s3-website.us-east-1.amazonaws.com/
```

### Step-04-05: Destroy and Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

### Step-04-06: Understand terraform get command
- We have used `terraform init` to download providers from terraform registry and at the same time to download `modules` present in local modules folder in terraform working directory. 
- Assuming we already have initialized using `terraform init` and later we have created `module` configs, we can `terraform get` to download the same.
- Whenever you add a new module to a configuration, Terraform must install the module before it can be used. 
- Both the `terraform get` and `terraform init` commands will install and update modules. 
- The `terraform init` command will also initialize backends and install plugins.
```
# Delete modules in .terraform folder
ls -lrt .terraform/modules
rm -rf .terraform/modules
ls -lrt .terraform/modules

# Terraform Get
terraform get
ls -lrt .terraform/modules
```
### Step-04-07: Major difference between Local and Remote Module
- When installing a remote module, Terraform will download it into the .terraform directory in your configuration's root directory. 
- When installing a local module, Terraform will instead refer directly to the source directory. 
- Because of this, Terraform will automatically notice changes to local modules without having to re-run terraform init or terraform get.

## Step-05: Conclusion
- Created a Terraform module
- Used local Terraform modules in your configuration
- Configured modules with variables
- Used module outputs



















======================================================================
***********read the file ./10-Terraform-Modules/10-02-Terraform-Build-a-Module/v3-build-a-module-to-host-static-website-on-aws-s3/modules/aws-s3-static-website-bucket/README.md ************
# AWS S3 static website bucket
- This module provisions AWS S3 buckets configured for static website hosting.
- This will be a demo S3 module

======================================================================
***********read the file ./11-Terraform-Cloud-and-Enterprise-Capabilities/11-01-Terraform-Cloud-Github-Integration/README.md ************
# Terraform Cloud & Github Integration

## Step-01: Introduction
- Create Github Repository on github.com
- Clone Github Repository to local desktop
- Copy & Check-In Terraform Configurations in to Github Repository
- Create Terraform Cloud Account
- Create Organization
- Create Workspace by integrating with Github.com Git Repo we recently created
- Learn about Workspace related Queue Plan, Runs, States, Variables and Settings


## Step-02: Create new github Repository
- **URL:** github.com
- Click on **Create a new repository**
- **Repository Name:** terraform-cloud-demo1
- **Description:** Terraform Cloud Demo 
- **Repo Type:** Public / Private
- **Initialize this repository with:**
- **CHECK** - Add a README file
- **CHECK** - Add .gitignore 
- **Select .gitignore Template:** Terraform
- **CHECK** - Choose a license
- **Select License:** Apache 2.0 License
- Click on **Create repository**

## Step-03: Review .gitignore created for Terraform
- Review .gitignore created for Terraform projects

## Step-04: Clone Github Repository to Local Desktop
```t
# Clone Github Repo
git clone https://github.com/<YOUR_GITHUB_ID>/<YOUR_REPO>.git
git clone https://github.com/stacksimplify/terraform-cloud-demo1.git
```

## Step-05: Copy files from terraform-manifests to local repo & Check-In Code
- List of files to be copied
  - apache-install.sh
  - c1-versions.tf
  - c2-variables.tf
  - c3-security-groups.tf
  - c4-ec2-instance.tf
  - c5-outputs.tf
  - c6-ami-datasource.tf
- Source Location: Section-11-01 - Inside terraform-manifests folder
- Destination Location: Newly cloned github repository folder in your local desktop `terraform-cloud-demo1`
- Verify locally before commiting to GIT Repository
```t
# Terraform Init
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan
```
- Check-In code to Remote Repository
```t
# GIT Status
git status

# Git Local Commit
git add .
git commit -am "TF Files First Commit"

# Push to Remote Repository
git push

# Verify the same on Remote Repository
https://github.com/stacksimplify/terraform-cloud-demo1.git
```

## Step-06: Sign-Up for Terraform Cloud - Free Account & Login
- **SignUp URL:** https://app.terraform.io/signup/account
- **Username:**
- **Email:**
- **Password:** 
- **Login URL:** https://app.terraform.io

## Step-07: Create Organization 
- **Organization Name:** hcta-demo1
- **Email Address:** stacksimplify@gmail.com
- Click on **Create Organization**

## Step-08: Create New Workspace
- Get in to newly created Organization
- Click on **New Workspace**
- **Choose your workflow:** V
  - Version Control Workflow
- **Connect to VCS**
  - **Connect to a version control provider:** github.com
  - NEW WINDOW: **Authorize Terraform Cloud:** Click on **Authorize Terraform Cloud Button**
  - NEW WINDOW: **Install Terraform Cloud**
  - **Select radio button:** Only select repositories
  - **Selected 1 Repository:** stacksimplify/terraform-cloud-demo1
  - Click on **Install**
- **Choose a Repository**
  - stacksimplify/terraform-cloud-demo1
- **Configure Settings**
  - **Workspace Name:** terraform-cloud-demo1 (Whatever populated automically leave to defaults) 
  - **Advanced Settings:** leave to defaults 
- Click on **Create Workspace**  
- You should see this message `Configuration uploaded successfully`


## Step-09: Configure Variables
- **Variable:** aws_region
  - key: aws_region
  - value: us-east-1
- **Variable:** instance_type
  - key: instance_type
  - value: t3.micro

## Step-10: Configre Environment Variables
- [Setup AWS Access Keys for Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#environment-variables)
- Configure AWS Access Key ID and Secret Access Key  
- **Environment Variable:** AWS_ACCESS_KEY_ID
  - Key: AWS_ACCESS_KEY_ID
  - Value: XXXXXXXXXXXXXXXXXXXXXX
- **Environment Variable:** AWS_SECRET_ACCESS_KEY
  - Key: AWS_SECRET_ACCESS_KEY
  - Value: YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY

## Step-11: Click on Queue Plan
- Go to Workspace -> Runs -> Queue Plan
- Review the plan generated in **Full Screen**
- **Add Comment:** First Run
- Click on **Confirm & Apply**
- **Add Comment:** First Run Approved
- Click on **Confirm Plan**
- Review Apply log output in **Full Screen**
- **Add Comment:** Successfully Provisioned, Verified in AWS

## Step-12: Review Terraform State
- Go to Workspace -> States
- Review the state file

## Step-13: Change Number of Instance
- Go to Local Desktop -> Local Repo -> c4-ec2-instance.tf -> Change count from 1 to 2
```t
# Change c4-ec2-instance.tf
count = 2

# GIT Status
git status

# Git Local Commit
git add .
git commit -am "Changed EC2 Instances from 1 to 2"

# Push to Remote Repository
git push

# Verify Terraform Cloud
Go to Workspace -> Runs 
Observation: 
1) New plan should be queued ->  Click on Current Plan and review logs in Full Screen
2) Click on **Confirm and Apply**
3) Add Comment: Approved for new EC2 Instance and Click on **Confirm Plan**
4) Verify Apply Logs in Full Screen
5) Review the update state in  Workspace -> States
6) Verify if new EC2 Instance got created
```

## Step-14: Review Workpace Settings
- Goto -> Workspace -> Settings
  - General Settings
  - Locking
  - Notifications
  - Run Triggers
  - SSH Key
  - Version Control

## Step-15: Destruction and Deletion
- Goto -> Workspace -> Settings -> Destruction and Deletion
- click on **Queue Destroy Plan** to delete the resources on cloud 
- Goto -> Workspac -> Runs -> Click on **Confirm & Apply**
- **Add Comment:** Approved for Deletion

======================================================================
***********read the file ./11-Terraform-Cloud-and-Enterprise-Capabilities/11-02-Share-Modules-in-Private-Module-Registry/BACKUP-2024/terraform-s3-website-module-manifests/README.md ************
# Terraform Module for Private Registry - AWS S3 Static website
- This module provisions AWS S3 buckets configured for static website hosting.
- This will be a demo S3 module

======================================================================
***********read the file ./11-Terraform-Cloud-and-Enterprise-Capabilities/11-02-Share-Modules-in-Private-Module-Registry/README.md ************
# Share Modules in Private Modules Registry

## Step-01: Introduction
- Create and version a GitHub repository for use in the private module registry
- Import a module into your organization's private module registry.
- Construct a root module to consume modules from the private registry.
- Over the process also learn about `terraform login` command

## Step-02: Create new github Repository for s3-website terraform module
### Step-02-01: Craete new Github Repository
- **URL:** github.com
- Click on **Create a new repository**
- Follow Naming Conventions for modules
  - terraform-PROVIDER-MODULE_NAME
  - **Sample:** terraform-aws-s3-website
- **Repository Name:** terraform-aws-s3-website
- **Description:** Terraform Modules to be shared in Private Registry
- **Repo Type:** Public / Private
- **Initialize this repository with:**
- **UN-CHECK** - Add a README file
- **CHECK** - Add .gitignore 
- **Select .gitignore Template:** Terraform
- **CHECK** - Choose a license
- **Select License:** Apache 2.0 License  (Optional)
- Click on **Create repository**
### Step-02-02: Create New Release Tag 1.0.0 in Repo
- Go to Right Navigation on github Repo -> Releases -> Create a new release
- **Tag Version:** 1.0.0
- **Release Title:** Release-1 terraform-aws-s3-website
- **Write:** Terraform Module for Private Registry - terraform-aws-s3-website
- Click on **Publish Release**


## Step-03: Clone Github Repository to Local Desktop
```t
# Clone Github Repo
git clone https://github.com/<YOUR_GITHUB_ID>/<YOUR_REPO>.git
git clone https://github.com/stacksimplify/terraform-aws-s3-website.git
```

## Step-04: Copy files from terraform-manifests to local repo & Check-In Code
- **Orignial Source Location:** 10-Terraform-Modules/10-02-Terraform-Build-a-Module/v3-build-a-module-to-host-static-website-on-aws-s3/modules/aws-s3-static-website-bucket
- **Source Location from this section:** terraform-s3-website-module-manifests
- **Destination Location:** Newly cloned github repository folder in your local desktop `terraform-module-s3-website`
- Check-In code to Remote Repository
```t
# GIT Status
git status

# Git Local Commit
git add .
git commit -am "TF Module Files First Commit"

# Push to Remote Repository
git push

# Verify the same on Remote Repository
https://github.com/stacksimplify/terraform-aws-s3-website.git
```

## Step-05: Add VCS Provider as Github using OAuth App in Terraform Cloud 

### Step-05-01: Add VCS Provider as Github using OAuth App in Terraform Cloud
- Login to Terraform Cloud
- Click on Modules Tab -> Click on Add Module -> Select Github(Custom)
- Should redirect to URL: https://github.com/settings/applications/new in new browser tab
- **Application Name:** Terraform Cloud (hctaprep) 
- **Homepage URL:**	https://app.terraform.io 
- **Application description:**	Terraform Cloud Integration with Github using OAuth 
- **Authorization callback URL:**	https://app.terraform.io/auth/f53695b8-9733-40f0-9853-89cb5396610b/callback 
- Click on **Register Application**
- Make a note of Client ID: 97e5219d6edd8986817e (Sample for reference)
- Generate new Client Secret: abcdefghijklmnopqrstuvwxyx

### Step-05-02: Add the below in Terraform Cloud
- Name: github-terraform-modules
- Client ID: 97e5219d6edd8986817e
- Client Secret: abcdefghijklmnopqrstuvwxyx
- Click on **Connect and Continue**
- Authorize Terraform Cloud (hctaprep) - Click on **Authorize StackSimplify**
- SSH Keypair (Optional): click on **Skip and Finish**

### Step-06: Import the Terraform Module from Github
- In above step, we have completed the VCS Setup with github
- Now lets go ahead and import the Terraform module from Github
- Login to Terraform Cloud
- Click on Modules Tab -> Click on Add Module -> Select Github(github-terraform-modules) (PRE-POPULATED) -> Select it
- **Choose a Repository:** terraform-module-s3-website
- Click on **Publish Module**

## Step-07: Review newly imported Module
- Login to Terraform Cloud -> Click on Modules Tab 
- Review the Module Tabs on Terraform Cloud
  - Readme
  - Inputs
  - Outputs
  - Dependencies
  - Resources
- Also review the following
  - Versions
  - Provision Instructions   

## Step-08: Create a configuration that uses the Private Registry module using Terraform CLI
### Step-08-01: Call Module from Terraform Work Directory (Root Module)
- CreateTerraform Configuration in Root Module by calling the newly published module in Terraform Private Registry
- c1-versions.tf
- c2-variables.tf : Review and discuss about changing bucket name due to AWS Unique constraints
- c3-s3bucket.tf
- c4-outputs.tf
```t
module "website_s3_bucket" {
  source  = "app.terraform.io/hctaprep/s3-website-internal/aws"
  version = "1.0.0"
  # insert required variables here
  bucket_name = var.my_s3_bucket
  tags = var.my_s3_tags  
}
```
### Step-08-02: Execute Terraform Commands
```t
# Terraform Initialize
terraform init
Observation: 
1. Should fail with error due to cli not having access to Private module registry in Terraform Cloud

# Terraform Login
terraform login
Observation: 
1) Should see message |Success! Terraform has obtained and saved an API token.|
2) Verify Terraform credentials file
cat /Users/<YOUR_USER>/.terraform.d/credentials.tfrc.json
cat /Users/kdaida/.terraform.d/credentials.tfrc.json
Additional Reference:
https://www.terraform.io/docs/cli/config/config-file.html#credentials-1
https://www.terraform.io/docs/cloud/registry/using.html#configuration

# Terraform Initialize
terraform init
Observation: 
1. Should pass and download modules and providers

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify 
1. Bucket has static website hosting enabled
2. Bucket has public read access enabled using policy
3. Bucket has "Block all public access" unchecked
```

### Step-08-03: Upload index.html and test
```t
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1051.s3-website.us-east-1.amazonaws.com/
```

### Step-08-04: Destroy and Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-09: Create a configuration that uses the Private Registry module using Terraform Cloud & Github
### Assignment
1. Create Github Repository
2. Check-In files from `terraform-manifests` folder in `11-02` section
3. Create a new Workspace in Terraform Cloud to connect with Github Repository
4. Execute `Queue Plan` to apply the changes and test


## Step-10: VCS Providers & Terraform Cloud
- [Configuration-Free GitHub Usage](https://www.terraform.io/docs/cloud/vcs/github-app.html)
- [Configuring GitHub.com Access (OAuth)](https://www.terraform.io/docs/cloud/vcs/github.html)
- [Configuring GitHub Enterprise Access](https://www.terraform.io/docs/cloud/vcs/github-enterprise.html)
- [Other Supported VCS Providers](https://www.terraform.io/docs/cloud/vcs/index.html)

======================================================================
***********read the file ./11-Terraform-Cloud-and-Enterprise-Capabilities/11-02-Share-Modules-in-Private-Module-Registry/terraform-s3-website-module-manifests/README.md ************
# Terraform Module for Private Registry - AWS S3 Static website
- This module provisions AWS S3 buckets configured for static website hosting.
- This will be a demo S3 module

======================================================================
***********read the file ./11-Terraform-Cloud-and-Enterprise-Capabilities/11-03-Terraform-Cloud-CLI-Driven-Workflow/README.md ************
# Terraform Cloud - CLI-Driven Workflow

## Step-01: Introduction
- Learn and practically implement `CLI-Driven Workflow` in Terraform Cloud

## Step-02: Review Terraform Configuration Files
- c1-versions.tf
- c2-variables.tf
- c3-s3bucket.tf
- c4-outputs.tf

## Step-03: Create Workspace with CLI Driven Workflow
- Login to [Terraform Cloud](https://app.terraform.io/)
- Select Organization -> hcta-demo1
- Click on **New Workspace**
- **Choose your workflow:** CLI-Driven Workflow
- **Workspace Name:** cli-driven-demo
- Click on **Create Workspace**

## Step-04: Add backend block in Terraform Settings c1-versions.tf
```t
terraform {
  backend "remote" {
    organization = "hcta-demo1"

    workspaces {
      name = "cli-driven-demo"
    }
  }
}
```

## Step-05: Execute Terraform Commands
```t
# Terraform Login
terraform login
Observation: 
1) Should see message |Success! Terraform has obtained and saved an API token.|
2) Verify Terraform credentials file
cat /Users/<YOUR_USER>/.terraform.d/credentials.tfrc.json
cat /Users/kdaida/.terraform.d/credentials.tfrc.json
Additional Reference:
https://www.terraform.io/docs/cli/config/config-file.html#credentials-1
https://www.terraform.io/docs/cloud/registry/using.html#configuration

# Terraform Initialize
terraform init
Observation: 
1. Should pass and download modules and providers

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan
Observation: Should fail with error due to AWS Provider credential configuration not done on Terraform Cloud for this respective workspace
```

## Step-06: Configre Environment Variables
- [Setup AWS Access Keys for Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#environment-variables)
- Go to Organization (hcta-demo1) -> Workspace(cli-driven-demo) -> Variables
- In environment variables, add the below two
- Configure AWS Access Key ID and Secret Access Key  
- **Environment Variable:** AWS_ACCESS_KEY_ID
  - Key: AWS_ACCESS_KEY_ID
  - Value: XXXXXXXXXXXXXXXXXXXXXX
- **Environment Variable:** AWS_SECRET_ACCESS_KEY
  - Key: AWS_SECRET_ACCESS_KEY
  - Value: YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY


## Step-07: Execute Terraform Commands
```t
# Terraform Plan
terraform plan
Observation: 
1) Terraform plan should pass now. 
2) Discuss about cost estimation and trial plan for 30 days

# Terraform Apply
terraform apply 

# Verify 
1. Bucket has static website hosting enabled
2. Bucket has public read access enabled using policy
3. Bucket has "Block all public access" unchecked
```


### Step-08: Upload index.html and test
```t
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1051.s3-website.us-east-1.amazonaws.com/
```

## Step-09: Verify the following
- Select Organization -> hcta-demo1
- Click on **New Workspace**
- **Choose your workflow:** CLI-Driven Workflow
- **Workspace Name:** cli-driven-demo
- Runs
- States

## Step-10: Make changes and execute Terraform commands
- Update `c2-variables.tf` with new tag
- Execute Terraform Commands
```t
# Terraform Plan
terraform plan
Observation: 
1) Verify Terraform cloud Runs tab
2) We should see the runs are taking place for changes in Terraform cloud

# Terraform Apply
terraform apply -auto-approve
```
### Step-11: Destroy and Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Additional References
- [CLI Configuration File](https://www.terraform.io/docs/cli/config/config-file.html#credentials)======================================================================
***********read the file ./11-Terraform-Cloud-and-Enterprise-Capabilities/11-04-Migrate-State-to-Terraform-Cloud/README.md ************
# Migrate State to Terraform Cloud

## Step-01: Introduction
- We are going to migrate State to Terraform Cloud

## Step-02: Review Terraform Manifests
- c1-versions.tf
- c2-variables.tf: 
  - **Important Note:** No default values provided for variables 
- c3-security-groups.tf
- c4-ec2-instance.tf
- c5-outputs.tf
- c6-ami-datasource.tf
- apache-install.sh


## Step-03: Execute Terraform Commands (First provision using local backend)
- First provision infra using local backend
- `terraform.tfstate` file will be created in local working directory
- In next steps, migrate it to Terraform Cloud
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve
```

## Step-04: Review your local state file
-  Review your local `terraform.tfstate` file once


## Step-05: Update remote backend in c1-versions.tf Terraform Block
### Step-05-01: Create New Workspace with CLI-Driven workflow
- Create New workspace with CLI-Driven workflow
- Login to [Terraform Cloud](https://app.terraform.io/)
- Select Organization -> hcta-demo1
- Click on **New Workspace**
- **Choose your workflow:** CLI-Driven Workflow
- **Workspace Name:** state-migration-demo
- Click on **Create Workspace**

### Step-05-02: Update remote backend in c1-versions.tf Terraform Block
```t
# Template
  backend "remote" {
    hostname      = "app.terraform.io"
    organization  = "<YOUR-ORG-NAME>"

    workspaces {
      name = "<SOME-NAME>"
    }
  }

# Replace Values
  backend "remote" {
    hostname      = "app.terraform.io"
    organization  = "hcta-demo1"  # Organization should already exists in Terraform Cloud

    workspaces {
      name = "state-migration-demo" 
      # Two cases: 
      # Case-1: If workspace already exists, should not have any state files in states tab
      # Case-2: If workspace not exists, during migration it will get created
    }
  }
```
- **Case-2 above for workspaces is failing with this error**
```
Kalyans-MacBook-Pro:terraform-manifests kdaida$ terraform init
Initializing the backend...
Error: Error looking up workspace
Workspace read failed: resource not found
Kalyans-MacBook-Pro:terraform-manifests kdaida$
```

## Step-06: Migrate State file to Terraform Cloud and Verify
```t
# Terraform Login
terraform login
Observation: 
1) Should see message |Success! Terraform has obtained and saved an API token.|
2) Verify Terraform credentials file
cat /Users/<YOUR_USER>/.terraform.d/credentials.tfrc.json
cat /Users/kdaida/.terraform.d/credentials.tfrc.json
Additional Reference:
https://www.terraform.io/docs/cli/config/config-file.html#credentials-1
https://www.terraform.io/docs/cloud/registry/using.html#configuration

# Terraform Initialize
terraform init
Observation: 
1) During reinitialization, Terraform presents a prompt saying that it will copy the state file to the new backend. 
2) Enter yes and Terraform will migrate the state from your local machine to Terraform Cloud.

# Verify in Terraform Cloud
1) New workspace should be created with name "state-migration-demo"
2) Verify "states" tab in workspace, we should find the state file
```

## Step-07: Add Variables & AWS Credentials in Environment Variables
### Step-07-01: Add Variables
```t
aws_region = us-east-1
instance_type = t3.micro
```
### Step-07-02: Configre Environment Variables
- [Setup AWS Access Keys for Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#environment-variables)
- Go to Organization (hcta-demo1) -> Workspace(state-migration-demo) -> Variables
- In environment variables, add the below two
- Configure AWS Access Key ID and Secret Access Key  
- **Environment Variable:** AWS_ACCESS_KEY_ID
  - Key: AWS_ACCESS_KEY_ID
  - Value: XXXXXXXXXXXXXXXXXXXXXX
- **Environment Variable:** AWS_SECRET_ACCESS_KEY
  - Key: AWS_SECRET_ACCESS_KEY
  - Value: YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY
 
## Step-08: Delete local terraform.tfstate
- First take backup and put it safe and delete it
```t
# Take backup
cp terraform.tfstate terraform.tfstate_local_before_migrate_to_TF_Cloud

# Delete
rm terraform.tfstate
``` 

## Step-09: Apply a new run from CLI
- Make a change and do  `terraform apply`
```t
# Change Instances from 1 to 2 (c4-ec2-instance.tf)
count = 2

# Terraform Apply
terraform apply 

# Verify in Terraform Cloud
1) Verify in Runs Tab in TF Cloud
2) Verify States Tab in TF Cloud
```

## Step-10: Destroy & Clean-Up
-  Destroy Resources from cloud this time instead of `terraform destroy` command
- Go to Organization (hcta-demo1) -> Workspace(state-migration-demo) -> Settings -> Destruction and Deletion
- Click on **Queue Destroy Plan**
```t
# Clean-Up files
rm -rf .terraform*
rm -rf terraform.tfstate*
```
======================================================================
***********read the file ./12-Terraform-Cloud-and-Sentinel/12-01-Terraform-Cloud-and-Sentinel-Policies/README.md ************
# Terraform Cloud and Sentinel Policies

## Step-01: Introduction
- We are going to learn the following in this section
- Enable Trial plan for 30 days on hcta-demo1 organization which will enable **Team & Governance** features of Terraform Cloud
- Implement CLI-Driven workflow using Terraform Cloud for a S3 bucket resource
- Understand about two sentinel policies
  - check-terraform-version.sentinel
  - restrict-s3-buckets.sentinel
- Create Github repository for Sentinel Policies to use them as Policy Sets in Terraform Cloud
- Create Policy Sets in Terraform Cloud and Apply to demo workspace
- Test if sentinel policies applied and worked successfully.  


## Step-02: Review Terraform manifests
- **c1-versions.tf**
  - We are going to add Terraform Cloud as backend and implement CLI-Driven workflow on Terraform Cloud for this usecase for our learning. 
- **c2-variables.tf**
  - Bucket Name and Tags have default values and due to unique constraint for s3 bucket names, please use different bucket name when you are implementing it. 
- **c3-s3bucket.tf**
  - We are using this S3 bucket resource from **10-02-Terraform-Build-a-Module/v2-host-static-website-on-s3-using-terraform-manifests**.
  - In addition, we are going to add new resource named **aws_s3_bucket_object**, which will upload the `static-files/index.html` automatically during `terraform apply`
```t
resource "aws_s3_bucket_object" "bucket" {
  acl          = "public-read"
  key          = "index.html"
  bucket       = aws_s3_bucket.s3_bucket.id
  content      = file("${path.module}/static-files/index.html")
  content_type = "text/html"
}
```  
- **c4-outputs.tf**
  - We are going to have only S3 website endpoint as output with and without http appended. 

## Step-03: Create CLI-Driven Workspace on Terraform Cloud
### Step-03-01: Enable Trial plan in hcta-demo1 organization
- Login to Terraform Cloud
- Goto -> Organizations (hcta-demo1) -> Settings -> Plan & Billing
- Click on **Change Plan**
- Select **Trial Plan: Try out the Team & Governance plan features for 30 days**
- Click on **Start Free Trial**

### Step-03-02: Create CLI-Driven Workspace in organization hcta-demo1
- Login to [Terraform Cloud](https://app.terraform.io/)
- Select Organization -> hcta-demo1
- Click on **New Workspace**
- **Choose your workflow:** CLI-Driven Workflow
- **Workspace Name:** sentinel-demo1
- Click on **Create Workspace**

### Step-03-03: Update c1-versions.tf with Terraform Backend in Terraform Block
```t
terraform {
  backend "remote" {
    organization = "hcta-demo1"

    workspaces {
      name = "sentinel-demo1"
    }
  }
}
```
### Step-03-04: Configre Environment Variables in Terraform Cloud for AWS Provider
- [Setup AWS Access Keys for Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#environment-variables)
- Go to Organization (hcta-demo1) -> Workspace(sentinel-demo1) -> Variables
- In environment variables, add the below two
- Configure AWS Access Key ID and Secret Access Key  
- **Environment Variable:** AWS_ACCESS_KEY_ID
  - Key: AWS_ACCESS_KEY_ID
  - Value: XXXXXXXXXXXXXXXXXXXXXX
- **Environment Variable:** AWS_SECRET_ACCESS_KEY
  - Key: AWS_SECRET_ACCESS_KEY
  - Value: YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY


### Step-03-05: Execute Terraform Commands
```t
# Terraform Login
terraform login
Observation: 
1) Should see message |Success! Terraform has obtained and saved an API token.|
2) Verify Terraform credentials file
cat /Users/<YOUR_USER>/.terraform.d/credentials.tfrc.json
cat /Users/kdaida/.terraform.d/credentials.tfrc.json
Additional Reference:
https://www.terraform.io/docs/cli/config/config-file.html#credentials-1
https://www.terraform.io/docs/cloud/registry/using.html#configuration


# Terrafrom Initialize
terraform init

# Terraform Plan
terraform plan
Observation: 
1) Cost Estimation with this bucket should be displayed as we enabled the trial plan for 30 days on this organization (hcta-demo1)

# Terraform Apply
terraform apply -auto-approve

# Verify 
1) Access S3 static website and test
2) Review the plan and apply logs in Terraform Cloud respective workspace
```

## Step-04: Review Sentinel Policies
- [Sentinel Documentation](https://www.terraform.io/docs/cloud/sentinel/index.html)
### Step-04-01: check-terraform-version.sentinel
```t
import "tfplan/v2" as tfplan
import "strings"

v = strings.split(tfplan.terraform_version, ".")
version_major = int(v[1])
version_minor = int(v[2])

main = rule {
    #version_major is 14 and version_minor >= 1
    version_major >= 14
}
```

### Step-04-02: restrict-s3-buckets.sentinel
```t
import "tfplan/v2" as tfplan

s3_buckets = filter tfplan.resource_changes as _, rc {
  rc.type is "aws_s3_bucket" and
  (rc.change.actions contains "create" or rc.change.actions is ["update"])
}

required_tags = [
    "Terraform",
    "Environment",
]

allowed_acls = [
    "private",
    "public-read",
]

bucket_tags = rule {
    all s3_buckets as _, instances {
        all required_tags as rt {
        instances.change.after.tags contains rt
        }
    }
}

acl_allowed = rule {
    all s3_buckets as _, buckets {
    buckets.change.after.acl in allowed_acls
    }
}

main = rule {
    (bucket_tags and acl_allowed) else false
}

```

### Step-04-03: sentinel.hcl
```t
policy "check-terraform-version" {
    source            = "./check-terraform-version.sentinel"        
    enforcement_level = "hard-mandatory"
}

policy "restrict-s3-buckets" {
    source            = "./restrict-s3-buckets.sentinel"           
    enforcement_level = "soft-mandatory"
}
```
## Step-05: Create Github Repository for Sentinel Policies (Policy Sets)
### Step-05-01: Create new github Repository
- **URL:** github.com
- Click on **Create a new repository**
- **Repository Name:** terraform-sentinel-policies
- **Description:** Terraform Cloud and Sentinel Policies Demo 
- **Repo Type:** Public / Private
- **Initialize this repository with:**
- **CHECK** - Add a README file
- **CHECK** - Add .gitignore 
- **Select .gitignore Template:** Terraform
- **CHECK** - Choose a license  (Optional)
- **Select License:** Apache 2.0 License
- Click on **Create repository**

## Step-05-02: Clone Github Repository to Local Desktop
```t
# Clone Github Repo
git clone https://github.com/<YOUR_GITHUB_ID>/<YOUR_REPO>.git
git clone https://github.com/stacksimplify/terraform-sentinel-policies.git
```

## Step-05-03: Copy files from terraform-sentinel-policies folder to local repo & Check-In Code
- List of files to be copied
  - check-terraform-version.sentinel
  - restrict-s3-buckets.sentinel
  - sentinel.hcl
- **Source Location:** Section-12-01 - Inside `terraform-sentinel-policies` folder
- **Destination Location:** Newly cloned github repository folder in your local desktop `terraform-sentinel-policies`
- **Check-In code to Remote Repository**
```t
# GIT Status
git status

# Git Local Commit
git add .
git commit -am "Sentinel Policies First Commit"

# Push to Remote Repository
git push

# Verify the same on Remote Repository
https://github.com/stacksimplify/terraform-sentinel-policies.git
```

## Step-06: Create Policy Sets in Terraform Cloud
- Go to Terraform Cloud -> Organization (hcta-demo1) -> Settings -> Policy Sets
- Click on **Connect a new Policy Set**
- Use existing VCS connection from previous section **github-terraform-modules** which we created using OAuth App concept
- **Choose Repository:** terraform-sentinel-policies.git
- **Description:** Demo Sentinel Policies
- **Scope of Policies:** Policies enforced on selected workspaces
- **Workspaces:** sentinel-demo
- Click on **Connect Policy Set**

## Step-07: Update static-files/index.html in terraform-manifests
- Update static-files/index.html in terraform-manifests
```t
# Terraform Plan
terraform plan
Observation: 
1) Changes related to index.html should be printed in terraform plan
2) Cost Estimation with this bucket should be displayed as we enabled the trial plan for 30 days on this organization (hcta-demo1)
3) Sentinel policy execution should be displayed

# Terraform Apply
terraform apply -auto-approve
Observation: 
1) Changes related to index.html should be printed in terraform apply
2) Cost Estimation with this bucket should be displayed as we enabled the trial plan for 30 days on this organization (hcta-demo1)
3) Sentinel policy execution should be displayed

# Verify in Terraform Cloud
Observations:
1) Cost Estimation should be displayed
2) Also Sentinel Policy check should be displayed
```

## Step-08: Verify Sentinel Policy Failure Scenario
### Step-08-01: c2-variables.tf
- Update required tags to different value
```t
# Before Change
variable "tags" {
  description = "Tages to set on the bucket"
  type        = map(string)
  default     = {
    Terraform = "true"
    Environment = "dev"
    newtag1 = "tag1"
    newtag2 = "tag2"
  }
}

# After Change
variable "tags" {
  description = "Tages to set on the bucket"
  type        = map(string)
  default     = {
    abcdef = "true"  # modified one required tag so that sentinel policy restrict-se-buckets.sentinel will fail
    Environment = "dev"
    newtag1 = "tag1"
    newtag2 = "tag2"
  }
}
```
### Step-08-02: Execute Terraform Commands
```t
# Terraform Plan
terraform plan
Observation: 
1) Changes related to tag should be printed in terraform plan
2) Sentinel policy execution should report a failure for policy restrict-s3-buckets.sentinel

# Terraform Apply
terraform apply -auto-approve
Observation: 
1) Changes related to tag should be printed in terraform apply
2) Sentinel policy execution should report a failure for policy restrict-s3-buckets.sentinel

# Verify in Terraform Cloud
Observation: 
1) Sentinel policy execution will fail and terraform apply will not be executed
```

## Step-09: Clean-Up & Destroy
```t
# Terraform Destroy
terraform destroy -auto-approve

# Clean-Up files
rm -rf .terraform*
```

## References 
- [Terraform & Sentinel](https://www.terraform.io/docs/cloud/sentinel/index.html)
- [Example Sentinel Policies](https://www.terraform.io/docs/cloud/sentinel/examples.html)
- [Sentinel Foundational Policies](https://github.com/hashicorp/terraform-foundational-policies-library)
- [Sentinel Enforcement Levels](https://docs.hashicorp.com/sentinel/concepts/enforcement-levels)
======================================================================
***********read the file ./12-Terraform-Cloud-and-Sentinel/12-02-Control-Costs-with-Sentinel-Policies/README.md ************
# Control Costs with Sentinel Policies

## Step-01: Introduction
- We are going to learn the following in this section
- Sentinel Cost Control Policies
- Apply them for Ec2 Instance and verify pass and fail cases

## Step-02: Review Sentinel Cost Control Policies
### Step-02-01: less-than-100-month.sentinel
- This policy uses the tfrun import to check that the new cost delta is no more than \$100
- The decimal import is used for more accurate math when working with currency numbers.
```t
import "tfrun"
import "decimal"

delta_monthly_cost = decimal.new(tfrun.cost_estimate.delta_monthly_cost)

main = rule {
    delta_monthly_cost.less_than(100)
}
```

### Step-02-02: sentinel.hcl
```t
policy "less-than-100-month" {
  source  = "./less-than-100-month.sentinel"
  enforcement_level = "soft-mandatory"
}
```

## Step-03: Copy Sentinel Cost Control Policies to terraform-sentinel-policies git repo
- Copy folder `terraform-sentinel-cost-control-policies` to Local git repository `terraform-sentinel-policies`
- **Check-In code to Remote Repository**
```t
# GIT Status
git status

# Git Local Commit
git add .
git commit -am "Sentinel Cost Control Policies Added in new folder"

# Push to Remote Repository
git push

# Verify the same on Remote Repository
https://github.com/stacksimplify/terraform-sentinel-policies.git
```

## Step-04: Add new Sentinel Policy Set in Terraform Cloud
- Go to Terraform Cloud -> Organization (hcta-demo1) -> Settings -> Policy Sets
- Click on **Connect a new Policy Set**
- Use existing VCS connection from previous section **github-terraform-modules** which we created using OAuth App concept
- **Choose Repository:** terraform-sentinel-policies.git
- **Name:** terraform-sentinel-cost-control-policies
- **Description:** terraform sentinel cost control policies
- **Policies Path:** terraform-sentinel-cost-control-policies
- **Scope of Policies:** Policies enforced on selected workspaces
- **Workspaces:** terraform-cloud-demo1
- Click on **Connect Policy Set**

## Step-05: Review our first Terraform Cloud Workspace
- Go to Terraform Cloud -> Organization (hcta-demo1) -> workspace (terraform-cloud-demo1)
### Step-05-01: Configre Environment Variables in Terraform Cloud for AWS Provider
- [Setup AWS Access Keys for Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#environment-variables)
- Go to Organization (hcta-demo1) -> Workspace(terraform-cloud-demo1) -> Variables
- In environment variables, add the below two
- Configure AWS Access Key ID and Secret Access Key  
- **Environment Variable:** AWS_ACCESS_KEY_ID
  - Key: AWS_ACCESS_KEY_ID
  - Value: XXXXXXXXXXXXXXXXXXXXXX
- **Environment Variable:** AWS_SECRET_ACCESS_KEY
  - Key: AWS_SECRET_ACCESS_KEY
  - Value: YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY

### Step-05-02: Pass Case: Queue Plan and Verify Cost Control Policies Applied
- Go to Organization (hcta-demo1) -> Workspace(terraform-cloud-demo1) 
- Queue Plan -> Cost-Control-Test-1-Pass-case
- Verify the following
  - Plan
  - Cost Estimate
  - Policy Check:  Policy check should pass
- Finally, Disacrd the Run

### Step-05-03: Fail Case: Queue Plan and Verify Cost Control Policies Applied
- Go to Organization (hcta-demo1) -> Workspace(terraform-cloud-demo1) -> Variables
- Update `instance_type` Variable
```t
# Before Change
instance_type = t3.micro

# After Change
instance_type = t3.2xlarge
```
- Queue Plan -> Cost-Control-Test-1-Fail-case
- Verify the following
  - Plan
  - Cost Estimate
  - Policy Check:  Policy check should fail
- Finally, Disacrd the Run
- Roll back `instance_type` to `t3.micro`

## Step-06: Sentinel Policies  - Conclusion
- We can create multiple sentinel policies in different folder paths in single github repository like `terraform-sentinel-policies`
- We can apply few of them at `Terraform Organization` level and few of them at `Terraform Workspace` level.
- Very flexible and conveniet.======================================================================
***********read the file ./12-Terraform-Cloud-and-Sentinel/12-03-Terraform-Foundational-Policies-using-Sentinel/README.md ************
# Terraform Foundational Policies using Sentinel

## Step-01: Introduction
- [Terraform Foundational Policies Library](https://github.com/hashicorp/terraform-foundational-policies-library)
- This repository contains a library of policies that can be used within Terraform Cloud to accelerate your adoption of policy as code.
- This is pre-built sentinel policies provided by Terraform

## Step-02: Review sentinel.hcl
- [Terraform Foundational Policies using Sentinel](https://github.com/hashicorp/terraform-foundational-policies-library)
- [Terraform Sentinel AWS CIS Networking Policies](https://github.com/hashicorp/terraform-foundational-policies-library/tree/master/cis/aws/networking)
- **Folder Name:** terraform-sentinel-cis-policies
```t
policy "aws-cis-4.1-networking-deny-public-ssh-acl-rules" {
  source = "https://raw.githubusercontent.com/hashicorp/terraform-foundational-policies-library/master/cis/aws/networking/aws-cis-4.1-networking-deny-public-ssh-acl-rules/aws-cis-4.1-networking-deny-public-ssh-acl-rules.sentinel"
  enforcement_level = "advisory"
}

policy "aws-cis-4.2-networking-deny-public-rdp-acl-rules" {
  source = "https://raw.githubusercontent.com/hashicorp/terraform-foundational-policies-library/master/cis/aws/networking/aws-cis-4.2-networking-deny-public-rdp-acl-rules/aws-cis-4.2-networking-deny-public-rdp-acl-rules.sentinel"
  enforcement_level = "advisory"
}

policy "aws-cis-4.3-networking-restrict-all-vpc-traffic-acl-rules" {
  source = "https://raw.githubusercontent.com/hashicorp/terraform-foundational-policies-library/master/cis/aws/networking/aws-cis-4.3-networking-restrict-all-vpc-traffic-acl-rules/aws-cis-4.3-networking-restrict-all-vpc-traffic-acl-rules.sentinel"
  enforcement_level = "advisory"
}
```

## Step-03: Copy Sentinel CIS Policies to terraform-sentinel-policies git repo
- Copy folder `terraform-sentinel-cis-policies` to Local git repository `terraform-sentinel-policies`
- **Check-In code to Remote Repository**
```t
# GIT Status
git status

# Git Local Commit
git add .
git commit -am "Sentinel CIS Policies Added in new folder"

# Push to Remote Repository
git push

# Verify the same on Remote Repository
https://github.com/stacksimplify/terraform-sentinel-policies.git
```

## Step-04: Add new Sentinel Policy Set in Terraform Cloud
- Go to Terraform Cloud -> Organization (hcta-demo1) -> Settings -> Policy Sets
- Click on **Connect a new Policy Set**
- Use existing VCS connection from previous section **github-terraform-modules** which we created using OAuth App concept
- **Choose Repository:** terraform-sentinel-policies.git
- **Name:** terraform-sentinel-cis-policies
- **Description:** terraform sentinel cis-policies
- **Policies Path:** terraform-sentinel-cis-policies
- **Scope of Policies:** Policies enforced on selected workspaces
- **Workspaces:** terraform-cloud-demo1
- Click on **Connect Policy Set**

## Step-05: Review our first Terraform Cloud Workspace
- Go to Terraform Cloud -> Organization (hcta-demo1) -> workspace (terraform-cloud-demo1)
- Queue Plan -> CIS-Policy-Test-1
- Verify the following
  - Plan
  - Cost Estimate
  - Policy Check:  Verify what all passed and failed
- Finally, Disacrd the Run

======================================================================
***********read the file ./13-Terraform-State-Import/README.md ************
# Terraform State Import

## Step-01: Introduction
### Some notes about Terraform Import Command
- Terraform is able to import existing infrastructure. 
- This allows you take resources you've created by some other means and bring it under Terraform management.
- This is a great way to slowly transition infrastructure to Terraform, or to be able to be confident that you can use Terraform in the future if it potentially doesn't support every feature you need today.
- [Full Reference](https://www.terraform.io/docs/cli/import/index.html)
### Two demos
- **Demo-1:** Create EC2 Instance manually and import state to manage it from Terraform
- **Demo-2:** Create S3 bucket manually and import state to mange it from Terraform


## Step-02: Create EC2 Instance manually using AWS mgmt Console
- Go to  Services -> Ec2 -> Instances -> Launch Instance
- **Step 1:** ChooseAMI: Amazon Linux 2 AMI (HVM), SSD Volume Type
- **Step 2:** Choose an Instance Type: t2.micro (leave to defaults)
- **Step 3:** Configure Instance Details: (Leave to defaults )
- **Step 4:** Add Storage (Leave to Defaults)
- **Step 5:** Add Tags
  - Key: Name
  - Value: State-Import-Demo
- **Step 6:** Configure Security Group: Select default VPC security group
- **Step 7:** Review Instance Launch
- Select an existing key pair: terraform-key
- Click on **Launch Instance**


## Step-03: Create Basic Terraform Configuration
- **Reference Folder:** v1-ec2-instance
- c1-versions.tf
- c2-ec2-instance.tf
- Create EC2 Instance Resource - Basic Version to get Terraform `Resource Type` and `Resource Local Name` we are going to use in Terraform
```t
# Create EC2 Instance Resource - Basic Version to get Terraform Resource Type and Resource Local Name we are going to use in Terraform
resource "aws_instance" "myec2vm" {
}
```

## Step-04: Run Terraform Import to import AWS EC2 Instance Resource to Terraform
- [Terraform AWS EC2 Instance Argument Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#argument-reference)
```t
# Terraform Initialize
terraform init

# Terraform Import Command for EC2 Instance
terraform import aws_instance.myec2vm <EC2-Instance-ID>
terraform import aws_instance.myec2vm i-0477f144280c37a7a
Observation:
1) terraform.tfstate file will be created locally in Terraform working directory
2) Review information about imported instance configuration in terraform.tfstate
```

## Step-05: Start Building local c2-ec2-instance.tf
- By referring `terraform.tfstate` file and parallely running `terraform plan` command make changes to your terraform configuration `c2-ec2-instance.tf` till you get the message `No changes. Infrastructure is up-to-date` for `terraform plan` output
```t
# Create EC2 Instance Resource 
resource "aws_instance" "myec2vm" {
  ami = "ami-038f1ca1bd58a5790"
  instance_type = "t2.micro"
  availability_zone = "us-east-1a"
  key_name = "terraform-key"
  tags = {
    "Name" = "State-Import-Demo"
  }
}
```
## Step-06: Modify EC2 Instance from Terraform
- You have created this EC2 instance manually, now you made it as terraform managed 
- Modify Instance type to `t2.small` and test
```t
# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve
Observation:
1) EC2 Instance Type on AWS Cloud should be changed from t2.micro to t2.small
```

## Step-06: Destroy EC2 Instance from Terraform 
- You have created this EC2 instance manually, now you made it as terraform managed
- Destroy that using terraform
```t
# Destroy Resource
terraform destroy  -auto-approve

# Clean-Up files
rm -rf .terraform*
rm -rf terraform.tfstate*

# Comment Resource Arguments for S3 bucket
Comment Arguments in c2-ec2-instance.tf so that when a student is using the demo, he can uncomment or write it as per their Ec2 Instance settings.
```
## NOT HAPPY - Lets do one more example with AWS S3 Bucket and learn little more about terraform state import


## Step-07: Create AWS S3 bucket manually using AWS mgmt Console
- Go to  Services -> S3 -> **Create Bucket**
- **Bucket Name:** state-import-bucket


## Step-08: Create Basic Terraform Configuration
- **Reference Folder:** v2-s3bucket
- c1-versions.tf
- c2-s3bucket.tf
- Create S3 Bucket Resource - Basic Version to get Terraform `Resource Type` and `Resource Local Name` we are going to use in Terraform
```t
# Create S3 Bucket
resource "aws_s3_bucket" "mybucket" {

}
```
## Step-09:Run Terraform Import Command to import AWS S3 bucket resource to Terraform
- [Terraform S3 Bucket Argument Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket#argument-reference)
```t
# Terraform Initialize
terraform init

# Terraform Import Command for AWS S3 bucket
terraform import aws_s3_bucket.mybucket <BUCKET_NAME>
terraform import aws_s3_bucket.mybucket state-import-bucket
Observation:
1) terraform.tfstate file will be created locally in Terraform working directory
2) Review information about imported instance configuration in terraform.tfstate
```

## Step-10: Start Building local c2-s3bucket.tf
- By referring `terraform.tfstate` file and parallely running `terraform plan` command make changes to your terraform configuration `c2-s3bucket.tf` till you get the message `No changes. Infrastructure is up-to-date` for `terraform plan` output
- For S3 buckets, there will be some default parameters from terraform will be set for two arguments
  - acl =  private
  - force_destroy = false
- Those even though you manually add it, you also need to do `terraform apply` once to make terraform happy.
```t
# Create S3 Bucket
resource "aws_s3_bucket" "mybucket" {
  bucket = "state-import-bucket"
  acl = "private" 
  force_destroy = false # default is false make this to true if any contents in bucket, so in next step you can destroy using terraform
}

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve
```


## Step-11: Destroy S3 Bucket from Terraform 
- You have created this S3 Bucket manually, now you made it as terraform managed
- Destroy that using terraform
```t
# Destroy Resource
terraform destroy  -auto-approve

# Clean-Up files
rm -rf .terraform*
rm -rf terraform.tfstate*

# Comment Resource Arguments for S3 bucket
Comment Arguments in c2-s3bucket.tf so that when a student is using the demo, he can uncomment or write it as per their bucket names and settings.
```======================================================================
***********read the file ./14-Terraform-Graph/README.md ************
# Terraform Graph

## Step-01: Introduction
- The `terraform graph` command is used to generate a visual representation of either a configuration or execution plan
- The output is in the DOT format, which can be used by [GraphViz](http://www.graphviz.org/) to generate charts.

## Step-02: Run Terraform Graph command
```t
# Terraform Initialize
terraform init

# Terraform Graph
terraform graph > dot1
Observation: 
This command will output DOT format text and store in file dot1
```

## Step-03: Online Graphviz Viewers
- [Graphviz-Online](https://dreampuf.github.io/GraphvizOnline/)
- [Edotor-Online](https://edotor.net/)
- Copy and paste the text from `dot1` file generated in step-02 in these online Graphviz Viewers
- Review the output

## Step-04: Clean-Up
```t
# Delete .terraform files
rm -rf .terraform*
```

## Step-05: Other Options - Offline Graphviz Installer
### Step-05-01: Pre-requisite notes
- Graphviz is unstable on MacOS 
- Graphviz needs xcode to be installed on MacOS which consumes huge disk space.
- With that said, we are going to do this demo on Windows Machine
- We are going to use Windows 2019 EC2 instance created on AWS for the same. 

### Step-05-02: Create Windows 2019 VM ready
- Create Windows 2019 VM on AWS Cloud
- Disable Browser security settings in Server-Manager
- Install Google Chrome
- Download & Install Terraform CLI
- Set `Path` for Terraform CLI
- Copy `terraform-manifests` folder from `section-14: Terraform Graph` of the course to Windows VM


### Step-05-03: Install Graphviz on Windows VM
- [Download Graphviz](http://www.graphviz.org/download/)
- Install Graphviz 
```t
# Switch Directory
cd c:\graphviz-demo\terraform-manifests

# Terraform Initialize
terraform init

# Terraform Graph
terraform graph > dot1
Observation: 
This command will output DOT format text

# Terraform Graph in Image format
terraform graph | dot -Tsvg > graph.svg

# Verify
open graph.svg in browser
```


## References
- [Terraform Graph](https://www.terraform.io/docs/cli/commands/graph.html)======================================================================
***********read the file ./15-Terraform-Expressions/15-01-Terraform-Functions/README.md ************
# Terraform Functions

## Step-01: Introduction
- We are going to learn about different Terraform Functions using Terraform console command
- In detail, we are going to learn about `templatefile` and `concat` functions with an AWS example

## Step-02: Numeric Functions
```t
# Terraform Console
terraform console

# Min Function: Takes one or more numbers and returns the smallest number from the set.
min(12, 13, 14)

# Max Function: Takes one or more numbers and returns the greatest number from the set.
max(12, 13, 14)

# pow Function: Calculates an exponent, by raising its first argument to the power of the second argument.
pow(3, 2)
```

## Step-03: String Functions
```t
# Terraform Console
terraform console

# Trim Function: Removes the specified characters from the start and end of the given string.
trim("?!hello?!", "!?")

# Trimprefix Function: Removes the specified prefix from the start of the given string. If the string does not start with the prefix, the string is returned unchanged.
trimprefix("helloworld", "hello")
trimprefix("helloworld", "cat")

# Trimsuffix Function: Removes the specified suffix from the end of the given string.
trimsuffix("helloworld", "world")

# Trimspace Function: Removes any space characters from the start and end of the given string.
trimspace("  hello\n\n")

# Join Function: Produces a string by concatenating together all elements of a given list of strings with the given delimiter
join(separator, list)
join(", ", ["foo", "bar", "baz"])

# Split Function: Produces a list by dividing a given string at all occurrences of a given separator.
split(separator, string)
split(",", "foo,bar,baz")

# Upper Functon: Converts all cased letters in the given string to uppercase.
upper("hello")
```

## Step-04: Collection Functions
```t
# Terraform Console
terraform console

# Concat Function: Takes two or more lists and combines them into a single list.
concat(["a", ""], ["b", "c"])

# Contains Function: Determines whether a given list or set contains a given single value as one of its elements.
contains(list, value)
contains(["a", "b", "c"], "a")
contains(["a", "b", "c"], "d")

# Distinct Function: Takes a list and returns a new list with any duplicate elements removed.
distinct(["a", "b", "a", "c", "d", "b"])

# Length Function: determines the length of a given list, map, or string.
length("hello")
length(["a", "b"])
length(["a", "b"])

# Lookup Function: Retrieves the value of a single element from a map, given its key. If the given key does not exist, the given default value is returned instead.
lookup(map, key, default)
lookup({a="ay", b="bee"}, "a", "what?")
Web: 
lookup({"web" = ["10.0.1.0/24","10.0.2.0/24"], "app" = ["10.0.11.0/24","10.0.12.0/24"], "db" = ["10.0.21.0/24","10.0.22.0/24"]}, "web", ["10.0.51.0/24", "10.0.52.0/24"])

App: 
lookup({"web" = ["10.0.1.0/24","10.0.2.0/24"], "app" = ["10.0.11.0/24","10.0.12.0/24"], "db" = ["10.0.21.0/24","10.0.22.0/24"]}, "app", ["10.0.51.0/24", "10.0.52.0/24"])

DB:
lookup({"web" = ["10.0.1.0/24","10.0.2.0/24"], "app" = ["10.0.11.0/24","10.0.12.0/24"], "db" = ["10.0.21.0/24","10.0.22.0/24"]}, "db", ["10.0.51.0/24", "10.0.52.0/24"])

Default: 
lookup({"web" = ["10.0.1.0/24","10.0.2.0/24"], "app" = ["10.0.11.0/24","10.0.12.0/24"], "db" = ["10.0.21.0/24","10.0.22.0/24"]}, "abcd", ["10.0.51.0/24", "10.0.52.0/24"])



# Merge Function: Takes an arbitrary number of maps or objects, and returns a single map or object that contains a merged set of elements from all arguments.
merge({a="b", c="d"}, {e="f", c="z"})
merge({a="b"}, {a=[1,2], c="z"}, {d=3})
```

## Step-05: Encoding Functions
```t
# Terraform Console
terraform console

# base64decode Function: Takes a string containing a Base64 character sequence and returns the original string.
base64decode("SGVsbG8gV29ybGQ=")

# base64encode Function: Applies Base64 encoding to a string.
base64encode("Hello World")
```

## Step-06: FileSystem Functions
```t
# Terraform Console
terraform console

# File Function: Reads the contents of a file at the given path and returns them as a string.
file("${path.module}/files/hello.txt")

# fileexists Function: Determines whether a file exists at a given path.
fileexists("${path.module}/files/hello.txt")

# templatefile Function: Reads the file at the given path and renders its content as a template using a supplied set of template variables.
templatefile(path, vars)
```

## Step-07: templatefile & concat Function - Review TF Files
- **Reference Folder:** terraform-manifests
### c1-versions.tf
- No changes
### c2-variables.tf
- Added variabled package_name
```t
variable "package_name" {
  description = "Provide Package that need to be installed with user_data"
  type = string
  default = "httpd"
}
```
### c3-security-groups.tf
- No changes
### c4-ec2-instance.tf
- Added `user_data =  templatefile("user_data.tmpl", {package_name = var.package_name})`
```t
# Create EC2 Instance - Amazon2 Linux
resource "aws_instance" "my-ec2-vm" {
  ami           = data.aws_ami.amzlinux.id 
  instance_type = var.instance_type
  key_name      = "terraform-key"
  #user_data = file("apache-install.sh")  
  user_data =  templatefile("user_data.tmpl", {package_name = var.package_name})
  vpc_security_group_ids = [aws_security_group.vpc-ssh.id, aws_security_group.vpc-web.id]
  tags = {
    "Name" = "TF-Functions-Demo-1"
  }
}
```
### c5-outputs.tf
- Added output with `concat` function
```t
# Concat Security Group IDs in Output
output "security_group_ids" {
  value = concat([aws_security_group.vpc-ssh.id, aws_security_group.vpc-web.id])
}
/* Note: This will return the IDs of the security groups attached to your web 
instance as a list. You can use these lists as inputs in submodules.*/
```
### c6-ami-datasource.tf
- No changes
### user_data.tmpl
- Contains the shell script which will install the package provided from terraform variables. 
```sh
#! /bin/bash
sudo yum update -y
sudo yum install -y ${package_name}
sudo yum list installed | grep ${package_name} >> /tmp/package-installed-list.txt
```

## Step-08: Execute Terraform Commands
- Verify the installed packages
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Review Outputs for concat function
security_group_ids = [
  "sg-09f936287ddfc3b14",
  "sg-01f8a08bcbc4b9590",
]

# Connect to EC2 VM
ssh -i private-key/terraform-key ec2-user@<PUBLIC-IP>
cat /tmp/package-installed-list.txt
```

## Step-09: Clean-Up
```t
# Destroy Resources
terraform destory -auto-approve

# Delete Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-10: Other Function Categories
- [Date and Time Functions](https://www.terraform.io/docs/language/functions/formatdate.html)
- [Hash and Crypto Functions](https://www.terraform.io/docs/language/functions/base64sha256.html)
- [IP Network Functions](https://www.terraform.io/docs/language/functions/cidrhost.html)
- [Type Conversion Functions](https://www.terraform.io/docs/language/functions/can.html)



## References
- [Terraform Functions](https://www.terraform.io/docs/language/functions/index.html)
======================================================================
***********read the file ./15-Terraform-Expressions/15-02-Terraform-Dynamic-Expressions/README.md ************
# Terraform Dynamic Expressions

## Step-01: Introduction
- Learn [Dynamic Expressions](https://www.terraform.io/docs/language/expressions/conditionals.html) in Terraform
- **Conditional Expression:** A conditional expression uses the value of a bool expression to select one of two values.
```t
# Example-1
condition ? true_val : false_val

# Example-2
var.a != "" ? var.a : "default-a"
```
- **Splat Expression:** A `splat expression` provides a more concise way to express a common operation that could otherwise be performed with a `for` expression.
- The special [*] symbol iterates over all of the elements of the list given to its left and accesses from each one the attribute name given on its right. 
```t
# With for expression
[for o in var.list : o.id]

# With Splat Expression [*]
var.list[*].id
```
- A splat expression can also be used to access attributes and indexes from lists of complex types by extending the sequence of operations to the right of the symbol:
```t
var.list[*].interfaces[0].name
aws_instance.example[*].id
```


## Step-02: Review Terraform Manifests
### c1-versions.tf
- Added new random provider in `required_providers` block
### c2-vairables.tf
  - Added new variables
  - availability_zones
  - name
  - team
  - high_availability
### c3-security-groups.tf
  - Added common tags
### c4-ec2-instance.tf
- Added Random ID resource block
- Added new locals block
- **Important Note:** Inside locals block we can add conditional expressions as below. 
```t
# Create Locals
locals {
  #name = var.name
  name  = (var.name != "" ? var.name : random_id.id.hex)
  owner = var.team
  common_tags = {
    Owner = local.owner
    DefaultUser = local.name 
  }
}
```
- Added Availability zone argument with count.index
- We will discuss about following conditional expressions here
```t
# In Locals Block: Conditional Expression
name  = (var.name != "" ? var.name : random_id.id.hex)

# In EC2 Resource Block: Conditional Expression
count = (var.high_availability == true ? 2 : 1)

# In EC2 Resource Block: count.index
availability_zone = var.availability_zones[count.index]
```
### c5-outputs.tf
- Added Splat expression [*] for all outputs
- Added Common Tags and ELB DNS Name as new outputs

### c6-ami-datasource.tf
- No changes

### c7-elb.tf
- Added this new resource
- We will be creating ELB only if "high_availability" variable value is true else it will not be created
```t
# Create ELB if high_availability=true
# In ELB Block: Conditional Expression
count = (var.high_availability == true ? 1 : 0)
```  

## Step-03: Execute Terraform Commands
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan: When Variable values, high_availability = false and name = "ec2-user"
terraform plan
Observation: 
1) Plan will generate for only 1 EC2 instance and 2 Security Groups.
2) ELB Resource will not be created with these variable options


# Terraform Plan: When Variable values, high_availability = true and name = ""
terraform plan
1) Plan will generate for only 2 EC2 instance, 2 Security Groups and 1 ELB
2) ELB Resource will be created with these variable options
3) name value will be a random value known after terraform apply completed

# Terraform Apply
terraform apply -auto-approve

# Verify
1) Verify Outputs
2) Verify EC2 Instances & Security Groups & Common Tags
3) Verify ELB & Common Tags for ELB
4) Access App using ELB DNS Name
```

## Step-04: Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*

# Uncomment and Comment right values in c2-variables.tf (Roll back to put ready for student demo)
high_availability = false 
name = "ec2-user"
```

======================================================================
***********read the file ./15-Terraform-Expressions/15-03-Terraform-Dynamic-Blocks/README.md ************
# Terraform Dynamic Blocks

## Step-01: Introduction
- Some resource types include repeatable nested blocks in their arguments, which do not accept expressions
- You can dynamically construct repeatable nested blocks like setting using a special dynamic block type, which is supported inside resource, data, provider, and provisioner blocks

## Step-02: Create / Review Terraform manifests
### c1-versions.tf
- Standard file without any changes
- `region` in provider block is hard-coded to `us-east-1`
### c2-security-groups-regular.tf
```t
resource "aws_security_group" "sg-regular" {
  name        = "demo-regular"
  description = "demo-regular"

  ingress {
    description = "description 0"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    description = "description 1"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    description = "description 2"
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    description = "description 3"
    from_port   = 8081
    to_port     = 8081
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    description = "description 4"
    from_port   = 7080
    to_port     = 7080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }      
  ingress {
    description = "description 5"
    from_port   = 7081
    to_port     = 7081
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }      
}
```

### c3-dynamic-blocks-for-security-groups.tf
- ingress.key = 0 and ingress.value = 80
- ingress.key = 1 and ingress.value = 443
- ingress.key = 2 and ingress.value = 8080  ....
```t
# Define Ports as a list in locals block
locals {
  ports = [80, 443, 8080, 8081, 7080, 7081]
}

# Create Security Group using Terraform Dynamic Block
resource "aws_security_group" "sg-dynamic" {
  name        = "dynamic-block-demo"
  description = "dynamic-block-demo"

  dynamic "ingress" {
    for_each = local.ports
    content {
      description = "description ${ingress.key}"
      from_port   = ingress.value
      to_port     = ingress.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
}
```

## Step-03: Execute Terraform Commands
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve
```

## Step-04: Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```======================================================================
***********read the file ./16-Terraform-Debug/README.md ************
# Terraform Debug

## Step-01: Introduction
- Learn about Terraform Debug
- TF_LOG & TF_LOG_PATH
- TF_LOG - Allowed Values or Desired Log Levels
- **TRACE:** Very detailed verbosity, shows every step taken by Terraform and produces enormous outputs with internal logs.
- **DEBUG:** describes what happens internally in a more concise way compared to TRACE.
- **ERROR:** shows errors that prevent Terraform from continuing.
- **WARN:** logs warnings, which may indicate misconfiguration or mistakes, but are not critical to execution
- **INFO:** shows general, high-level messages about the execution process.
- **Important Note:** 


## Step-02: Setup Trace logging in Terraform
```t
# Terrafrom Trace Log Settings
export TF_LOG=TRACE
export TF_LOG_PATH="terraform-trace.log"
echo $TF_LOG
echo $TF_LOG_PATH

# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Terraform Destroy
terraform destroy -auto-approve

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*
rm terraform-trace.log
```


## Step-03: Setup these Environment Variables permanently in your desktops
### Linux Bash
- Open your `.bashrc` which is located in your $home directory 
```t
# Linux Bash
cd $HOME
vi .bashrc

# Terraform log settings
export TF_LOG=TRACE
export TF_LOG_PATH="terraform-trace.log"

# Verify after saving the file in new terminal 
$ echo $TF_LOG
TRACE
$ echo $TF_LOG_PATH
terraform-trace.log
```
### Windows Powershell
- Setup using Powershell profile
- Open `$profile` command in a PowerShell
- Once that file is opened add the following lines.
- Now close and reopen the console and type the following to verify that it worked.
```t
# Windows Powershell - Terraform log settings
$env:TF_LOG="TRACE"
$env:TF_LOG_PATH="terraform.txt"

# Open new powershell window & Verify
echo $env:TF_LOG
echo $env:TF_LOG_PATH
```
### MAC OS
- Update the values in `.bash_profile` at the end of file
```t
# MAC OS
cd $HOME
vi .bash_profile

# Terraform log settings
export TF_LOG=TRACE
export TF_LOG_PATH="terraform-trace.log"

# Verify after saving the file in new terminal 
$ echo $TF_LOG
TRACE
$ echo $TF_LOG_PATH
terraform-trace.log
```

## Step-04: Terraform Crash Log
- If Terraform ever crashes (a "panic" in the Go runtime), it saves a log file with the debug logs from the session as well as the panic message and backtrace to `crash.log`.
- Generally speaking, this log file is meant to be passed along to the developers via a GitHub Issue. 
- As a user, you're not required to dig into this file.
- [How to read a crash log?](https://www.terraform.io/docs/internals/debugging.html#interpreting-a-crash-log)======================================================================
***********read the file ./17-Exam-Preparation/README.md ************
# Exam Preparation

## Step-00: Don't Stop Get Certified
- If you have completed all the sections from 01 to 16, go ahead and get certified. 
- You will definitely pass the exam with 70 to 85% score with above knowledge.
- If you expect to score more and more, then you also need to go through below listed Terraform materials once

## Step-01: Core Topics
1. Understand infrastructure as code (IaC) concepts
2. Understand Terraform's purpose (vs other IaC)
3. Understand Terraform basics
4. Use the Terraform CLI (outside of core workflow)
5. Interact with Terraform modules
6. Navigate Terraform workflow
7. Implement and maintain state
8. Read, generate, and modify configuration
9. Understand Terraform Cloud and Enterprise capabilities

## Step-02: Questions Break down in real exam
- Total: 57 Questions
- 40 to 45 questions mostly will be straight forward, concept oriented questions which we can answer if we implemented above 50 practicals
  - Above all the sections in this course will mostly cover you. 
- 12 questions will be absolute practical oriented about asking some basic commands, asking about how to solve this error message etc. 
  - Practicals above will be hands-on which gives the ability to solve such questions
  - Out of these 12, 4 to 5 might be super tricky but don't worry about them. 


## Step-03: Review Terraform Guides
- [Study Guide](https://learn.hashicorp.com/tutorials/terraform/associate-study)
- [Review Guide](https://learn.hashicorp.com/tutorials/terraform/associate-review)


## Step-04: Review Terraform Language Documentation
- [Terraform Language Guide](https://www.terraform.io/docs/language/index.html)
- Go through this on high-level. Quick walk-through will suffice======================================================================
***********read the file ./18-Exam-Registration/README.md ************
# Exam Registration

## Step-01: Exam Registration
- Exam is conducted on PSI Exam platform
- **Pre-requisite-1:** You should already have a account on `github.com` so you can use it. The email you have used for github, you should have access to that same email id to check emails. Hashicorp via PSI Exams will communicate on that email id. 
- **Pre-requisite-2:** Create your account on  [youracclaim](https://www.youracclaim.com/) with same email id so when you have completed your exam and certified, your badge will be posted to **youracclaim** so you can share the same in social media platforms like linkedin, and also have a record of your badge online. 
- [Register for Exam](https://hashicorp-certifications.zendesk.com/hc/en-us/articles/360049382552)
- Once registered, email confirmation will be sent to you. 

## Step-02: Review Exam Taker Handbook
- [Exam Taker Handbook](https://hashicorp-certifications.zendesk.com/hc/en-us/articles/360048211571)

## Step-03: System Requirements
- Very very important. 
- Do all the things a day before
- [System Requirements](https://hashicorp-certifications.zendesk.com/hc/en-us/articles/360048446631)
- [Click here to test your system's compatibility](https://syscheck.bridge.psiexams.com/)
- If you are on MAC, you need to provide additional permissions when it asks to run system check like
- Connect Webcam or your laptop inbuilt cam
- Connect Microphone (No Head-Phones)
- Connect wired ehternet line to your laptop or dekstop for good internet speed
- As a fail back internet, enable mobile hotspot on your mobile and put that outside your room to avoid distractions
- Install PSI Secure Browser

## Step-04: Exam Day
- Login 45 mins before to PSI Exam platform
- Exam Launch URL will get enabled 30 mins before scheduled Time
- Complete the checks 
### Step-04-01: First Time checks
- System will ask you for these checks
1. Identity Card Verification
2. 360 Degree Room View
3. Hand Sleeves and Ears check

### Step-04-02: Second Time Checks
- Online Proctor will communicate with you chat and perform these checks
1. Identity Card Verification
2. 360 Degree Room View
3. Hand Sleeves and Ears check

### Step-04-03: Proctor will Release the Exam for you to Start
1. Don't tense.
2. Be cool and start answering questions
3. If you have any doubt about any question, Click on **Flag Button** so we can come back later
4. Don't Flag many questions later difficult to check all of them. 
5. Exam Time: 60 Minutes Questions: 57
6. I took 35 minutes to complete all the questions due to the fact most of the questions (40 to 45) will be straight forward. 
7. Review the questions which you flagged and answer them. 
8. Complete the exam
9. Complete the Survey questions
10. You will get the status of your exam (pass / fail)
11. Email will be sent to you by **youracclaim** with your badge information
12. Email will be sent to you by **HashiCorp** about your score report 
======================================================================
***********read the file ./BACKUP-2024/01-Infrastructure-as-Code-IaC-Basics/README.md ************
# Infrastructure as Code Basics

## Step-01: Understand Problems with Traditional way of Managing Infrastructure
- Time it takes for building multiple environments
- Issues we face with different environments
- Scale-Up and Scale-Down On-Demand

## Step-02: Discuss how IaC with Terraform Solves them
- Visibility
- Stability
- Scalability
- Security
- Audit======================================================================
***********read the file ./BACKUP-2024/02-Terraform-Basics/02-01-Install-Tools-TerraformCLI-AWSCLI-VSCodeIDE/README.md ************
# Terraform & AWS CLI Installation

## Step-01: Introduction
- Install Terraform CLI
- Install AWS CLI
- Install VS Code Editor
- Install HashiCorp Terraform plugin for VS Code


## Step-02: MACOS: Terraform Install
- [Download Terraform MAC](https://www.terraform.io/downloads.html)
- [Install CLI](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- Unzip the package
```
# Copy binary zip file to a folder
mkdir /Users/<YOUR-USER>/Documents/terraform-install
COPY Package to "terraform-install" folder

# Unzip
unzip <PACKAGE-NAME>
unzip terraform_0.14.3_darwin_amd64.zip

# Copy terraform binary to /usr/local/bin
echo $PATH
mv terraform /usr/local/bin

# Verify Version
terraform version

# To Uninstall Terraform (NOT REQUIRED)
rm -rf /usr/local/bin/terraform
``` 

## Step-03: MACOS: IDE for Terraform - VS Code Editor
- [Microsoft Visual Studio Code Editor](https://code.visualstudio.com/download)
- [Hashicorp Terraform Plugin for VS Code](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)


### Step-04: MACOS: Install AWS CLI
- [AWS CLI Install](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
- [Install AWS CLI - MAC](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-mac.html#cliv2-mac-install-cmd)

```
# Install AWS CLI V2
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
which aws
aws --version

# Uninstall AWS CLI V2 (NOT REQUIRED)
which aws
ls -l /usr/local/bin/aws
sudo rm /usr/local/bin/aws
sudo rm /usr/local/bin/aws_completer
sudo rm -rf /usr/local/aws-cli
```


## Step-05: MACOS: Configure AWS Credentials 
- **Pre-requisite:** Should have AWS Account.
  - [Create an AWS Account](https://portal.aws.amazon.com/billing/signup?nc2=h_ct&src=header_signup&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start)
- Generate Security Credentials using AWS Management Console
  - Go to Services -> IAM -> Users -> "Your-Admin-User" -> Security Credentials -> Create Access Key
- Configure AWS credentials using SSH Terminal on your local desktop
```
# Configure AWS Credentials in command line
$ aws configure
AWS Access Key ID [None]: AKIASUF7DEFKSIAWMZ7K
AWS Secret Access Key [None]: WL9G9Tl8lGm7w9t7B3NEDny1+w3N/K5F3HWtdFH/
Default region name [None]: us-east-1
Default output format [None]: json

# Verify if we are able list S3 buckets
aws s3 ls
```
- Verify the AWS Credentials Profile
```
cat $HOME/.aws/credentials 
```

## Step-06: WindowsOS: Terraform & AWS CLI Install
- [Download Terraform](https://www.terraform.io/downloads.html)
- [Install CLI](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- Unzip the package
- Create new folder `terraform-bins`
- Copy the `terraform.exe` to a `terraform-bins`
- Set PATH in windows 
- Install [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)

## Step-07: LinuxOS: Terraform & AWS CLI Install
- [Download Terraform](https://www.terraform.io/downloads.html)
- [Linux OS - Terraform Install](https://learn.hashicorp.com/tutorials/terraform/install-cli)======================================================================
***********read the file ./BACKUP-2024/02-Terraform-Basics/02-02-Terraform-Command-Basics/README.md ************
# Terraform Command Basics

## Step-01: Introduction
- Understand basic Terraform Commands
  - terraform init
  - terraform validate
  - terraform plan
  - terraform apply
  - terraform destroy      

## Step-02: Review terraform manifest for EC2 Instance
- **Pre-Conditions-1:** Ensure you have **default-vpc** in that respective region
- **Pre-Conditions-2:** Ensure AMI you are provisioning exists in that region if not update AMI ID 
- **Pre-Conditions-3:** Verify your AWS Credentials in **$HOME/.aws/credentials**
```t
# Terraform Settings Block
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      #version = "~> 3.21" # Optional but recommended in production
    }
  }
}

# Provider Block
provider "aws" {
  profile = "default" # AWS Credentials Profile configured on your local desktop terminal  $HOME/.aws/credentials
  region  = "us-east-1"
}

# Resource Block
resource "aws_instance" "ec2demo" {
  ami           = "ami-04d29b6f966df1537" # Amazon Linux in us-east-1, update as per your region
  instance_type = "t2.micro"
}
```

## Step-03: Terraform Core Commands
```t
# Initialize Terraform
terraform init

# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
```

## Step-04: Verify the EC2 Instance in AWS Management Console
- Go to AWS Management Console -> Services -> EC2
- Verify newly created EC2 instance



## Step-05: Destroy Infrastructure
```t
# Destroy EC2 Instance
terraform destroy

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-08: Conclusion
- Re-iterate what we have learned in this section
- Learned about Important Terraform Commands
  - terraform init
  - terraform validate
  - terraform plan
  - terraform apply
  - terraform destroy     



======================================================================
***********read the file ./BACKUP-2024/02-Terraform-Basics/02-03-Terraform-Language-Syntax/README.md ************
# Terraform Configuration Language Syntax

## Step-01: Introduction
- Understand Terraform Language Basics
  - Understand Blocks
  - Understand Arguments, Attributes & Meta-Arguments
  - Understand Identifiers
  - Understand Comments
  


## Step-02: Terraform Configuration Language Syntax
- Understand Blocks
- Understand Arguments
- Understand Identifiers
- Understand Comments
- [Terraform Configuration](https://www.terraform.io/docs/configuration/index.html)
- [Terraform Configuration Syntax](https://www.terraform.io/docs/configuration/syntax.html)
```t
# Template
<BLOCK TYPE> "<BLOCK LABEL>" "<BLOCK LABEL>"   {
  # Block body
  <IDENTIFIER> = <EXPRESSION> # Argument
}

# AWS Example
resource "aws_instance" "ec2demo" { # BLOCK
  ami           = "ami-04d29b6f966df1537" # Argument
  instance_type = var.instance_type # Argument with value as expression (Variable value replaced from varibales.tf
}
```

## Step-03: Understand about Arguments, Attributes and Meta-Arguments.
- Arguments can be `required` or `optional`
- Attribues format looks like `resource_type.resource_name.attribute_name`
- Meta-Arguments change a resource type's behavior (Example: count, for_each)
- [Additional Reference](https://learn.hashicorp.com/tutorials/terraform/resource?in=terraform/configuration-language) 
- [Resource: AWS Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
- [Resource: AWS Instance Argument Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#argument-reference)
- [Resource: AWS Instance Attribute Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#attributes-reference)
- [Resource: Meta-Arguments](https://www.terraform.io/docs/language/meta-arguments/depends_on.html)

## Step-04: Understand about Terraform Top-Level Blocks
- Discuss about Terraform Top-Level blocks
  - Terraform Settings Block
  - Provider Block
  - Resource Block
  - Input Variables Block
  - Output Values Block
  - Local Values Block
  - Data Sources Block
  - Modules Block

======================================================================
***********read the file ./BACKUP-2024/03-Terraform-Fundamental-Blocks/03-01-Terraform-Block/README.md ************
# Terraform Block 

## Step-01: Introduction
- Understand about Terraform Block and its importance
- Understand how to handle version constraints for Terraform Version and Provider Version in Terraform Block

### Step-02: Understand about Terraform Settings Block
- Required Terraform Version
- Provider Requirements
- Terraform backends
- Experimental Language Features
- Passing Metadata to Providers
- Review the file named **sample-terraform-settings.tf** for more understading

## Step-03: Create a simple terraform block and play with required_version
- `required_version` focuses on underlying Terraform CLI installed on your desktop
- If the running version of Terraform on your local desktop doesn't match the constraints specified in your terraform block, Terraform will produce an error and exit without taking any further actions.
- By changing the versions try `terraform init` and observe whats happening
```
Play with Terraform Version
  required_version = "~> 0.14.3" 
  required_version = "= 0.14.3"    
  required_version = "= 0.14.4"  
  required_version = ">= 0.13"   
  required_version = "= 0.13"    
  required_version = "~> 0.13"   
 

# Terraform Block
terraform {
  required_version = "~> 0.14"
}

# To view my Terraform CLI Version installed on my desktop
terraform version

# Initialize Terraform
terraform init
```
## Step-04: Add Provider and play with Provider version
- `required_providers` block specifies all of the providers required by the current module, mapping each local provider name to a source address and a version constraint.

```
Play with Provider Version
      version = "~> 3.0"            
      version = ">= 3.0.0, < 3.1.0"
      version = ">= 3.0.0, <= 3.1.0"
      version = "~> 2.0"
      version = "~> 3.0"   
```

```
# Terraform Init with upgrade option to change provider version
terraform init -upgrade
```


## Step-05: Clean-Up
```
# Delete Terraform Folders & Files
rm -rf .terraform*
```

## References
- [Terraform Version Constraints](https://www.terraform.io/docs/configuration/version-constraints.html)
- [Terraform Versions - Best Practices](https://www.terraform.io/docs/configuration/version-constraints.html#best-practices)

======================================================================
***********read the file ./BACKUP-2024/03-Terraform-Fundamental-Blocks/03-02-Provider-Block/README.md ************
# Terraform Provider Block

## Step-01: Introduction
- What are Terraform Providers?
- What Providers Do?
- Where do providers reside (Terraform Registry)?
- How to use Providers?
- What are Provider Badges?


## Step-02: Terraform Providers
- What are Terraform Providers?
- What Providers Do?
- Where do providers reside (Terraform Registry)?


## Step-03: Provider Requirements
- Define required providers in Terraform Block
- Understand about three things about defining `required_providers` in terraform block
  - local names
  - source
  - version
```t
# Terraform Block
terraform {
  required_version = "~> 0.14.4"
  required_providers {
    aws = { 
      source = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
}
```


## Step-04: Provider Block  
- Create a Provider Block for AWS
```t
# Provider Block
provider "aws" {
  region = "us-east-1"
  profile = "default"  # defining it is optional for default profile
}
```
- Discuss about [Authentication Types](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication) 
- Static Credentials - NOT A RECOMMENDED Option
- Environment variables
- Shared credentials/configuration file ( We are going to use this)
  - Verify your `cat $HOME/.aws/credentials`
  - If not, use `aws configure` to configure the aws credentials

```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration files
terraform validate

# Execute Terraform Plan
terraform plan
```  

## Step-05: Add a Resource Block to create a AWS VPC
- [AWS VPC Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)
```t
resource "aws_vpc" "myvpc" {
  cidr_block = "10.0.0.0/16"
  tags = {
    "Name" = "myvpc"
  }
}
```

## Step-06: Execute Terraform commands to create AWS VPC
```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration files
terraform validate

# Execute Terraform Plan
terraform plan

# Create Resources using Terraform Apply
terraform apply -auto-approve
```  

## Step-07: Clean-Up 
```t
# Destroy Terraform Resources
terraform destroy -auto-approve

# Delete Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```


## References
- [Terraform Providers](https://www.terraform.io/docs/configuration/providers.html)
- [AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS VPC](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)======================================================================
***********read the file ./BACKUP-2024/03-Terraform-Fundamental-Blocks/03-03-Multiple-Provider-Configurations/README.md ************
# Multiple Provider Configurations

## Step-01: Introduction
- Understand and Implement Multiple Provider Configurations

## Step-02: How to define multiple provider configuration of same Provider?
- Understand about default provider
- Understand and define multiple provider configurations of same provider
```t
# Provider-1 for us-east-1 (Default Provider)
provider "aws" {
  region = "us-east-1"
  profile = "default"
}

# Provider-2 for us-west-1
provider "aws" {
  region = "us-west-1"
  profile = "default"
  alias = "aws-west-1"
}
```

## Step-03: How to reference the non-default provider configuration in a resource?
```t
# Resource Block to Create VPC in us-west-1
resource "aws_vpc" "vpc-us-west-1" {
  cidr_block = "10.2.0.0/16"
  #<PROVIDER NAME>.<ALIAS>
  provider = aws.aws-west-1
  tags = {
    "Name" = "vpc-us-west-1"
  }
}
```

## Step-04: Execute Terraform Commands
```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Generate Terraform Plan
terraform plan

# Create Resources
terraform apply -auto-approve

# Verify the same
1. Verify the VPC created in us-east-1
2. Verify the VPC created in us-west-2
```

## Step-05: Clean-Up 
```t
# Destroy Terraform Resources
terraform destroy -auto-approve

# Delete Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```



## References
- [Provider Meta Argument](https://www.terraform.io/docs/configuration/meta-arguments/resource-provider.html)======================================================================
***********read the file ./BACKUP-2024/03-Terraform-Fundamental-Blocks/03-04-Providers-Dependency-Lock-File/README.md ************
# Providers - Dependency Lock File

## Step-01: Introduction
- Understand the importance of Dependency Lock File which is introduced in Terraform v0.14

## Step-02: Review the Terraform Manifests
- c1-versions.tf
  - Discuss about Terraform, AWS and Random Pet Provider Versions
- c2-s3bucket.tf
  - Discuss about Random Pet Resource
  - Discuss about S3 Bucket Resource
- .terraform.lock.hcl
  - Discuss about Provider Version, Version Constraints and Hashes

## Step-03: Initialize and apply the configuration
```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration files
terraform validate

# Execute Terraform Plan
terraform plan

# Create Resources using Terraform Apply
terraform apply
```
- Review **.terraform.lock.hcl**
  - Discuss about versions
  - Compare `.terraform.lock.hcl-ORIGINAL` & `.terraform.lock.hcl`
  - Backup `.terraform.lock.hcl` as `.terraform.lock.hcl-FIRST-INIT` 
```
# Backup
cp .terraform.lock.hcl .terraform.lock.hcl-FIRST-INIT
```

## Step-04: Upgrade the AWS provider version
- For AWS Provider, with version constraint `version = ">= 2.0.0"`, it is going to upgrade to latest `3.x.x` version with command `terraform init -upgrade` 
```t
# Upgrade Provider Version
terraform init -upgrade
```
- Review **.terraform.lock.hcl**
  - Discuss about AWS Versions
  - Compare `.terraform.lock.hcl-FIRST-INIT` & `.terraform.lock.hcl`

## Step-05: Apply Terraform configuration with latest AWS Provider
- Should fail due to S3 related latest changes came in AWS v3.x provider when compared to AWS v2.x provider
```
# Terraform Apply
terraform apply
```

## Step-06: Comment region in S3 Resource and try Terraform Apply
- It should work. 
- When we do a major version upgrade to providers, it might break few features. 
- So with `.terraform.lock.hcl`, we can avoid this type of issues.
```
# Comment Region Attribute
# Resource Block: Create AWS S3 Bucket
resource "aws_s3_bucket" "sample" {
  bucket = random_pet.petname.id
  acl    = "public-read"

  #region = "us-west-2"
}

# Terraform Apply
terraform apply
```

## Step-07: Clean-Up
```
# Destroy Resources
terraform destroy

# Delete Terraform Files
rm -rf .terraform    # We are not removing files named ".terraform.lock.hcl, .terraform.lock.hcl-ORIGINAL" which are needed for this demo for you.
rm -rf terraform.tfstate*
```

## References
- [Random Pet Provider](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/pet)
- [Dependency Lock File](https://www.terraform.io/docs/configuration/dependency-lock.html)
- [Terraform New Features in v0.14](https://learn.hashicorp.com/tutorials/terraform/provider-versioning?in=terraform/0-14)
- [AWS S3 Bucket Region - Read Only in AWS Provider V3.x](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/guides/version-3-upgrade#region-attribute-is-now-read-only)======================================================================
***********read the file ./BACKUP-2024/04-Terraform-Resources/04-01-Resource-Syntax-and-Behavior/README.md ************
# Terraform Resource Syntax & Behavior

## Step-01: Introduction
- Understand Resource Syntax
- Understand Resource Behavior
- Understanding Terraform State File
  - terraform.tfstate
- Understanding Desired and Current States (High Level Only)

## Step-02: Understand Resource Syntax
- We are going to understand about below concepts from Resource Syntax perspective
- Resource Block
- Resource Type
- Resource Local Name
- Resource Arguments
- Resource Meta-Arguments

## Step-03: Understand Resource Behavior
- We are going to understand resource behavior in combination with Terraform State
  - Create Resource
  - Update in-place Resources
  - Destroy and Re-create Resources
  - Destroy Resource  


## Step-04: Resource: Create Resource: Create EC2 Instance
```
# Initialize Terraform
terraform init

Observation: 
1) Successfully downloaded providers in .terraform folder
2) Created lock file named ".terraform.lock.hcl"

# Validate Terraform configuration files
terraform validate
Observation: No files changed / added in current working directory

# Format Terraform configuration files
terraform fmt
Observations: *.tf files will change to format them if any format changes exists

# Review the terraform plan
terraform plan 
Observation-1: Nothing happens during the first run from terraform state perspective
Observation-2: From Resource Behavior perspective you can see "+ create", we are creating 

# Create Resources 
terraform apply -auto-approve
Observation: 
1) Creates terraform.tfstate file in local working directory
2) Creates actual resource in AWS Cloud
```
- **Important Note:** Here we have seen example for **Create Resource**


## Step-05: Understanding Terraform State File
- What is Terraform State ? 
  - It is the primary core thing for terraform to function
  - In a short way, its the underlying database containing the resources information which are provisioning using Terraform
  - **Primary Purpose:** To store bindings between objects in a remote system and resource instances declared in your configuration. 
  - When Terraform creates a remote object in response to a change of configuration, it will record the identity of that remote object against a particular resource instance, and then potentially update or delete that object in response to future configuration changes.
- Terraform state file created when we first run the `terraform apply`
- Terraform state file is created locally in working directory.
- If required, we can confiure the `backend block` in `terraform block` which will allow us to store state file remotely.  Storing remotely is recommended option which we will see in the next section of the course. 

## Step-06: Review terraform.tfstate file
- Terraform State files are JSON based
- Manual editing of Terraform state files is highly not recommended
- Review `terraform.tfstate` file step by step


## Step-07: Resource: Update In-Place: Make changes by adding new tag to EC2 Instance 
- Add a new tag in `c2-ec2-instance.tf`
```
# Add this for EC2 Instance tags
    "tag1" = "Update-test-1"
```
- Review Terraform Plan
```
# Review the terraform plan
terraform plan 
Observation: You should see "~ update in-place" 
"Plan: 0 to add, 1 to change, 0 to destroy."

# Create / Update Resources 
terraform apply -auto-approve
Observation: "Apply complete! Resources: 0 added, 1 changed, 0 destroyed."
```
- **Important Note:** Here we have seen example for **update in-place**


## Step-08: Resource: Destroy and Re-create Resources: Update Availability Zone
- This will destroy the EC2 Instance in 1 AZ and re-creates in other AZ
```
# Before
  availability_zone = "us-east-1a"

# After
  availability_zone = "us-east-1b"  
```
```
# Review the terraform plan
terraform plan 
Observation: 
1) -/+ destroy and then create replacement
2) # aws_instance.my-ec2-vm must be "replaced"
3) # aws_instance.my-ec2-vm must be "replaced" - This parameter forces replacement
4) "Plan: 1 to add, 0 to change, 1 to destroy."

# Create / Update Resources 
terraform apply -auto-approve
Observation: "Apply complete! Resources: 1 added, 0 changed, 1 destroyed."
```


## Step-09: Resource: Destroy Resource
```
# Destroy Resource
terraform destroy 
Observation: 
1) - destroy
2) # aws_instance.my-ec2-vm will be destroyed
3) Plan: 0 to add, 0 to change, 1 to destroy
4) Destroy complete! Resources: 1 destroyed
```

## Step-10: Understand Desired and Current States (High-Level Only)
- **Desired State:** Local Terraform Manifest (All *.tf files)
- **Current State:**  Real Resources present in your cloud

## Step-11: Clean-Up
```
# Destroy Resource
terraform destroy -auto-approve 

# Remove Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## References
- [Terraform State](https://www.terraform.io/docs/language/state/index.html)
- [Manipulating Terraform State](https://www.terraform.io/docs/cli/state/index.html)======================================================================
***********read the file ./BACKUP-2024/04-Terraform-Resources/04-02-Meta-Argument-depends_on/README.md ************
# Terraform Resource Meta-Argument depends_on

## Step-01: Introduction
- Create 9 aws resources in a step by step manner
- Create Terraform Block
- Create Provider Block
- Create 9 Resource Blocks
  - Create VPC
  - Create Subnet
  - Create Internet Gateway
  - Create Route Table
  - Create Route in Route Table for Internet Access
  - Associate Route Table with Subnet
  - Create Security Group in the VPC with port 80, 22 as inbound open
  - Create EC2 Instance in respective new vpc, new subnet created above with a static key pair, associate Security group created earlier
  - Create Elastic IP Address and Associate to EC2 Instance
  - Use `depends_on` Resource Meta-Argument attribute when creating Elastic IP  

## Step-02: Pre-requisite - Create a EC2 Key Pair
- Create EC2 Key pair `terraform-key` and download pem file and put ready for SSH login

## Step-03: c1-versions.tf - Create Terraform & Provider Blocks 
- Create Terraform Block
- Create Provider Block
```
# Terraform Block
terraform {
  required_version = ">= 1.4" 
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}

# Provider Block
provider "aws" {
  region = "us-east-1"
  profile = "default"
}
```
## Step-04: c2-vpc.tf - Create VPC Resources 
### Step-04-01: Create VPC using AWS Management Console
- Create VPC Manually and understand all the resources we are going to create. Delete that VPC and start writing the VPC template using terraform
### Step-04-02: Create VPC using Terraform
- Create VPC Resources listed below  
  - Create VPC
  - Create Subnet
  - Create Internet Gateway
  - Create Route Table
  - Create Route in Route Table for Internet Access
  - Associate Route Table with Subnet
  - Create Security Group in the VPC with port 80, 22 as inbound open
```
# Resource Block
# Resource-1: Create VPC
resource "aws_vpc" "vpc-dev" {
  cidr_block = "10.0.0.0/16"

  tags = {
    "name" = "vpc-dev"
  }
}

# Resource-2: Create Subnets
resource "aws_subnet" "vpc-dev-public-subnet-1" {
  vpc_id = aws_vpc.vpc-dev.id
  cidr_block = "10.0.1.0/24"
  availability_zone = "us-east-1a"
  map_public_ip_on_launch = true
}


# Resource-3: Internet Gateway
resource "aws_internet_gateway" "vpc-dev-igw" {
  vpc_id = aws_vpc.vpc-dev.id
}

# Resource-4: Create Route Table
resource "aws_route_table" "vpc-dev-public-route-table" {
  vpc_id = aws_vpc.vpc-dev.id
}

# Resource-5: Create Route in Route Table for Internet Access
resource "aws_route" "vpc-dev-public-route" {
  route_table_id = aws_route_table.vpc-dev-public-route-table.id 
  destination_cidr_block = "0.0.0.0/0"
  gateway_id = aws_internet_gateway.vpc-dev-igw.id 
}


# Resource-6: Associate the Route Table with the Subnet
resource "aws_route_table_association" "vpc-dev-public-route-table-associate" {
  route_table_id = aws_route_table.vpc-dev-public-route-table.id 
  subnet_id = aws_subnet.vpc-dev-public-subnet-1.id
}

# Resource-7: Create Security Group
resource "aws_security_group" "dev-vpc-sg" {
  name = "dev-vpc-default-sg"
  vpc_id = aws_vpc.vpc-dev.id
  description = "Dev VPC Default Security Group"

  ingress {
    description = "Allow Port 22"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "Allow Port 80"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    description = "Allow all ip and ports outboun"
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

}
```

## Step-05: c3-ec2-instance.tf - Create EC2 Instance Resource
- Review `apache-install.sh`
```sh
#! /bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo service httpd start  
sudo systemctl enable httpd
echo "<h1>Welcome to StackSimplify ! AWS Infra created using Terraform in us-east-1 Region</h1>" > /var/www/html/index.html
```
- Create EC2 Instance Resource
```
# Resource-8: Create EC2 Instance
resource "aws_instance" "my-ec2-vm" {
  ami = "ami-0be2609ba883822ec" # Amazon Linux
  instance_type = "t2.micro"
  subnet_id = aws_subnet.vpc-dev-public-subnet-1.id
  key_name = "terraform-key"
	#user_data = file("apache-install.sh")
  user_data = <<-EOF
    #!/bin/bash
    sudo yum update -y
    sudo yum install httpd -y
    sudo systemctl enable httpd
    sudo systemctl start httpd
    echo "<h1>Welcome to StackSimplify ! AWS Infra created using Terraform in us-east-1 Region</h1>" > /var/www/html/index.html
    EOF  
  vpc_security_group_ids = [ aws_security_group.dev-vpc-sg.id ]
}
```

## Step-06: c4-elastic-ip.tf - Create Elastic IP Resource
- Create Elastic IP Resource
- Add a Resource Meta-Argument `depends_on` ensuring Elastic IP gets created only after AWS Internet Gateway in a VPC is present or created
```
# Resource-9: Create Elastic IP
resource "aws_eip" "my-eip" {
  instance = aws_instance.my-ec2-vm.id
  vpc = true
  depends_on = [ aws_internet_gateway.vpc-dev-igw ]
}
```

## Step-07: Execute Terraform commands to Create Resources using Terraform
```
# Initialize Terraform
terraform init

# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
```

## Step-08: Verify the Resources
- Verify VPC
- Verify EC2 Instance
- Verify Elastic IP
- Review the `terraform.tfstate` file
- Access Apache Webserver Static page using Elastic IP
```
# Access Application
http://<AWS-ELASTIC-IP>
```

## Step-09: Destroy Terraform Resources
```
# Destroy Terraform Resources
terraform destroy

# Remove Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## References 
- [Elastic IP creation depends on VPC Internet Gateway](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eip)
- [Resource: aws_vpc](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)
- [Resource: aws_subnet](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet)
- [Resource: aws_internet_gateway](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/internet_gateway)
- [Resource: aws_route_table](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route_table)
- [Resource: aws_route](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route)
- [Resource: aws_route_table_association](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/route_table_association)
- [Resource: aws_security_group](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group)
- [Resource: aws_instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
- [Resource: aws_eip](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eip)======================================================================
***********read the file ./BACKUP-2024/04-Terraform-Resources/04-03-Meta-Argument-count/README.md ************
# Terraform Resource Meta-Argument count

## Step-01: Introduction
- Understand Resource Meta-Argument `count`
- Also implement count and count index practically 

## Step-02: Create 5 EC2 Instances using Terraform
- In general, 1 EC2 Instance Resource in Terraform equals to 1 EC2 Instance in Real AWS Cloud
- 5 EC2 Instance Resources = 5 EC2 Instances in AWS Cloud
- With `Meta-Argument count` this is going to become super simple. 
- Lets see how. 
```t
# Create EC2 Instance
resource "aws_instance" "web" {
  ami = "ami-047a51fa27710816e" # Amazon Linux
  instance_type = "t2.micro"
  count = 5
  tags = {
    "Name" = "web"
  }
}
```
- **Execute Terraform Commands**
```t
# Initialize Terraform
terraform init

# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
```
- Verify EC2 Instances and its Name


## Step-03: Understand about count index
- If we currently see all our EC2 Instances has the same name `web`
- Lets name them by using count index `web-0, web-1, web-2, web-3, web-4`
```t
# Create EC2 Instance
resource "aws_instance" "web" {
  ami = "ami-047a51fa27710816e" # Amazon Linux
  instance_type = "t2.micro"
  count = 5
  tags = {
    #"Name" = "web"
    "Name" = "web-${count.index}"
  }
}
```
- **Execute Terraform Commands**
```t
# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
```
- Verify EC2 Instances


## Step-04: Destroy Terraform Resources
```t
# Destroy Terraform Resources
terraform destroy

# Remove Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## References
- [Resources: Count Meta-Argument](https://www.terraform.io/docs/language/meta-arguments/count.html)======================================================================
***********read the file ./BACKUP-2024/04-Terraform-Resources/04-04-Meta-Argument-for_each/README.md ************
# Terraform Resource Meta-Argument for_each

## Step-01: Introduction
- Understand about Meta-Argument `for_each`
- Implement `for_each` with **Maps**
- Implement `for_each` with **Set of Strings**

## Step-02: Implement for_each with Maps
- **Reference Folder:** v1-for_each-maps
- **Use case:** Create four S3 buckets using for_each maps 
- **c2-s3bucket.tf**
```t
# Create S3 Bucket per environment with for_each and maps
# https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket

resource "aws_s3_bucket" "mys3bucket" {

  # for_each Meta-Argument
  for_each = {
    dev  = "my-dapp-bucket"
    qa   = "my-qapp-bucket"
    stag = "my-sapp-bucket"
    prod = "my-papp-bucket"
  }

  bucket = "${each.key}-${each.value}"
  #acl    = "private" # This argument is deprecated, so commenting it. 
  

  tags = {
    Environment = each.key
    bucketname  = "${each.key}-${each.value}"
    eachvalue   = each.value
  }
}

```

## Step-03: Execute Terraform Commands
```t
# Switch to Working Directory
cd v1-for_each-maps

# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Format Terraform Configuration Files
terraform fmt

# Generate Terraform Plan
terraform plan
Observation: 
1) Four buckets creation will be generated in plan
2) Review Resource Names ResourceType.ResourceLocalName[each.key]
2) Review bucket name (each.key+each.value)
3) Review bucket tags

# Create Resources
terraform apply
Observation: 
1) 4 S3 buckets should be created
2) Review bucket names and tags in AWS Management console

# Destroy Resources
terraform destroy

# Clean-Up 
rm -rf .terraform*
rm -rf terraform.tfstate*
```


## Step-04: Implement for_each with toset "Strings"
- **Reference Folder:** v2-for_each-toset
- **Use case:** Create four IAM Users using for_each toset strings 
- **c2-iamuser.tf**
```t
# Create 4 IAM Users
resource "aws_iam_user" "myuser" {
  for_each = toset( ["Jack", "James", "Madhu", "Dave"] )
  name     = each.key
}
```

## Step-05: Execute Terraform Commands
```t
# Switch to Working Directory
cd v2-for_each-toset

# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Format Terraform Configuration Files
terraform fmt

# Generate Terraform Plan
terraform plan
Observation: 
1) Four IAM users creation will be generated in plan
2) Review Resource Names ResourceType.ResourceLocalName[each.key]
2) Review IAM User name (each.key)

# Create Resources
terraform apply
Observation: 
1) 4 IAM users should be created
2) Review IAM users in AWS Management console

# Destroy Resources
terraform destroy

# Clean-Up 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Reference
- [Resource Meta-Argument: for_each](https://www.terraform.io/docs/language/meta-arguments/for_each.html)
- [Resource: AWS S3 Bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket)
- [Resource: AWS IAM User](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_user)======================================================================
***********read the file ./BACKUP-2024/04-Terraform-Resources/04-05-Meta-Argument-lifecycle/README.md ************
# Terraform Resource Meta-Argument lifecycle

## Step-01: Introduction
- lifecyle Meta-Argument block contains 3 arguments
  - create_before_destroy
  - prevent_destroy
  - ignore_changes
- Understand and implement each of these 3 things practically in a step by step manner  

## Step-02: lifecyle - create_before_destroy
- Default Behavior of a Resource: Destroy Resource & re-create Resource
- With Lifecycle Block we can change that using `create_before_destroy=true`
  - First new resource will get created
  - Second old resource will get destroyed
- **Add Lifecycle Block inside Resource Block to alter behavior**  
```t
  lifecycle {
    create_before_destroy = true
  }
```  
### Step-02-01: Observe without Lifecycle Block
```t
# Switch to Working Directory
cd v1-create_before_destroy

# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Format Terraform Configuration Files
terraform fmt

# Generate Terraform Plan
terraform plan

# Create Resources
terraform apply -auto-approve

# Modify Resource Configuration
Change Availability Zone from us-east-1a to us-east-1b

# Apply Changes
terraform apply -auto-approve
Observation: 
1) First us-east-1a resource will be destroyed
2) Second us-east-1b resource will get
```
### Step-02-02: With Lifecycle Block
- Add Lifecycle block in the resource (Uncomment lifecycle block)
```t
# Generate Terraform Plan
terraform plan

# Apply Changes
terraform apply -auto-approve

# Modify Resource Configuration
Change Availability Zone from us-east-1b to us-east-1a

# Apply Changes
terraform apply -auto-approve
Observation: 
1) First us-east-1a resource will get created
2) Second us-east-1b resource will get deleted
```
### Step-02-03: Clean-Up Resources
```t
# Destroy Resources
terraform destroy -auto-approve

# Clean-Up 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-03: lifecycle - prevent_destroy
### Step-03-01: Introduction
- This meta-argument, when set to true, will cause Terraform to reject with an error any plan that would destroy the infrastructure object associated with the resource, as long as the argument remains present in the configuration.
- This can be used as a measure of safety against the accidental replacement of objects that may be costly to reproduce, such as database instances
- However, it will make certain configuration changes impossible to apply, and will prevent the use of the `terraform destroy` command once such objects are created, and so this option should be used `sparingly`.
- Since this argument must be present in configuration for the protection to apply, note that this setting does not prevent the remote object from being destroyed if the resource block were removed from configuration entirely: in that case, the `prevent_destroy` setting is removed along with it, and so Terraform will allow the destroy operation to succeed.
```t
  lifecycle {
    prevent_destroy = true # Default is false
  }
```
### Step-03-02: Execute Terraform Commands
```t
# Switch to Working Directory
cd v2-prevent_destroy

# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Format Terraform Configuration Files
terraform fmt

# Generate Terraform Plan
terraform plan

# Create Resources
terraform apply -auto-approve

# Destroy Resource
terraform destroy 
Observation: 
Kalyans-MacBook-Pro:v2-prevent_destroy kdaida$ terraform destroy -auto-approve
Error: Instance cannot be destroyed
  on c2-ec2-instance.tf line 2:
   2: resource "aws_instance" "web" {
Resource aws_instance.web has lifecycle.prevent_destroy set, but the plan
calls for this resource to be destroyed. To avoid this error and continue with
the plan, either disable lifecycle.prevent_destroy or reduce the scope of the
plan using the -target flag.


# Remove/Comment Lifecycle block
- Remove or Comment lifecycle block and clean-up

# Destroy Resource after removing lifecycle block
terraform destroy

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*
```


## Step-04: lifecyle - ignore_changes
### Step-04-01: Create an EC2 Changes, make manual changes and understand the behavior
- Create EC2 Instance
```t
# Switch to Working Directory
cd v3-ignore_changes

# Initialize Terraform
terraform init

# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
```
### Step-04-02: Update the tag by going to AWS management console
- Add a new tag manually to EC2 Instance
- Try `terraform apply` now
- Terraform will find the difference in configuration on remote side when compare to local and tries to remove the manual change when we execute `terraform apply`
```t
# Add new tag manually
WebServer = Apache

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
Observation: 
1) It will remove the changes which we manually added using AWS Management console
```

### Step-04-03: Add the lifecyle - ignore_changes block
- Enable the block in `c2-ec2-instance.tf`

```t
   lifecycle {
    ignore_changes = [
      # Ignore changes to tags, e.g. because a management agent
      # updates these based on some ruleset managed elsewhere.
      tags,
    ]
  }
```
- Add new tags manually using AWS mgmt console for the EC2 Instance
```t
# Add new tag manually
WebServer = Apache2
ignorechanges = test1

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
Observation: 
1) Manual changes should not be touched. They should be ignored by terraform
2) Verify the same on AWS management console

# Destroy Resource
terraform destroy -auto-approve

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## References
- [Resource Meat-Argument: Lifecycle](https://www.terraform.io/docs/language/meta-arguments/lifecycle.html)======================================================================
***********read the file ./BACKUP-2024/04-Terraform-Resources/04-06-Provisioners/README.md ************
# Provisioners
-  We are going to learn Terraform Provisioners in detail in [Section-09](https://github.com/stacksimplify/hashicorp-certified-terraform-associate/tree/master/09-Terraform-Provisioners) of this course. 
======================================================================
***********read the file ./BACKUP-2024/05-Terraform-Variables/05-01-Terraform-Input-Variables/README.md ************
# Terraform Input Variables

## Step-00: Introduction
- **v1:** Input Variables - Basics
- **v2:** Provide Input Variables when prompted during terraform plan or apply
- **v3:** Override default variable values using CLI argument `-var` 
- **v4:** Override default variable values using Environment Variables
- **v5:** Provide Input Variables using `terraform.tfvars` files
- **v6:** Provide Input Variables using `<any-name>.tfvars` file with CLI 
argument `-var-file`
- **v7:** Provide Input Variables using `auto.tfvars` files
- **v8-01:** Implement complex type constructors like `list` 
- **v8-02:** Implement complex type constructors like `maps`
- **v9:** Implement Custom Validation Rules in Variables
- **v10:** Protect Sensitive Input Variables
- **v11:** Understand about `File` function

## Pre-requisite
- Create a new EC2 Key pair with name as `terraform-key`
- In all the templates listed below V1 to V12, we will be using `key_name      = "terraform-key"` incase if you want to login to EC2 Instance you can use this key


## Step-01: Input Variables Basics 
- **Reference Sub folder:** v1-Input-Variables-Basic
- Create / Review the terraform manifests
  - c1-versions.tf
  - c2-variables.tf
  - c3-security-groups.tf
  - c4-ec2-instance.tf
- We are going to define `c3-variables.tf` and define the below listed variables
  - aws_region is a variable of type `string`
  - ec2_ami_id is a variable of type `string`
  - ec2_instance_count is a variable of type `number`
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan

# Create Resources
terraform apply

# Access Application
http://<Public-IP-Address>

# Clean-Up
terraform destroy -auto-approve
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-02: Input Variables Assign When Prompted
- **Reference Sub folder:** v2-Input-Variables-Assign-when-prompted
- Add a new variable in `variables.tf` named `ec2_instance_type` without any default value. 
- As the variable doesn't have any default value when you execute `terraform plan` or `terraform apply` it will prompt for the variable. 

```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan
```

## Step-03: Input Variables Override default value with cli argument `-var`
- **Reference Sub folder:** v3-Input-Variables-Override-default-with-cli
- We are going to override the default values defined in `variables.tf` by providing new values using the `-var` argument using CLI
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Option-1 (Always provide -var for both plan and apply)
# Review the terraform plan
terraform plan -var="ec2_instance_type=t3.large" -var="ec2_instance_count=1"

# Create Resources (optional)
terraform apply -var="ec2_instance_type=t3.large" -var="ec2_instance_count=1"

# Option-2 (Generate plan file with -var and use that with apply)
# Generate Terraform plan file
terraform plan -var="ec2_instance_type=t3.large" -var="ec2_instance_count=1" -out v3out.plan

# Create / Deploy Terraform Resources using Plan file
terraform apply v3out.plan 
```

## Step-04: Input Variables Override with Environment Variables
- **Reference Sub folder:** v4-Input-Variables-Override-with-Environment-Variables
- Set environment variables and execute `terraform plan` to see if it overrides default values 
```t
# Sample
export TF_VAR_variable_name=value

# SET Environment Variables
export TF_VAR_ec2_instance_count=1
export TF_VAR_ec2_instance_type=t3.large
echo $TF_VAR_ec2_instance_count, $TF_VAR_ec2_instance_type

# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan

# UNSET Environment Variables after demo
unset TF_VAR_ec2_instance_count
unset TF_VAR_ec2_instance_type
echo $TF_VAR_ec2_instance_count, $TF_VAR_ec2_instance_type
```

## Step-05: Assign Input Variables from terraform.tfvars
- **Reference Sub folder:** v5-Input-Variables-Assign-with-terraform-tfvars
- Create a file named `terraform.tfvars` and define variables
- If the file name is `terraform.tfvars`, terraform will auto-load the variables present in this file by overriding the `default` values in `variables.tf`
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan

# Create Resources
terraform apply

# Access Application
http://<Elastic-IP-Address>
```

## Step-06: Assign Input Variables with -var-file argument
- **Reference Sub folder:** v6-Input-Variables-Assign-with-tfvars-var-file
- If we plan to use different names for  `.tfvars` files, then we need to explicitly provide the argument `-var-file` during the `terraform plan or apply`
- We will use following things in this example
  - **c2-variables.tf:** aws_region variable will be picked with default value
  - **terraform.tfvars:** ec2_instance_count variable will be picked from this file
  - **web.tfvars:** ec2_instance_type variable will be picked from this file
  - **app.tfvars:** ec2_instance_type variable will be picked from this file
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan -var-file="web.tfvars"
terraform plan -var-file="app.tfvars"
```

## Step-07: Auto load input variables with .auto.tfvars files
- **Reference Sub folder:** v7-Input-Variables-Assign-with-auto-tfvars
- We will create a file with extension as `.auto.tfvars`. 
- With this extension, whatever may be the file name, the variables inside these files will be auto loaded during `terraform plan or apply`
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 
```
## Step-08: Implement complex type cosntructors like `list` and `maps`
- **Reference Sub folder:** v8-Input-Variables-Lists-Maps
- [Type Constraints](https://www.terraform.io/docs/language/expressions/types.html)
### Step-08-01: Implement Vairable Type as List
- **list (or tuple):** a sequence of values, like ["us-west-1a", "us-west-1c"]. Elements in a list or tuple are identified by consecutive whole numbers, starting with zero.
- Implement List function for variable `ec2_instance_type`
```t
# Implement List Function in variables.tf
variable "ec2_instance_type" {
  description = "EC2 Instance Type"
  type = list(string)
  default = ["t3.micro", "t3.small", "t3.medium"]
}

# Reference Values from List in ec2-instance.tf
instance_type = var.ec2_instance_type[0] --> t3.micro
instance_type = var.ec2_instance_type[1] --> t3.small
instance_type = var.ec2_instance_type[2] --> t3.medium

# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 
```

### Step-08-02: Implement Vairable Type as Map
- **map (or object):** a group of values identified by named labels, like {name = "Mabel", age = 52}.
- Implement Map function for variable `ec2_instance_tags`
```t
# Implement Map Function for tags
variable "ec2_instance_tags" {
  description = "EC2 Instance Tags"
  type = map(string)
  default = {
    "Name" = "ec2-web"
    "Tier" = "Web"
  }

# Reference Values from Map in ec2-instance.tf
tags = var.ec2_instance_tags  

# Implement Map Function for Instance Type
# Important Note: comment "ec2_instance_type" variable with list function
variable "ec2_instance_type_map" {
  description = "EC2 Instance Type using maps"
  type = map(string)
  default = {
    "small-apps" = "t3.micro"
    "medium-apps" = "t3.medium" 
    "big-apps" = "t3.large"
  }

# Reference Instance Type from Maps Variables
instance_type = var.ec2_instance_type_map["small-apps"]
instance_type = var.ec2_instance_type_map["medium-apps"]
instance_type = var.ec2_instance_type_map["big-apps"]

# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 
```



## Step-09: Implement Custom Validation Rules in Variables
- **Reference Sub folder:** v9-Input-Variables-Validation-Rules
- Understand and implement custom validation rules in variables
- [Terraform Console](https://www.terraform.io/docs/cli/commands/console.html)
- The `terraform console` command provides an interactive console for evaluating expressions.
### Step-09-01: Learn Terraform Length Function
- [Terraform Length Function](https://www.terraform.io/docs/language/functions/length.html)
```t
# Go to Terraform Console
terraform console

# Test length function
Template: length()
length("hi")
length("hello")
length(["a", "b", "c"]) # List
length({"key" = "value"}) # Map
length({"key1" = "value1", "key2" = "value2" }) #Map
```

### Step-09-02: Learn Terraform SubString Function
- [Terraform Sub String Function](https://www.terraform.io/docs/language/functions/substr.html)
```t
# Go to Terraform Console
terraform console

# Test substr function
Template: substr(string, offset, length)
substr("stack simplify", 1, 4)
substr("stack simplify", 0, 6)
substr("stack simplify", 0, 1)
substr("stack simplify", 0, 0)
substr("stack simplify", 0, 10)
```

### Step-09-03: Implement Validation Rule for ec2_ami_id variable
```t
variable "ec2_ami_id" {
  description = "AMI ID"
  type = string  
  default = "ami-0be2609ba883822ec"
  validation {
    condition = length(var.ec2_ami_id) > 4 && substr(var.ec2_ami_id, 0, 4) == "ami-"
    error_message = "The ec2_ami_id value must be a valid AMI id, starting with \"ami-\"."
  }
}
```
- **Run Terraform commands**
```
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan
```

## Step-10: Protect Sensitive Input Variables
- **Reference Sub folder:** v10-Sensitive-Input-Variables
- [AWS RDS DB Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/db_instance)
- [Vault Provider](https://learn.hashicorp.com/tutorials/terraform/secrets-vault?in=terraform/secrets)
- When using environment variables to set sensitive values, keep in mind that those values will be in your environment and command-line history
`Example: export TF_VAR_db_username=admin TF_VAR_db_password=adifferentpassword`
- When you use sensitive variables in your Terraform configuration, you can use them as you would any other variable. 
- Terraform will `redact` these values in command output and log files, and raise an error when it detects that they will be exposed in other ways.
- **Important Note-1:** Never check-in `secrets.tfvars` to git repositories
- **Important Note-2:** Terraform state file contains values for these sensitive variables `terraform.tfstate`. You must keep your state file secure to avoid exposing this data.
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan -var-file="secrets.tfvars"

# Create Resources
terraform apply -var-file="secrets.tfvars"

# Verify Terraform State files
grep password terraform.tfstate
grep username terraform.tfstate 

# Destroy Resources
terraform destroy var-file="secrets.tfvars"

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*
```

### Variable Definition Precedence
- [Terraform Variable Definition Precedence](https://www.terraform.io/docs/language/values/variables.html#variable-definition-precedence)


## Step-11: Understand about `File` function
- **Reference Sub folder:** v11-File-Function
- Understand about [Terraform File Function](https://www.terraform.io/docs/language/functions/file.html)

```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 

# Create Resources
terraform apply 

# Access Application
http://<Public-IP>

# Destroy Resources
terraform destroy -auto-approve
```


## References
- [Terraform Input Variables](https://www.terraform.io/docs/language/values/variables.html)
- [Resource: AWS EC2 Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
- [Resource: AWS Security Group](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group)
- [Sensitive Variables - Additional Reference](https://learn.hashicorp.com/tutorials/terraform/sensitive-variables)



======================================================================
***********read the file ./BACKUP-2024/05-Terraform-Variables/05-02-Terraform-Output-Values/README.md ************
# Terraform Output Values

## Step-01: Introduction
- Understand about Output Values and implement them
- Query outputs using `terraform output`
- Understand about redacting secure attributes in output values
- Generate machine-readable output

## Step-02: Basics of Output Values
- **Reference Sub folder:** terraform-manifests
- Understand Output Values
- You can export both Argument & Attribute References
- [Terraform AWS EC2 Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 

# Create Resources
terraform apply -auto-approve

# Access Application
http://<Public-IP>
http://<Public-DNS>
```

## Step-03: Query Terraform Outputs
- Terraform will load the project state in state file, so that using `terraform output` command we can query the state file. 
```t
# Terraform Output Commands
terraform output
terraform output ec2_publicdns
```


## Step-04: Output Values - Suppressing Sensitive Values in Output
- We can redact the sensitive outputs using `sensitve = true` in output block
- This will only redact the cli output for terraform plan and apply
- When you query using `terraform output`, you will be able to fetch exact values from `terraform.tfstate` files
- Add `sensitve = true` for output `ec2_publicdns`
```t
# Attribute Reference - Create Public DNS URL with http:// appended
output "ec2_publicdns" {
  description = "Public DNS URL of an EC2 Instance"
  value = "http://${aws_instance.my-ec2-vm.public_dns}"
  sensitive = true
}
```
- Test the flow
```t
# Terraform Apply
terraform apply -auto-approve
Observation: You should see the value as sensitive

# Query using terraform output
terraform output ec2_publicdns
Observation: You should get non-redacted original value from terraform.tfstate file
```

## Step-05: Generate machine-readable output
```t
# Generate machine-readable output
terraform output -json
```

## Step-06: Destroy Resources
```t
# Destroy Resources
terraform destroy -auto-approve

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*
```


## References
- [Terraform Output Values](https://www.terraform.io/docs/language/values/outputs.html)======================================================================
***********read the file ./BACKUP-2024/05-Terraform-Variables/05-03-Terraform-Local-Values/README.md ************
# Terraform Local Values

## Step-01: Introduction
- Understand DRY Principle
- What is local value in terraform?
- When To Use Local Values?
- What is the problem locals are solving ?

```
What is DRY Principle ?
Don't repeat yourself

What is local value in terraform?
The local block defines one or more local variables within a module. 
A local value assigns a name to an terraform expression, allowing it to be used multiple times within a module without repeating it.

When To Use Local Values?
Local values can be helpful to avoid repeating the same values or expressions multiple times in a configuration
If overused they can also make a configuration hard to read by future maintainers by hiding the actual values used.
Use local values only in moderation, in situations where a single value or result is used in many places and that value is likely to be changed in future. The ability to easily change the value in a central place is the key advantage of local values.

What is the problem locals are solving ?
Currently terraform doesn’t allow variable substitution within variables. The terraform way of doing this is by using local values or locals where you can somehow keep your code DRY.

Another use case (at least for me) for locals is to shorten references on upstream terraform projects as seen below. This will make your terraform templates/modules more readable.
```

## Step-02: Create / Review Terraform configuration files
- c1-versions.tf
- c2-variables.tf
- c3-s3-bucket.tf


## Step-03: Test the Terraform configuration using commands
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 

# Create Resources (Optional)
terraform apply -auto-approve
```

## References
- [Terraform Local values](https://www.terraform.io/docs/language/values/locals.html)======================================================================
***********read the file ./BACKUP-2024/06-Terraform-Datasources/README.md ************
# Terraform Datasources

## Step-01: Introduction
- Understand about Datasources in Terraform
- Implement a sample usecase with Datasources.
- Get the latest Amazon Linux 2 AMI ID using datasources and reference that value when creating EC2 Instance resource `ami = data.aws_ami.amzlinux.id`

## Step-02: Create a Datasource to fetch latest AMI ID
- Create or review manifest `c6-ami-datasource.tf`
- Go to AWS Mgmt Console -> Services -> EC2 -> Images -> AMI 
- Search for "Public Images" -> Provide AMI ID
  - We can get AMI Name format
  - We can get Owner Name
  - Visibility
  - Platform
  - Root Device Type
  - and many more info here. 
- Accordingly using this information build your filters in datasource

## Step-03: Reference the datasource in ec2-instance.tf
```t
  ami           = data.aws_ami.amzlinux.id 
```

## Step-04: Test using Terraform commands
```t
# Initialize Terraform
terraform init

# Validate Terraform configuration files
terraform validate

# Format Terraform configuration files
terraform fmt

# Review the terraform plan
terraform plan 

# Create Resources (Optional)
terraform apply -auto-approve

# Access Application
http://<Public-DNS>

# Destroy Resources
terraform destroy -auto-approve
```


## References
- [AWS EC2 AMI Datasource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/ami)
======================================================================
***********read the file ./BACKUP-2024/07-Terraform-State/07-01-Terraform-Remote-State-Storage-and-Locking/README.md ************
# Terraform Remote State Storage & Locking

## Step-01: Introduction
- Understand Terraform Backends
- Understand about Remote State Storage and its advantages
- This state is stored by default in a local file named "terraform.tfstate", but it can also be stored remotely, which works better in a team environment.
- Create AWS S3 bucket to store `terraform.tfstate` file and enable backend configurations in terraform settings block
- Understand about **State Locking** and its advantages
- Create DynamoDB Table and  implement State Locking by enabling the same in Terraform backend configuration

## Step-02: Create S3 Bucket
- Go to Services -> S3 -> Create Bucket
- **Bucket name:** terraform-stacksimplify
- **Region:** US-East (N.Virginia)
- **Bucket settings for Block Public Access:** leave to defaults
- **Bucket Versioning:** Enable
- Rest all leave to **defaults**
- Click on **Create Bucket**
- **Create Folder**
  - **Folder Name:** dev
  - Click on **Create Folder**


## Step-03: Terraform Backend Configuration
- **Reference Sub-folder:** terraform-manifests
- [Terraform Backend as S3](https://www.terraform.io/docs/language/settings/backends/s3.html)
- Add the below listed Terraform backend block in `Terrafrom Settings` block in `main.tf`
```t
# Terraform Backend Block
  backend "s3" {
    bucket = "terraform-stacksimplify"
    key    = "dev/terraform.tfstate"
    region = "us-east-1"    
  }
```

## Step-04: Test with Remote State Storage Backend
```t
# Initialize Terraform
terraform init

Observation: 
Successfully configured the backend "s3"! Terraform will automatically
use this backend unless the backend configuration changes.

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate

# Validate Terraform configuration files
terraform validate

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate

# Format Terraform configuration files
terraform fmt

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate

# Review the terraform plan
terraform plan 

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate

# Create Resources 
terraform apply -auto-approve

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate
Observation: Finally at this point you should see the terraform.tfstate file in s3 bucket

# Access Application
http://<Public-DNS>
```

## Step-05: Bucket Versioning - Test
- Update in `c2-variables.tf` 
```t
variable "instance_type" {
  description = "EC2 Instance Type - Instance Sizing"
  type = string
  #default = "t2.micro"
  default = "t2.small"
}
```
- Execute Terraform Commands
```t
# Review the terraform plan
terraform plan 

# Create Resources 
terraform apply -auto-approve

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev/terraform.tfstate
Observation: New version of terraform.tfstate file will be created
```


## Step-06: Destroy Resources
- Destroy Resources and Verify Bucket Versioning
```t
# Destroy Resources
terraform destroy -auto-approve
```
## Step-07: Terraform State Locking Introduction
- Understand about Terraform State Locking Advantages

## Step-08: Add State Locking Feature using DynamoDB Table
- Create Dynamo DB Table
  - **Table Name:** terraform-dev-state-table
  - **Partition key (Primary Key):** LockID (Type as String)
  - **Table settings:** Use default settings (checked)
  - Click on **Create**

## Step-09: Update DynamoDB Table entry in backend in c1-versions.tf
- Enable State Locking in `c1-versions.tf`
```t
  # Adding Backend as S3 for Remote State Storage with State Locking
  backend "s3" {
    bucket = "terraform-stacksimplify"
    key    = "dev2/terraform.tfstate"
    region = "us-east-1"  

    # For State Locking
    dynamodb_table = "terraform-dev-state-table"
  }
```

## Step-10: Execute Terraform Commands
```t
# Initialize Terraform 
terraform init

# Review the terraform plan
terraform plan 
Observation: 
1) Below messages displayed at start and end of command
Acquiring state lock. This may take a few moments...
Releasing state lock. This may take a few moments...
2) Verify DynamoDB Table -> Items tab

# Create Resources 
terraform apply -auto-approve

# Verify S3 Bucket for terraform.tfstate file
bucket-name/dev2/terraform.tfstate
Observation: New version of terraform.tfstate file will be created in dev2 folder
```

## Step-11: Destroy Resources
- Destroy Resources and Verify Bucket Versioning
```t
# Destroy Resources
terraform destroy -auto-approve

# Clean-Up Files
rm -rf .terraform*
rm -rf terraform.tfstate*  # This step not needed as e are using remote state storage here
```

## References 
- [AWS S3 Backend](https://www.terraform.io/docs/language/settings/backends/s3.html)
- [Terraform Backends](https://www.terraform.io/docs/language/settings/backends/index.html)
- [Terraform State Storage](https://www.terraform.io/docs/language/state/backends.html)
- [Terraform State Locking](https://www.terraform.io/docs/language/state/locking.html)
- [Remote Backends - Enhanced](https://www.terraform.io/docs/language/settings/backends/remote.html)======================================================================
***********read the file ./BACKUP-2024/07-Terraform-State/07-02-Terraform-State-Commands/README.md ************
# Terraform State Commands

## Step-01: Introduction
- Terraform Commands
  - terraform show
  - terraform refresh
  - terraform plan (internally calls refresh)
  - terraform state
  - terraform force-unlock   
  - terraform taint
  - terraform untaint
  - terraform apply target command  

## Pre-requisite: c1-versions.tf
- Update your backend block with your bucket name, key and region
- Also update your dynamodb table name
```t
# Terraform Block
terraform {
  required_version = "~> 0.14" # which means any version equal & above 0.14 like 0.15, 0.16 etc and < 1.xx
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
  # Adding Backend as S3 for Remote State Storage
  backend "s3" {
    bucket = "terraform-stacksimplify"
    key    = "statecommands/terraform.tfstate"
    region = "us-east-1" 

    # Enable during Step-09     
    # For State Locking
    dynamodb_table = "terraform-dev-state-table"    
    
  }
}
```  

## Step-02: Terraform Show Command to review Terraform plan files
- The `terraform show` command is used to provide human-readable output from a state or plan file. 
- This can be used to inspect a plan to ensure that the planned operations are expected, or to inspect the current state as
- Terraform plan output files are binary files. We can read them using `terraform show` command
```t
# Initialize Terraform
terraform init

# Create Plan 
terraform plan -out=v1plan.out

# Read the plan 
terraform show v1plan.out

# Read the plan in json format
terraform show -json v1plan.out
```

## Step-03: Terraform Show command to Read State files
- By default, in the working directory if we have `terraform.tfstate` file, when we provide the command `terraform show` it will read the state file automatically and display output.  
```t
# Terraform Show
terraform show
Observation: It should say "No State" because we will still didn't create any resources yet and no state file in current working directory

# Create Resources
terraform apply -auto-approve

# Terraform Show 
terraform show
Observation: It should display the state file
```

## Step-04: Understand terraform refresh in detail
- This commands comes under **Terraform Inspecting State**
- Understanding `terraform refresh` clears a lot of doubts in our mind and terraform state file and state feature
- The terraform refresh command is used to reconcile the state Terraform knows about (via its state file) with the real-world infrastructure. 
- This can be used to detect any drift from the last-known state, and to update the state file.
- This does not modify infrastructure, but does modify the state file. If the state is changed, this may cause changes to occur during the next plan or apply.
- **terraform refresh:** Update local state file against real resources in cloud
- **Desired State:** Local Terraform Manifest (All *.tf files)
- **Current State:**  Real Resources present in your cloud
- **Command Order of Execution:** refresh, plan, make a decision, apply
- Why? Lets understand that in detail about this order of execution
### Step-04-01: Add a new tag to EC2 Instance using AWS Management Console
```t
"demotag" = "refreshtest"
```

### Step-04-02: Execute terraform plan  
- You should observe no changes to local state file because plan does the comparison in memory 
- Verify S3 Bucket - no update to tfstate file about the change 
```t
# Execute Terraform plan
terraform plan 
```
### Step-04-03: Execute terraform refresh 
- You should see terraform state file updated with new demo tag
```
# Execute terraform refresh
terraform refresh

# Review terraform state file
1) terraform show
2) Also verify S3 bucket, new version of terraform.tfstate file will be created
```
### Step-04-04: Why you need to the execution in this order (refresh, plan, make a decision, apply) ?
- There are changes happened in your infra manually and not via terraform.
- Now decision to be made if you want those changes or not.
- **Choice-1:** If you dont want those changes proceed with terraform apply so manual changes we have done on our cloud EC2 Instance will be removed.
- **Choice-2:** If you want those changes, refer `terraform.tfstate` file about changes and embed them in your terraform manifests (example: c4-ec2-instance.tf) and proceed with flow (referesh, plan, review execution plan and apply)

### Step-04-05: I picked choice-2, so i will update the tags in c4-ec2-instance.tf
- Update in c4-ec2-instance.tf
```t
  tags = {
    "Name" = "amz-linux-vm"
    "demotag" = "refreshtest"
  }
```
### Step-04-06: Execute the commands to make our manual change official in terraform manifests and tfstate files perspective  
```t
# Terraform Refresh
terraform refresh

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve
```


## Step-05: Terraform State Command
### Step-05-01: Terraform State List and Show commands
- These two commands comes under **Terraform Inspecting State**
- **terraform state list:**  This command is used to list resources within a Terraform state.
- **terraform  state show:** This command is used to show the attributes of a single resource in the Terraform state.
```t
# List Resources from Terraform State
terraform state list

# Show the attributes of a single resource from Terraform State
terraform  state show data.aws_ami.amzlinux
terraform  state show aws_instance.my-ec2-vm
```
### Step-05-02: Terraform State mv command
- This commands comes under **Terraform Moving Resources**
- This command will move an item matched by the address given to the
 destination address. 
- This command can also move to a destination address
 in a completely different state file
- Very dangerous command
- Very advanced usage command
- Results will be unpredictable if concept is not clear about terraform state files mainly  desired state and current state.  
- Try this in production environments, only  when everything worked well in lower environments. 
```t
# Terraform List Resources
terraform state list

# Terraform State Move Resources to different name
terraform state mv -dry-run aws_instance.my-ec2-vm aws_instance.my-ec2-vm-new 
terraform state mv aws_instance.my-ec2-vm aws_instance.my-ec2-vm-new 
ls -lrta

Observation: 
1) It should create a backup file of terraform.tfstate as something like this "terraform.tfstate.1611929587.backup"
1) It renamed the name of "my-ec2-vm" in state file to "my-ec2-vm-new". 
2) Run terraform plan and observe what happens in next run of terraform plan and apply
-----------------------------
# WRONG APPROACH 
-----------------------------
# WRONG APPROACH OF MOVING TO TERRAFORM PLAN AND APPLY AFTER ABOVE CHANGE terraform state mv CHANGE
# WE NEED TO UPDATE EQUIVALENT RESOURCE in terraform manifests to match the same new name. 

# Terraform Plan
terraform plan
Observation: It will show "Plan: 1 to add, 0 to change, 1 to destroy."
1 to add: New EC2 Instance will be added
1 to destroy: Old EC2 instance will be destroyed

DON'T DO TERRAFORM APPLY because it shows make changes. Nothing changed other than state file local naming of a resource. Ideally nothing on current state (real cloud environment should not change due to this)
-----------------------------

-----------------------------
# RIGHT APPROACH
-----------------------------
Update "c3-ec2-instance.tf"
Before: resource "aws_instance" "my-ec2-vm" {
After: resource "aws_instance" "my-ec2-vm-new" {  

Update all references of this resources in your terraform manifests
Update c5-outputs.tf
Before: value = aws_instance.my-ec2-vm.public_ip
After: value = aws_instance.my-ec2-vm-new.public_ip

Before: value = aws_instance.my-ec2-vm.public_dns
After: value = aws_instance.my-ec2-vm-new.public_dns

Now run terraform plan and you should see no changes to Infra

# Terraform Plan
terraform plan
Observation: 
1) Message-1: No changes. Infrastructure is up-to-date
2) Message-2: This means that Terraform did not detect any differences between your
configuration and real physical resources that exist. As a result, no
actions need to be performed.
 
```
### Step-05-03: Terraform State rm command
- This commands comes under **Terraform Moving Resources**
- The `terraform state rm` command is used to remove items from the Terraform state. 
- This command can remove single resources, single instances of a resource, entire modules, and more.
```t
# Terraform List Resources
terraform state list

# Remove Resources from Terraform State
terraform state rm -dry-run aws_instance.my-ec2-vm-new
terraform state rm aws_instance.my-ec2-vm-new
Observation: 
1) Firstly takes backup of current state file before removing (example: terraform.tfstate.1611930284.backup)
2) Removes it from terraform.tfstate file

# Terraform Plan
terraform plan
Observation: It will tell you that resource is not in state file but same is present in your terraform manifests (03-ec2-instace.tf - DESIRED STATE). Do you want to re-create it?
This will re-create new EC2 instance excluding one created earlier and running

Make a  Choice
-------------
Choice-1: You want this resource to be running on cloud but should not be managed by terraform. Then remove its references in terraform manifests(DESIRED STATE). So that the one running in AWS cloud (current infra) this instance will be independent of terraform. 
Choice-2: You want a new resource to be created without deleting other one (non-terraform managed resource now in current state). Run terraform plan and apply
LIKE THIS WE NEED TO MAKE DECISIONS ON WHAT WOULD BE OUR OUTCOME OF REMOVING A RESOURCE FROM STATE.

PRIMARY REASON for this is command is that respective resource need to be removed from as terraform managed. 

# Run Terraform Plan
terraform plan

# Run Terraform Apply
terraform apply 
```

# Step-05-04: Terraform State replace-provider command
- This commands comes under **Terraform Moving Resources**
- [Terraform State Replace Provider](https://www.terraform.io/docs/cli/commands/state/replace-provider.html)


### Step-05-05: Terraform State pull / push command
- This command comes under **Terraform Disaster Recovery Concept**
- **terraform state pull:** 
  - The `terraform state pull` command is used to manually download and output the state from remote state.
  - This command also works with local state.
  - This command will download the state from its current location and output the raw format to stdout.
- **terraform state push:** The `terraform state push` command is used to manually upload a local state file to remote state. 

```t
# Other State Commands (Pull / Push)
terraform state pull
terraform state push
```

## Step-06: Terraform force-unlock command
- This command comes under **Terraform Disaster Recovery Concept**
- Manually unlock the state for the defined configuration.
- This will not modify your infrastructure. 
- This command removes the lock on the state for the current configuration. 
- The behavior of this lock is dependent on the backend (aws s3 with dynamodb for state locking etc) being used. 
- **Important Note:** Local state files cannot be unlocked by another process.
```t
# Manually Unlock the State
terraform force-unlock LOCK_ID
```

## Step-07: Terraform taint & untaint commands
-  These commands comes under **Terraform Forcing Re-creation of Resources**
- When a resource declaration is modified, Terraform usually attempts to update the existing resource in place (although some changes can require destruction and re-creation, usually due to upstream API limitations).
- **Example:** A virtual machine that configures itself with cloud-init on startup might no longer meet your needs if the cloud-init configuration changes.
- **terraform taint:** The `terraform taint` command manually marks a Terraform-managed resource as tainted, forcing it to be destroyed and recreated on the next apply.
- **terraform untaint:** 
  - The terraform untaint command manually unmarks a Terraform-managed resource as tainted, restoring it as the primary instance in the state. 
  - This reverses either a manual terraform taint or the result of provisioners failing on a resource.
  - This command will not modify infrastructure, but does modify the state file in order to unmark a resource as tainted.
```t
# List Resources from state
terraform state list

# Taint a Resource
terraform taint <RESOURCE_NAME_IN_TERRAFORM_LOCALLY>
terraform taint aws_instance.my-ec2-vm-new

# Terraform Plan
terraform plan
Observation: 
Message: "-/+ destroy and then create replacement"

# Untaint a Resource
terraform untaint <RESOURCE_NAME_IN_TERRAFORM_LOCALLY>
terraform untaint aws_instance.my-ec2-vm-new

# Terraform Plan
terraform plan
Observation: 
Message: "No changes. Infrastructure is up-to-date."
```


## Step-08: Terraform Resource Targeting - Plan, Apply (-target) Option
- The `-target` option can be used to focus Terraform's attention on only a subset of resources. 
- [Terraform Resource Targeting](https://www.terraform.io/docs/cli/commands/plan.html#resource-targeting)
- This targeting capability is provided for exceptional circumstances, such as recovering from mistakes or working around Terraform limitations.
-  It is not recommended to use `-target` for routine operations, since this can lead to undetected configuration drift and confusion about how the true state of resources relates to configuration.
- Instead of using `-target` as a means to operate on isolated portions of very large configurations, prefer instead to break large configurations into several smaller configurations that can each be independently applied.
```t
# Lets make two changes
Change-1: Add new tag in c4-ec2-instance.tf
    "target" = "Target-Test-1"
Change-2: Add additional inbound rule in "vpc-web" security group for port 8080
  ingress {
    description = "Allow Port 8080"
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
Change-3: Add new EC2 Resource  
# New VM
resource "aws_instance" "my-demo-vm" {
  ami           = data.aws_ami.amzlinux.id 
  instance_type = var.instance_type
  tags = {
    "Name" = "demo-vm1"
  }
}

# List Resources from state
terraform state list

# Terraform plan
terraform plan -target=aws_instance.my-ec2-vm-new
Observation:
0) Message: "Plan: 0 to add, 2 to change, 0 to destroy."
1) It is updating Change-1 because we are targeting that resource "aws_instance.my-ec2-vm-new"
2) It is updating change-2 "vpc-web" because its a dependent resource for "aws_instance.my-ec2-vm-new"
3) It is not touching the new resource which we are creating now. It will be in terraform configuration but not getting provisioned when we are using -target

# Terraform Apply
terraform apply -target=aws_instance.my-ec2-vm-new

```

## Step-09: Terraform Destroy & Clean-Up
```t
# Destory Resources
terraform destroy -auto-approve

# Clean-Up Files
rm -rf .terraform*
rm -rf v1plan.out

# Kalyan - Not to forgot to change these things after Recording
# Ensure below two lines are in commented state in c4-ec2-instance.tf
1) This is required for students demo for this entire section to implement in a step by step manner from beginning
    #"demotag" = "refreshtest"  # Enable during Step-04-05
    # "target" = "Target-Test-1" # Enable during step-08
2) Rename the local name for EC2 Instance from "my-ec2-vm-new" to "my-ec2-vm"
Changed during step 05-02: aws_instance.my-ec2-vm-new
Roll back for the students to have a seamless demo

3) Roll back changes you have made in step-08
3.1: New tag in EC2 Instance
3.2: New rule in vpc-web security group
3.3: New EC2 Instance resource in c4-ec2-instance.tf
```

## References
- [Terraform State Command](https://www.terraform.io/docs/cli/commands/state/index.html)
- [Terraform Inspect State](https://www.terraform.io/docs/cli/state/inspect.html)
- [Terraform Moving Resources](https://www.terraform.io/docs/cli/state/move.html)
- [Terraform Disaster Recovery](https://www.terraform.io/docs/cli/state/recover.html)
- [Terraform Taint](https://www.terraform.io/docs/cli/state/taint.html)
- [Terraform State](https://www.terraform.io/docs/language/state/index.html)
- [Manipulating Terraform State](https://www.terraform.io/docs/cli/state/index.html)
- [Additional Reference](https://www.hashicorp.com/blog/detecting-and-managing-drift-with-terraform)
======================================================================
***********read the file ./BACKUP-2024/08-Terraform-Workspaces/README.md ************
# Terraform Workspaces

## Step-01: Introduction
- We are going to create 2 more workspaces (dev,qa) in addition to default workspace
- Update our terraform manifests to support `terraform workspace` 
  - Primarily for security group name to be unique for each workspace
  - In the same way for EC2 VM Instance Name tag. 
- Master the below listed `terraform workspace` commands
  - terraform workspace show
  - terraform workspace list
  - terraform workspace new
  - terraform workspace select
  - terraform workspace delete


## Step-02: Update terraform manifests to support multiple workspaces
- **Sub-folder we are working on:** v1-local-backend
- Ideally, AWS don't allow to create a security group with same name twice. 
- With that said, we need to change our security group names in our `c2-security-groups.tf`
- At the same time, just for reading convenience we can also have our EC2 Instance `Name tag` also updated inline with workspace name. 
- What is **${terraform.workspace}**? - It will get the workspace name 
- **Popular Usage-1:** Using the workspace name as part of naming or tagging behavior
- **Popular Usage-2:** Referencing the current workspace is useful for changing behavior based on the workspace. For example, for non-default workspaces, it may be useful to spin up smaller cluster sizes.
```t
# Change-1: Security Group Names
  name        = "vpc-ssh-${terraform.workspace}"
  name        = "vpc-web-${terraform.workspace}"  

# Change-2: For non-default workspaces, it may be useful to spin up smaller cluster sizes.
  count = terraform.workspace == "default" ? 2 : 1  
This will create 2 instances if we are in default workspace and in any other workspaces it will create 1 instance

# Change-3: EC2 Instance Name tag
    "Name" = "vm-${terraform.workspace}-${count.index}"

# Change-4: Outputs
  value = aws_instance.my-ec2-vm.*.public_ip
  value = aws_instance.my-ec2-vm.*.public_dns    
You can create a list of all of the values of a given attribute for the items in the collection with a star. For instance, aws_instance.my-ec2-vm.*.id will be a list of all of the Public IP of the instances.  
```

## Step-03: Create resources in default workspaces
- Default Workspace: Every initialized working directory has at least one workspace. 
- If you haven't created other workspaces, it is a workspace named **default**
- For a given working directory, only one workspace can be selected at a time.
- Most Terraform commands (including provisioning and state manipulation commands) only interact with the currently selected workspace.
```t
# Terraform Init
terraform init 

# List Workspaces
terraform workspace list

# Output Current Workspace using show
terraform workspace show

# Terraform Plan
terraform plan
Observation: This should show us two instances based on the statement in EC2 Instance Resource "count = terraform.workspace == "default" ? 2 : 1" because we are creating this in default workspace

# Terraform Apply
terraform apply -auto-approve

# Verify
Verify the same in AWS Management console
Observation: 
1) Two instances should be created with name as "vm-default-0, vm-default-1")
2) Security Groups should be created with names as "vpc-ssh-default, vpc-web-default)
3) Observe the outputs on CLI, you should see list of Public IP and Public DNS 
```

## Step-04: Create New Workspace and Provision Infra 
```t
# Create New Workspace
terraform workspace new dev

# Verify the folder
cd terraform.tfstate.d 
cd dev
ls
cd ../../

# Terraform Plan
terraform plan
Observation:  This should show us creating only 1 instance based on statement "count = terraform.workspace == "default" ? 2 : 1" as we are creating this in non-default workspace named dev

# Terraform Apply
terraform apply -auto-approve

# Verify Dev Workspace statefile
cd terraform.tfstate.d/dev
ls
cd ../../
Observation: You should fine "terraform.tfstate" in "current-working-directory/terraform.tfstate.d/dev" folder

# Verify EC2 Instance in AWS mgmt console
Observation:
1) Name should be with "vm-dev-0"
2) Security Group names should be as "vpc-ssh-dev, vpc-web-dev"
```

## Step-05: Switch workspace and destroy resources
- Switch workspace from dev to default and destroy resources in default workspace
```t
# Show current workspace
terraform workspace show

# List Worksapces
terraform workspace list

# Workspace select
terraform workspace select default

# Delete Resources from default workspace
terraform destroy 

# Verify
1) Verify in AWS Mgmt Console (both instances and security groups should be deleted)
```

## Step-06: Delete dev workspace
- We cannot delete "default" workspace
- We can delete workspaces which we created (dev, qa etc)
```t
# Delete Dev Workspace
terraform workspace delete dev
Observation: Workspace "dev" is not empty.
Deleting "dev" can result in dangling resources: resources that
exist but are no longer manageable by Terraform. Please destroy
these resources first.  If you want to delete this workspace
anyway and risk dangling resources, use the '-force' flag.

# Switch to Dev Workspace
terraform workspace select dev

# Destroy Resources
terraform destroy -auto-approve

# Delete Dev Workspace
terraform workspace delete dev
Observation:
Workspace "dev" is your active workspace.
You cannot delete the currently active workspace. Please switch
to another workspace and try again.

# Switch Workspace to default
terraform workspace select default

# Delete Dev Workspace
terraform workspace delete dev
Observation: Successfully delete workspace dev

# Verify 
In AWS mgmt console, all EC2 instances should be deleted
```

## Step-07: Clean-Up Local folder
```t
# Clean-Up local folder
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-08: Terraform Workspaces in combination with Terraform Backend (Remote State Storage)
### Step-08-01: Review terraform manifest (primarily c1-versions.tf)
- **Reference Sub-Folder:** v2-remote-backend
- Only change in the template is in **c1-versions.tf**, we will have the remote backend block which we learned during section **07-01-Terraform-Remote-State-Storage-and-Locking**
```t
  # Adding Backend as S3 for Remote State Storage
  backend "s3" {
    bucket = "terraform-stacksimplify"
    key    = "workspaces/terraform.tfstate"
    region = "us-east-1"  
  # For State Locking
    dynamodb_table = "terraform-dev-state-table"           
  }
```
### Step-08-02: Provision infra using default workspace
```t
# Initialize Terraform
terraform init

# List Terraform Workspaces
terraform workspace list

# Show current Terraform workspace
terraform workspace show

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Review State file in S3 Bucket for default workspace
Go to AWS Mgmt Console -> Services -> S3 -> terraform-stacksimplify -> workspaces -> terraform.tfstate
```
### Step-08-03: Create new workspace dev and provison infra using that workspace
```t
# List Terraform Workspaces
terraform workspace list

# Create New Terraform Workspace
terraform workspace new dev

# List Terraform Workspaces
terraform workspace list

# Show current Terraform workspace
terraform workspace show

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Review State file in S3 Bucket for dev workspace
Go to AWS Mgmt Console -> Services -> S3 -> terraform-stacksimplify -> env:/ -> dev -> workspaces -> terraform.tfstate
```

### Step-08-04: Destroy resources in both workspaces (default, dev)

```t
# Show current Terraform workspace
terraform workspace show

# Destroy Resources in Dev Workspace
terraform destroy -auto-approve

# Show current Terraform workspace
terraform workspace show

# Select other workspace
terraform workspace select default

# Show current Terraform workspace
terraform workspace show

# Destroy Resources in default Workspace
terraform destroy -auto-approve

# Delete Dev Workspace
terraform workspace delete dev
```
### Step-08-05: Try deleting default workspace and understand what happens

```t
# Try deleting default workspace
terraform workspace delete default
Observation: 
1) Workspace "default" is your active workspace.
2) You cannot delete the currently active workspace. Please switch
to another workspace and try again.

# Create new workspace
terraform workspace new test1

# Show current Terraform workspace
terraform workspace show

# Try deleting default workspace now
terraform workspace delete default
Observation:
1) can't delete default state
```

### Step-08-06: Clean-Up
```t
# Switch workspace to default
terraform workspace select default

# Delete test1 workspace
terraform workspace delete test1

# Clean-Up Terraform local folders
rm -rf .terraform*

# Clean-Up State files in S3 bucket 
Go to S3 Bucket and delete files
```


## References
- [Terraform Workspaces](https://www.terraform.io/docs/language/state/workspaces.html)
- [Managing Workspaces](https://www.terraform.io/docs/cli/workspaces/index.html)======================================================================
***********read the file ./BACKUP-2024/09-Terraform-Provisioners/09-01-File-Provisioner/README.md ************
# Terraform Provisioners

## Step-00: Provisioner Concepts
- Generic Provisioners
  - file
  - local-exec
  - remote-exec
- Provisioner Timings
  - Creation-Time Provisioners (by default)
  - Destroy-Time Provisioners  
- Provisioner Failure Behavior
  - continue
  - fail
- Provisioner Connections
- Provisioner Without a Resource  (Null Resource)

## Pre-requisites
- Create a EC2 Key pain with name `terraform-key` and copy the `terraform-key.pem` file in the folder `private-key` in `terraform-manifest` folder
- Connection Block for provisioners uses this to connect to newly created EC2 instance to copy files using `file provisioner`, execute scripts using `remote-exec provisioner`

## Step-01: Introduction
- Understand about File Provisioners
- Create Provisioner Connections block required for File Provisioners
- We will also discuss about **Creation-Time Provisioners (by default)**
- Understand about Provisioner Failure Behavior
  - continue
  - fail
- Discuss about Destroy-Time Provisioners    


## Step-02: File Provisioner & Connection Block
- **Referencec Sub-Folder:** terraform-manifests
- Understand about file provisioner & Connection Block
- **Connection Block**
  - We can have connection block inside resource block for all provisioners 
  -[or] We can have connection block inside a provisioner block for that respective provisioner
- **Self Object**
  - **Important Technical Note:** Resource references are restricted here because references create dependencies. Referring to a resource by name within its own block would create a dependency cycle.
  - Expressions in provisioner blocks cannot refer to their parent resource by name. Instead, they can use the special **self object.**
  - The **self object** represents the provisioner's parent resource, and has all of that resource's attributes. 
```t
  # Connection Block for Provisioners to connect to EC2 Instance
  connection {
    type = "ssh"
    host = self.public_ip # Understand what is "self"
    user = "ec2-user"
    password = ""
    private_key = file("private-key/terraform-key.pem")
  }  
```

## Step-03: Create multiple provisioners of various types
- **Creation-Time Provisioners:** 
- By default, provisioners run when the resource they are defined within is created. 
- Creation-time provisioners are only run during creation, not during updating or any other lifecycle. 
- They are meant as a means to perform bootstrapping of a system.
- If a creation-time provisioner fails, the resource is marked as tainted. 
- A tainted resource will be planned for destruction and recreation upon the next terraform apply.
- Terraform does this because a failed provisioner can leave a resource in a semi-configured state. 
- Because Terraform cannot reason about what the provisioner does, the only way to ensure proper creation of a resource is to recreate it. This is tainting.
- You can change this behavior by setting the on_failure attribute, which is covered in detail below.

```t

 # Copies the file-copy.html file to /tmp/file-copy.html
  provisioner "file" {
    source      = "apps/file-copy.html"
    destination = "/tmp/file-copy.html"
  }

  # Copies the string in content into /tmp/file.log
  provisioner "file" {
    content     = "ami used: ${self.ami}" # Understand what is "self"
    destination = "/tmp/file.log"
  }

  # Copies the app1 folder to /tmp - FOLDER COPY
  provisioner "file" {
    source      = "apps/app1"
    destination = "/tmp"
  }

  # Copies all files and folders in apps/app2 to /tmp - CONTENTS of FOLDER WILL BE COPIED
  provisioner "file" {
    source      = "apps/app2/" # when "/" at the end is added - CONTENTS of FOLDER WILL BE COPIED
    destination = "/tmp"
  }
 # Copies the file-copy.html file to /tmp/file-copy.html
  provisioner "file" {
    source      = "apps/file-copy.html"
    destination = "/tmp/file-copy.html"
  }

  # Copies the string in content into /tmp/file.log
  provisioner "file" {
    content     = "ami used: ${self.ami}" # Understand what is "self"
    destination = "/tmp/file.log"
  }

  # Copies the app1 folder to /tmp - FOLDER COPY
  provisioner "file" {
    source      = "apps/app1"
    destination = "/tmp"
  }

  # Copies all files and folders in apps/app2 to /tmp - CONTENTS of FOLDER WILL BE COPIED
  provisioner "file" {
    source      = "apps/app2/" # when "/" at the end is added - CONTENTS of FOLDER WILL BE COPIED
    destination = "/tmp"
  }
```

## Step-04: Create Resources using Terraform commands

```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify - Login to EC2 Instance
chmod 400 private-key/terraform-key.pem 
ssh -i private-key/terraform-key.pem ec2-user@IP_ADDRESSS_OF_YOUR_VM
ssh -i private-key/terraform-key.pem ec2-user@54.197.54.126
Verify /tmp for all files copied
cd /tmp
ls -lrta /tmp

# Clean-up
terraform destroy -auto-approve
rm -rf terraform.tfsate*
```

## Step-05: Failure Behavior: Understand Decision making when provisioner fails (continue / fail)
- By default, provisioners that fail will also cause the Terraform apply itself to fail. The on_failure setting can be used to change this. The allowed values are:
  - **continue:** Ignore the error and continue with creation or destruction.
  - **fail:** (Default Behavior) Raise an error and stop applying (the default behavior). If this is a creation provisioner, taint the resource.  
- Try copying a file to Apache static content folder "/var/www/html" using file-provisioner
- This will fail because, the user you are using to copy these files is "ec2-user" for amazon linux vm. This user don't have access to folder "/var/www/html/" top copy files. 
- We need to use sudo to do that. 
- All we know is we cannot copy it directly, but we know we have already copied this file in "/tmp" using file provisioner
- **Try two scenarios**
  - No `on_failure` attribute (Same as `on_failure = fail`) - default what happens It will Raise an error and stop applying. If this is a creation provisioner, it will taint the resource.
  - When `on_failure = continue`, will continue creating resources
  - **Verify:**  Verify `terraform.tfstate` for  `"status": "tainted"`
```t
# Test-1: Without on_failure attribute which will fail terraform apply
 # Copies the file-copy.html file to /var/www/html/file-copy.html
  provisioner "file" {
    source      = "apps/file-copy.html"
    destination = "/var/www/html/file-copy.html"
   }
###### Verify:  Verify terraform.tfstate for  "status": "tainted"

# Test-2: With on_failure = continue
 # Copies the file-copy.html file to /var/www/html/file-copy.html
  provisioner "file" {
    source      = "apps/file-copy.html"
    destination = "/var/www/html/file-copy.html"
    on_failure  = continue 
   }
###### Verify:  Verify terraform.tfstate for  "status": "tainted"  
```
```t

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify
Login to EC2 Instance
Verify /tmp, /var/www/html for all files copied
```

## Step-06: Clean-Up Resources & local working directory
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-07: Destroy Time Provisioners
- Discuss about this concept
- [Destroy Time Provisioners](https://www.terraform.io/docs/language/resources/provisioners/syntax.html#destroy-time-provisioners)
- Inside a provisioner when you add this statement `when    = destroy` it will provision this during the resource destroy time
```t
resource "aws_instance" "web" {
  # ...

  provisioner "local-exec" {
    when    = destroy 
    command = "echo 'Destroy-time provisioner'"
  }
}
```======================================================================
***********read the file ./BACKUP-2024/09-Terraform-Provisioners/09-02-remote-exec-provisioner/README.md ************
# Terraform remote-exec Provisioner

## Step-00: Pre-requisites
- Create a EC2 Key pain with name `terraform-key` and copy the `terraform-key.pem` file in the folder `private-key` in `terraform-manifest` folder
- Connection Block for provisioners uses this to connect to newly created EC2 instance to copy files using `file provisioner`, execute scripts using `remote-exec provisioner`

## Step-01: Introduction
- Understand about **remote-exec** Provisioner
- The `remote-exec` provisioner invokes a script on a remote resource after it is created. 
- This can be used to run a configuration management tool, bootstrap into a cluster, etc. 

## Step-02: Create / Review Provisioner configuration
- **Usecase:** 
1. We will copy a file named `file-copy.html` using `File Provisioner` to "/tmp" directory
2. Using `remote-exec provisioner`, using linux commands we will in-turn copy the file to Apache Webserver static content directory `/var/www/html` and access it via browser once it is provisioned
```t
 # Copies the file-copy.html file to /tmp/file-copy.html
  provisioner "file" {
    source      = "apps/file-copy.html"
    destination = "/tmp/file-copy.html"
  }

# Copies the file to Apache Webserver /var/www/html directory
  provisioner "remote-exec" {
    inline = [
      "sleep 120",  # Will sleep for 120 seconds to ensure Apache webserver is provisioned using user_data
      "sudo cp /tmp/file-copy.html /var/www/html"
    ]
  }
```

## Step-03: Review Terraform manifests & Execute Terraform Commands
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify
1) Login to EC2 Instance
chmod 400 private-key/terraform-key.pem 
ssh -i private-key/terraform-key.pem ec2-user@IP_ADDRESSS_OF_YOUR_VM
ssh -i private-key/terraform-key.pem ec2-user@54.197.54.126

2) Verify /tmp for file named file-copy.html all files copied (ls -lrt /tmp/file-copy.html)
3) Verify /var/www/html for a file named file-copy.html (ls -lrt /var/www/html/file-copy.html)
4) Access via browser http://<Public-IP>/file-copy.html
```
## Step-04: Clean-Up Resources & local working directory
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

======================================================================
***********read the file ./BACKUP-2024/09-Terraform-Provisioners/09-03-local-exec-provisioner/README.md ************
# Terraform local-exec Provisioner

## Step-00: Pre-requisites
- Create a EC2 Key pain with name `terraform-key` and copy the `terraform-key.pem` file in the folder `private-key` in `terraform-manifest` folder
- Connection Block for provisioners uses this to connect to newly created EC2 instance to copy files using `file provisioner`, execute scripts using `remote-exec provisioner`

## Step-01: Introduction
- Understand about **local-exec** Provisioner
- The `local-exec` provisioner invokes a local executable after a resource is created. 
- This invokes a process on the machine running Terraform, not on the resource. 

## Step-02: Review local-exec provisioner code
- We will create one provisioner during creation-time. It will output private ip of the instance in to a file named `creation-time-private-ip.txt`
- We will create one more provisioner during destroy time. It will output destroy time with date in to a file named `destroy-time.txt`
- **C3-ec2-instance.tf**
```t
  # local-exec provisioner (Creation-Time Provisioner - Triggered during Create Resource)
  provisioner "local-exec" {
    command = "echo ${aws_instance.my-ec2-vm.private_ip} >> creation-time-private-ip.txt"
    working_dir = "local-exec-output-files/"
    #on_failure = continue
  }

  # local-exec provisioner - (Destroy-Time Provisioner - Triggered during Destroy Resource)
  provisioner "local-exec" {
    when    = destroy
    command = "echo Destroy-time provisioner Instanace Destroyed at `date` >> destroy-time.txt"
    working_dir = "local-exec-output-files/"
  }  
```


## Step-03: Review Terraform manifests & Execute Terraform Commands
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify
Verify the file in folder "local-exe-output-files/creation-time-private-ip.txt"

```
## Step-04: Clean-Up Resources & local working directory
```t
# Terraform Destroy
terraform destroy -auto-approve

# Verify
Verify the file in folder "local-exe-output-files/destroy-time.txt"

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

======================================================================
***********read the file ./BACKUP-2024/09-Terraform-Provisioners/09-04-Null-Resource/README.md ************
# Terraform Null Resource

## Step-01: Introduction
- Understand about [Null Provider](https://registry.terraform.io/providers/hashicorp/null/latest/docs)
- Understand about [Null Resource](https://www.terraform.io/docs/language/resources/provisioners/null_resource.html)
- Understand about [Time Provider](https://registry.terraform.io/providers/hashicorp/time/latest/docs)
- **Usecase:** Force a resource to update based on a changed null_resource
- Create `time_sleep` resource to wait for 90 seconds after EC2 Instance creation
- Create Null resource with required provisioners
  - File Provisioner: Copy apps/app1 folder to /tmp
  - Remote Exec Provisioner: Copy app1 folder from /tmp to /var/www/htnl
- Over the process we will learn about
  - null_resource
  - time_sleep resource
  - We will also learn how to Force a resource to update based on a changed null_resource using `timestamp function` and `triggers` in `null_resource`


## Step-02: Define null provider in Terraform Settings Block
- Update null provider info listed below in **c1-versions.tf**
```t
    null = {
      source = "hashicorp/null"
      version = "~> 3.0.0"
    }
```

## Step-03: Define Time Provider in Terraform Settings Block
- Update time provider info listed below in **c1-versions.tf**
```t
    time = {
      source = "hashicorp/time"
      version = "~> 0.6.0"
    }  
```

## Step-04: Create / Review the c4-ec2-instance.tf terraform configuration
### Step-04-01: Create Time Sleep Resource
- This resource will wait for 90 seconds after EC2 Instance creation.
- This wait time will give EC2 Instance to provision the Apache Webserver and create all its relevant folders
- Primarily if we want to copy static content we need Apache webserver static folder `/var/www/html`
```t
# Wait for 90 seconds after creating the above EC2 Instance 
resource "time_sleep" "wait_90_seconds" {
  depends_on = [aws_instance.my-ec2-vm]
  create_duration = "90s"
}
```
### Step-04-02: Create Null Resource
- Create Null resource with `triggers` with `timestamp()` function which will trigger for every `terraform apply`
- This `Null Resource` will help us to sync the static content from our local folder to EC2 Instnace as and when required.
- Also only changes will applied using only `null_resource` when `terraform apply` is run. In other words, when static content changes, how will we sync those changes to EC2 Instance using terraform - This is one simple solution.
- Primarily the focus here is to learn the following
  - null_resource
  - null_resource trigger
  - How trigger works based on timestamp() function ?
  - Provisioners in Null Resource
```t
# Sync App1 Static Content to Webserver using Provisioners
resource "null_resource" "sync_app1_static" {
  depends_on = [ time_sleep.wait_90_seconds ]
  triggers = {
    always-update =  timestamp()
  }

  # Connection Block for Provisioners to connect to EC2 Instance
  connection {
    type = "ssh"
    host = aws_instance.my-ec2-vm.public_ip 
    user = "ec2-user"
    password = ""
    private_key = file("private-key/terraform-key.pem")
  }  

 # Copies the app1 folder to /tmp
  provisioner "file" {
    source      = "apps/app1"
    destination = "/tmp"
  }

# Copies the /tmp/app1 folder to Apache Webserver /var/www/html directory
  provisioner "remote-exec" {
    inline = [
      "sudo cp -r /tmp/app1 /var/www/html"
    ]
  }
}
```

## Step-05: Execute Terraform Commands
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify
ssh -i private-key/terraform-key.pem ec2-user@<PUBLIC-IP>
ls -lrt /tmp
ls -lrt /tmp/app1
ls -lrt /var/www/html
ls -lrt /var/www/html/app1
http://<public-ip>/app1/file1.html
http://<public-ip>/app1/file2.html
```

## Step-06: Create new file locally in app1 folder
- Create a new file named `file3.html`
- Also updated `file1.html` with some additional info
- **file3.html**
```html
<h1>>App1 File3</h1
```
- **file1.html**
```html
<h1>>App1 File1 - Updated</h1
```

## Step-07: Execute Terraform plan and apply commands
```t
# Terraform Plan
terraform plan
Observation: You should see changes for "null_resource.sync_app1_static" because trigger will have new timestamp when you fired the terraform plan command

# Terraform Apply
terraform apply -auto-approve

# Verify
chmod 400 private-key/terraform-key.pem
ssh -i private-key/terraform-key.pem ec2-user@<PUBLIC-IP>
ls -lrt /tmp
ls -lrt /tmp/app1
ls -lrt /var/www/html
ls -lrt /var/www/html/app1
http://<public-ip>/app1/file1.html
http://<public-ip>/app1/file3.html
```

## Step-08: Clean-Up Resources & local working directory
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```



## References
- [Resource: time_sleep](https://registry.terraform.io/providers/hashicorp/time/latest/docs/resources/sleep)
- [Time Provider](https://registry.terraform.io/providers/hashicorp/time/latest/docs)
======================================================================
***********read the file ./BACKUP-2024/10-Terraform-Modules/10-01-Terraform-Modules-Basics/README.md ************
# Terraform Module Basics

## Step-01: Introduction
1. Introduction - Module Basics  
  - Root Module
  - Child Module
  - Published Modules (Terraform Registry)

2. Module Basics 
  - Defining a Child Module
    - Source (Mandatory)
    - Version
    - Meta-arguments (count, for_each, providers, depends_on, )
    - Accessing Module Output Values
    - Tainting resources within a module

## Step-02: Defining a Child Module
- We need to understand about the following
  - Module Source (Mandatory): To start with we will use Terraform Registry
  - Module Version (Optional): Recommended to use module version
- Create a simple EC2 Instance module
  - c1-versions.tf: standard
  - c2-variables.tf: standard
  - c3-ami-datasource.tf: standard
  - c4-ec2instance-module.tf: We will focus on building this template  
```t
# AWS EC2 Instance Module

module "ec2_cluster" {
  source                 = "terraform-aws-modules/ec2-instance/aws"
  version                = "~> 2.0"

  name                   = "my-modules-demo"
  instance_count         = 2

  ami                    = data.aws_ami.amzlinux.id
  instance_type          = "t2.micro"
  key_name               = "terraform-key"
  monitoring             = true
  vpc_security_group_ids = ["sg-08b25c5a5bf489ffa"]  # Get Default VPC Security Group ID and replace
  subnet_id              = "subnet-4ee95470" # Get one public subnet id from default vpc and replace
  user_data               = file("apache-install.sh")

  tags = {
    Name        = "Modules-Demo"
    Terraform   = "true"
    Environment = "dev"
  }
}
```

## Step-03: Define Outputs from a EC2 Instance Module
- c5-outputs.tf: We will output the EC2 Instance Module attributes (Public DNS and Public IP)
```t
# Output variable definitions

output "ec2_instance_public_ip" {
  description = "Public IP addresses of EC2 instances"
  value       = module.ec2_cluster.*.public_ip
}

output "ec2_instance_public_dns" {
  description = "Public IP addresses of EC2 instances"
  value       = module.ec2_cluster.*.public_dns
}
```

## Step-04: Execute Terraform Commands
```t
# Terraform Init
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-apporve

# Verify 
1) Verify in AWS management console , if EC2 Instances got created.
2) Update default security group of a VPC to allow port 80 from internet
3) Access Apache Webserver
http://<Public-IP-VM1>
http://<Public-IP-VM2>
```

## Step-05: Tainting Resources in a Module
- The **taint command** can be used to taint specific resources within a module
- **Very Very Important Note:** It is not possible to taint an entire module. Instead, each resource within the module must be tainted separately.
```t
# List Resources from State
terraform state list

# Taint a Resource
terraform taint <RESOURCE-NAME>
terraform taint module.ec2_cluster.aws_instance.this[0]

# Terraform Plan
terraform plan
Observation: First VM will be destroyed and re-created

# Terraform Apply
terraform apply -auto-approve
```

## Step-06: Clean-Up Resources & local working directory
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-07: Meta-Arguments for Modules
- Meta-Argument concepts are going to be same as how we learned during Resources section.
  - count
  - for_each
  - providers
  - depends_on
- [Meta-Arguments for Modules](https://www.terraform.io/docs/language/modules/syntax.html#meta-arguments)


## References
- [Terraform EC2 Instance Module](https://registry.terraform.io/modules/terraform-aws-modules/ec2-instance/aws/latest)======================================================================
***********read the file ./BACKUP-2024/10-Terraform-Modules/10-02-Terraform-Build-a-Module/Oldv1- ************
======================================================================
***********read the file backup/v3-build-a-module-to-host-static-website-on-aws-s3/modules/aws-s3-static-website-bucket/README.md ************
======================================================================
***********read the file ./BACKUP-2024/10-Terraform-Modules/10-02-Terraform-Build-a-Module/README.md ************
# Build a Terraform Module

## Step-01: Introduction
- Build a Terraform Module
    - Create a Terraform module
    - Use local Terraform modules in your configuration
    - Configure modules with variables
    - Use module outputs
    - We are going to write a local re-usable module for the following usecase.
- **Usecase: Hosting a static website with AWS S3 buckets**
1. Create an S3 Bucket
2. Create Public Read policy for the bucket
3. Once above two are ready, we can deploy Static Content 
4. For steps, 1 and 2 we are going to create a re-usable module in Terraform
- **How are we going to do this?**
- We are going to do this in 3 sections
- **Section-1 - Full Manual:** Create Static Website on S3 using AWS Management Consoleand host static content and test 
- **Section-2 - Terraform Resources:** Automate section-1 using Terraform Resources
- **Section-3 - Terraform Modules:** Create a re-usable module for hosting static website by referencing section-2 terraform configuration files. 

## Step-02: Hosting a Static Website with AWS S3 using AWS Management Console
- **Reference Sub-folder:** v1-create-static-website-on-s3-using-aws-mgmt-console
- We are going to host a static website with AWS S3 using AWS Management console
### Step-02-01: Create AWS S3 Bucket
- Go to AWS Services -> S3 -> Create Bucket 
- **Bucket Name:** mybucket-1045 (Note: Bucket name should be unique across AWS)
- **Region:** US.East (N.Virginia)
- Rest all leave to defaults
- Click on **Create Bucket**

### Step-02-02: Enable Static website hosting
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Properties Tab -> At the end
- Edit to enable **Static website hosting**
- **Static website hosting:** enable
- **Index document:** index.html
- Click on **Save Changes**

### Step-02-03: Remove Block public access (bucket settings)
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Permissions Tab 
- Edit **Block public access (bucket settings)** 
- Uncheck **Block all public access**
- Click on **Save Changes**
- Provide text `confirm` and Click on **Confirm**

### Step-02-04: Add Bucket policy for public read by bucket owners
- Update your bucket name in the below listed policy
- **Location:** v1-create-static-website-on-s3-using-aws-mgmt-console/policy-public-read-access-for-website.json
```json
{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Sid": "PublicReadGetObject",
          "Effect": "Allow",
          "Principal": "*",
          "Action": [
              "s3:GetObject"
          ],
          "Resource": [
              "arn:aws:s3:::mybucket-1045/*"
          ]
      }
  ]
}
```
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Permissions Tab 
- Edit -> **Bucket policy** -> Copy paste the policy above with your bucket name
- Click on **Save Changes**

### Step-02-05: Upload index.html
- **Location:** v1-create-static-website-on-s3-using-aws-mgmt-console/index.html
- Go to AWS Services -> S3 -> Buckets -> mybucket-1045 -> Objects Tab 
- Upload **index.html**

### Step-02-06: Access Static Website using S3 Website Endpoint
- Access the newly uploaded index.html to S3 bucket using browser
```
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1045.s3-website.us-east-1.amazonaws.com/
```

### Step-02-07: Conclusion
- We have used multiple manual steps to host a static website on AWS
- Now all the above manual steps automate using Terraform in next step

## Step-03: Create Terraform Configuration to Host a Static Website on AWS S3
- **Reference Sub-folder:** v2-host-static-website-on-s3-using-terraform-manifests
- We are going to host a static website on AWS S3 using general terraform configuration files
### Step-03-01: Create Terraform Configuration Files step by step
1. versions.tf
2. main.tf
3. variables.tf
4. outputs.tf
5. terraform.tfvars

### Step-03-02: Execute Terraform Commands & Verify the bucket
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify 
1. Bucket has static website hosting enabled
2. Bucket has public read access enabled using policy
3. Bucket has "Block all public access" unchecked
```

### Step-03-03: Upload index.html and test
```
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1046.s3-website.us-east-1.amazonaws.com/
```
### Step-03-04: Destroy and Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```


### Step-03-05: Conclusion
- Using above terraform configurations we have hosted a static website in AWS S3 in seconds. 
- In next step, we will convert these **terraform configuration files** to a Module which will be re-usable just by calling it.


## Step-04: Build a Terraform Module to Host a Static Website on AWS S3
- **Reference Sub-folder:** v3-build-a-module-to-host-static-website-on-aws-s3
- We will build a Terraform module to host a static website on AWS S3

### Step-04-01: Create Module Folder Structure
- We are going to create `modules` folder and in that we are going to create a module named `aws-s3-static-website-bucket`
- We will copy required files from previous section for this respective module.
- Terraform Working Directory: v3-build-a-module-to-host-static-website-on-aws-s3
    - modules
        - Module-1: aws-s3-static-website-bucket
            - main.tf
            - variables.tf
            - outputs.tf
            - README.md
            - LICENSE
- Inside `modules/aws-s3-static-website-bucket`, copy below listed three files from `v2-host-static-website-on-s3-using-terraform-manifests`
    - main.tf
    - variables.tf
    - outputs.tf


### Step-04-02: Call Module from Terraform Work Directory (Root Module)
- Create Terraform Configuration in Root Module by calling the newly created module
- c1-versions.tf
- c2-variables.tf
- c3-s3bucket.tf
- c4-outputs.tf
```t
module "website_s3_bucket" {
  source = "./modules/aws-s3-static-website-bucket"
  bucket_name = var.my_s3_bucket
  tags = var.my_s3_tags
}
```
### Step-04-03: Execute Terraform Commands
```
# Terraform Initialize
terraform init
Observation: 
1. Verify ".terraform", you will find "modules" folder in addition to "providers" folder
2. Verify inside ".terraform/modules" folder too.

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify 
1. Bucket has static website hosting enabled
2. Bucket has public read access enabled using policy
3. Bucket has "Block all public access" unchecked
```

### Step-04-04: Upload index.html and test
```
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1047.s3-website.us-east-1.amazonaws.com/
```

### Step-04-05: Destroy and Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

### Step-04-06: Understand terraform get command
- We have used `terraform init` to download providers from terraform registry and at the same time to download `modules` present in local modules folder in terraform working directory. 
- Assuming we already have initialized using `terraform init` and later we have created `module` configs, we can `terraform get` to download the same.
- Whenever you add a new module to a configuration, Terraform must install the module before it can be used. 
- Both the `terraform get` and `terraform init` commands will install and update modules. 
- The `terraform init` command will also initialize backends and install plugins.
```
# Delete modules in .terraform folder
ls -lrt .terraform/modules
rm -rf .terraform/modules
ls -lrt .terraform/modules

# Terraform Get
terraform get
ls -lrt .terraform/modules
```
### Step-04-07: Major difference between Local and Remote Module
- When installing a remote module, Terraform will download it into the .terraform directory in your configuration's root directory. 
- When installing a local module, Terraform will instead refer directly to the source directory. 
- Because of this, Terraform will automatically notice changes to local modules without having to re-run terraform init or terraform get.

## Step-05: Conclusion
- Created a Terraform module
- Used local Terraform modules in your configuration
- Configured modules with variables
- Used module outputs



















======================================================================
***********read the file ./BACKUP-2024/10-Terraform-Modules/10-02-Terraform-Build-a-Module/v3-build-a-module-to-host-static-website-on-aws-s3/modules/aws-s3-static-website-bucket/README.md ************
# AWS S3 static website bucket
- This module provisions AWS S3 buckets configured for static website hosting.
- This will be a demo S3 module

======================================================================
***********read the file ./BACKUP-2024/11-Terraform-Cloud-and-Enterprise-Capabilities/11-01-Terraform-Cloud-Github-Integration/README.md ************
# Terraform Cloud & Github Integration

## Step-01: Introduction
- Create Github Repository on github.com
- Clone Github Repository to local desktop
- Copy & Check-In Terraform Configurations in to Github Repository
- Create Terraform Cloud Account
- Create Organization
- Create Workspace by integrating with Github.com Git Repo we recently created
- Learn about Workspace related Queue Plan, Runs, States, Variables and Settings


## Step-02: Create new github Repository
- **URL:** github.com
- Click on **Create a new repository**
- **Repository Name:** terraform-cloud-demo1
- **Description:** Terraform Cloud Demo 
- **Repo Type:** Public / Private
- **Initialize this repository with:**
- **CHECK** - Add a README file
- **CHECK** - Add .gitignore 
- **Select .gitignore Template:** Terraform
- **CHECK** - Choose a license
- **Select License:** Apache 2.0 License
- Click on **Create repository**

## Step-03: Review .gitignore created for Terraform
- Review .gitignore created for Terraform projects

## Step-04: Clone Github Repository to Local Desktop
```t
# Clone Github Repo
git clone https://github.com/<YOUR_GITHUB_ID>/<YOUR_REPO>.git
git clone https://github.com/stacksimplify/terraform-cloud-demo1.git
```

## Step-05: Copy files from terraform-manifests to local repo & Check-In Code
- List of files to be copied
  - apache-install.sh
  - c1-versions.tf
  - c2-variables.tf
  - c3-security-groups.tf
  - c4-ec2-instance.tf
  - c5-outputs.tf
  - c6-ami-datasource.tf
- Source Location: Section-11-01 - Inside terraform-manifests folder
- Destination Location: Newly cloned github repository folder in your local desktop `terraform-cloud-demo1`
- Verify locally before commiting to GIT Repository
```t
# Terraform Init
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan
```
- Check-In code to Remote Repository
```t
# GIT Status
git status

# Git Local Commit
git add .
git commit -am "TF Files First Commit"

# Push to Remote Repository
git push

# Verify the same on Remote Repository
https://github.com/stacksimplify/terraform-cloud-demo1.git
```

## Step-06: Sign-Up for Terraform Cloud - Free Account & Login
- **SignUp URL:** https://app.terraform.io/signup/account
- **Username:**
- **Email:**
- **Password:** 
- **Login URL:** https://app.terraform.io

## Step-07: Create Organization 
- **Organization Name:** hcta-demo1
- **Email Address:** stacksimplify@gmail.com
- Click on **Create Organization**

## Step-08: Create New Workspace
- Get in to newly created Organization
- Click on **New Workspace**
- **Choose your workflow:** V
  - Version Control Workflow
- **Connect to VCS**
  - **Connect to a version control provider:** github.com
  - NEW WINDOW: **Authorize Terraform Cloud:** Click on **Authorize Terraform Cloud Button**
  - NEW WINDOW: **Install Terraform Cloud**
  - **Select radio button:** Only select repositories
  - **Selected 1 Repository:** stacksimplify/terraform-cloud-demo1
  - Click on **Install**
- **Choose a Repository**
  - stacksimplify/terraform-cloud-demo1
- **Configure Settings**
  - **Workspace Name:** terraform-cloud-demo1 (Whatever populated automically leave to defaults) 
  - **Advanced Settings:** leave to defaults 
- Click on **Create Workspace**  
- You should see this message `Configuration uploaded successfully`


## Step-09: Configure Variables
- **Variable:** aws_region
  - key: aws_region
  - value: us-east-1
- **Variable:** instance_type
  - key: instance_type
  - value: t3.micro

## Step-10: Configre Environment Variables
- [Setup AWS Access Keys for Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#environment-variables)
- Configure AWS Access Key ID and Secret Access Key  
- **Environment Variable:** AWS_ACCESS_KEY_ID
  - Key: AWS_ACCESS_KEY_ID
  - Value: XXXXXXXXXXXXXXXXXXXXXX
- **Environment Variable:** AWS_SECRET_ACCESS_KEY
  - Key: AWS_SECRET_ACCESS_KEY
  - Value: YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY

## Step-11: Click on Queue Plan
- Go to Workspace -> Runs -> Queue Plan
- Review the plan generated in **Full Screen**
- **Add Comment:** First Run
- Click on **Confirm & Apply**
- **Add Comment:** First Run Approved
- Click on **Confirm Plan**
- Review Apply log output in **Full Screen**
- **Add Comment:** Successfully Provisioned, Verified in AWS

## Step-12: Review Terraform State
- Go to Workspace -> States
- Review the state file

## Step-13: Change Number of Instance
- Go to Local Desktop -> Local Repo -> c4-ec2-instance.tf -> Change count from 1 to 2
```t
# Change c4-ec2-instance.tf
count = 2

# GIT Status
git status

# Git Local Commit
git add .
git commit -am "Changed EC2 Instances from 1 to 2"

# Push to Remote Repository
git push

# Verify Terraform Cloud
Go to Workspace -> Runs 
Observation: 
1) New plan should be queued ->  Click on Current Plan and review logs in Full Screen
2) Click on **Confirm and Apply**
3) Add Comment: Approved for new EC2 Instance and Click on **Confirm Plan**
4) Verify Apply Logs in Full Screen
5) Review the update state in  Workspace -> States
6) Verify if new EC2 Instance got created
```

## Step-14: Review Workpace Settings
- Goto -> Workspace -> Settings
  - General Settings
  - Locking
  - Notifications
  - Run Triggers
  - SSH Key
  - Version Control

## Step-15: Destruction and Deletion
- Goto -> Workspace -> Settings -> Destruction and Deletion
- click on **Queue Destroy Plan** to delete the resources on cloud 
- Goto -> Workspac -> Runs -> Click on **Confirm & Apply**
- **Add Comment:** Approved for Deletion

======================================================================
***********read the file ./BACKUP-2024/11-Terraform-Cloud-and-Enterprise-Capabilities/11-02-Share-Modules-in-Private-Module-Registry/README.md ************
# Share Modules in Private Modules Registry

## Step-01: Introduction
- Create and version a GitHub repository for use in the private module registry
- Import a module into your organization's private module registry.
- Construct a root module to consume modules from the private registry.
- Over the process also learn about `terraform login` command

## Step-02: Create new github Repository for s3-website terraform module
### Step-02-01: Craete new Github Repository
- **URL:** github.com
- Click on **Create a new repository**
- Follow Naming Conventions for modules
  - terraform-PROVIDER-MODULE_NAME
  - **Sample:** terraform-aws-s3-website
- **Repository Name:** terraform-aws-s3-website
- **Description:** Terraform Modules to be shared in Private Registry
- **Repo Type:** Public / Private
- **Initialize this repository with:**
- **UN-CHECK** - Add a README file
- **CHECK** - Add .gitignore 
- **Select .gitignore Template:** Terraform
- **CHECK** - Choose a license
- **Select License:** Apache 2.0 License  (Optional)
- Click on **Create repository**
### Step-02-02: Create New Release Tag 1.0.0 in Repo
- Go to Right Navigation on github Repo -> Releases -> Create a new release
- **Tag Version:** 1.0.0
- **Release Title:** Release-1 terraform-aws-s3-website
- **Write:** Terraform Module for Private Registry - terraform-aws-s3-website
- Click on **Publish Release**


## Step-03: Clone Github Repository to Local Desktop
```t
# Clone Github Repo
git clone https://github.com/<YOUR_GITHUB_ID>/<YOUR_REPO>.git
git clone https://github.com/stacksimplify/terraform-aws-s3-website.git
```

## Step-04: Copy files from terraform-manifests to local repo & Check-In Code
- **Orignial Source Location:** 10-Terraform-Modules/10-02-Terraform-Build-a-Module/v3-build-a-module-to-host-static-website-on-aws-s3/modules/aws-s3-static-website-bucket
- **Source Location from this section:** terraform-s3-website-module-manifests
- **Destination Location:** Newly cloned github repository folder in your local desktop `terraform-module-s3-website`
- Check-In code to Remote Repository
```t
# GIT Status
git status

# Git Local Commit
git add .
git commit -am "TF Module Files First Commit"

# Push to Remote Repository
git push

# Verify the same on Remote Repository
https://github.com/stacksimplify/terraform-aws-s3-website.git
```

## Step-05: Add VCS Provider as Github using OAuth App in Terraform Cloud 

### Step-05-01: Add VCS Provider as Github using OAuth App in Terraform Cloud
- Login to Terraform Cloud
- Click on Modules Tab -> Click on Add Module -> Select Github(Custom)
- Should redirect to URL: https://github.com/settings/applications/new in new browser tab
- **Application Name:** Terraform Cloud (hctaprep) 
- **Homepage URL:**	https://app.terraform.io 
- **Application description:**	Terraform Cloud Integration with Github using OAuth 
- **Authorization callback URL:**	https://app.terraform.io/auth/f53695b8-9733-40f0-9853-89cb5396610b/callback 
- Click on **Register Application**
- Make a note of Client ID: 97e5219d6edd8986817e (Sample for reference)
- Generate new Client Secret: abcdefghijklmnopqrstuvwxyx

### Step-05-02: Add the below in Terraform Cloud
- Name: github-terraform-modules
- Client ID: 97e5219d6edd8986817e
- Client Secret: abcdefghijklmnopqrstuvwxyx
- Click on **Connect and Continue**
- Authorize Terraform Cloud (hctaprep) - Click on **Authorize StackSimplify**
- SSH Keypair (Optional): click on **Skip and Finish**

### Step-06: Import the Terraform Module from Github
- In above step, we have completed the VCS Setup with github
- Now lets go ahead and import the Terraform module from Github
- Login to Terraform Cloud
- Click on Modules Tab -> Click on Add Module -> Select Github(github-terraform-modules) (PRE-POPULATED) -> Select it
- **Choose a Repository:** terraform-module-s3-website
- Click on **Publish Module**

## Step-07: Review newly imported Module
- Login to Terraform Cloud -> Click on Modules Tab 
- Review the Module Tabs on Terraform Cloud
  - Readme
  - Inputs
  - Outputs
  - Dependencies
  - Resources
- Also review the following
  - Versions
  - Provision Instructions   

## Step-08: Create a configuration that uses the Private Registry module using Terraform CLI
### Step-08-01: Call Module from Terraform Work Directory (Root Module)
- CreateTerraform Configuration in Root Module by calling the newly published module in Terraform Private Registry
- c1-versions.tf
- c2-variables.tf : Review and discuss about changing bucket name due to AWS Unique constraints
- c3-s3bucket.tf
- c4-outputs.tf
```t
module "website_s3_bucket" {
  source  = "app.terraform.io/hctaprep/s3-website-internal/aws"
  version = "1.0.0"
  # insert required variables here
  bucket_name = var.my_s3_bucket
  tags = var.my_s3_tags  
}
```
### Step-08-02: Execute Terraform Commands
```t
# Terraform Initialize
terraform init
Observation: 
1. Should fail with error due to cli not having access to Private module registry in Terraform Cloud

# Terraform Login
terraform login
Observation: 
1) Should see message |Success! Terraform has obtained and saved an API token.|
2) Verify Terraform credentials file
cat /Users/<YOUR_USER>/.terraform.d/credentials.tfrc.json
cat /Users/kdaida/.terraform.d/credentials.tfrc.json
Additional Reference:
https://www.terraform.io/docs/cli/config/config-file.html#credentials-1
https://www.terraform.io/docs/cloud/registry/using.html#configuration

# Terraform Initialize
terraform init
Observation: 
1. Should pass and download modules and providers

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Verify 
1. Bucket has static website hosting enabled
2. Bucket has public read access enabled using policy
3. Bucket has "Block all public access" unchecked
```

### Step-08-03: Upload index.html and test
```t
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1051.s3-website.us-east-1.amazonaws.com/
```

### Step-08-04: Destroy and Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-09: Create a configuration that uses the Private Registry module using Terraform Cloud & Github
### Assignment
1. Create Github Repository
2. Check-In files from `terraform-manifests` folder in `11-02` section
3. Create a new Workspace in Terraform Cloud to connect with Github Repository
4. Execute `Queue Plan` to apply the changes and test


## Step-10: VCS Providers & Terraform Cloud
- [Configuration-Free GitHub Usage](https://www.terraform.io/docs/cloud/vcs/github-app.html)
- [Configuring GitHub.com Access (OAuth)](https://www.terraform.io/docs/cloud/vcs/github.html)
- [Configuring GitHub Enterprise Access](https://www.terraform.io/docs/cloud/vcs/github-enterprise.html)
- [Other Supported VCS Providers](https://www.terraform.io/docs/cloud/vcs/index.html)

======================================================================
***********read the file ./BACKUP-2024/11-Terraform-Cloud-and-Enterprise-Capabilities/11-02-Share-Modules-in-Private-Module-Registry/terraform-s3-website-module-manifests/README.md ************
# Terraform Module for Private Registry - AWS S3 Static website
- This module provisions AWS S3 buckets configured for static website hosting.
- This will be a demo S3 module

======================================================================
***********read the file ./BACKUP-2024/11-Terraform-Cloud-and-Enterprise-Capabilities/11-03-Terraform-Cloud-CLI-Driven-Workflow/README.md ************
# Terraform Cloud - CLI-Driven Workflow

## Step-01: Introduction
- Learn and practically implement `CLI-Driven Workflow` in Terraform Cloud

## Step-02: Review Terraform Configuration Files
- c1-versions.tf
- c2-variables.tf
- c3-s3bucket.tf
- c4-outputs.tf

## Step-03: Create Workspace with CLI Driven Workflow
- Login to [Terraform Cloud](https://app.terraform.io/)
- Select Organization -> hcta-demo1
- Click on **New Workspace**
- **Choose your workflow:** CLI-Driven Workflow
- **Workspace Name:** cli-driven-demo
- Click on **Create Workspace**

## Step-04: Add backend block in Terraform Settings c1-versions.tf
```t
terraform {
  backend "remote" {
    organization = "hcta-demo1"

    workspaces {
      name = "cli-driven-demo"
    }
  }
}
```

## Step-05: Execute Terraform Commands
```t
# Terraform Login
terraform login
Observation: 
1) Should see message |Success! Terraform has obtained and saved an API token.|
2) Verify Terraform credentials file
cat /Users/<YOUR_USER>/.terraform.d/credentials.tfrc.json
cat /Users/kdaida/.terraform.d/credentials.tfrc.json
Additional Reference:
https://www.terraform.io/docs/cli/config/config-file.html#credentials-1
https://www.terraform.io/docs/cloud/registry/using.html#configuration

# Terraform Initialize
terraform init
Observation: 
1. Should pass and download modules and providers

# Terraform Validate
terraform validate

# Terraform Format
terraform fmt

# Terraform Plan
terraform plan
Observation: Should fail with error due to AWS Provider credential configuration not done on Terraform Cloud for this respective workspace
```

## Step-06: Configre Environment Variables
- [Setup AWS Access Keys for Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#environment-variables)
- Go to Organization (hcta-demo1) -> Workspace(cli-driven-demo) -> Variables
- In environment variables, add the below two
- Configure AWS Access Key ID and Secret Access Key  
- **Environment Variable:** AWS_ACCESS_KEY_ID
  - Key: AWS_ACCESS_KEY_ID
  - Value: XXXXXXXXXXXXXXXXXXXXXX
- **Environment Variable:** AWS_SECRET_ACCESS_KEY
  - Key: AWS_SECRET_ACCESS_KEY
  - Value: YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY


## Step-07: Execute Terraform Commands
```t
# Terraform Plan
terraform plan
Observation: 
1) Terraform plan should pass now. 
2) Discuss about cost estimation and trial plan for 30 days

# Terraform Apply
terraform apply 

# Verify 
1. Bucket has static website hosting enabled
2. Bucket has public read access enabled using policy
3. Bucket has "Block all public access" unchecked
```


### Step-08: Upload index.html and test
```t
# Endpoint Format
http://example-bucket.s3-website.Region.amazonaws.com/

# Replace Values (Bucket Name, Region)
http://mybucket-1051.s3-website.us-east-1.amazonaws.com/
```

## Step-09: Verify the following
- Select Organization -> hcta-demo1
- Click on **New Workspace**
- **Choose your workflow:** CLI-Driven Workflow
- **Workspace Name:** cli-driven-demo
- Runs
- States

## Step-10: Make changes and execute Terraform commands
- Update `c2-variables.tf` with new tag
- Execute Terraform Commands
```t
# Terraform Plan
terraform plan
Observation: 
1) Verify Terraform cloud Runs tab
2) We should see the runs are taking place for changes in Terraform cloud

# Terraform Apply
terraform apply -auto-approve
```
### Step-11: Destroy and Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Additional References
- [CLI Configuration File](https://www.terraform.io/docs/cli/config/config-file.html#credentials)======================================================================
***********read the file ./BACKUP-2024/11-Terraform-Cloud-and-Enterprise-Capabilities/11-04-Migrate-State-to-Terraform-Cloud/README.md ************
# Migrate State to Terraform Cloud

## Step-01: Introduction
- We are going to migrate State to Terraform Cloud

## Step-02: Review Terraform Manifests
- c1-versions.tf
- c2-variables.tf: 
  - **Important Note:** No default values provided for variables 
- c3-security-groups.tf
- c4-ec2-instance.tf
- c5-outputs.tf
- c6-ami-datasource.tf
- apache-install.sh


## Step-03: Execute Terraform Commands (First provision using local backend)
- First provision infra using local backend
- `terraform.tfstate` file will be created in local working directory
- In next steps, migrate it to Terraform Cloud
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve
```

## Step-04: Review your local state file
-  Review your local `terraform.tfstate` file once


## Step-05: Update remote backend in c1-versions.tf Terraform Block
### Step-05-01: Create New Workspace with CLI-Driven workflow
- Create New workspace with CLI-Driven workflow
- Login to [Terraform Cloud](https://app.terraform.io/)
- Select Organization -> hcta-demo1
- Click on **New Workspace**
- **Choose your workflow:** CLI-Driven Workflow
- **Workspace Name:** state-migration-demo
- Click on **Create Workspace**

### Step-05-02: Update remote backend in c1-versions.tf Terraform Block
```t
# Template
  backend "remote" {
    hostname      = "app.terraform.io"
    organization  = "<YOUR-ORG-NAME>"

    workspaces {
      name = "<SOME-NAME>"
    }
  }

# Replace Values
  backend "remote" {
    hostname      = "app.terraform.io"
    organization  = "hcta-demo1"  # Organization should already exists in Terraform Cloud

    workspaces {
      name = "state-migration-demo" 
      # Two cases: 
      # Case-1: If workspace already exists, should not have any state files in states tab
      # Case-2: If workspace not exists, during migration it will get created
    }
  }
```
- **Case-2 above for workspaces is failing with this error**
```
Kalyans-MacBook-Pro:terraform-manifests kdaida$ terraform init
Initializing the backend...
Error: Error looking up workspace
Workspace read failed: resource not found
Kalyans-MacBook-Pro:terraform-manifests kdaida$
```

## Step-06: Migrate State file to Terraform Cloud and Verify
```t
# Terraform Login
terraform login
Observation: 
1) Should see message |Success! Terraform has obtained and saved an API token.|
2) Verify Terraform credentials file
cat /Users/<YOUR_USER>/.terraform.d/credentials.tfrc.json
cat /Users/kdaida/.terraform.d/credentials.tfrc.json
Additional Reference:
https://www.terraform.io/docs/cli/config/config-file.html#credentials-1
https://www.terraform.io/docs/cloud/registry/using.html#configuration

# Terraform Initialize
terraform init
Observation: 
1) During reinitialization, Terraform presents a prompt saying that it will copy the state file to the new backend. 
2) Enter yes and Terraform will migrate the state from your local machine to Terraform Cloud.

# Verify in Terraform Cloud
1) New workspace should be created with name "state-migration-demo"
2) Verify "states" tab in workspace, we should find the state file
```

## Step-07: Add Variables & AWS Credentials in Environment Variables
### Step-07-01: Add Variables
```t
aws_region = us-east-1
instance_type = t3.micro
```
### Step-07-02: Configre Environment Variables
- [Setup AWS Access Keys for Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#environment-variables)
- Go to Organization (hcta-demo1) -> Workspace(state-migration-demo) -> Variables
- In environment variables, add the below two
- Configure AWS Access Key ID and Secret Access Key  
- **Environment Variable:** AWS_ACCESS_KEY_ID
  - Key: AWS_ACCESS_KEY_ID
  - Value: XXXXXXXXXXXXXXXXXXXXXX
- **Environment Variable:** AWS_SECRET_ACCESS_KEY
  - Key: AWS_SECRET_ACCESS_KEY
  - Value: YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY
 
## Step-08: Delete local terraform.tfstate
- First take backup and put it safe and delete it
```t
# Take backup
cp terraform.tfstate terraform.tfstate_local_before_migrate_to_TF_Cloud

# Delete
rm terraform.tfstate
``` 

## Step-09: Apply a new run from CLI
- Make a change and do  `terraform apply`
```t
# Change Instances from 1 to 2 (c4-ec2-instance.tf)
count = 2

# Terraform Apply
terraform apply 

# Verify in Terraform Cloud
1) Verify in Runs Tab in TF Cloud
2) Verify States Tab in TF Cloud
```

## Step-10: Destroy & Clean-Up
-  Destroy Resources from cloud this time instead of `terraform destroy` command
- Go to Organization (hcta-demo1) -> Workspace(state-migration-demo) -> Settings -> Destruction and Deletion
- Click on **Queue Destroy Plan**
```t
# Clean-Up files
rm -rf .terraform*
rm -rf terraform.tfstate*
```
======================================================================
***********read the file ./BACKUP-2024/12-Terraform-Cloud-and-Sentinel/12-01-Terraform-Cloud-and-Sentinel-Policies/README.md ************
# Terraform Cloud and Sentinel Policies

## Step-01: Introduction
- We are going to learn the following in this section
- Enable Trial plan for 30 days on hcta-demo1 organization which will enable **Team & Governance** features of Terraform Cloud
- Implement CLI-Driven workflow using Terraform Cloud for a S3 bucket resource
- Understand about two sentinel policies
  - check-terraform-version.sentinel
  - restrict-s3-buckets.sentinel
- Create Github repository for Sentinel Policies to use them as Policy Sets in Terraform Cloud
- Create Policy Sets in Terraform Cloud and Apply to demo workspace
- Test if sentinel policies applied and worked successfully.  


## Step-02: Review Terraform manifests
- **c1-versions.tf**
  - We are going to add Terraform Cloud as backend and implement CLI-Driven workflow on Terraform Cloud for this usecase for our learning. 
- **c2-variables.tf**
  - Bucket Name and Tags have default values and due to unique constraint for s3 bucket names, please use different bucket name when you are implementing it. 
- **c3-s3bucket.tf**
  - We are using this S3 bucket resource from **10-02-Terraform-Build-a-Module/v2-host-static-website-on-s3-using-terraform-manifests**.
  - In addition, we are going to add new resource named **aws_s3_bucket_object**, which will upload the `static-files/index.html` automatically during `terraform apply`
```t
resource "aws_s3_bucket_object" "bucket" {
  acl          = "public-read"
  key          = "index.html"
  bucket       = aws_s3_bucket.s3_bucket.id
  content      = file("${path.module}/static-files/index.html")
  content_type = "text/html"
}
```  
- **c4-outputs.tf**
  - We are going to have only S3 website endpoint as output with and without http appended. 

## Step-03: Create CLI-Driven Workspace on Terraform Cloud
### Step-03-01: Enable Trial plan in hcta-demo1 organization
- Login to Terraform Cloud
- Goto -> Organizations (hcta-demo1) -> Settings -> Plan & Billing
- Click on **Change Plan**
- Select **Trial Plan: Try out the Team & Governance plan features for 30 days**
- Click on **Start Free Trial**

### Step-03-02: Create CLI-Driven Workspace in organization hcta-demo1
- Login to [Terraform Cloud](https://app.terraform.io/)
- Select Organization -> hcta-demo1
- Click on **New Workspace**
- **Choose your workflow:** CLI-Driven Workflow
- **Workspace Name:** sentinel-demo1
- Click on **Create Workspace**

### Step-03-03: Update c1-versions.tf with Terraform Backend in Terraform Block
```t
terraform {
  backend "remote" {
    organization = "hcta-demo1"

    workspaces {
      name = "sentinel-demo1"
    }
  }
}
```
### Step-03-04: Configre Environment Variables in Terraform Cloud for AWS Provider
- [Setup AWS Access Keys for Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#environment-variables)
- Go to Organization (hcta-demo1) -> Workspace(sentinel-demo1) -> Variables
- In environment variables, add the below two
- Configure AWS Access Key ID and Secret Access Key  
- **Environment Variable:** AWS_ACCESS_KEY_ID
  - Key: AWS_ACCESS_KEY_ID
  - Value: XXXXXXXXXXXXXXXXXXXXXX
- **Environment Variable:** AWS_SECRET_ACCESS_KEY
  - Key: AWS_SECRET_ACCESS_KEY
  - Value: YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY


### Step-03-05: Execute Terraform Commands
```t
# Terraform Login
terraform login
Observation: 
1) Should see message |Success! Terraform has obtained and saved an API token.|
2) Verify Terraform credentials file
cat /Users/<YOUR_USER>/.terraform.d/credentials.tfrc.json
cat /Users/kdaida/.terraform.d/credentials.tfrc.json
Additional Reference:
https://www.terraform.io/docs/cli/config/config-file.html#credentials-1
https://www.terraform.io/docs/cloud/registry/using.html#configuration


# Terrafrom Initialize
terraform init

# Terraform Plan
terraform plan
Observation: 
1) Cost Estimation with this bucket should be displayed as we enabled the trial plan for 30 days on this organization (hcta-demo1)

# Terraform Apply
terraform apply -auto-approve

# Verify 
1) Access S3 static website and test
2) Review the plan and apply logs in Terraform Cloud respective workspace
```

## Step-04: Review Sentinel Policies
- [Sentinel Documentation](https://www.terraform.io/docs/cloud/sentinel/index.html)
### Step-04-01: check-terraform-version.sentinel
```t
import "tfplan/v2" as tfplan
import "strings"

v = strings.split(tfplan.terraform_version, ".")
version_major = int(v[1])
version_minor = int(v[2])

main = rule {
    #version_major is 14 and version_minor >= 1
    version_major >= 14
}
```

### Step-04-02: restrict-s3-buckets.sentinel
```t
import "tfplan/v2" as tfplan

s3_buckets = filter tfplan.resource_changes as _, rc {
  rc.type is "aws_s3_bucket" and
  (rc.change.actions contains "create" or rc.change.actions is ["update"])
}

required_tags = [
    "Terraform",
    "Environment",
]

allowed_acls = [
    "private",
    "public-read",
]

bucket_tags = rule {
    all s3_buckets as _, instances {
        all required_tags as rt {
        instances.change.after.tags contains rt
        }
    }
}

acl_allowed = rule {
    all s3_buckets as _, buckets {
    buckets.change.after.acl in allowed_acls
    }
}

main = rule {
    (bucket_tags and acl_allowed) else false
}

```

### Step-04-03: sentinel.hcl
```t
policy "check-terraform-version" {
    source            = "./check-terraform-version.sentinel"        
    enforcement_level = "hard-mandatory"
}

policy "restrict-s3-buckets" {
    source            = "./restrict-s3-buckets.sentinel"           
    enforcement_level = "soft-mandatory"
}
```
## Step-05: Create Github Repository for Sentinel Policies (Policy Sets)
### Step-05-01: Create new github Repository
- **URL:** github.com
- Click on **Create a new repository**
- **Repository Name:** terraform-sentinel-policies
- **Description:** Terraform Cloud and Sentinel Policies Demo 
- **Repo Type:** Public / Private
- **Initialize this repository with:**
- **CHECK** - Add a README file
- **CHECK** - Add .gitignore 
- **Select .gitignore Template:** Terraform
- **CHECK** - Choose a license  (Optional)
- **Select License:** Apache 2.0 License
- Click on **Create repository**

## Step-05-02: Clone Github Repository to Local Desktop
```t
# Clone Github Repo
git clone https://github.com/<YOUR_GITHUB_ID>/<YOUR_REPO>.git
git clone https://github.com/stacksimplify/terraform-sentinel-policies.git
```

## Step-05-03: Copy files from terraform-sentinel-policies folder to local repo & Check-In Code
- List of files to be copied
  - check-terraform-version.sentinel
  - restrict-s3-buckets.sentinel
  - sentinel.hcl
- **Source Location:** Section-12-01 - Inside `terraform-sentinel-policies` folder
- **Destination Location:** Newly cloned github repository folder in your local desktop `terraform-sentinel-policies`
- **Check-In code to Remote Repository**
```t
# GIT Status
git status

# Git Local Commit
git add .
git commit -am "Sentinel Policies First Commit"

# Push to Remote Repository
git push

# Verify the same on Remote Repository
https://github.com/stacksimplify/terraform-sentinel-policies.git
```

## Step-06: Create Policy Sets in Terraform Cloud
- Go to Terraform Cloud -> Organization (hcta-demo1) -> Settings -> Policy Sets
- Click on **Connect a new Policy Set**
- Use existing VCS connection from previous section **github-terraform-modules** which we created using OAuth App concept
- **Choose Repository:** terraform-sentinel-policies.git
- **Description:** Demo Sentinel Policies
- **Scope of Policies:** Policies enforced on selected workspaces
- **Workspaces:** sentinel-demo
- Click on **Connect Policy Set**

## Step-07: Update static-files/index.html in terraform-manifests
- Update static-files/index.html in terraform-manifests
```t
# Terraform Plan
terraform plan
Observation: 
1) Changes related to index.html should be printed in terraform plan
2) Cost Estimation with this bucket should be displayed as we enabled the trial plan for 30 days on this organization (hcta-demo1)
3) Sentinel policy execution should be displayed

# Terraform Apply
terraform apply -auto-approve
Observation: 
1) Changes related to index.html should be printed in terraform apply
2) Cost Estimation with this bucket should be displayed as we enabled the trial plan for 30 days on this organization (hcta-demo1)
3) Sentinel policy execution should be displayed

# Verify in Terraform Cloud
Observations:
1) Cost Estimation should be displayed
2) Also Sentinel Policy check should be displayed
```

## Step-08: Verify Sentinel Policy Failure Scenario
### Step-08-01: c2-variables.tf
- Update required tags to different value
```t
# Before Change
variable "tags" {
  description = "Tages to set on the bucket"
  type        = map(string)
  default     = {
    Terraform = "true"
    Environment = "dev"
    newtag1 = "tag1"
    newtag2 = "tag2"
  }
}

# After Change
variable "tags" {
  description = "Tages to set on the bucket"
  type        = map(string)
  default     = {
    abcdef = "true"  # modified one required tag so that sentinel policy restrict-se-buckets.sentinel will fail
    Environment = "dev"
    newtag1 = "tag1"
    newtag2 = "tag2"
  }
}
```
### Step-08-02: Execute Terraform Commands
```t
# Terraform Plan
terraform plan
Observation: 
1) Changes related to tag should be printed in terraform plan
2) Sentinel policy execution should report a failure for policy restrict-s3-buckets.sentinel

# Terraform Apply
terraform apply -auto-approve
Observation: 
1) Changes related to tag should be printed in terraform apply
2) Sentinel policy execution should report a failure for policy restrict-s3-buckets.sentinel

# Verify in Terraform Cloud
Observation: 
1) Sentinel policy execution will fail and terraform apply will not be executed
```

## Step-09: Clean-Up & Destroy
```t
# Terraform Destroy
terraform destroy -auto-approve

# Clean-Up files
rm -rf .terraform*
```

## References 
- [Terraform & Sentinel](https://www.terraform.io/docs/cloud/sentinel/index.html)
- [Example Sentinel Policies](https://www.terraform.io/docs/cloud/sentinel/examples.html)
- [Sentinel Foundational Policies](https://github.com/hashicorp/terraform-foundational-policies-library)
- [Sentinel Enforcement Levels](https://docs.hashicorp.com/sentinel/concepts/enforcement-levels)
======================================================================
***********read the file ./BACKUP-2024/12-Terraform-Cloud-and-Sentinel/12-02-Control-Costs-with-Sentinel-Policies/README.md ************
# Control Costs with Sentinel Policies

## Step-01: Introduction
- We are going to learn the following in this section
- Sentinel Cost Control Policies
- Apply them for Ec2 Instance and verify pass and fail cases

## Step-02: Review Sentinel Cost Control Policies
### Step-02-01: less-than-100-month.sentinel
- This policy uses the tfrun import to check that the new cost delta is no more than \$100
- The decimal import is used for more accurate math when working with currency numbers.
```t
import "tfrun"
import "decimal"

delta_monthly_cost = decimal.new(tfrun.cost_estimate.delta_monthly_cost)

main = rule {
    delta_monthly_cost.less_than(100)
}
```

### Step-02-02: sentinel.hcl
```t
policy "less-than-100-month" {
  source  = "./less-than-100-month.sentinel"
  enforcement_level = "soft-mandatory"
}
```

## Step-03: Copy Sentinel Cost Control Policies to terraform-sentinel-policies git repo
- Copy folder `terraform-sentinel-cost-control-policies` to Local git repository `terraform-sentinel-policies`
- **Check-In code to Remote Repository**
```t
# GIT Status
git status

# Git Local Commit
git add .
git commit -am "Sentinel Cost Control Policies Added in new folder"

# Push to Remote Repository
git push

# Verify the same on Remote Repository
https://github.com/stacksimplify/terraform-sentinel-policies.git
```

## Step-04: Add new Sentinel Policy Set in Terraform Cloud
- Go to Terraform Cloud -> Organization (hcta-demo1) -> Settings -> Policy Sets
- Click on **Connect a new Policy Set**
- Use existing VCS connection from previous section **github-terraform-modules** which we created using OAuth App concept
- **Choose Repository:** terraform-sentinel-policies.git
- **Name:** terraform-sentinel-cost-control-policies
- **Description:** terraform sentinel cost control policies
- **Policies Path:** terraform-sentinel-cost-control-policies
- **Scope of Policies:** Policies enforced on selected workspaces
- **Workspaces:** terraform-cloud-demo1
- Click on **Connect Policy Set**

## Step-05: Review our first Terraform Cloud Workspace
- Go to Terraform Cloud -> Organization (hcta-demo1) -> workspace (terraform-cloud-demo1)
### Step-05-01: Configre Environment Variables in Terraform Cloud for AWS Provider
- [Setup AWS Access Keys for Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#environment-variables)
- Go to Organization (hcta-demo1) -> Workspace(terraform-cloud-demo1) -> Variables
- In environment variables, add the below two
- Configure AWS Access Key ID and Secret Access Key  
- **Environment Variable:** AWS_ACCESS_KEY_ID
  - Key: AWS_ACCESS_KEY_ID
  - Value: XXXXXXXXXXXXXXXXXXXXXX
- **Environment Variable:** AWS_SECRET_ACCESS_KEY
  - Key: AWS_SECRET_ACCESS_KEY
  - Value: YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY

### Step-05-02: Pass Case: Queue Plan and Verify Cost Control Policies Applied
- Go to Organization (hcta-demo1) -> Workspace(terraform-cloud-demo1) 
- Queue Plan -> Cost-Control-Test-1-Pass-case
- Verify the following
  - Plan
  - Cost Estimate
  - Policy Check:  Policy check should pass
- Finally, Disacrd the Run

### Step-05-03: Fail Case: Queue Plan and Verify Cost Control Policies Applied
- Go to Organization (hcta-demo1) -> Workspace(terraform-cloud-demo1) -> Variables
- Update `instance_type` Variable
```t
# Before Change
instance_type = t3.micro

# After Change
instance_type = t3.2xlarge
```
- Queue Plan -> Cost-Control-Test-1-Fail-case
- Verify the following
  - Plan
  - Cost Estimate
  - Policy Check:  Policy check should fail
- Finally, Disacrd the Run
- Roll back `instance_type` to `t3.micro`

## Step-06: Sentinel Policies  - Conclusion
- We can create multiple sentinel policies in different folder paths in single github repository like `terraform-sentinel-policies`
- We can apply few of them at `Terraform Organization` level and few of them at `Terraform Workspace` level.
- Very flexible and conveniet.======================================================================
***********read the file ./BACKUP-2024/12-Terraform-Cloud-and-Sentinel/12-03-Terraform-Foundational-Policies-using-Sentinel/README.md ************
# Terraform Foundational Policies using Sentinel

## Step-01: Introduction
- [Terraform Foundational Policies Library](https://github.com/hashicorp/terraform-foundational-policies-library)
- This repository contains a library of policies that can be used within Terraform Cloud to accelerate your adoption of policy as code.
- This is pre-built sentinel policies provided by Terraform

## Step-02: Review sentinel.hcl
- [Terraform Foundational Policies using Sentinel](https://github.com/hashicorp/terraform-foundational-policies-library)
- [Terraform Sentinel AWS CIS Networking Policies](https://github.com/hashicorp/terraform-foundational-policies-library/tree/master/cis/aws/networking)
- **Folder Name:** terraform-sentinel-cis-policies
```t
policy "aws-cis-4.1-networking-deny-public-ssh-acl-rules" {
  source = "https://raw.githubusercontent.com/hashicorp/terraform-foundational-policies-library/master/cis/aws/networking/aws-cis-4.1-networking-deny-public-ssh-acl-rules/aws-cis-4.1-networking-deny-public-ssh-acl-rules.sentinel"
  enforcement_level = "advisory"
}

policy "aws-cis-4.2-networking-deny-public-rdp-acl-rules" {
  source = "https://raw.githubusercontent.com/hashicorp/terraform-foundational-policies-library/master/cis/aws/networking/aws-cis-4.2-networking-deny-public-rdp-acl-rules/aws-cis-4.2-networking-deny-public-rdp-acl-rules.sentinel"
  enforcement_level = "advisory"
}

policy "aws-cis-4.3-networking-restrict-all-vpc-traffic-acl-rules" {
  source = "https://raw.githubusercontent.com/hashicorp/terraform-foundational-policies-library/master/cis/aws/networking/aws-cis-4.3-networking-restrict-all-vpc-traffic-acl-rules/aws-cis-4.3-networking-restrict-all-vpc-traffic-acl-rules.sentinel"
  enforcement_level = "advisory"
}
```

## Step-03: Copy Sentinel CIS Policies to terraform-sentinel-policies git repo
- Copy folder `terraform-sentinel-cis-policies` to Local git repository `terraform-sentinel-policies`
- **Check-In code to Remote Repository**
```t
# GIT Status
git status

# Git Local Commit
git add .
git commit -am "Sentinel CIS Policies Added in new folder"

# Push to Remote Repository
git push

# Verify the same on Remote Repository
https://github.com/stacksimplify/terraform-sentinel-policies.git
```

## Step-04: Add new Sentinel Policy Set in Terraform Cloud
- Go to Terraform Cloud -> Organization (hcta-demo1) -> Settings -> Policy Sets
- Click on **Connect a new Policy Set**
- Use existing VCS connection from previous section **github-terraform-modules** which we created using OAuth App concept
- **Choose Repository:** terraform-sentinel-policies.git
- **Name:** terraform-sentinel-cis-policies
- **Description:** terraform sentinel cis-policies
- **Policies Path:** terraform-sentinel-cis-policies
- **Scope of Policies:** Policies enforced on selected workspaces
- **Workspaces:** terraform-cloud-demo1
- Click on **Connect Policy Set**

## Step-05: Review our first Terraform Cloud Workspace
- Go to Terraform Cloud -> Organization (hcta-demo1) -> workspace (terraform-cloud-demo1)
- Queue Plan -> CIS-Policy-Test-1
- Verify the following
  - Plan
  - Cost Estimate
  - Policy Check:  Verify what all passed and failed
- Finally, Disacrd the Run

======================================================================
***********read the file ./BACKUP-2024/13-Terraform-State-Import/README.md ************
# Terraform State Import

## Step-01: Introduction
### Some notes about Terraform Import Command
- Terraform is able to import existing infrastructure. 
- This allows you take resources you've created by some other means and bring it under Terraform management.
- This is a great way to slowly transition infrastructure to Terraform, or to be able to be confident that you can use Terraform in the future if it potentially doesn't support every feature you need today.
- [Full Reference](https://www.terraform.io/docs/cli/import/index.html)
### Two demos
- **Demo-1:** Create EC2 Instance manually and import state to manage it from Terraform
- **Demo-2:** Create S3 bucket manually and import state to mange it from Terraform


## Step-02: Create EC2 Instance manually using AWS mgmt Console
- Go to  Services -> Ec2 -> Instances -> Launch Instance
- **Step 1:** ChooseAMI: Amazon Linux 2 AMI (HVM), SSD Volume Type
- **Step 2:** Choose an Instance Type: t2.micro (leave to defaults)
- **Step 3:** Configure Instance Details: (Leave to defaults )
- **Step 4:** Add Storage (Leave to Defaults)
- **Step 5:** Add Tags
  - Key: Name
  - Value: State-Import-Demo
- **Step 6:** Configure Security Group: Select default VPC security group
- **Step 7:** Review Instance Launch
- Select an existing key pair: terraform-key
- Click on **Launch Instance**


## Step-03: Create Basic Terraform Configuration
- **Reference Folder:** v1-ec2-instance
- c1-versions.tf
- c2-ec2-instance.tf
- Create EC2 Instance Resource - Basic Version to get Terraform `Resource Type` and `Resource Local Name` we are going to use in Terraform
```t
# Create EC2 Instance Resource - Basic Version to get Terraform Resource Type and Resource Local Name we are going to use in Terraform
resource "aws_instance" "myec2vm" {
}
```

## Step-04: Run Terraform Import to import AWS EC2 Instance Resource to Terraform
- [Terraform AWS EC2 Instance Argument Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#argument-reference)
```t
# Terraform Initialize
terraform init

# Terraform Import Command for EC2 Instance
terraform import aws_instance.myec2vm <EC2-Instance-ID>
terraform import aws_instance.myec2vm i-0477f144280c37a7a
Observation:
1) terraform.tfstate file will be created locally in Terraform working directory
2) Review information about imported instance configuration in terraform.tfstate
```

## Step-05: Start Building local c2-ec2-instance.tf
- By referring `terraform.tfstate` file and parallely running `terraform plan` command make changes to your terraform configuration `c2-ec2-instance.tf` till you get the message `No changes. Infrastructure is up-to-date` for `terraform plan` output
```t
# Create EC2 Instance Resource 
resource "aws_instance" "myec2vm" {
  ami = "ami-038f1ca1bd58a5790"
  instance_type = "t2.micro"
  availability_zone = "us-east-1a"
  key_name = "terraform-key"
  tags = {
    "Name" = "State-Import-Demo"
  }
}
```
## Step-06: Modify EC2 Instance from Terraform
- You have created this EC2 instance manually, now you made it as terraform managed 
- Modify Instance type to `t2.small` and test
```t
# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve
Observation:
1) EC2 Instance Type on AWS Cloud should be changed from t2.micro to t2.small
```

## Step-06: Destroy EC2 Instance from Terraform 
- You have created this EC2 instance manually, now you made it as terraform managed
- Destroy that using terraform
```t
# Destroy Resource
terraform destroy  -auto-approve

# Clean-Up files
rm -rf .terraform*
rm -rf terraform.tfstate*

# Comment Resource Arguments for S3 bucket
Comment Arguments in c2-ec2-instance.tf so that when a student is using the demo, he can uncomment or write it as per their Ec2 Instance settings.
```
## NOT HAPPY - Lets do one more example with AWS S3 Bucket and learn little more about terraform state import


## Step-07: Create AWS S3 bucket manually using AWS mgmt Console
- Go to  Services -> S3 -> **Create Bucket**
- **Bucket Name:** state-import-bucket


## Step-08: Create Basic Terraform Configuration
- **Reference Folder:** v2-s3bucket
- c1-versions.tf
- c2-s3bucket.tf
- Create S3 Bucket Resource - Basic Version to get Terraform `Resource Type` and `Resource Local Name` we are going to use in Terraform
```t
# Create S3 Bucket
resource "aws_s3_bucket" "mybucket" {

}
```
## Step-09:Run Terraform Import Command to import AWS S3 bucket resource to Terraform
- [Terraform S3 Bucket Argument Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket#argument-reference)
```t
# Terraform Initialize
terraform init

# Terraform Import Command for AWS S3 bucket
terraform import aws_s3_bucket.mybucket <BUCKET_NAME>
terraform import aws_s3_bucket.mybucket state-import-bucket
Observation:
1) terraform.tfstate file will be created locally in Terraform working directory
2) Review information about imported instance configuration in terraform.tfstate
```

## Step-10: Start Building local c2-s3bucket.tf
- By referring `terraform.tfstate` file and parallely running `terraform plan` command make changes to your terraform configuration `c2-s3bucket.tf` till you get the message `No changes. Infrastructure is up-to-date` for `terraform plan` output
- For S3 buckets, there will be some default parameters from terraform will be set for two arguments
  - acl =  private
  - force_destroy = false
- Those even though you manually add it, you also need to do `terraform apply` once to make terraform happy.
```t
# Create S3 Bucket
resource "aws_s3_bucket" "mybucket" {
  bucket = "state-import-bucket"
  acl = "private" 
  force_destroy = false # default is false make this to true if any contents in bucket, so in next step you can destroy using terraform
}

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve
```


## Step-11: Destroy S3 Bucket from Terraform 
- You have created this S3 Bucket manually, now you made it as terraform managed
- Destroy that using terraform
```t
# Destroy Resource
terraform destroy  -auto-approve

# Clean-Up files
rm -rf .terraform*
rm -rf terraform.tfstate*

# Comment Resource Arguments for S3 bucket
Comment Arguments in c2-s3bucket.tf so that when a student is using the demo, he can uncomment or write it as per their bucket names and settings.
```======================================================================
***********read the file ./BACKUP-2024/14-Terraform-Graph/README.md ************
# Terraform Graph

## Step-01: Introduction
- The `terraform graph` command is used to generate a visual representation of either a configuration or execution plan
- The output is in the DOT format, which can be used by [GraphViz](http://www.graphviz.org/) to generate charts.

## Step-02: Run Terraform Graph command
```t
# Terraform Initialize
terraform init

# Terraform Graph
terraform graph > dot1
Observation: 
This command will output DOT format text and store in file dot1
```

## Step-03: Online Graphviz Viewers
- [Graphviz-Online](https://dreampuf.github.io/GraphvizOnline/)
- [Edotor-Online](https://edotor.net/)
- Copy and paste the text from `dot1` file generated in step-02 in these online Graphviz Viewers
- Review the output

## Step-04: Clean-Up
```t
# Delete .terraform files
rm -rf .terraform*
```

## Step-05: Other Options - Offline Graphviz Installer
### Step-05-01: Pre-requisite notes
- Graphviz is unstable on MacOS 
- Graphviz needs xcode to be installed on MacOS which consumes huge disk space.
- With that said, we are going to do this demo on Windows Machine
- We are going to use Windows 2019 EC2 instance created on AWS for the same. 

### Step-05-02: Create Windows 2019 VM ready
- Create Windows 2019 VM on AWS Cloud
- Disable Browser security settings in Server-Manager
- Install Google Chrome
- Download & Install Terraform CLI
- Set `Path` for Terraform CLI
- Copy `terraform-manifests` folder from `section-14: Terraform Graph` of the course to Windows VM


### Step-05-03: Install Graphviz on Windows VM
- [Download Graphviz](http://www.graphviz.org/download/)
- Install Graphviz 
```t
# Switch Directory
cd c:\graphviz-demo\terraform-manifests

# Terraform Initialize
terraform init

# Terraform Graph
terraform graph > dot1
Observation: 
This command will output DOT format text

# Terraform Graph in Image format
terraform graph | dot -Tsvg > graph.svg

# Verify
open graph.svg in browser
```


## References
- [Terraform Graph](https://www.terraform.io/docs/cli/commands/graph.html)======================================================================
***********read the file ./BACKUP-2024/15-Terraform-Expressions/15-01-Terraform-Functions/README.md ************
# Terraform Functions

## Step-01: Introduction
- We are going to learn about different Terraform Functions using Terraform console command
- In detail, we are going to learn about `templatefile` and `concat` functions with an AWS example

## Step-02: Numeric Functions
```t
# Terraform Console
terraform console

# Min Function: Takes one or more numbers and returns the smallest number from the set.
min(12, 13, 14)

# Max Function: Takes one or more numbers and returns the greatest number from the set.
max(12, 13, 14)

# pow Function: Calculates an exponent, by raising its first argument to the power of the second argument.
pow(3, 2)
```

## Step-03: String Functions
```t
# Terraform Console
terraform console

# Trim Function: Removes the specified characters from the start and end of the given string.
trim("?!hello?!", "!?")

# Trimprefix Function: Removes the specified prefix from the start of the given string. If the string does not start with the prefix, the string is returned unchanged.
trimprefix("helloworld", "hello")
trimprefix("helloworld", "cat")

# Trimsuffix Function: Removes the specified suffix from the end of the given string.
trimsuffix("helloworld", "world")

# Trimspace Function: Removes any space characters from the start and end of the given string.
trimspace("  hello\n\n")

# Join Function: Produces a string by concatenating together all elements of a given list of strings with the given delimiter
join(separator, list)
join(", ", ["foo", "bar", "baz"])

# Split Function: Produces a list by dividing a given string at all occurrences of a given separator.
split(separator, string)
split(",", "foo,bar,baz")

# Upper Functon: Converts all cased letters in the given string to uppercase.
upper("hello")
```

## Step-04: Collection Functions
```t
# Terraform Console
terraform console

# Concat Function: Takes two or more lists and combines them into a single list.
concat(["a", ""], ["b", "c"])

# Contains Function: Determines whether a given list or set contains a given single value as one of its elements.
contains(list, value)
contains(["a", "b", "c"], "a")
contains(["a", "b", "c"], "d")

# Distinct Function: Takes a list and returns a new list with any duplicate elements removed.
distinct(["a", "b", "a", "c", "d", "b"])

# Length Function: determines the length of a given list, map, or string.
length("hello")
length(["a", "b"])
length(["a", "b"])

# Lookup Function: Retrieves the value of a single element from a map, given its key. If the given key does not exist, the given default value is returned instead.
lookup(map, key, default)
lookup({a="ay", b="bee"}, "a", "what?")
Web: 
lookup({"web" = ["10.0.1.0/24","10.0.2.0/24"], "app" = ["10.0.11.0/24","10.0.12.0/24"], "db" = ["10.0.21.0/24","10.0.22.0/24"]}, "web", ["10.0.51.0/24", "10.0.52.0/24"])

App: 
lookup({"web" = ["10.0.1.0/24","10.0.2.0/24"], "app" = ["10.0.11.0/24","10.0.12.0/24"], "db" = ["10.0.21.0/24","10.0.22.0/24"]}, "app", ["10.0.51.0/24", "10.0.52.0/24"])

DB:
lookup({"web" = ["10.0.1.0/24","10.0.2.0/24"], "app" = ["10.0.11.0/24","10.0.12.0/24"], "db" = ["10.0.21.0/24","10.0.22.0/24"]}, "db", ["10.0.51.0/24", "10.0.52.0/24"])

Default: 
lookup({"web" = ["10.0.1.0/24","10.0.2.0/24"], "app" = ["10.0.11.0/24","10.0.12.0/24"], "db" = ["10.0.21.0/24","10.0.22.0/24"]}, "abcd", ["10.0.51.0/24", "10.0.52.0/24"])



# Merge Function: Takes an arbitrary number of maps or objects, and returns a single map or object that contains a merged set of elements from all arguments.
merge({a="b", c="d"}, {e="f", c="z"})
merge({a="b"}, {a=[1,2], c="z"}, {d=3})
```

## Step-05: Encoding Functions
```t
# Terraform Console
terraform console

# base64decode Function: Takes a string containing a Base64 character sequence and returns the original string.
base64decode("SGVsbG8gV29ybGQ=")

# base64encode Function: Applies Base64 encoding to a string.
base64encode("Hello World")
```

## Step-06: FileSystem Functions
```t
# Terraform Console
terraform console

# File Function: Reads the contents of a file at the given path and returns them as a string.
file("${path.module}/files/hello.txt")

# fileexists Function: Determines whether a file exists at a given path.
fileexists("${path.module}/files/hello.txt")

# templatefile Function: Reads the file at the given path and renders its content as a template using a supplied set of template variables.
templatefile(path, vars)
```

## Step-07: templatefile & concat Function - Review TF Files
- **Reference Folder:** terraform-manifests
### c1-versions.tf
- No changes
### c2-variables.tf
- Added variabled package_name
```t
variable "package_name" {
  description = "Provide Package that need to be installed with user_data"
  type = string
  default = "httpd"
}
```
### c3-security-groups.tf
- No changes
### c4-ec2-instance.tf
- Added `user_data =  templatefile("user_data.tmpl", {package_name = var.package_name})`
```t
# Create EC2 Instance - Amazon2 Linux
resource "aws_instance" "my-ec2-vm" {
  ami           = data.aws_ami.amzlinux.id 
  instance_type = var.instance_type
  key_name      = "terraform-key"
  #user_data = file("apache-install.sh")  
  user_data =  templatefile("user_data.tmpl", {package_name = var.package_name})
  vpc_security_group_ids = [aws_security_group.vpc-ssh.id, aws_security_group.vpc-web.id]
  tags = {
    "Name" = "TF-Functions-Demo-1"
  }
}
```
### c5-outputs.tf
- Added output with `concat` function
```t
# Concat Security Group IDs in Output
output "security_group_ids" {
  value = concat([aws_security_group.vpc-ssh.id, aws_security_group.vpc-web.id])
}
/* Note: This will return the IDs of the security groups attached to your web 
instance as a list. You can use these lists as inputs in submodules.*/
```
### c6-ami-datasource.tf
- No changes
### user_data.tmpl
- Contains the shell script which will install the package provided from terraform variables. 
```sh
#! /bin/bash
sudo yum update -y
sudo yum install -y ${package_name}
sudo yum list installed | grep ${package_name} >> /tmp/package-installed-list.txt
```

## Step-08: Execute Terraform Commands
- Verify the installed packages
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Review Outputs for concat function
security_group_ids = [
  "sg-09f936287ddfc3b14",
  "sg-01f8a08bcbc4b9590",
]

# Connect to EC2 VM
ssh -i private-key/terraform-key ec2-user@<PUBLIC-IP>
cat /tmp/package-installed-list.txt
```

## Step-09: Clean-Up
```t
# Destroy Resources
terraform destory -auto-approve

# Delete Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## Step-10: Other Function Categories
- [Date and Time Functions](https://www.terraform.io/docs/language/functions/formatdate.html)
- [Hash and Crypto Functions](https://www.terraform.io/docs/language/functions/base64sha256.html)
- [IP Network Functions](https://www.terraform.io/docs/language/functions/cidrhost.html)
- [Type Conversion Functions](https://www.terraform.io/docs/language/functions/can.html)



## References
- [Terraform Functions](https://www.terraform.io/docs/language/functions/index.html)
======================================================================
***********read the file ./BACKUP-2024/15-Terraform-Expressions/15-02-Terraform-Dynamic-Expressions/README.md ************
# Terraform Dynamic Expressions

## Step-01: Introduction
- Learn [Dynamic Expressions](https://www.terraform.io/docs/language/expressions/conditionals.html) in Terraform
- **Conditional Expression:** A conditional expression uses the value of a bool expression to select one of two values.
```t
# Example-1
condition ? true_val : false_val

# Example-2
var.a != "" ? var.a : "default-a"
```
- **Splat Expression:** A `splat expression` provides a more concise way to express a common operation that could otherwise be performed with a `for` expression.
- The special [*] symbol iterates over all of the elements of the list given to its left and accesses from each one the attribute name given on its right. 
```t
# With for expression
[for o in var.list : o.id]

# With Splat Expression [*]
var.list[*].id
```
- A splat expression can also be used to access attributes and indexes from lists of complex types by extending the sequence of operations to the right of the symbol:
```t
var.list[*].interfaces[0].name
aws_instance.example[*].id
```


## Step-02: Review Terraform Manifests
### c1-versions.tf
- Added new random provider in `required_providers` block
### c2-vairables.tf
  - Added new variables
  - availability_zones
  - name
  - team
  - high_availability
### c3-security-groups.tf
  - Added common tags
### c4-ec2-instance.tf
- Added Random ID resource block
- Added new locals block
- **Important Note:** Inside locals block we can add conditional expressions as below. 
```t
# Create Locals
locals {
  #name = var.name
  name  = (var.name != "" ? var.name : random_id.id.hex)
  owner = var.team
  common_tags = {
    Owner = local.owner
    DefaultUser = local.name 
  }
}
```
- Added Availability zone argument with count.index
- We will discuss about following conditional expressions here
```t
# In Locals Block: Conditional Expression
name  = (var.name != "" ? var.name : random_id.id.hex)

# In EC2 Resource Block: Conditional Expression
count = (var.high_availability == true ? 2 : 1)

# In EC2 Resource Block: count.index
availability_zone = var.availability_zones[count.index]
```
### c5-outputs.tf
- Added Splat expression [*] for all outputs
- Added Common Tags and ELB DNS Name as new outputs

### c6-ami-datasource.tf
- No changes

### c7-elb.tf
- Added this new resource
- We will be creating ELB only if "high_availability" variable value is true else it will not be created
```t
# Create ELB if high_availability=true
# In ELB Block: Conditional Expression
count = (var.high_availability == true ? 1 : 0)
```  

## Step-03: Execute Terraform Commands
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan: When Variable values, high_availability = false and name = "ec2-user"
terraform plan
Observation: 
1) Plan will generate for only 1 EC2 instance and 2 Security Groups.
2) ELB Resource will not be created with these variable options


# Terraform Plan: When Variable values, high_availability = true and name = ""
terraform plan
1) Plan will generate for only 2 EC2 instance, 2 Security Groups and 1 ELB
2) ELB Resource will be created with these variable options
3) name value will be a random value known after terraform apply completed

# Terraform Apply
terraform apply -auto-approve

# Verify
1) Verify Outputs
2) Verify EC2 Instances & Security Groups & Common Tags
3) Verify ELB & Common Tags for ELB
4) Access App using ELB DNS Name
```

## Step-04: Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*

# Uncomment and Comment right values in c2-variables.tf (Roll back to put ready for student demo)
high_availability = false 
name = "ec2-user"
```

======================================================================
***********read the file ./BACKUP-2024/15-Terraform-Expressions/15-03-Terraform-Dynamic-Blocks/README.md ************
# Terraform Dynamic Blocks

## Step-01: Introduction
- Some resource types include repeatable nested blocks in their arguments, which do not accept expressions
- You can dynamically construct repeatable nested blocks like setting using a special dynamic block type, which is supported inside resource, data, provider, and provisioner blocks

## Step-02: Create / Review Terraform manifests
### c1-versions.tf
- Standard file without any changes
- `region` in provider block is hard-coded to `us-east-1`
### c2-security-groups-regular.tf
```t
resource "aws_security_group" "sg-regular" {
  name        = "demo-regular"
  description = "demo-regular"

  ingress {
    description = "description 0"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    description = "description 1"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    description = "description 2"
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    description = "description 3"
    from_port   = 8081
    to_port     = 8081
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    description = "description 4"
    from_port   = 7080
    to_port     = 7080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }      
  ingress {
    description = "description 5"
    from_port   = 7081
    to_port     = 7081
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }      
}
```

### c3-dynamic-blocks-for-security-groups.tf
- ingress.key = 0 and ingress.value = 80
- ingress.key = 1 and ingress.value = 443
- ingress.key = 2 and ingress.value = 8080  ....
```t
# Define Ports as a list in locals block
locals {
  ports = [80, 443, 8080, 8081, 7080, 7081]
}

# Create Security Group using Terraform Dynamic Block
resource "aws_security_group" "sg-dynamic" {
  name        = "dynamic-block-demo"
  description = "dynamic-block-demo"

  dynamic "ingress" {
    for_each = local.ports
    content {
      description = "description ${ingress.key}"
      from_port   = ingress.value
      to_port     = ingress.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
}
```

## Step-03: Execute Terraform Commands
```t
# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve
```

## Step-04: Clean-Up
```t
# Terraform Destroy
terraform destroy -auto-approve

# Delete Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```======================================================================
***********read the file ./BACKUP-2024/16-Terraform-Debug/README.md ************
# Terraform Debug

## Step-01: Introduction
- Learn about Terraform Debug
- TF_LOG & TF_LOG_PATH
- TF_LOG - Allowed Values or Desired Log Levels
- **TRACE:** Very detailed verbosity, shows every step taken by Terraform and produces enormous outputs with internal logs.
- **DEBUG:** describes what happens internally in a more concise way compared to TRACE.
- **ERROR:** shows errors that prevent Terraform from continuing.
- **WARN:** logs warnings, which may indicate misconfiguration or mistakes, but are not critical to execution
- **INFO:** shows general, high-level messages about the execution process.
- **Important Note:** 


## Step-02: Setup Trace logging in Terraform
```t
# Terrafrom Trace Log Settings
export TF_LOG=TRACE
export TF_LOG_PATH="terraform-trace.log"
echo $TF_LOG
echo $TF_LOG_PATH

# Terraform Initialize
terraform init

# Terraform Validate
terraform validate

# Terraform Plan
terraform plan

# Terraform Apply
terraform apply -auto-approve

# Terraform Destroy
terraform destroy -auto-approve

# Clean-Up
rm -rf .terraform*
rm -rf terraform.tfstate*
rm terraform-trace.log
```


## Step-03: Setup these Environment Variables permanently in your desktops
### Linux Bash
- Open your `.bashrc` which is located in your $home directory 
```t
# Linux Bash
cd $HOME
vi .bashrc

# Terraform log settings
export TF_LOG=TRACE
export TF_LOG_PATH="terraform-trace.log"

# Verify after saving the file in new terminal 
$ echo $TF_LOG
TRACE
$ echo $TF_LOG_PATH
terraform-trace.log
```
### Windows Powershell
- Setup using Powershell profile
- Open `$profile` command in a PowerShell
- Once that file is opened add the following lines.
- Now close and reopen the console and type the following to verify that it worked.
```t
# Windows Powershell - Terraform log settings
$env:TF_LOG="TRACE"
$env:TF_LOG_PATH="terraform.txt"

# Open new powershell window & Verify
echo $env:TF_LOG
echo $env:TF_LOG_PATH
```
### MAC OS
- Update the values in `.bash_profile` at the end of file
```t
# MAC OS
cd $HOME
vi .bash_profile

# Terraform log settings
export TF_LOG=TRACE
export TF_LOG_PATH="terraform-trace.log"

# Verify after saving the file in new terminal 
$ echo $TF_LOG
TRACE
$ echo $TF_LOG_PATH
terraform-trace.log
```

## Step-04: Terraform Crash Log
- If Terraform ever crashes (a "panic" in the Go runtime), it saves a log file with the debug logs from the session as well as the panic message and backtrace to `crash.log`.
- Generally speaking, this log file is meant to be passed along to the developers via a GitHub Issue. 
- As a user, you're not required to dig into this file.
- [How to read a crash log?](https://www.terraform.io/docs/internals/debugging.html#interpreting-a-crash-log)======================================================================
***********read the file ./BACKUP-2024/17-Exam-Preparation/README.md ************
# Exam Preparation

## Step-00: Don't Stop Get Certified
- If you have completed all the sections from 01 to 16, go ahead and get certified. 
- You will definitely pass the exam with 70 to 85% score with above knowledge.
- If you expect to score more and more, then you also need to go through below listed Terraform materials once

## Step-01: Core Topics
1. Understand infrastructure as code (IaC) concepts
2. Understand Terraform's purpose (vs other IaC)
3. Understand Terraform basics
4. Use the Terraform CLI (outside of core workflow)
5. Interact with Terraform modules
6. Navigate Terraform workflow
7. Implement and maintain state
8. Read, generate, and modify configuration
9. Understand Terraform Cloud and Enterprise capabilities

## Step-02: Questions Break down in real exam
- Total: 57 Questions
- 40 to 45 questions mostly will be straight forward, concept oriented questions which we can answer if we implemented above 50 practicals
  - Above all the sections in this course will mostly cover you. 
- 12 questions will be absolute practical oriented about asking some basic commands, asking about how to solve this error message etc. 
  - Practicals above will be hands-on which gives the ability to solve such questions
  - Out of these 12, 4 to 5 might be super tricky but don't worry about them. 


## Step-03: Review Terraform Guides
- [Study Guide](https://learn.hashicorp.com/tutorials/terraform/associate-study)
- [Review Guide](https://learn.hashicorp.com/tutorials/terraform/associate-review)


## Step-04: Review Terraform Language Documentation
- [Terraform Language Guide](https://www.terraform.io/docs/language/index.html)
- Go through this on high-level. Quick walk-through will suffice======================================================================
***********read the file ./BACKUP-2024/18-Exam-Registration/README.md ************
# Exam Registration

## Step-01: Exam Registration
- Exam is conducted on PSI Exam platform
- **Pre-requisite-1:** You should already have a account on `github.com` so you can use it. The email you have used for github, you should have access to that same email id to check emails. Hashicorp via PSI Exams will communicate on that email id. 
- **Pre-requisite-2:** Create your account on  [youracclaim](https://www.youracclaim.com/) with same email id so when you have completed your exam and certified, your badge will be posted to **youracclaim** so you can share the same in social media platforms like linkedin, and also have a record of your badge online. 
- [Register for Exam](https://hashicorp-certifications.zendesk.com/hc/en-us/articles/360049382552)
- Once registered, email confirmation will be sent to you. 

## Step-02: Review Exam Taker Handbook
- [Exam Taker Handbook](https://hashicorp-certifications.zendesk.com/hc/en-us/articles/360048211571)

## Step-03: System Requirements
- Very very important. 
- Do all the things a day before
- [System Requirements](https://hashicorp-certifications.zendesk.com/hc/en-us/articles/360048446631)
- [Click here to test your system's compatibility](https://syscheck.bridge.psiexams.com/)
- If you are on MAC, you need to provide additional permissions when it asks to run system check like
- Connect Webcam or your laptop inbuilt cam
- Connect Microphone (No Head-Phones)
- Connect wired ehternet line to your laptop or dekstop for good internet speed
- As a fail back internet, enable mobile hotspot on your mobile and put that outside your room to avoid distractions
- Install PSI Secure Browser

## Step-04: Exam Day
- Login 45 mins before to PSI Exam platform
- Exam Launch URL will get enabled 30 mins before scheduled Time
- Complete the checks 
### Step-04-01: First Time checks
- System will ask you for these checks
1. Identity Card Verification
2. 360 Degree Room View
3. Hand Sleeves and Ears check

### Step-04-02: Second Time Checks
- Online Proctor will communicate with you chat and perform these checks
1. Identity Card Verification
2. 360 Degree Room View
3. Hand Sleeves and Ears check

### Step-04-03: Proctor will Release the Exam for you to Start
1. Don't tense.
2. Be cool and start answering questions
3. If you have any doubt about any question, Click on **Flag Button** so we can come back later
4. Don't Flag many questions later difficult to check all of them. 
5. Exam Time: 60 Minutes Questions: 57
6. I took 35 minutes to complete all the questions due to the fact most of the questions (40 to 45) will be straight forward. 
7. Review the questions which you flagged and answer them. 
8. Complete the exam
9. Complete the Survey questions
10. You will get the status of your exam (pass / fail)
11. Email will be sent to you by **youracclaim** with your badge information
12. Email will be sent to you by **HashiCorp** about your score report 
======================================================================
***********read the file ./README.md ************
# HashiCorp Certified: Terraform Associate - 50 Practical Demos
[![Image](https://stacksimplify.com/course-images/hashicorp-certified-terraform-associate-highest-rated.png "HashiCorp Certified: Terraform Associate - 50 Practical Demos")](https://links.stacksimplify.com/hashicorp-certified-terraform-associate)

## Course Modules
01. Infrastructure as Code (IaC)
02. Install Tools on MacOs, LinuxOS and WindowsOS
03. Command Basics
04. Language Syntax
05. Settings Block
06. Providers Block
07. Multiple Providers usage
08. Dependency Lock File Importance
09. Resources Syntax and Behavior
10. Resources Meta-Argument - depends_on
11. Resources Meta-Argument - count
12. Resources Meta-Argument - for_each
13. Resources Meta-Argument - lifecycle
14. Input Variables - Basics
15. Input Variables - Assign When Prompted
16. Input Variables - Override default with cli var
17. Input Variables - Override with environment variables
18. Input Variables - Assign with terraform.tfvars
19. Input Variables - Assign with tfvars var-file argument
20. Input Variables - Assign with auto tfvars
21. Input Variables - Lists
22. Input Variables - Maps
23. Input Variables - Validation Rules
24. Input Variables - Sensitive Input Variables
25. File Function
26. Output Values
27. Local Values
28. Datasources
29. Backends - Remote State Storage
30. State Commands
31. CLI Workspaces with local backend
32. CLI Workspaces with remote backend
33. File Provisioner
34. local-exec Provisioner
35. remote-exec Provisioner
36. Null Resource
37. Modules from Public Registry
38. Build Local Module
39. Terraform Cloud - VCS-Driven Worflow
40. Terraform Cloud - CLI-Driven Worflow
41. Terraform Cloud - Share modules in private module registry
42. Migrate State to Terraform Cloud
43. Basic Sentinel Policies
44. Cost Control Sentinel Policies
45. CIS Sentinel Policies
46. State Import
47. Graph
48. Functions
49. Dynamic Expressions
50. Dynamic Blocks


## What will students learn in your course?
- You will learn to master Terraform in a practical perspective with 40+ demo's
- You will learn each and every concept of Terraform (basic to advanced)
- You will learn to write and understand Terraform Resource Behavior in combination with all the Meta-Arguments
- You will learn each and every way (10 types) you can implement the Terraform Input Variables
- You will learn in detail about Terrafrom State, Remote Backends, Terraform Cloud Backends and many Terraform State commands
- You will learn and implement Terraform CLI based workspaces
- You will learn and implement all Terraform Provisioners 
- You will learn and implement Terraform Modules with all 3 types (Public Modules, Local Modules and Private Registry modules on Terraform Cloud)
- You will learn and implement two important usecases on Terraform Cloud (VCS-Driven and CLI-Driven Workflows)
- You will learn about sentinel policies and implement 3 types of sentinel policies
- You will learn and implement Terraform Dynamic Expressions, Dynamic Blocks and Terraform Functions
- You will also learn and implement Terraform Datasources concept

## Are there any course requirements or prerequisites?
- You must have an AWS Cloud account to follow with me for hands-on activities.
- You don't need to have any basic knowledge of Terraform. Course will get started from very very basics of Terraform and take you to very advanced levels



## Who are your target students?
- Infrastructure Architects or Sysadmins or Developers who are planning to master Terraform
- Any beginner who is interested in learning IaC Infrastructure as Code current trending tool Terraform 
- Anyone who want to learn Terraform from a practical perspective 

## Github Repositories used for this course
- [HashiCorp Certified: Terraform Associate](https://github.com/stacksimplify/hashicorp-certified-terraform-associate)
- [Terraform Cloud Demo](https://github.com/stacksimplify/terraform-cloud-demo1)
- [Terraform Private Module Registry](https://github.com/stacksimplify/terraform-aws-s3-website)
- [Terraform Sentinel Policies](https://github.com/stacksimplify/terraform-sentinel-policies)
- [Course PPT Presentation](https://github.com/stacksimplify/hashicorp-certified-terraform-associate/tree/master/presentation)
- **Important Note:** Please go to these repositories and FORK these repositories and make use of them during the course.


## Each of my courses come with
- Amazing Hands-on Step By Step Learning Experiences
- Real Implementation Experience
- Friendly Support in the Q&A section
- 30 Day "No Questions Asked" Money Back Guarantee!

## My Other AWS Courses
- [Udemy Enroll](https://github.com/stacksimplify/udemy-enroll)

## Stack Simplify Udemy Profile
- [Udemy Profile](https://www.udemy.com/user/kalyan-reddy-9/)

# AWS EKS - Elastic Kubernetes Service - Masterclass
[![Image](https://stacksimplify.com/course-images/AWS-EKS-Kubernetes-Masterclass-DevOps-Microservices-course.png "AWS EKS Kubernetes - Masterclass")](https://www.udemy.com/course/aws-eks-kubernetes-masterclass-devops-microservices/?referralCode=257C9AD5B5AF8D12D1E1)


# Azure Kubernetes Service with Azure DevOps and Terraform 
[![Image](https://stacksimplify.com/course-images/azure-kubernetes-service-with-azure-devops-and-terraform.png "Azure Kubernetes Service with Azure DevOps and Terraform")](https://www.udemy.com/course/azure-kubernetes-service-with-azure-devops-and-terraform/?referralCode=2499BF7F5FAAA506ED42)

# Azure - HashiCorp Certified: Terraform Associate - 70 Demos
[![Image](https://stacksimplify.com/course-images/azure-hashicorp-certified-terraform-associate-70demos.png "Azure - HashiCorp Certified: Terraform Associate - 70 Demos")](https://links.stacksimplify.com/azure-hashicorp-certified-terraform-associate)


## Additional References
- [Certification Curriculum](https://www.hashicorp.com/certification/terraform-associate)
- [Certification Preparation](https://learn.hashicorp.com/collections/terraform/certification)
- [Study Guide](https://learn.hashicorp.com/tutorials/terraform/associate-study?in=terraform/certification)
- [Exam Review Guide](https://learn.hashicorp.com/tutorials/terraform/associate-review?in=terraform/certification)
- [Sample Questions](https://learn.hashicorp.com/tutorials/terraform/associate-questions?in=terraform/certification)

======================================================================
