# CLAUDE.md

Claude Code configuration for **ai_agency_self_hosted** project.

---

## 📋 Project Overview

Self-hosted AI agency platform.

---

## 🏗️ Architecture

```
ai_agency_self_hosted/
├── .claude/
│   ├── settings.json         ← Permissions + hooks
│   ├── commands/             ← Slash commands (/cmd)
│   └── skills/               ← Auto-invoked capabilities
├── .mcp.json                 ← MCP server connections (if needed)
├── CLAUDE.md                 ← This file (auto-loaded)
└── README.md
```

---

## 🎯 Rules

1. **Read Before Write** - Never modify unseen code
2. **Minimal Changes** - Only what's necessary
3. **Security First** - Never commit secrets
4. **Test First** - Ensure changes don't break existing functionality

---

## 🔐 Security

- Sandbox mode enabled
- Never commit `.env`, credentials, API keys
- Review all third-party dependencies

---

**Created:** 2026-01-03 | **Owner:** austinsolomon
