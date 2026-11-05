# Kustomize Structure and Hierarchy

Each app under `apps/` uses Kustomize with a three-layer hierarchy:

## Structure

```
apps/<app>/
├── base/              # Base Kubernetes manifests
├── providers/         # Provider-specific configurations (aws, gcp, k3d)
└── overlays/          # Environment-specific overlays (aws-prod, gcp-prod, k3d-dev)
```

## Hierarchy

1. **base/** - Contains the core Kubernetes resources (Deployments, Services, Ingress, etc.)
   - Foundation layer with no environment-specific configuration

2. **providers/** - Provider-specific layer that references `base/`
   - Customizes resources for specific infrastructure providers (AWS, Google Cloud, k3d)
   - Example: `providers/k3d/` references `../../base`

3. **overlays/** - Environment-specific layer that references `providers/`
   - Applies environment-specific patches, image tags, and configurations
   - Example: `overlays/aws-prod/` references `../../providers/aws` and adds patches

## Deployment

ArgoCD in the [kube](https://github.com/VWongDev/kube) repository discovers `apps/*/overlays/<environment>` and creates an Application per app.

## Usage

Build for a specific environment:
```bash
kustomize build apps/<app>/overlays/aws-prod
```
