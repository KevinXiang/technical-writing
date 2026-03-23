# Deployment Guide

## Overview

This guide covers deploying the E-Commerce Platform across different environments.

## Prerequisites

- Docker 20.x or higher
- Docker Compose 2.x or higher
- Kubernetes cluster (for production)
- kubectl configured (for production)
- Cloud provider account (AWS/GCP/Azure)

## Development Deployment

### Local Development

For local development setup, see the main README Quick Start section.

### Docker Compose (Development)

The development environment runs locally using Docker Compose:

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

## Staging Deployment

### Environment Setup

```bash
# Deploy to staging
kubectl apply -f k8s/staging/

# Verify deployment
kubectl get pods -n staging

# Port forward to access
kubectl port-forward svc/api-gateway 3000:3000 -n staging
```

### Deployment Steps

1. Ensure all prerequisites are installed
2. Configure environment variables for staging
3. Apply Kubernetes manifests: `kubectl apply -f k8s/staging/`
4. Verify all pods are running: `kubectl get pods -n staging`
5. Test the staging environment

## Production Deployment

### Environment Setup

```bash
# Deploy to production
kubectl apply -f k8s/production/

# Verify deployment
kubectl get pods -n production

# Check rollout status
kubectl rollout status deployment/api-gateway -n production
```

### Deployment Steps

1. Ensure all prerequisites are installed
2. Configure production environment variables and secrets
3. Create Kubernetes secrets: `kubectl create secret generic app-secrets -n production`
4. Apply Kubernetes manifests: `kubectl apply -f k8s/production/`
5. Verify deployment health: `kubectl get pods -n production`
6. Configure monitoring and alerting

### Scaling

#### Horizontal Scaling

```bash
# Scale specific service
kubectl scale deployment order-service --replicas=3 -n production

# Configure auto-scaling
kubectl autoscale deployment order-service \
  --cpu-percent=70 \
  --min=2 --max=10 -n production
```

### Monitoring

#### Health Checks

```bash
# Check service health
curl http://api-gateway/health

# Check all services
kubectl get pods -n production
```

#### Logs

```bash
# View service logs
kubectl logs -f deployment/order-service -n production

# View all logs
kubectl logs -l app=ecommerce -n production --all-containers=true
```

## Troubleshooting

### Pods Not Starting

```bash
# Check pod status
kubectl describe pod <pod-name> -n production

# Check logs
kubectl logs <pod-name> -n production
```

### Service Not Accessible

```bash
# Check service endpoints
kubectl get endpoints -n production

# Check ingress
kubectl get ingress -n production
```

## See Also

- [Main README](../../README.md)
- [Individual module documentation](../../modules/)
