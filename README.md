# UnidealLtu - GitOps

The application source code is kept separately in [UnidealLtu-app](https://github.com/UnidealLtu/UnidealLtu-app).

## Environments

All three environments run in the same K3s cluster, with a separate Kubernetes namespace for each.

| Environment | Namespace | Argo CD application | Sync |
| --- | --- | --- | --- |
| Development | `unidealltu-dev` | `unidealltu-dev` | Automatic |
| Staging | `unidealltu-staging` | `unidealltu-staging` | Manual |
| Production | `unidealltu-prod` | `unidealltu-prod` | Manual |

Each environment has its own values file in `environments/`.

## Repository structure

```text
UnidealLtu-gitops/
├── argocd/
│   ├── unidealltu-dev.yaml
│   ├── unidealltu-staging.yaml
│   └── unidealltu-prod.yaml
├── charts/
│   └── unidealltu/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│           ├── configmap.yaml
│           ├── deployment.yaml
│           └── service.yaml
├── environments/
│   ├── development/values.yaml
│   ├── staging/values.yaml
│   └── production/values.yaml
└── README.md
```