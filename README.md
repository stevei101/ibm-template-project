# Project Template - stevei101 Organization

A comprehensive template repository for creating new projects in the `stevei101` GitHub organization. This template provides a solid foundation with integrated workflows for infrastructure, container builds, and Kubernetes deployments.

## 🚀 Quick Start

### Create a New Project

```bash
# Use GitHub's template feature
gh repo create my-new-project --template stevei101/ibm-template-project

# Or clone and customize
git clone https://github.com/stevei101/ibm-template-project.git my-new-project
cd my-new-project
```

### Initialize Your Project

1. **Update project name** in `README.md` and configuration files
2. **Configure secrets** in GitHub repository settings (see [Required Secrets](#required-secrets))
3. **Customize workflows** in `.github/workflows/` for your specific needs
4. **Set up infrastructure** by following the [Infrastructure Setup](#infrastructure-setup) guide

## 📦 What's Included

### Project Structure

```
.
├── .github/
│   └── workflows/
│       ├── infrastructure.yml      # Terraform infrastructure workflows
│       ├── build.yml               # Container build workflows
│       ├── deploy.yml              # Kubernetes deployment workflows
│       └── security.yml            # Security scanning workflows
├── src/                            # Your application source code
│   ├── frontend/                   # Frontend application
│   └── backend/                    # Backend application
├── terraform/                      # Terraform configurations (optional)
├── k8s/                            # Kubernetes manifests (optional)
├── docs/                           # Project documentation
├── .gitignore                      # Git ignore patterns
└── README.md                       # This file
```

### Integrated Workflows

This template includes pre-configured GitHub Actions workflows that integrate with:

- **Infrastructure Repository** (`stevei101/infrastructure`) - Terraform and infrastructure management
- **Podman/Kustomize Repository** (`stevei101/podman-kustomize-k8s-deploy-gha`) - Container builds and K8s deployments

## 🔗 Repository Ecosystem

This template is part of the `stevei101` organization's modular infrastructure:

| Repository | Purpose | Usage |
|------------|---------|-------|
| **ibm-template-project** | Project template | Start here for new projects |
| **infrastructure** | Terraform & IaC | Infrastructure as Code workflows |
| **podman-kustomize-k8s-deploy-gha** | Containers & K8s | Container builds and deployments |

### Learn More

- [Infrastructure Repository](https://github.com/stevei101/infrastructure)
- [Podman/Kustomize Repository](https://github.com/stevei101/podman-kustomize-k8s-deploy-gha)
- [Repository Ecosystem Guide](https://github.com/stevei101/infrastructure/blob/main/docs/REPOSITORY_ECOSYSTEM.md)

## 🏗️ Infrastructure Setup

### Using Terraform Workflows

The template includes a workflow that uses the infrastructure repository's reusable workflows:

```yaml
# .github/workflows/infrastructure.yml
name: Infrastructure

on:
  push:
    paths: ['terraform/**']
  pull_request:
    paths: ['terraform/**']

jobs:
  terraform:
    uses: stevei101/infrastructure/.github/workflows/terraform-agentnav-reusable.yml@main
    secrets:
      GCP_PROJECT_ID: ${{ secrets.GCP_PROJECT_ID }}
      TF_CLOUD_ORGANIZATION: ${{ secrets.TF_CLOUD_ORGANIZATION }}
      TF_WORKSPACE: ${{ secrets.TF_WORKSPACE }}
      TF_API_TOKEN: ${{ secrets.TF_API_TOKEN }}
      WIF_PROVIDER: ${{ secrets.WIF_PROVIDER }}
      WIF_SERVICE_ACCOUNT: ${{ secrets.WIF_SERVICE_ACCOUNT }}
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Security Scanning

TFSec security scanning is also available:

```yaml
# .github/workflows/security.yml
jobs:
  tfsec:
    uses: stevei101/infrastructure/.github/workflows/tfsec-scan-reusable.yml@main
    with:
      terraform_path: 'terraform'
      minimum_severity: 'MEDIUM'
      fail_on_issues: true
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## 🐳 Container Builds

### Using Podman Build Workflows

For containerized applications, use the Podman build workflows:

```yaml
# .github/workflows/build.yml
name: Build Containers

on:
  push:
    paths: ['src/**']

jobs:
  build:
    uses: stevei101/podman-kustomize-k8s-deploy-gha/.github/workflows/podman-build-reusable.yml@main
    with:
      backend_path: 'src/backend'
      frontend_path: 'src/frontend'
      build_target: 'production'
    secrets:
      GCP_PROJECT_ID: ${{ secrets.GCP_PROJECT_ID }}
      WIF_PROVIDER: ${{ secrets.WIF_PROVIDER }}
      WIF_SERVICE_ACCOUNT: ${{ secrets.WIF_SERVICE_ACCOUNT }}
```

## ☸️ Kubernetes Deployments

### Using Kustomize Deployment Workflows

For Kubernetes deployments:

```yaml
# .github/workflows/deploy.yml
name: Deploy to Kubernetes

on:
  workflow_dispatch:
    inputs:
      environment:
        type: choice
        options: [development, staging, production]

jobs:
  deploy:
    uses: stevei101/podman-kustomize-k8s-deploy-gha/.github/workflows/k8s-deploy.yml@main
    with:
      environment: ${{ github.event.inputs.environment }}
      image_tag: ${{ github.sha }}
    secrets:
      GCP_PROJECT_ID: ${{ secrets.GCP_PROJECT_ID }}
      WIF_PROVIDER: ${{ secrets.WIF_PROVIDER }}
      WIF_SERVICE_ACCOUNT: ${{ secrets.WIF_SERVICE_ACCOUNT }}
```

## 🔐 Required Secrets

Configure these secrets in your GitHub repository settings:

### Infrastructure Secrets
- `GCP_PROJECT_ID` - Google Cloud Project ID
- `TF_CLOUD_ORGANIZATION` - Terraform Cloud organization name
- `TF_WORKSPACE` - Terraform Cloud workspace name
- `TF_API_TOKEN` - Terraform Cloud API token
- `WIF_PROVIDER` - Workload Identity Federation provider
- `WIF_SERVICE_ACCOUNT` - WIF service account email

### Container Build Secrets
- `GCP_PROJECT_ID` - Google Cloud Project ID
- `WIF_PROVIDER` - Workload Identity Federation provider
- `WIF_SERVICE_ACCOUNT` - WIF service account email

### Automatic Secrets
- `GITHUB_TOKEN` - Automatically provided by GitHub Actions

## 📚 Documentation

- [Infrastructure Setup Guide](docs/INFRASTRUCTURE_SETUP.md)
- [Container Build Guide](docs/CONTAINER_BUILD.md)
- [Kubernetes Deployment Guide](docs/K8S_DEPLOYMENT.md)
- [Repository Ecosystem](docs/ECOSYSTEM.md)

## 🛠️ Customization

### 1. Update Project Name

Replace `ibm-template-project` with your project name in:
- `README.md`
- `.github/workflows/*.yml` (if customizing)
- Configuration files

### 2. Configure Workflows

Edit workflow files in `.github/workflows/` to match your project structure:
- Update paths (e.g., `src/backend`, `src/frontend`)
- Adjust triggers and conditions
- Add project-specific steps

### 3. Add Your Code

- Place frontend code in `src/frontend/`
- Place backend code in `src/backend/`
- Add Terraform configs in `terraform/` (if needed)
- Add K8s manifests in `k8s/` (if needed)

## 🎯 Best Practices

1. **Start Small**: Begin with basic workflows, add complexity as needed
2. **Use Reusable Workflows**: Leverage the infrastructure and Podman/Kustomize repos
3. **Security First**: Always scan for secrets before committing
4. **Document Changes**: Update README and docs as you customize
5. **Test Locally**: Test workflows locally when possible

## 🤝 Contributing

When enhancing this template:

1. Keep it generic and reusable
2. Document all additions
3. Test workflows before committing
4. Update this README with new features

## 📖 Related Resources

- [Infrastructure Repository](https://github.com/stevei101/infrastructure)
- [Podman/Kustomize Repository](https://github.com/stevei101/podman-kustomize-k8s-deploy-gha)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Terraform Documentation](https://www.terraform.io/docs)
- [Kustomize Documentation](https://kustomize.io/)

## 📝 License

This template follows the same license as the parent organization's projects.

---

**Happy Coding!** 🚀

For questions or issues, please open an issue in the respective repository or contact the `stevei101` organization maintainers.
