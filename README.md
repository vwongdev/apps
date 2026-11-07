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

## Updating Images
Source repos call the [`bump-image`](.github/workflows/bump-image.yaml) reusable workflow after pushing images. It sets the tag of every `app-name/...` image in the app's `*-prod` overlays and opens a pull request.

```yaml
bump-image:
  needs: build
  uses: VWongDev/apps/.github/workflows/bump-image.yaml@main
  with:
    app: app-name
    # tag: defaults to the caller's commit SHA
    # commit-message: defaults to "chore(app-name): Update image to latest commit"
  secrets: inherit
```

The calling repo needs `BUMP_APP_ID` and `BUMP_APP_PRIVATE_KEY` secrets for a GitHub App installed on this repo with read and write access to contents and pull requests.
