# Repository Ecosystem Guide

This guide explains how the `stevei101` organization's repositories work together.

## Overview

The `stevei101` organization uses a modular approach with specialized repositories:

```
stevei101 Organization
│
├── 📦 ibm-template-project (This Template)
│   └── Starting point for new projects
│
├── 🏗️ infrastructure
│   └── Terraform & Infrastructure as Code
│
├── 🐳 podman-kustomize-k8s-deploy-gha
│   └── Container builds & Kubernetes deployments
│
└── 📁 Your Project Repositories
    └── Use the template and reference the infrastructure repos
```

## How It Works

### 1. Start with Template

Create your project from this template:

```bash
gh repo create my-project --template stevei101/ibm-template-project
```

### 2. Use Infrastructure Workflows

Your project's `.github/workflows/infrastructure.yml` calls:

```yaml
uses: stevei101/infrastructure/.github/workflows/terraform-agentnav-reusable.yml@main
```

This provides:
- Terraform validation
- TFLint scanning
- TFSec security scanning
- Terraform plan/apply
- PR comments with results

### 3. Use Container Build Workflows

Your project's `.github/workflows/build.yml` calls:

```yaml
uses: stevei101/podman-kustomize-k8s-deploy-gha/.github/workflows/podman-build-reusable.yml@main
```

This provides:
- Podman container builds
- Image tagging (latest + commit SHA)
- Push to Artifact Registry
- Multi-service support

### 4. Use Deployment Workflows

Your project's `.github/workflows/deploy.yml` can use:

```yaml
uses: stevei101/podman-kustomize-k8s-deploy-gha/.github/workflows/k8s-deploy.yml@main
```

This provides:
- Kubernetes deployments
- Environment-specific configs
- Helm or Kustomize support

## Benefits

✅ **Consistency**: All projects use the same infrastructure patterns  
✅ **Maintainability**: Update workflows once, all projects benefit  
✅ **Best Practices**: Built-in security scanning and validation  
✅ **Speed**: Get started quickly with pre-configured workflows  
✅ **Flexibility**: Customize as needed for your project  

## Learn More

- [Infrastructure Repository](https://github.com/stevei101/infrastructure)
- [Podman/Kustomize Repository](https://github.com/stevei101/podman-kustomize-k8s-deploy-gha)
- [Infrastructure Ecosystem Docs](https://github.com/stevei101/infrastructure/blob/main/docs/REPOSITORY_ECOSYSTEM.md)

