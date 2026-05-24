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

## Bootstrap-only components

Argo CD itself is installed with Helm before the root application exists. Keep its Helm values in:

- `bootstrap/argocd-values.yaml`

Use it when installing or upgrading Argo CD:

```bash
helm upgrade --install argocd argo/argo-cd \
  --namespace argocd \
  --create-namespace \
  --values bootstrap/argocd-values.yaml
```

The `configs.params.server.insecure` value is required because Traefik terminates TLS for `argocd.outegro.dev`; Argo CD must serve plain HTTP behind the ingress.

## Current Platform

- cert-manager
- Bitnami Sealed Secrets
