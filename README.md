# UnidealLtu – GitOps

K3s runs one Kubernetes cluster with 3 namespaces:

| Environment | Namespace | Argo CD Application | Helm values file | Sync |
|---|---|---|---|---|
| Development | `unidealltu-dev` | `unidealltu-dev` | `values-dev.yaml` | Automatic |
| Staging | `unidealltu-staging` | `unidealltu-staging` | `values-staging.yaml` | Manual |
| Production | `unidealltu-prod` | `unidealltu-prod` | `values-prod.yaml` | Manual |

## Before first push

GitHub owner is configured as `UnidealLtu` in all 3 files under `argocd/`. Ensure that the `UnidealLtu-gitops` repository exists under that account or organization. Change the URL if you use a different GitHub owner.

The starter chart deploys ONE sample Nginx web service in each namespace to validate Helm and Argo CD. It **does not** yet deploy React, FastAPI, Keycloak, databases, RabbitMQ, or actual marketplace features. Add those later as separate workloads or Helm releases. There is **no public Ingress** by default because your DNS and HTTPS hostnames haven't been selected.

## Verify templates locally

```bash
helm lint charts/unidealltu
helm template unidealltu-dev charts/unidealltu -f charts/unidealltu/values-dev.yaml -n unidealltu-dev
```

## Bootstrap on your VM

Prerequisite: Argo CD is already installed in namespace `argocd`; if it isn't, complete the course Argo CD tutorial first. For a private GitHub GitOps repository, connect it in the Argo CD web UI before creating the apps.

```bash
kubectl --kubeconfig ~/.kube/config get pods -n argocd
kubectl --kubeconfig ~/.kube/config apply -n argocd -f argocd/
kubectl --kubeconfig ~/.kube/config get applications -n argocd
```

Development syncs automatically. Open the Argo CD UI and click Sync on the staging and production apps to deploy them the first time. Subsequent updates to staging/production also require manual Sync.

```bash
kubectl --kubeconfig ~/.kube/config get pods -n unidealltu-dev
kubectl --kubeconfig ~/.kube/config get pods -n unidealltu-staging
kubectl --kubeconfig ~/.kube/config get pods -n unidealltu-prod
kubectl --kubeconfig ~/.kube/config port-forward -n unidealltu-dev svc/unidealltu-dev 8080:80
```

Keep the port-forward terminal running, then run `curl http://127.0.0.1:8080` from a second VM terminal. Repeat on staging or production with their respective namespaces and service names. Do not expect public URLs until DNS + TLS + Ingress are configured.

## Later

Add individual Helm templates for React, Listings Service, Favorites Service, databases and authentication. Build immutable images in your application repository, and promote tested image versions through GitOps commits/PRs. Never store database passwords, JWT signing secrets or GitHub tokens in plaintext in this repository.
