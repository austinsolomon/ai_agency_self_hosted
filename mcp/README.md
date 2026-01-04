# MCP Integrations

This directory contains Model Context Protocol (MCP) server integrations for the AI Agency platform.

## What is MCP?

Model Context Protocol (MCP) is an open protocol that standardizes how applications provide context to AI models. MCP servers expose tools, resources, and prompts that AI assistants can use to interact with external systems.

## Available Integrations

### n8n-mcp

Located in `./n8n-mcp/`

An MCP server that provides AI assistants with comprehensive access to n8n node documentation, properties, and operations.

**Features:**
- 543 n8n nodes documentation
- Node properties and operations
- 2,709 workflow templates
- Real-world examples and configurations

**Quick Start:**
```bash
cd mcp/n8n-mcp
npm install
npm start
```

See `./n8n-mcp/README.md` for detailed setup instructions.

## Adding MCP to Claude Desktop

To connect these MCP servers to Claude Desktop, update your Claude Desktop config:

**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
**Linux:** `~/.config/Claude/claude_desktop_config.json`

Example configuration:
```json
{
  "mcpServers": {
    "n8n-mcp": {
      "command": "npx",
      "args": ["n8n-mcp"],
      "env": {
        "MCP_MODE": "stdio",
        "LOG_LEVEL": "error",
        "DISABLE_CONSOLE_OUTPUT": "true",
        "N8N_API_URL": "http://localhost:5678",
        "N8N_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

## Integration with n8n

Our n8n instance running in Docker can be integrated with the n8n-mcp server to enable AI-powered workflow building and management.

1. Get your n8n API key from http://localhost:5678
2. Configure the MCP server with your n8n instance URL and API key
3. Restart Claude Desktop to load the new configuration

## Directory Structure

```
mcp/
├── README.md           ← This file
└── n8n-mcp/           ← n8n MCP server integration
    ├── src/           ← Source code
    ├── docs/          ← Documentation
    ├── examples/      ← Example configurations
    └── README.md      ← n8n-mcp specific docs
```

## Learn More

- [MCP Documentation](https://modelcontextprotocol.io/)
- [n8n-mcp GitHub](https://github.com/czlonkowski/n8n-mcp)
- [Claude Desktop](https://claude.ai/download)
