 Terraform module
Terraform module which creates S3 buckets, Cloud watch Logs, VPC,VPC Peerings,,Route 53 EC2 resources.
FOr following ENV 
UAT-BR. UAT-CA  CAT-EU PROD-BR and staging


There are independent submodules:

zones - to manage Route53 zones
records - to manage Route53 records
This module currently does not have all arguments supported by the Terraform AWS providers.

Terraform versions
Terraform 0.12. Pin module version to ~> v1.0. Submit pull-requests to master branch.

Usage
Create Route53 zones and records
module "zones" {
  source  = "terraform-aws-modules/route53/aws//modules/zones"
  version = "~> 1.0"

  zones = {
    "terraform-aws-modules-example.com" = {
      comment = "terraform-aws-modules-examples.com (production)"
      tags = {
        env = "production"
      }
    }

    "myapp.com" = {
      comment = "myapp.com"
    }
  }
}

module "records" {
  source  = "terraform-aws-modules/route53/aws//modules/records"
  version = "~> 1.0"

  zone_name = keys(module.zones.this_route53_zone_zone_id)[0]

  records = [
    {
      name    = "apigateway1"
      type    = "A"
      alias   = {
        name    = "d-10qxlbvagl.execute-api.eu-west-1.amazonaws.com"
        zone_id = "ZLY8HYME6SFAD"
      }
    },
    {
      name    = ""
      type    = "A"
      ttl     = 3600
      records = [
        "10.10.10.10",
      ]
    },
  ]

  depends_on = [module.zones]
}
Note that depends_on in modules is available since Terraform 0.13.

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

Authors
Module managed by Anton Babenko.

License
Apache 2 Licensed. See LICENSE for full details.
