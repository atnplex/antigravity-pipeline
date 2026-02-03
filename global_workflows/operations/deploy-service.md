---
description: Deploy services to VPS1, VPS2 (condo), or Unraid
---

# Deploy Service Workflow

## Server Inventory

| Name | Role | Tailscale IP |
|------|------|-------------|
| vps | VPS1 - Primary | 100.102.55.88 |
| condo | VPS2 - Secondary | 100.67.88.109 |
| unraid | Media/Storage | 100.64.xxx.xxx |

## Docker Compose Deploy

### Pull Latest

```bash
ssh alex@<server-ip> "cd /path/to/service && git pull origin main"
```

### Rebuild and Start

```bash
ssh alex@<server-ip> "cd /path/to/service && docker compose up -d --build"
```

### View Logs

```bash
ssh alex@<server-ip> "docker logs <container> --tail 50 -f"
```

## Service Locations

| Service | Server | Path |
|---------|--------|------|
| antigravity-manager | condo | /home/alex/docker/antigravity-manager |

## Health Checks

### Antigravity Manager

```bash
curl -s http://100.67.88.109:8045/health
```
