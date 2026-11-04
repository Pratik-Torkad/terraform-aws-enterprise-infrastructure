# Enterprise AWS Infrastructure with Terraform

A portfolio learning project for provisioning a secure, modular AWS foundation with Terraform.

> **Status:** Initial scaffold. Not yet deployed or validated in a live AWS account.

## Goals

- Build a reusable VPC with public and private subnets across two Availability Zones.
- Use Terraform variables and outputs rather than hard-coded environment-specific values.
- Keep Terraform state out of Git and prepare for secure remote state.
- Add formatting and validation checks to CI.
- Document deployment, verification, cost awareness, and cleanup.

## Architecture

- One VPC.
- Two public and two private subnets across two Availability Zones.
- Internet Gateway for public subnet routing.
- Optional single NAT Gateway for private subnet egress (billable).
- Separate public/private route tables when NAT is enabled.

## Repository layout

```text
.
├── .github/workflows/terraform-ci.yml
├── .gitignore
├── SECURITY.md
└── infra/
    ├── backend.tf.example
    ├── main.tf
    ├── outputs.tf
    ├── providers.tf
    ├── terraform.tfvars.example
    ├── variables.tf
    └── versions.tf
```

## Prerequisites

- Terraform CLI 1.10 or later.
- AWS CLI configured with a least-privilege IAM identity.
- An AWS account where you are authorized to create resources.

## Start locally

1. Install Terraform and AWS CLI.
2. Configure AWS credentials through an approved method (for example, an AWS CLI profile or short-lived environment credentials). Never commit credentials.
3. Copy the example variables file:

   ```bash
   cd infra
   cp terraform.tfvars.example terraform.tfvars
   ```

4. Review every value and expected AWS charges.
5. Initialize and validate:

   ```bash
   terraform init
   terraform fmt -check -recursive
   terraform validate
   terraform plan
   ```

6. Only after reviewing the plan and confirming the account, region, CIDRs, and charges, explicitly run `terraform apply`.

Do not run `apply` merely to test the code. The NAT Gateway is disabled by default to avoid unnecessary charges during initial validation.

## Remote state

Terraform state may contain sensitive infrastructure metadata. For the first learning iteration, use local state and keep it out of Git. `backend.tf.example` is a reference only. Configure a remote S3 backend in a separate bootstrap process after creating a dedicated state bucket with versioning, encryption, public-access blocking, and tightly scoped IAM permissions. Do not put access keys in backend configuration.

## CI

The GitHub Actions workflow runs Terraform formatting and validation on pull requests and pushes that change `infra/**`. It does not authenticate to AWS and does not deploy anything.

## Cost and cleanup

A NAT Gateway, public IPv4 addresses, and data transfer can incur charges. Inspect the plan and AWS pricing before deployment. After a test deployment, run `terraform destroy` from the same initialized working directory and verify the resources are removed. Retain state securely until cleanup is verified.

## Learning milestones

- [ ] Review the architecture and AWS networking concepts.
- [ ] Run `terraform fmt -check -recursive`.
- [ ] Run `terraform init` and `terraform validate`.
- [ ] Inspect a saved plan and understand every resource.
- [ ] Deploy only in a personal/test account after checking cost.
- [ ] Verify networking in the AWS console/CLI.
- [ ] Destroy test resources and document results.

## Portfolio honesty

Do not describe this as production-deployed or claim uptime, deployment counts, or cost savings until you have actually deployed, measured, and documented those results. Update this README with genuine command outputs, screenshots, and lessons learned as you complete each milestone.
