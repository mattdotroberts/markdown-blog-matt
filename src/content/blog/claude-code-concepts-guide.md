---
title: "Claude Code Concepts: The Guide I Wish I Had"
description: "MCP servers, skills, hooks, agents, tools—Claude Code has a lot of terminology. Here's the reference I made after getting confused by all of it."
pubDate: 2025-01-19
tags: ["claude-code", "ai", "developer-tools"]
featured: true
draft: false
---

When I started using Claude Code, I kept running into terms I didn't quite understand. MCP servers. Skills. Hooks. Agents. They all sounded similar but clearly did different things. The documentation exists, but I wanted a single reference that explained what's what and when to use each.

So I made one.

---

## Extension Mechanisms

### MCP (Model Context Protocol) Servers

External processes that provide tools, resources, and prompts to Claude.

| Aspect | Description |
|--------|-------------|
| **What it is** | Separate server process Claude connects to |
| **Protocol** | Communicates via stdio or HTTP |
| **Language** | Can be written in any language (Python, TypeScript, Go, etc.) |
| **Persistence** | Configured in settings, persists across sessions |
| **Use cases** | Database access, API integrations, custom tooling |

**Example:** An MCP server that provides tools to query your PostgreSQL database or interact with external APIs.

**Configuration:** `~/.claude/settings.json` or project-level `.claude/settings.json`

---

### Skills

Prompt-based workflows that define how Claude should handle specific tasks.

| Aspect | Description |
|--------|-------------|
| **What it is** | Specialized instructions/prompts for common tasks |
| **Invocation** | Slash commands like `/commit`, `/review-pr` |
| **Execution** | Runs within the main conversation context |
| **Scope** | Can be built-in or custom-defined |

**Example:** The `/commit` skill provides instructions for creating well-formatted git commits with proper messages.

---

### Plugins

Extensions that add features to the Claude Code CLI itself.

| Aspect | Description |
|--------|-------------|
| **What it is** | Bundled extensions for core functionality |
| **Status** | May not be active in all configurations |

---

### MCP vs Skills: The Key Difference

| | MCP Servers | Skills |
|---|-------------|--------|
| **Purpose** | Add new capabilities | Standardize workflows |
| **Runs as** | Separate process | Within conversation |
| **Invoked via** | Tool calls (automatic) | Slash commands (manual) |
| **Provides** | New tools/resources | Guided instructions |

MCP servers give Claude new abilities. Skills tell Claude how to use its abilities for specific tasks.

---

## Agents vs Tools

This distinction tripped me up at first. Both execute actions, but they work very differently.

### Tools

Direct actions executed in the current session.

- `Bash`, `Read`, `Write`, `Edit`, `Grep`, `Glob`, etc.
- Run synchronously
- Return raw output
- General-purpose primitives

### Agents (via the Task Tool)

Specialized subprocesses that handle complex tasks autonomously.

- Spawn with their own context
- Can run in background
- Return summarized results
- Domain-specific (e.g., `django-expert`, `react-component-architect`)

**When to use agents:**
- Complex multi-step research
- Domain-specific tasks matching an agent's expertise
- Tasks requiring extensive exploration

Think of tools as individual commands and agents as specialists you delegate entire jobs to.

---

## Hooks vs Skills

| | Hooks | Skills |
|---|-------|--------|
| **Trigger** | Automatic (on events) | Manual (`/command`) |
| **Purpose** | Automation/validation | Guided workflows |
| **Configuration** | Settings file | Built-in or custom |
| **Examples** | Run linter on file save | `/commit` for commits |

**Hooks** run shell commands automatically in response to events (tool calls, prompt submission, etc.).

**Skills** are invoked explicitly by the user with slash commands.

The distinction: hooks are reactive automation, skills are proactive workflows.

---

## Plan Mode vs Regular Mode

### Regular Mode
- Full access to all tools
- Can read, write, and edit files
- Execute commands freely

### Plan Mode
- Research and design only
- No file writes/edits allowed
- Used for planning implementation before execution
- Requires user approval to exit

**Enter:** `EnterPlanMode` tool
**Exit:** `ExitPlanMode` tool (requires user approval)

Plan mode is useful when you want Claude to think through a problem before touching any code.

---

## Configuration: CLAUDE.md vs Settings

| | CLAUDE.md | Settings |
|---|-----------|----------|
| **Location** | Project root | `~/.claude/settings.json` |
| **Scope** | Project-specific | Global or project |
| **Format** | Natural language markdown | JSON |
| **Version control** | Yes (committed with repo) | Typically not |
| **Purpose** | Project instructions/context | Tool configuration |

### CLAUDE.md
Project-specific instructions read at session start. Contains coding standards, project context, and guidance for Claude. Write it like you're onboarding a new developer.

### Settings
Configuration for MCP servers, hooks, permissions, and other Claude Code features. This is where you wire up external tools and automation.

---

## Background vs Foreground Tasks

### Foreground (Default)
- Blocks until complete
- Results shown immediately
- Use for quick operations

### Background
- Set `run_in_background: true`
- Continues while conversation proceeds
- Check status with `TaskOutput` tool
- Use for long-running operations

---

## Web Tools: WebFetch vs WebSearch

| | WebFetch | WebSearch |
|---|----------|-----------|
| **Input** | Specific URL | Search query |
| **Output** | Page content (as markdown) | Search results with links |
| **Use case** | Known resources/documentation | Discovery/research |
| **Caching** | 15-minute cache | No cache |

**WebFetch:** "Get the content from this specific page"
**WebSearch:** "Find pages about this topic"

---

## Quick Reference

| Feature | Type | Invocation | Purpose |
|---------|------|------------|---------|
| MCP Server | External process | Automatic (tools) | Add capabilities |
| Skill | Prompt workflow | `/command` | Standardize tasks |
| Hook | Shell automation | Automatic (events) | Validate/automate |
| Agent | Subprocess | `Task` tool | Complex tasks |
| Tool | Direct action | Tool call | Primitives |
| Plan Mode | Session mode | `EnterPlanMode` | Design before build |
| CLAUDE.md | Project config | Auto-loaded | Project context |
| Settings | JSON config | N/A | Global config |

---

## The Mental Model

After sorting through all this, here's how I think about it:

- **Tools** are Claude's hands—direct actions like reading files or running commands
- **Agents** are specialists Claude can delegate to
- **MCP servers** extend what Claude can do
- **Skills** standardize how Claude does common tasks
- **Hooks** automate responses to events
- **Settings** configure the machinery
- **CLAUDE.md** gives Claude project context

Once these concepts clicked, working with Claude Code became much more intuitive. Hopefully this reference saves you the same confusion I went through.
