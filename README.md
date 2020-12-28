 Terraform module
Terraform module which creates S3 buckets, Cloud watch Logs, VPC,VPC Peerings,,Route 53 EC2 , Databases and IAM Roles and Policiesresources.

FOr following ENV 
UAT-BR. 
UAT-CA  
CAT-EU 
PROD-BR 
staging

1. FOR UAT-BR 

There are independent submodules VPC 

module "vpc" {
  source  = "app.terraform.io/InTouchHealth/vpc/aws"
  version = "1.0.5"

  name = "${local.name}-vpc"

  cidr = "10.219.32.0/19"
  ### 10.219.48.0 to 10.219.60.0 is unused so far as well as private/public/hl7 have an additional /22 available

  azs = ["${local.aws_region}a", "${local.aws_region}b", "${local.aws_region}c"]
  private_subnets = [
    "10.219.32.0/22",
    "10.219.36.0/22",
    "10.219.40.0/22",
    #"10.219.44.0/22", # Left as an extra for infra or adding a 4th az
  ]
  public_subnets = [
    "10.219.48.0/22",
    "10.219.52.0/22",
    "10.219.56.0/22",
    #"10.219.60.0/22", # Left as an extra for infra or adding a 4th az
Note that depends_on in modules is available since Terraform 0.13.

2. FOR UAT-CA

####################################################
# Module: VPC
#---------------------------------------
# VPCs, Subnets, Internet Gateway, NAT Gateway, etc.
####################################################

module "vpc" {
  source  = "app.terraform.io/InTouchHealth/vpc/aws"
  version = "1.0.5"

  name = "${local.name}-vpc"

  cidr = "10.219.0.0/19"
  ### 10.219.48.0 to 10.219.60.0 is unused so far as well as private/public/hl7 have an additional /22 available

  azs = ["${local.aws_region}a", "${local.aws_region}b", "${local.aws_region}d"]
  private_subnets = [
    "10.219.0.0/22",
    "10.219.4.0/22",
    "10.219.8.0/22",
    #"10.219.12.0/22", # Left as an extra for infra or adding a 4th az
  ]
  public_subnets = [
    "10.219.16.0/22",
    "10.219.20.0/22",
    "10.219.24.0/22",
    #"10.219.28.0/22", # Left as an extra for infra or adding a 4th az
  ]

3. FOR CAT-EU 
####################################################
# Module: VPC
#---------------------------------------
# VPCs, Subnets, Internet Gateway, NAT Gateway, etc.
####################################################

module "vpc" {
  source  = "app.terraform.io/InTouchHealth/vpc/aws"
  version = "1.0.5"

  name = "${local.name}-vpc"

  cidr = "10.219.64.0/21"
  ### 10.219.48.0 to 10.219.60.0 is unused so far as well as private/public/hl7 have an additional /22 available

  azs = ["${local.aws_region}a", "${local.aws_region}b", "${local.aws_region}c"]
  private_subnets = [
    "10.219.64.0/24",
    "10.219.65.0/24",
    "10.219.66.0/24",
    #"10.219.67.0/24", # Left as an extra for infra or adding a 4th az
  ]
  public_subnets = [
    "10.219.68.0/24",
    "10.219.69.0/24",
    "10.219.70.0/24",
    #"10.219.71.0/24", # Left as an extra for infra or adding a 4th az
  ]

4. FOR PROD-BR 
   ####################################################
# Module: VPC
#---------------------------------------
# VPCs, Subnets, Internet Gateway, NAT Gateway, etc.
####################################################

module "vpc" {
  source  = "app.terraform.io/InTouchHealth/vpc/aws"
  version = "0.0.9"

  name = "${local.name}-vpc"

  cidr = "10.213.0.0/18"
  ### 10.213.48.0 to 10.213.60.0 is unused so far as well as private/public/hl7 have an additional /22 available

  azs = ["${local.aws_region}a", "${local.aws_region}b", "${local.aws_region}c"]
  private_subnets = [
    "10.213.0.0/22",
    "10.213.4.0/22",
    "10.213.8.0/22",
    #"10.213.12.0/22", # Left as an extra for infra or adding a 4th az
  ]
  public_subnets = [
    "10.213.16.0/22",
    "10.213.20.0/22",
    "10.213.24.0/22",
    #"10.213.28.0/22", # Left as an extra for infra or adding a 4th az
  ]
  hl7_subnets = [
    "10.213.32.0/22",
    "10.213.36.0/22",
    "10.213.40.0/22",
    #"10.213.44.0/22", # Left as an extra for infra or adding a 4th az

5. FOR Staging
    ####################################################
# Module: VPC
#---------------------------------------
# VPCs, Subnets, Internet Gateway, NAT Gateway, etc.
####################################################

module "vpc" {
  source  = "app.terraform.io/InTouchHealth/vpc/aws"
  version = "0.0.9"

  name = "${local.name}-vpc"

  cidr = "10.213.0.0/18"
  ### 10.213.48.0 to 10.213.60.0 is unused so far as well as private/public/hl7 have an additional /22 available

  azs = ["${local.aws_region}a", "${local.aws_region}b", "${local.aws_region}c"]
  private_subnets = [
    "10.213.0.0/22",
    "10.213.4.0/22",
    "10.213.8.0/22",
    #"10.213.12.0/22", # Left as an extra for infra or adding a 4th az
  ]
  public_subnets = [
    "10.213.16.0/22",
    "10.213.20.0/22",
    "10.213.24.0/22",
    #"10.213.28.0/22", # Left as an extra for infra or adding a 4th az
  ]
  hl7_subnets = [
    "10.213.32.0/22",
    "10.213.36.0/22",
    "10.213.40.0/22",
    #"10.213.44.0/22", # Left as an extra for infra or adding a 4th az
  ]

Examples
Complete Route53 zones and records example which shows how to create Route53 records of various types like S3 bucket and CloudFront distribution.
Conditional creation
Sometimes you need to have a way to create resources conditionally but Terraform does not allow to use count inside module block, so the solution is to specify argument create.

Requirements
Name	Version
terraform	>= 0.12.6, < 0.14
aws	>= 2.49, < 4.0
Providers
No provider.

Inputs
No input.

Outputs
No output.




