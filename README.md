# Outegro VPS Infra

Minimal GitOps repository for the `outegro.dev` VPS k3s cluster.

## Shape

- k3s is installed manually on the VPS.
- Argo CD is bootstrapped manually through Helm.
- Platform components are managed by Argo CD.
- Secrets are stored in Git only as Bitnami Sealed Secrets.
- Helm is preferred for platform components whenever a stable chart exists.

## Bootstrap

Apply the root application once:

```bash
kubectl apply -f bootstrap/root-application.yaml
```

Argo CD then reads `argocd/apps` from this repository and syncs platform applications.

## Current Platform

- cert-manager
- Bitnami Sealed Secrets

