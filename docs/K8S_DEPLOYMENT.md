# Kubernetes Deployment Guide

This guide explains how to deploy to Kubernetes using Kustomize patterns from the `stevei101/podman-kustomize-k8s-deploy-gha` repository.

## Overview

The Podman/Kustomize repository provides:
- Kustomize base and overlay patterns
- Kubernetes manifest examples
- Deployment workflow templates
- Environment-specific configurations

## Quick Start

### 1. Create Kubernetes Manifests

Create your Kubernetes manifests in the `k8s/` directory:

```bash
mkdir -p k8s/{base,overlays/{development,staging,production}}
```

### 2. Set Up Kustomize

Create `k8s/base/kustomization.yaml`:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - namespace.yaml
  - deployment.yaml
  - service.yaml
```

### 3. Create Environment Overlays

Create `k8s/overlays/development/kustomization.yaml`:

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

bases:
  - ../../base

patchesStrategicMerge:
  - deployment-patch.yaml
```

### 4. Configure Deployment Workflow

The template includes `.github/workflows/deploy.yml`. Customize it for your needs:

```yaml
- name: Deploy with Kustomize
  run: |
    kubectl apply -k k8s/overlays/${{ github.event.inputs.environment }}
```

## Deployment Patterns

### Using Kustomize

```bash
# Development
kubectl apply -k k8s/overlays/development

# Staging
kubectl apply -k k8s/overlays/staging

# Production
kubectl apply -k k8s/overlays/production
```

### Using Helm

If you prefer Helm:

```bash
helm upgrade --install my-app ./helm/my-app \
  --namespace my-app \
  --create-namespace \
  -f values-production.yaml
```

## Examples

See the Podman/Kustomize repository for examples:
- [Kubernetes Manifests](https://github.com/stevei101/podman-kustomize-k8s-deploy-gha/tree/main/k8s/manifests)
- [Kustomize Plan](https://github.com/stevei101/podman-kustomize-k8s-deploy-gha/blob/main/kustomize/kustomize_plan.md)

## Resources

- [Podman/Kustomize Repository](https://github.com/stevei101/podman-kustomize-k8s-deploy-gha)
- [Kustomize Documentation](https://kustomize.io/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)

