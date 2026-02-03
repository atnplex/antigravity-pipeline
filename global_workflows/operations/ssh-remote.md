---
description: SSH connection shortcuts for servers
---

# SSH Remote Workflow

// turbo-all

## Quick Connect

### VPS1 (Primary)

```bash
ssh alex@100.102.55.88
```

### VPS2 / Condo

```bash
ssh alex@100.67.88.109
```

## Multi-Command Execution

### Run command on VPS1

```bash
ssh alex@100.102.55.88 "<command>"
```

### Run command on Condo

```bash
ssh alex@100.67.88.109 "<command>"
```

## Common Remote Tasks

### Check Docker containers

```bash
ssh alex@<ip> "docker ps"
```

### Check disk space

```bash
ssh alex@<ip> "df -h"
```

### Check memory

```bash
ssh alex@<ip> "free -h"
```

### Tail logs

```bash
ssh alex@<ip> "docker logs <container> --tail 100 -f"
```
