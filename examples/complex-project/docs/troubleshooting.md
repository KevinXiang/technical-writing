# Troubleshooting Guide

> Common issues and solutions for the E-Commerce Platform

## Overview

This guide covers common issues and their solutions when working with the E-Commerce Platform.

## Common Issues

### Service Won't Start

**Symptoms:** Service fails to start or crashes immediately

**Solutions:**
```bash
# Check service logs
make logs-service-auth

# Verify environment variables
cat .env

# Check dependencies
docker-compose ps

# Restart service
make restart-service-auth
```

### Database Connection Failed

**Symptoms:** Service can't connect to database

**Solutions:**
```bash
# Check database status
docker-compose ps postgres

# Verify connection string
echo $DATABASE_URL

# Restart database
docker-compose restart postgres

# Check database logs
docker-compose logs postgres
```

### Message Queue Issues

**Symptoms:** Services not receiving messages

**Solutions:**
```bash
# Check RabbitMQ status
docker-compose ps rabbitmq

# List queues
docker-compose exec rabbitmq rabbitmqctl list_queues

# Check queue depth
docker-compose exec rabbitmq rabbitmqctl list_queues name messages

# Restart RabbitMQ
docker-compose restart rabbitmq
```

### High Memory Usage

**Symptoms:** Service using excessive memory

**Solutions:**
```bash
# Check resource usage
docker stats

# Restart service
make restart-service-auth

# Check for memory leaks
# Review metrics in Grafana
```

### Order Processing Stuck

**Symptoms:** Orders not processing

**Solutions:**
```bash
# Check order service logs
make logs-service-order

# Check message queue
docker-compose exec rabbitmq rabbitmqctl list_queues

# Verify payment service
curl http://localhost:3006/health

# Check for stuck orders
# Query database for pending orders
```

## Debug Procedures

### Enable Debug Logging

```bash
# Set log level to debug
export LOG_LEVEL=debug

# Restart service
make restart-service-auth
```

### Check Service Health

```bash
# Health check all services
for service in auth user product order inventory payment notification; do
  echo "Checking $service..."
  curl http://localhost:3000/health
done
```

### View Distributed Traces

```bash
# Access Jaeger UI
open http://localhost:16686

# Search traces by service
# Filter by operation name
# Analyze trace timeline
```

## Log Analysis

### View Logs

```bash
# View all logs
docker-compose logs

# View specific service logs
docker-compose logs auth-service

# Follow logs
docker-compose logs -f order-service

# View last 100 lines
docker-compose logs --tail=100 payment-service
```

### Search Logs

```bash
# Search for errors
docker-compose logs | grep ERROR

# Search for specific order
docker-compose logs | grep order-123

# Search for time range
docker-compose logs --since="2024-01-01T00:00:00" --until="2024-01-01T01:00:00"
```

## Getting Help

### Check Documentation

- [Development Guide](development.md)
- [Architecture Documentation](architecture.md)
- [API Documentation](api/)

### Search Issues

```bash
# Search existing GitHub issues
# https://github.com/example/ecommerce-platform/issues
```

### Create New Issue

When creating an issue, include:
- Description of problem
- Steps to reproduce
- Expected vs actual behavior
- Environment details
- Logs and error messages
- Screenshots if applicable

### Contact

- **Email:** support@example.com
- **Slack:** #ecommerce-platform
- **On-call:** +1-555-123-4567

## Performance Issues

### Slow Response Times

**Solutions:**
```bash
# Check service metrics
curl http://localhost:3000/metrics

# Check database query performance
# Review slow query log

# Check cache hit rate
# Redis info command

# Review Grafana dashboards
```

### High CPU Usage

**Solutions:**
```bash
# Check CPU usage
docker stats

# Profile service
# Use language-specific profiler

# Scale services
kubectl scale deployment/auth-service --replicas=3
```

---

**Last Updated:** 2024-03-23
