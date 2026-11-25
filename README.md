# Unideal - GitOps

The application source code is kept separately in [Unideal-app](https://github.com/Unideal/Unideal-app).

## Environments

All three environments run in the same K3s cluster, with a separate Kubernetes namespace for each.

| Environment | Namespace | Argo CD application | Sync |
| --- | --- | --- | --- |
| Development | `unideal-dev` | `unideal-dev` | Automatic |
| Staging | `unideal-staging` | `unideal-staging` | Manual |
| Production | `unideal-prod` | `unideal-prod` | Manual |

Each environment has its own values file in `environments/`.

## Repository structure

```text
Unideal-gitops/
├── argocd/
│   ├── unideal-dev.yaml
│   ├── unideal-staging.yaml
│   └── unideal-prod.yaml
├── charts/
│   └── unideal/
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