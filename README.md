# GitOps Portfolio Guestbook

A Kubernetes guestbook application demonstrating GitOps practices with Kustomize for environment-specific deployments.

## Architecture

This application consists of:
- **Frontend**: PHP web application that serves the guestbook interface
- **Redis Leader**: Primary Redis instance for write operations
- **Redis Followers**: Read-only Redis replicas for scaling read operations

## Structure

```
k8s-manifests/
├── base/                    # Base Kubernetes manifests
│   ├── frontend-*           # Frontend deployment and service
│   ├── redis-leader-*       # Redis leader deployment and service
│   ├── redis-follower-*     # Redis follower deployment and service
│   └── kustomization.yaml   # Base kustomization
└── overlays/                # Environment-specific overlays
    ├── beta/                # Beta environment configuration
    └── prod/                # Production environment configuration
```

## Deployment

### Prerequisites
- Kubernetes cluster
- kubectl configured
- kustomize (or kubectl with kustomize support)

### Deploy to Beta
```bash
kubectl apply -k k8s-manifests/overlays/beta
```

### Deploy to Production
```bash
kubectl apply -k k8s-manifests/overlays/prod
```

## Environment Differences

- **Beta**: 2 frontend replicas, reduced resources, v5 frontend image
- **Production**: 5 frontend replicas, higher resources with limits, v4 frontend image

## Access the Application

After deployment, get the external IP:
```bash
kubectl get service frontend -n <namespace>
```

Navigate to the external IP in your browser to access the guestbook.