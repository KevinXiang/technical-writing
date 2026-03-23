# Deployment Guide

> Deployment instructions for the E-Commerce Platform

## Overview

This guide covers deploying the E-Commerce Platform to different environments.

## Prerequisites

- Docker 20.x+
- Kubernetes cluster (for production)
- kubectl configured
- AWS/GCP/Azure account (for cloud deployment)

## Environment Setup

### Development Environment

The development environment runs locally using Docker Compose:

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

### Staging Environment

```bash
# Deploy to staging
kubectl apply -f k8s/staging/

# Verify deployment
kubectl get pods -n staging

# Port forward to access
kubectl port-forward svc/api-gateway 3000:3000 -n staging
```

### Production Environment

```bash
# Deploy to production
kubectl apply -f k8s/production/

# Verify deployment
kubectl get pods -n production

# Check rollout status
kubectl rollout status deployment/api-gateway -n production
```

## Configuration

### Environment Variables

Each service requires specific environment variables:

| Variable | Description | Required |
|----------|-------------|----------|
| `DATABASE_URL` | PostgreSQL connection string | Yes |
| `REDIS_URL` | Redis connection string | Yes |
| `RABBITMQ_URL` | RabbitMQ connection string | Yes |
| `JWT_SECRET` | JWT signing secret | Yes |
| `PAYMENT_GATEWAY_KEY` | Payment gateway API key | Yes |

### Secrets Management

Use Kubernetes secrets for sensitive data:

```bash
# Create secret
kubectl create secret generic app-secrets \
  --from-literal=jwt-secret=YOUR_SECRET \
  --from-literal=database-url=YOUR_DB_URL \
  -n production
```

## Scaling

### Horizontal Scaling

```bash
# Scale specific service
kubectl scale deployment order-service --replicas=3 -n production

# Configure auto-scaling
kubectl autoscale deployment order-service \
  --cpu-percent=70 \
  --min=2 --max=10 -n production
```

### Vertical Scaling

Adjust resource requests in deployment manifests:

```yaml
resources:
  requests:
    cpu: "500m"
    memory: "512Mi"
  limits:
    cpu: "2000m"
    memory: "2Gi"
```

## Monitoring

### Health Checks

```bash
# Check service health
curl http://api-gateway/health

# Check all services
kubectl get pods -n production
```

### Logs

```bash
# View service logs
kubectl logs -f deployment/order-service -n production

# View all logs
kubectl logs -l app=ecommerce -n production --all-containers=true
```

## Rollback

```bash
# Rollback to previous version
kubectl rollout undo deployment/order-service -n production

# Rollback to specific revision
kubectl rollout undo deployment/order-service --to-revision=2 -n production
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

- [User Management Deployment](../modules/user-management/#deployment)
- [Product Catalog Deployment](../modules/product-catalog/#deployment)
- [Order Processing Deployment](../modules/order-processing/#deployment)
- [Payment Processing Deployment](../modules/payment-processing/#deployment)
