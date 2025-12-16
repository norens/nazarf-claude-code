# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **Nazarf's Claude Code Setup** - a Claude Code plugin providing 14 slash commands and 11 specialized AI agents optimized for modern web development workflows (Next.js, TypeScript, React, Supabase).

**Repository Type**: Plugin/Configuration (not a traditional application)
- No build system required
- No test suite (commands are markdown-based prompts)
- Distribution via GitHub and plugin marketplaces

## Installation & Testing

### Install Plugin
```bash
# Add marketplace
/plugin marketplace add norens/nazarf-claude-code

# Install plugin
/plugin install nazarf-claude-code
```

### Test Commands
```bash
# Test any slash command
/code-explain
/feature-plan
/api-new

# Verify agents activate on relevant queries
"Should I use WebSockets or SSE?"  # tech-stack-researcher activates
```

### Uninstall
```bash
/plugin uninstall nazarf-claude-code
```

### Local Development Testing
```bash
# Clone and install from local path
git clone https://github.com/norens/nazarf-claude-code.git
/plugin marketplace add /absolute/path/to/nazarf-claude-code
/plugin install nazarf-claude-code
```

## Repository Structure

```
nazarf-claude-code/
├── .claude-plugin/
│   ├── plugin.json           # Plugin metadata
│   ├── marketplace.json      # Marketplace registration metadata
│   └── MCP-SERVERS.md        # Documentation for integrated MCP servers
│
├── commands/                 # 14 slash commands (auto-discovered)
│   ├── api/                  # API route creation/testing/protection
│   ├── misc/                 # Dev utilities (lint, optimize, cleanup, docs)
│   ├── ui/                   # Component and page generation
│   ├── supabase/             # Supabase type gen and edge functions
│   └── new-task.md           # Task complexity analyzer
│
├── agents/                   # 11 specialized AI agents (auto-discovered)
│   ├── Architecture: tech-stack-researcher, system-architect, backend-architect,
│   │                 frontend-architect, requirements-analyst
│   ├── Quality: refactoring-expert, performance-engineer, security-engineer
│   └── Docs: technical-writer, learning-guide, deep-research-agent
│
└── .claude/                  # Local settings (not part of plugin)
    └── settings.local.json
```

## Command Architecture

Commands are markdown files with YAML front-matter:

```yaml
---
description: Brief description for UI
model: claude-sonnet-4-5  # Model to use for execution
---

[System prompt with $ARGUMENTS placeholder for user input]
```

**Key Points**:
- Commands are invoked via `/command-name` in Claude Code
- User input is injected via `$ARGUMENTS` variable
- Each command file is a complete system prompt defining behavior
- Located in `commands/` directory (organized by category)

## Agent Architecture

Agents are markdown files that auto-activate based on context:

```yaml
---
name: agent-name
description: When this agent should activate (shown to Claude)
model: sonnet
color: green  # UI color hint
---

[Comprehensive behavioral guidance and expertise definition]
```

**Key Points**:
- Agents activate automatically when user queries match descriptions
- Multiple agents can be active in a single conversation
- Provide domain-specific expertise (architecture, security, performance, etc.)
- Located in `agents/` directory (auto-discovered by Claude Code)

## Plugin Configuration

**File**: `.claude-plugin/plugin.json`

Contains only plugin metadata:
- name, version, description
- author information
- license

**Auto-discovery**: Commands and agents are automatically discovered from `commands/` and `agents/` directories - no need to register them in plugin.json.

## MCP Servers

Three pre-configured Model Context Protocol servers:

1. **context7**: Up-to-date library documentation (`npx -y @upstash/context7-mcp`)
2. **playwright**: Browser automation and testing (`npx @playwright/mcp@latest`)
3. **supabase**: Database operations (`npx -y @supabase/mcp-server-supabase`)

See `.claude-plugin/MCP-SERVERS.md` for usage documentation.

## Target Technology Stack

Commands and agents are optimized for:

**Frontend**:
- React 19
- Next.js 15 (App Router)
- TypeScript (strict mode)
- Tailwind CSS

**Backend**:
- Next.js API Routes
- Edge Runtime
- Node.js

