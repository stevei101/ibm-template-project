# Container Build Guide

This guide explains how to build containers using the `stevei101/podman-kustomize-k8s-deploy-gha` repository workflows.

## Overview

The Podman/Kustomize repository provides reusable workflows for:
- Building container images with Podman
- Pushing to Google Artifact Registry
- Tagging images (latest + commit SHA)
- Multi-service support (frontend, backend)

## Quick Start

### 1. Create Dockerfiles

Create Dockerfiles for your services:

```bash
# src/backend/Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "main.py"]
```

```bash
# src/frontend/Dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY . .
RUN npm install && npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
```

### 2. Configure Workflow

The template includes `.github/workflows/build.yml` which is already configured. Update paths if needed:

```yaml
with:
  backend_path: 'src/backend'      # Your backend path
  frontend_path: 'src/frontend'    # Your frontend path
```

### 3. Set Up Secrets

Configure these secrets in your GitHub repository:

- `GCP_PROJECT_ID` - Your Google Cloud Project ID
- `WIF_PROVIDER` - Workload Identity Federation provider
- `WIF_SERVICE_ACCOUNT` - WIF service account email

### 4. Configure Artifact Registry

Ensure you have an Artifact Registry repository set up in GCP:

```bash
gcloud artifacts repositories create app-images \
  --repository-format=docker \
  --location=us-central1
```

## Workflow Details

The build workflow:

1. **Authenticates** to GCP using Workload Identity Federation
2. **Installs** Podman
3. **Builds** container images for each service
4. **Tags** images with `latest` and commit SHA
5. **Pushes** to Artifact Registry

## Customization

### Single Service

If you only have one service:

```yaml
# Only build backend
with:
  backend_path: 'src/backend'
  frontend_path: ''  # Leave empty to skip
```

### Custom Registry

```yaml
with:
  registry_location: 'europe-west1'
  registry_name: 'my-custom-registry'
```

### Build Target

For multi-stage Dockerfiles:

```yaml
with:
  build_target: 'production'  # or 'development', 'test'
```

## Local Development

You can also use Podman locally:

```bash
# Install Podman
brew install podman  # macOS
# or
sudo apt-get install podman  # Linux

# Build locally
podman build -t my-app:latest src/backend

# Run locally
podman run -p 8000:8000 my-app:latest
```

## Resources

- [Podman/Kustomize Repository](https://github.com/stevei101/podman-kustomize-k8s-deploy-gha)
- [Podman Documentation](https://docs.podman.io/)
- [Google Artifact Registry](https://cloud.google.com/artifact-registry)

