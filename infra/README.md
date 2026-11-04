# Infrastructure module

This directory contains the Terraform root module for the learning environment.

## Resources managed

- VPC with DNS support and hostnames enabled.
- Two public and two private subnets distributed across the first two available Availability Zones.
- Internet Gateway and public route table.
- Optional single NAT Gateway, Elastic IP, private route table, and private-subnet associations.

## Defaults and cost controls

The default configuration disables the NAT Gateway. Keep `enable_nat_gateway = false` while learning Terraform syntax and validating the module. Enabling NAT creates billable AWS resources.

## Commands

Run these commands from this directory:

```bash
terraform init
terraform fmt -check -recursive
terraform validate
terraform plan
```

A successful `plan` is not permission to deploy. Review the selected AWS account, region, CIDRs, planned resources, and estimated costs before explicitly applying anything.
