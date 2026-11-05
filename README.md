# Multicloud Kubernetes Apps
This repository contains manifests for apps deployed by ArgoCD to clusters provisioned by [kube](https://github.com/VWongDev/kube).

## Repository Layout
Each app has its own directory under `apps/`, layered in build order.

1. `base/`: Manifests shared by all providers.
2. `providers/`: Patches for each infrastructure provider.
3. `overlays/`: Environment configuration such as hosts and images, discovered by ArgoCD.

## Prerequisites
The tools are installed automatically with:
- [Nix](https://nixos.org/download) with flakes enabled
- [direnv](https://direnv.net) hooked into your shell

Load the tools on entering the repository:
```sh
direnv allow
```

Alternatively, install the packages listed in [`flake.nix`](flake.nix) manually.

## Usage
Build the manifests for an environment:
```sh
kustomize build apps/app-name/overlays/aws-prod
```
