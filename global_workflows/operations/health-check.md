---
description: Health check endpoints for running services
---

# Health Check Workflow

// turbo-all

## Service Health Endpoints

### Antigravity Manager (condo)

```bash
curl -s http://100.67.88.109:8045/health
```

### Check WebSocket

```bash
curl -s http://100.67.88.109:8045/ws
```

## Docker Container Health

### List running containers

```bash
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
```

### Check specific container

```bash
docker inspect --format='{{.State.Health.Status}}' <container>
```

## Aggregate Health Check

```bash
# Check all critical services
for ip in 100.102.55.88 100.67.88.109; do
  echo "=== $ip ==="
  ssh alex@$ip "docker ps --format '{{.Names}}: {{.Status}}'"
done
```
