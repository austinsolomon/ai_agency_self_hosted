---
description: Initialize development environment and start n8n server
---

# Initialize Development Environment

## Run

1. Copy environment file:
!`cp secret_dot_env .env`

2. Open Docker Desktop:
!`Start-Process -FilePath "C:\Program Files\Docker\Docker\Docker Desktop.exe"`

3. Wait for Docker to initialize:
!`sleep 5`

4. Start services (n8n + n8n-mcp + monitoring):
!`docker-compose up -d`

5. Wait for Grafana to start:
!`sleep 10`

6. Open Grafana dashboard:
!`Start-Process "http://localhost:3001"`

7. Open n8n in browser:
!`Start-Process "http://localhost:5678"`

8. Check containers:
!`docker ps --filter name=n8n --filter name=n8n-mcp --filter name=grafana --filter name=prometheus --filter name=cadvisor`

## Report

Output status in bullet points:
- Environment: configured
- Docker Desktop: opened
- Grafana: opened at http://localhost:3001
- n8n container: running
- n8n-mcp container: running
- Browser: opened to http://localhost:5678
