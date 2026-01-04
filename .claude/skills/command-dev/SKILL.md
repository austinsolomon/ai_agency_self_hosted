---
name: command-dev
description: Create and develop Claude Code slash commands with YAML frontmatter, dynamic arguments, and best practices
trigger: Create command, add slash command, write custom command, command frontmatter
version: 1.0.0
---

# Command Development Skill

Guide for creating Claude Code slash commands with proper structure and best practices.

## Command File Format

Commands are Markdown files (`.md`) with optional YAML frontmatter:

```markdown
---
description: Brief description for /help
argument-hint: [arg1] [arg2]
allowed-tools: Read, Bash(git:*)
---

Command prompt content here.
Use $ARGUMENTS or $1, $2 for arguments.
Use @path/to/file to include files.
Use !`bash command` to execute and include output.
```

## Command Locations

- `.claude/commands/` - Project-level commands (shared with team)
- `~/.claude/commands/` - User-level commands (personal)

## Key Features

### Dynamic Arguments
- `$ARGUMENTS` - All arguments as single string
- `$1`, `$2`, `$3`, etc. - Individual positional arguments

### File References
- `@path/to/file` - Include file contents in prompt

### Bash Execution
- `` !`command` `` - Execute bash command and include output

## Frontmatter Fields

| Field | Purpose | Example |
|-------|---------|---------|
| `description` | Brief description shown in /help | `"Deploy application"` |
| `argument-hint` | Document expected arguments | `[environment] [version]` |
| `allowed-tools` | Restrict tool access | `Read, Bash(docker:*)` |
| `model` | Specify model to use | `sonnet`, `opus`, `haiku` |

## Common Patterns

### Simple Command
```markdown
---
description: Check Docker status
allowed-tools: Bash(docker:*)
---

Check Docker containers and services status:
!`docker ps`
!`docker-compose ps`
```

### Command with Arguments
```markdown
---
description: Start specific service
argument-hint: [service-name]
allowed-tools: Bash(docker:*)
---

Starting service: $1
!`docker-compose up -d $1`
```

### Command with File Reference
```markdown
---
description: Analyze configuration
argument-hint: [config-file]
---

Analyze configuration file:
@$1

Provide recommendations for improvements.
```

## Best Practices

1. **Single responsibility** - One clear purpose per command
2. **Clear descriptions** - Make commands discoverable via /help
3. **Document arguments** - Always use `argument-hint`
4. **Minimal tools** - Use most restrictive `allowed-tools` possible
5. **Handle errors** - Consider missing arguments and error states
6. **Test thoroughly** - Verify all features work as expected

## Development Workflow

1. Create `.md` file in `.claude/commands/`
2. Add minimal frontmatter (description at minimum)
3. Write clear, focused prompt
4. Test with `/command-name`
5. Refine based on results

## Tool Restrictions

Use `allowed-tools` to limit what Claude can do:

```yaml
allowed-tools: Read, Bash(git:*)  # Only Read and git commands
allowed-tools: Bash(docker:*)      # Only docker commands
allowed-tools: Read, Write         # Read and Write only
```

## Example: /start Command

```markdown
---
description: Initialize development environment
allowed-tools: Read, Bash(cp:*, docker:*, docker-compose:*)
---

# Initialize Development Environment

1. Copy environment configuration
2. Verify Docker is running
3. Start n8n server
4. Verify services are ready

Execute:
!`cp secret_dot_env .env 2>/dev/null && echo "✓ Environment configured" || echo "✗ Failed to copy .env"`
!`docker ps >/dev/null 2>&1 && echo "✓ Docker running" || echo "✗ Docker not running"`
!`docker-compose ps`
!`curl -s http://localhost:5678 >/dev/null && echo "✓ n8n ready at localhost:5678" || echo "✗ n8n not responding"`

Report status in bullet points.
```
