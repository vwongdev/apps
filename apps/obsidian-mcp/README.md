# Obsidian MCP
Manifests for [obsidian-mcp](https://github.com/VWongDev/obsidian-mcp).

## Secrets
The ExternalSecrets in [`base/secrets.yaml`](base/secrets.yaml) read two secrets from the ClusterSecretStore `secrets`: AWS Secrets Manager in the cluster's account and region, or GCP Secret Manager in the cluster's project. Each is a JSON object whose keys become environment variables:

| Secret | Key | Value |
|---|---|---|
| `obsidian-mcp-sync` | `OBSIDIAN_AUTH_TOKEN` | Obsidian account token, from the [sync client](https://github.com/VWongDev/obsidian-mcp#sync-client) login |
| | `VAULT_NAME` | Remote vault name or ID |
| | `VAULT_PASSWORD` | End-to-end encryption password; omit if the vault has none |
| `obsidian-mcp-server` | `ISSUER_URL` | Identity provider issuer, such as `https://<tenant>.auth0.com/` |
| | `ALLOWED_SUBJECTS` | Comma-separated token subjects allowed access, such as `auth0\|123` |

`PUBLIC_URL` and `AUDIENCE` differ per deployment, so each overlay sets them instead.

Create them from JSON files, such as `sync.json`:
```json
{"OBSIDIAN_AUTH_TOKEN": "...", "VAULT_NAME": "...", "VAULT_PASSWORD": "..."}
```
```sh
# AWS
aws secretsmanager create-secret --name obsidian-mcp-sync --secret-string file://sync.json
# GCP
gcloud secrets create obsidian-mcp-sync --data-file sync.json
```

On k3d, add them to the local secret store in [kube](https://github.com/VWongDev/kube) instead.
