---
title: "Claude Code Concepts: The Guide I Wish I Had"
description: "MCP servers, skills, hooks, agents, tools—Claude Code has a lot of terminology. Here's the reference I made after getting confused by all of it."
pubDate: 2026-01-19
tags: ["claude-code", "ai", "developer-tools"]
featured: true
draft: false
---

When I started using Claude Code, I kept running into terms I didn't quite understand. MCP servers. Skills. Hooks. Agents. They all sounded similar but clearly did different things. The documentation exists, but I wanted a single reference that explained what's what and when to use each.

So I made one.

---

## MCP (Model Context Protocol) Servers

External processes that give Claude new capabilities.

- **Runs as:** A separate server process Claude connects to
- **Protocol:** Communicates via stdio or HTTP
- **Language:** Can be written in Python, TypeScript, Go—anything
- **Persistence:** Configured once, available across all sessions

**Use cases:** Database access, API integrations, custom tooling.

For example, you might have an MCP server that lets Claude query your PostgreSQL database:

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://localhost/mydb"]
    }
  }
}
```

Configure these in `~/.claude/settings.json` (global) or `.claude/settings.json` (project).

---

## Skills

Prompt-based workflows invoked with slash commands.

- **What they are:** Specialized instructions for common tasks
- **Invocation:** `/commit`, `/review-pr`, etc.
- **Execution:** Runs within your current conversation

The `/commit` skill, for example, tells Claude exactly how to create well-formatted git commits—what to check, how to structure the message, when to include co-author attribution.

Skills standardize *how* Claude does things. MCP servers expand *what* Claude can do.

---

## MCP vs Skills

This was my first point of confusion. Here's the distinction:

**MCP Servers:**
- Add new tools and capabilities
- Run as separate processes
- Invoked automatically when Claude needs them

**Skills:**
- Provide guided workflows for existing capabilities
- Run within the conversation
- Invoked manually with `/command`

Think of it this way: an MCP server might give Claude the ability to query Jira. A skill would define the workflow for how Claude should triage and update tickets.

---

## Tools vs Agents

Both execute actions, but they work very differently.

### Tools

Direct primitives Claude uses in the current session:

```
Bash    - Run shell commands
Read    - Read file contents
Write   - Create files
Edit    - Modify existing files
Grep    - Search file contents
Glob    - Find files by pattern
```

Tools are synchronous, return raw output, and are general-purpose.

### Agents

Specialized subprocesses for complex tasks:

```
django-expert           - Django development
react-component-architect - React patterns
rails-backend-expert    - Ruby on Rails
performance-optimizer   - Profiling and optimization
```

Agents spawn with their own context, can run in the background, and return summarized results. Claude delegates entire jobs to them.

**When to use agents:**
- Complex multi-step research
- Domain-specific tasks matching an agent's expertise
- Tasks requiring extensive codebase exploration

---

## Hooks vs Skills

Another source of confusion. Both automate things, but differently.

**Hooks** are reactive—they run automatically in response to events:

```json
{
  "hooks": {
    "postToolUse": [
      {
        "tool": "Write",
        "command": "npm run lint --fix $CLAUDE_FILE_PATH"
      }
    ]
  }
}
```

This runs the linter automatically after Claude writes a file.

**Skills** are proactive—you invoke them explicitly with `/commit` or `/review-pr`.

Hooks = automatic responses to events.
Skills = manual workflows you trigger.

---

## Plan Mode

Sometimes you want Claude to think before acting.

**Regular Mode:**
- Full access to all tools
- Can read, write, edit files
- Executes commands freely

**Plan Mode:**
- Research and exploration only
- No file modifications allowed
- Must get approval before exiting

Enter with `EnterPlanMode`, exit with `ExitPlanMode` (requires your approval).

Useful when you want Claude to fully understand a problem before touching any code.

---

## Configuration Files

Two places to configure Claude Code:

### CLAUDE.md

Lives in your project root. Contains instructions in plain markdown:

```markdown
# CLAUDE.md

## Build Commands
- `npm run dev` - Start dev server
- `npm run test` - Run tests

## Code Style
- Use TypeScript strict mode
- Prefer functional components
- Always add tests for new features
```

This is project-specific context. Write it like you're onboarding a new developer. It gets committed with your repo.

### Settings

Lives at `~/.claude/settings.json`. Configures the machinery:

```json
{
  "mcpServers": { ... },
  "hooks": { ... },
  "permissions": { ... }
}
```

Settings are typically global and not version-controlled.

---

## Background Tasks

By default, tasks block until complete. For long-running operations:

```
run_in_background: true
```

The task continues while your conversation proceeds. Check status with the `TaskOutput` tool.

---

## WebFetch vs WebSearch

**WebFetch** — Get content from a specific URL:
> "Fetch the docs at https://docs.example.com/api"

**WebSearch** — Find pages about a topic:
> "Search for React 19 new features"

WebFetch has a 15-minute cache. WebSearch doesn't cache.

---

## Quick Reference

**Adding capabilities:**
- MCP Server → new tools from external processes

**Standardizing workflows:**
- Skill → guided process via `/command`

**Automating responses:**
- Hook → shell command on events

**Delegating complex work:**
- Agent → specialized subprocess via Task tool

**Direct actions:**
- Tool → primitives like Read, Write, Bash

**Planning before doing:**
- Plan Mode → research-only, no edits

**Project context:**
- CLAUDE.md → instructions in your repo

**Global config:**
- Settings → JSON at `~/.claude/settings.json`

---

## The Mental Model

After sorting through all this, here's how I think about it:

- **Tools** are Claude's hands—direct actions like reading files or running commands
- **Agents** are specialists Claude delegates entire jobs to
- **MCP servers** extend what Claude can do
- **Skills** standardize how Claude does common tasks
- **Hooks** automate responses to events
- **Settings** configure the machinery
- **CLAUDE.md** gives Claude project context

Once these concepts clicked, working with Claude Code became much more intuitive. Hopefully this reference saves you the same confusion I went through.
