# Infrastructure Setup Guide

This guide explains how to set up infrastructure for your project using the `stevei101/infrastructure` repository.

## Overview

The infrastructure repository provides reusable Terraform workflows that handle:
- Terraform validation
- Security scanning (TFSec)
- Linting (TFLint)
- Planning and applying changes
- PR comments with results

## Quick Start

### 1. Add Terraform Configuration

Create your Terraform files in the `terraform/` directory:

```bash
mkdir -p terraform
cd terraform
```

### 2. Configure Workflow

The template includes `.github/workflows/infrastructure.yml` which is already configured to use the infrastructure repository's reusable workflows.

### 3. Set Up Secrets

Configure these secrets in your GitHub repository:

- `GCP_PROJECT_ID` - Your Google Cloud Project ID
- `TF_CLOUD_ORGANIZATION` - Terraform Cloud organization
- `TF_WORKSPACE` - Terraform Cloud workspace name
- `TF_API_TOKEN` - Terraform Cloud API token
- `WIF_PROVIDER` - Workload Identity Federation provider
- `WIF_SERVICE_ACCOUNT` - WIF service account email

### 4. Create Terraform Cloud Workspace

1. Go to [Terraform Cloud](https://app.terraform.io)
2. Create a new workspace for your project
3. Set the workspace name in your GitHub secret `TF_WORKSPACE`

## Workflow Details

The infrastructure workflow:

1. **Validates** your Terraform code
2. **Scans** for security issues with TFSec
3. **Lints** with TFLint
4. **Plans** changes (on PRs)
5. **Applies** changes (on main branch push)

## Customization

### Change Working Directory

If your Terraform code is in a different location:

```yaml
jobs:
  terraform:
    uses: stevei101/infrastructure/.github/workflows/terraform-agentnav-reusable.yml@main
    with:
      working_directory: 'infra/terraform'  # Custom path
```

### Add Security Scanning

The template includes a separate security workflow (`.github/workflows/security.yml`) that runs TFSec scans.

## Examples

See the infrastructure repository for examples:
- [Agentnav Terraform](https://github.com/stevei101/infrastructure/tree/main/terraform/agentnav)
- [Product Baseline Terraform](https://github.com/stevei101/infrastructure/tree/main/terraform/product-baseline-opensource)

## Troubleshooting

### Workflow Not Running

- Check that your Terraform files are in the `terraform/` directory
- Verify path filters in workflow triggers
- Ensure secrets are configured

### Terraform Cloud Errors

- Verify workspace exists and is accessible
- Check `TF_API_TOKEN` is valid
- Ensure `TF_WORKSPACE` matches workspace name

### Security Scan Failures

- Review TFSec findings in workflow logs
- Fix or ignore (with comments) security issues
- Adjust `minimum_severity` if needed

## Resources

- [Infrastructure Repository](https://github.com/stevei101/infrastructure)
- [Terraform Documentation](https://www.terraform.io/docs)
- [TFSec Documentation](https://aquasecurity.github.io/tfsec/)

