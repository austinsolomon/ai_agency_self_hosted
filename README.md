# AI Agency Self-Hosted

A self-hosted AI agency platform powered by n8n with monitoring and MCP integration.

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         SERVICES                                │
├──────────────┬──────────────┬──────────────┬───────────────────┤
│     n8n      │   n8n-mcp    │  prometheus  │     grafana       │
│  :5678       │   :3000      │   :9090      │     :3001         │
│  Workflows   │  AI Bridge   │   Metrics    │   Dashboards      │
└──────────────┴──────────────┴──────────────┴───────────────────┘
```

## Prerequisites

- Docker Desktop installed and running
- Windows OS (Linux/Mac also supported)

## Quick Start

### 1. Create Environment File

Create `secret_dot_env` with your credentials:

```bash
# n8n credentials
N8N_USER=admin
N8N_PASSWORD=your-secure-password

# n8n-mcp authentication
N8N_MCP_AUTH_TOKEN=your-mcp-token
N8N_API_KEY=your-n8n-api-key

# Grafana (optional)
GRAFANA_ADMIN_USER=admin
GRAFANA_ADMIN_PASSWORD=admin
```

### 2. Run /start Command

```
/start
```

This copies your environment, starts Docker Desktop, launches all services, and opens n8n in your browser.

### 3. Access Services

| Service | URL | Purpose |
|---------|-----|---------|
| n8n | http://localhost:5678 | Workflow builder |
| n8n-mcp | http://localhost:3000 | AI assistant bridge |
| Grafana | http://localhost:3001 | Monitoring dashboard |
| Prometheus | http://localhost:9090 | Metrics storage |

## Services

### n8n (Port 5678)
Workflow automation engine. Build automations with 500+ integrations.

### n8n-mcp (Port 3000)
Model Context Protocol server enabling AI assistants (Claude) to:
- Access 543+ n8n node documentation
- Browse 2,709 workflow templates
- Manage workflows via API

### Prometheus (Port 9090)
Time-series metrics database. Scrapes and stores container metrics.

### Grafana (Port 3001)
Visualization platform with pre-configured n8n monitoring dashboard.

## Managing Services

```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# View logs
docker-compose logs -f n8n

# Restart specific service
docker-compose restart n8n

# Update images
docker-compose pull && docker-compose up -d

# Start with GPU monitoring (requires NVIDIA GPU)
docker-compose --profile gpu up -d
```

## Data Persistence

| Volume | Purpose |
|--------|---------|
| `n8n_data` | Workflows, credentials, execution history |
| `prometheus_data` | Metrics time-series data |
| `grafana_data` | Dashboards, users, settings |
| `./n8n-local-files/` | Local files accessible to n8n workflows |

## Project Structure

```
ai_agency_self_hosted/
├── .claude/                  # Claude Code configuration
│   ├── commands/start.md     # /start command
│   ├── skills/command-dev/   # Command development skill
│   └── settings.json         # Permissions
├── mcp/                      # MCP integrations
│   └── n8n-mcp/              # n8n MCP server source
├── monitoring/               # Observability config
│   ├── prometheus/           # Prometheus scrape config
│   └── grafana/              # Dashboards & provisioning
├── n8n-local-files/          # Workflow file storage (empty)
├── docker-compose.yml        # Service orchestration
├── secret_dot_env            # Environment template (create this)
├── .env                      # Active environment (generated)
└── README.md                 # This file
```

## MCP Integration with Claude Desktop

Add to `%APPDATA%\Claude\claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "n8n-mcp": {
      "command": "npx",
      "args": ["n8n-mcp"],
      "env": {
        "MCP_MODE": "stdio",
        "N8N_API_URL": "http://localhost:5678",
        "N8N_API_KEY": "your-api-key"
      }
    }
  }
}
```

## Windows Notes

- **cadvisor** has limited Windows support - container metrics may be incomplete
- Ensure Docker Desktop is set to start on login for auto-start functionality
- Use `/start` command for the smoothest startup experience

## Security

Files excluded from version control:
- `.env` - Runtime credentials
- `secret_dot_env` - Credential template
- `n8n-local-files/` - User data
