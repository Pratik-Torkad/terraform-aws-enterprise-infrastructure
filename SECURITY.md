# Security

- Never commit AWS access keys, secret keys, session tokens, private keys, or Terraform state.
- Use short-lived credentials or an AWS IAM role with least privilege.
- Review every Terraform plan before applying it.
- Keep state in a dedicated encrypted, versioned S3 bucket with restricted access when moving to remote state.
- If a credential is exposed, revoke or rotate it in AWS immediately.
- This repository is a learning scaffold, not a production-certified module.
