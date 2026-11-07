# Git Commit Naming Conventions

This repository follows conventional commit message format to maintain clear, searchable commit history.

## Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

## Commit Types

- **feat** - New feature or functionality
- **fix** - Bug fix
- **docs** - Documentation changes
- **refactor** - Code restructuring without changing functionality
- **chore** - Maintenance tasks, dependency updates
- **ci** - CI/CD workflow changes
- **ai** - AI agent documentation or tooling changes

## Scope

Scope indicates the area of the codebase affected. Multiple scopes can be used when changes affect multiple areas:

- **<app>** - An app's manifests, named after its directory under `apps/` (e.g. `hit-counter`)
- **aws** - Amazon Web Services provider configuration
- **gcp** - Google Cloud provider configuration
- **k3d** - k3d local development configuration
- **deps** - Dependency updates
- **global** - Repository-wide changes

When an app scope is used, it comes first, followed by any other scopes in alphabetical order.

Provider scopes (`aws`, `gcp`, `k3d`) mark provider-specific changes. Omit them when the change applies the same way across providers and another scope already covers it, such as updating an app's image tags in every overlay.

## Examples

```
feat(hit-counter): Add base manifests
feat(hit-counter, aws): Add AWS overlay
fix(hit-counter, gcp): Attach managed certificate to ingress
chore(deps): Update kustomize to v5
ci(global): Validate Kustomize builds in workflow
ai(global): Add initial agent documentation
```

## Subject Line Rules

- Use imperative mood ("Add" not "added" or "adds")
- First letter capitalized
- No period at the end
- Maximum 72 characters
- Be specific and descriptive

## Body (Optional)

Use body for:
- Explaining what and why (not how)
- Reference to issues/PRs

## Footer (Optional)

Use footer for:
- Issue references: `Closes #123`, `Fixes #456`

## GitOps Considerations

Since ArgoCD syncs from `main` branch:
- Each commit to `main` triggers deployment
- Commit messages should clearly indicate deployment impact
- Use `feat` and `fix` types to indicate user-visible changes
- Use `chore` and `refactor` for non-deployment changes
