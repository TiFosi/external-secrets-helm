This chart deploys an ExternalSecret resource, a CRD provided by the External Secrets Operator, which allows you to fetch secrets from external secret stores (e.g., HashiCorp Vault) and create a Kubernetes Secret from them. (see https://external-secrets.io)

Example usage:

```yaml
# values.yaml
storeName: vault-cluster-secret-store
secretName: "{{ .Release.Name }}-external-secret"
refreshInterval: 1h
remoteRefPath: secret/path/in/vault
secrets:
- key: SOME_SECRET
- key: ANOTHER_SECRET
  remoteKey: key-is-different-in-vault
- key: SOME_OTHER_SECRET
  remoteRefPath: secret/path/in/vault/different
```

## Parameters

### 

**Prerequisites:**
- External Secrets Operator 0.15.1+ installed with all CRDs `ExternalSecret`, `ClusterSecretStore`, and `SecretStore`
- A secret store (`ClusterSecretStore` or `SecretStore`) created and configured to access the external store (e.g., HashiCorp Vault, AWS Secrets Manager, etc.)

| Name                       | Description                                                                                                                                                                                      | Value                                 |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------- |
| `storeName`                | The name of the secret store                                                                                                                                                                     | `""`                                  |
| `storeKind`                | The kind of the secret store. `ClusterSecretStore` or `SecretStore`. If undefined (the default), set to `null` or `""` then `ClusterSecretStore` is used.                                        |                                       |
| `secretName`               | The name of the ExternalSecret & the Kubernetes Secret that will be created                                                                                                                      | `{{ .Release.Name }}-external-secret` |
| `refreshInterval`          | The interval at which the ExternalSecret will be refreshed                                                                                                                                       | `1h`                                  |
| `creationPolicy`           | The creation policy of the ExternalSecret. `Owner`, `Merge`, or `None`. If undefined (the default), set to `null` or `""`, then `Owner` is used.                                                 |                                       |
| `remoteRefPath`            | The path in the external store (e.g., Vault) where all the secrets defined in the `secrets` array are stored. If not provided, the parameter must be set for each secret in the `secrets` array. | `secret/path/in/vault`                |
| `secrets`                  | Array of secrets to be retrieved from the external store and stored in the Kubernetes Secret                                                                                                     | `[]`                                  |
| `secrets.[].key`           | The key of the secret in the Kubernetes Secret.                                                                                                                                                  |                                       |
| `secrets.[].remoteRefPath` | The path to the secret in the external store if it is different from `remoteRefPath` or `remoteRefPath` is not provided.                                                                         |                                       |
| `secrets.[].remoteKey`     | The key of the secret in the external store if it is different from `secrets.[].key`.                                                                                                            |                                       |
