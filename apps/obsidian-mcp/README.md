# Obsidian MCP
Manifests for [obsidian-mcp](https://github.com/VWongDev/obsidian-mcp).

## Secrets
[`base/secrets.yaml`](base/secrets.yaml) syncs the `obsidian-mcp-sync` JSON secret from AWS Secrets Manager or GCP Secret Manager. Each key becomes an environment variable.

| Key | Value |
|---|---|
| `OBSIDIAN_AUTH_TOKEN` | Obsidian [account token](https://github.com/VWongDev/obsidian-mcp#sync-client) |
| `VAULT_PASSWORD` | Encryption password, if any |

[`base/kustomization.yaml`](base/kustomization.yaml) sets `ISSUER_URL`, `ALLOWED_SUBJECTS` and `VAULT_NAME`. Overlays set `PUBLIC_URL` and `AUDIENCE`.

```sh
aws secretsmanager create-secret --name obsidian-mcp-sync --secret-string file://sync.json
gcloud secrets create obsidian-mcp-sync --data-file sync.json
```

On k3d, use kube's [secret store example](https://github.com/VWongDev/kube/blob/main/providers/k3d/secret-store.example.yaml).