**Database**:
- Supabase (PostgreSQL)
- Row Level Security (RLS) policies

**Development**:
- TypeScript strict mode (never use `any`)
- Zod for validation
- Modern ES6+ patterns

## Command Implementation Philosophy

All commands follow these principles:

1. **Type Safety**: No `any` types, comprehensive TypeScript typing
2. **Validation**: Zod schemas for runtime validation at API boundaries
3. **Error Handling**: Consistent error response formats, appropriate HTTP status codes
4. **Security**: Input sanitization, CORS configuration, rate limiting considerations
5. **Best Practices**: Follow Next.js 15, React 19, and modern web standards
6. **Solo Developer Optimized**: Pragmatic approaches balancing quality and velocity

## Making Changes to Plugin

### Adding a New Command

1. Create markdown file in `commands/[category]/command-name.md`
2. Use YAML front-matter format (see examples in existing commands)
3. Commands are auto-discovered - no registration needed
4. Test with `/command-name` after reinstalling plugin

### Adding a New Agent

1. Create markdown file in `agents/agent-name.md`
2. Use YAML front-matter with `name`, `description`, `model`, `color`
3. Agents are auto-discovered - no registration needed
4. Description should clearly define when agent activates
5. Test by asking questions that should trigger agent

### Updating Plugin Version

1. Increment version in `.claude-plugin/plugin.json`
2. Update `.claude-plugin/marketplace.json` if needed
3. Commit and push to GitHub
4. Users can update with `/plugin update nazarf-claude-code`

## Distribution

### GitHub (Primary)
- Repository serves as source of truth
- Users install via `/plugin install norens/nazarf-claude-code`
- Updates via `/plugin update nazarf-claude-code`

### Community Marketplaces
Can be submitted to:
- Claude Code Plugins Marketplace
- CC Plugins Curated Marketplace (github.com/ccplugins/marketplace)
- Claude Code Plugins Plus (github.com/jeremylongshore/claude-code-plugins-plus)

See `PUBLISHING.md` for detailed submission instructions.

## File Editing Guidelines

When editing command/agent files:

1. **Preserve YAML front-matter**: Must be first thing in file, enclosed in `---`
2. **Test `$ARGUMENTS` injection**: Ensure user input placeholder works correctly
3. **Maintain markdown formatting**: Content is rendered as system prompts
4. **Keep focused**: Each command should do one thing well
5. **Match existing patterns**: Review similar commands for consistency

## Git Workflow

- **Main branch**: All development on `main` (no separate dev branches)
- **Commits**: Clear, descriptive messages
- **Versioning**: Semantic versioning in `plugin.json`

Current version: 1.0.0

## Requirements

- Claude Code 2.0.13+
- Node.js (for MCP server execution via npx)
- No other runtime dependencies

## Common Tasks

### Verify Plugin Structure
```bash
# Check all required files exist
ls .claude-plugin/plugin.json
ls .claude-plugin/marketplace.json
ls commands/
ls agents/
```

### View Plugin Configuration
```bash
cat .claude-plugin/plugin.json
```

### Test Command Locally
After editing a command file, reinstall:
```bash
/plugin uninstall nazarf-claude-code
/plugin install nazarf-claude-code
/your-command-name
```

### Validate JSON Configuration
```bash
# Ensure plugin.json is valid JSON
cat .claude-plugin/plugin.json | python -m json.tool
```

## Documentation Files

- **README.md**: User-facing installation and feature documentation
- **QUICK-START.md**: 5-minute guide for publishing plugins
- **PUBLISHING.md**: Detailed marketplace submission instructions
- **MCP-SERVERS.md**: MCP server usage and troubleshooting
- **CLAUDE.md**: This file - guidance for Claude Code instances

## Philosophy

This plugin emphasizes:
- **Productivity through automation**: Reduce repetitive scaffolding
- **Knowledge codification**: Transform best practices into reusable AI-assisted workflows
- **Type safety first**: Never compromise on TypeScript strict typing
- **Research-driven decisions**: AI-powered tech recommendations with evidence
- **Solo developer focus**: Pragmatic balance of quality and velocity
