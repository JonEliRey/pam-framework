---
name: pam-agent-creator
description: Creates PAM-compliant agents with correct cluster namespace, memory scope, skill preloading, and domain declaration. Walks users through every decision about the agent's professional identity and evaluates the result.
---

# PAM Agent Creator

USE WHEN: create pam agent, new pam agent, pam agent creator, build agent for pam, create agent following pam

## Overview

This skill creates PAM-compliant agents by walking you through the professional identity, domain scope, memory configuration, and skill preloading decisions. It generates the agent file in the correct cluster-namespaced location and evaluates the result.

**Before starting:** If Claude Code sub-agents documentation hasn't been checked in the last 7 days, fetch `code.claude.com/docs/en/sub-agents` and note any changes to supported frontmatter fields (currently 15: name, description, model, effort, maxTurns, tools, disallowedTools, skills, memory, background, isolation, hooks, mcpServers, permissionMode, plus any new ones). Present changes to the user.

## Interactive Workflow

### Step 1 — Professional Identity

Ask the user:

1. **What is this agent's name?** (becomes `name` field — must be globally unique per PAM convention)
2. **What domain does this agent specialize in?** (one sentence — becomes `description` and informs domain scope)
3. **What cluster does this agent belong to?** (determines filesystem location: `.claude/agents/<cluster>/<name>.md`)
4. **Is this a domain agent (full PAM governance) or a simple persona agent (passenger)?**
   - Domain agent: gets hooks, scoped MCP, permission modes, SCP discipline — deploys to native location
   - Simple persona: no governance requirements — can stay in a plugin

### Step 2 — Memory Configuration

Ask the user:

5. **What memory scope should this agent have?** Present the three options:
   - `user` — global memory, persists across all workspaces (for system-wide agents like the Chief of Staff)
   - `project` — workspace-scoped memory (for project-specific domain agents)
   - `local` — project-only memory (for tightly scoped agents)
6. **What should this agent remember across sessions?** (informs what gets written to the agent's MEMORY.md)

### Step 3 — Skill Preloading

Ask the user:

7. **Which skills should this agent preload (eager-load at startup)?** These become part of the agent's persona.
   - The cluster's root context skill (usually required for domain agents)
   - Domain-specific workflow skills
   - Any skills that define the agent's core behavior
8. **Are there skills this agent should discover progressively (not preloaded)?** These are available but not loaded until invoked.

### Step 4 — Capabilities and Constraints

Ask the user:

9. **What tools does this agent need?** (becomes `tools` field)
10. **What tools should this agent NOT have?** (becomes `disallowedTools` field)
11. **What model should this agent use?** (becomes `model` field — default to the best available for domain agents, lighter models for simple tasks)
12. **Should this agent run in isolation?** (for unattended work: `isolation: worktree`)

### Step 5 — Generate the Agent

Generate the agent file with:
- All 15 documented frontmatter fields populated (or omitted with reason)
- PAM custom metadata fields (cluster, domain_scope — as comments since not runtime-parsed)
- A body that defines the agent's professional identity, responsibilities, and behavioral guidelines
- Reference to the cluster root context skill in `skills:` frontmatter
- Memory scope declaration

Write to: `.claude/agents/<cluster>/<name>.md`

### Step 6 — Evaluate

Verify:
- Agent file parses correctly
- All required frontmatter fields present
- `name` is globally unique (grep all `.claude/agents/` for collision)
- `skills:` references resolve to existing skill files
- `memory:` scope is a valid value (user/project/local)
- Cluster directory exists (or create it)
- No template placeholders remaining

### Step 7 — Report

Present: file created, cluster, memory scope, preloaded skills, evaluation results, next steps (register in settings.json if needed, add to cluster's domain.yaml).

## PAM Standards Enforced

1. **Cluster-namespaced filesystem location** (`.claude/agents/<cluster>/<name>.md`)
2. **Globally unique `name`** field (checked against all existing agents)
3. **Memory scope declared** (`memory: user|project|local`)
4. **Cluster root context skill preloaded** (for domain agents)
5. **Professional identity in body** (not just frontmatter — the body IS the system prompt)
6. **Domain agent vs persona agent explicitly declared** (affects deployment and governance)
