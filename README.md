# cmb-k8s-delete-me

Kubernetes configurations for CMB applications managed by ArgoCD.

## Project Structure

```
.
├── cmb-apps/              # ArgoCD Application definitions
│   ├── application.yaml   # cmb-portfolio ArgoCD app
│   ├── feed.yaml          # cmb-feed ArgoCD app
│   └── infrastructure.yaml # cmb-infrastructure ArgoCD app (shared resources)
├── cmb-infrastructure/    # Shared infrastructure (namespaces, etc.)
│   └── namespace.yaml     # cmb-production namespace
├── cmb-portfolio/         # Portfolio application manifests
│   └── deploy.yaml        # Portfolio nginx deployment
└── cmb-feed/              # Feed application manifests
    └── deploy.yaml        # Feed nginx deployment
```

## ArgoCD Applications

### 1. cmb-infrastructure
Manages shared resources like namespaces to avoid SharedResourceWarning conflicts.

### 2. cmb-portfolio
Portfolio application deployment (nginx:1.27.5-alpine) - 3 replicas

### 3. cmb-feed
Feed application deployment (nginx:1.27-alpine) - 3 replicas

## Deployment Order

1. Apply infrastructure first (creates namespace):
```bash
kubectl apply -f cmb-apps/infrastructure.yaml
```

2. Then apply applications:
```bash
kubectl apply -f cmb-apps/application.yaml  # cmb-portfolio
kubectl apply -f cmb-apps/feed.yaml         # cmb-feed
```

## Harbor Registry

Using private Harbor registry: `harbor.coinmarketboards.com/dev-test/`

Available images:
- `nginx:latest` (pushed)
- `nginx:1.27-alpine` (needs to be pushed if used)
- `nginx:1.27.5-alpine` (needs to be pushed if used)

### Push images to Harbor:

```bash
# Pull from Docker Hub
docker pull nginx:1.27.5-alpine

# Tag for Harbor
docker tag nginx:1.27.5-alpine harbor.coinmarketboards.com/dev-test/nginx:1.27.5-alpine

# Push to Harbor
docker push harbor.coinmarketboards.com/dev-test/nginx:1.27.5-alpine
```

## Monitoring

```bash
# Check all pods in production namespace
kubectl get pods -n cmb-production

# Watch pod status
kubectl get pods -n cmb-production -w

# Check ArgoCD applications
kubectl get applications -n argocd

# Sync ArgoCD app manually
argocd app sync cmb-portfolio
argocd app sync cmb-feed
```
