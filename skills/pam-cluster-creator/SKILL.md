---
name: pam-cluster-creator
description: Creates PAM-compliant clusters with root context skill, domain.yaml, registry directory, agent namespace, and setup skill. Walks users through organizing a team of agents around a shared domain.
---

# PAM Cluster Creator

USE WHEN: create pam cluster, new pam cluster, pam cluster creator, build cluster for pam, organize agents into cluster, create team of agents

## Overview

This skill creates a complete PAM-compliant cluster — the organizational unit that groups agents sharing a domain. It generates the root context skill, domain.yaml, registry directory structure, agent namespace directory, and optionally a setup skill. It walks you through every decision about the cluster's domain, agents, and shared context.

**Before starting:** If Claude Code documentation hasn't been checked in the last 7 days, fetch relevant docs (skills, sub-agents, plugins) and note any changes. Present changes to the user.

## Interactive Workflow

### Step 1 — Cluster Identity

Ask the user:

1. **What domain does this cluster cover?** (e.g., "source control and CI/CD", "content creation", "security operations")
2. **What should this cluster be called?** (lowercase-hyphenated — becomes the directory name and namespace prefix)
3. **What agents will belong to this cluster?** (list names and roles — these get created via pam-agent-creator)

### Step 2 — Shared Context

Ask the user:

4. **What directives should ALL agents in this cluster share?** Examples:
   - Environment variables and credential locations (e.g., "GitHub PAT: `$GH_PAT_OPS`")
   - Tool conventions (e.g., "Always use `--jq` with `gh` CLI when scripting")
   - Domain rules (e.g., "Never modify files outside `.github/`")
   - Standing instructions (e.g., "Rate-limit budget: conserve below 200 remaining quota")
5. **Are there cross-cluster interfaces?** (other clusters this one communicates with, and through which skills)

### Step 3 — Domain Boundaries

Ask the user:

6. **What file/path patterns does this cluster OWN?** (becomes `domain.yaml` includes)
7. **What is explicitly OUT OF SCOPE?** (becomes `domain.yaml` excludes and non_goals)
8. **What are the cluster's responsibilities in plain language?** (becomes `domain.yaml` responsibilities)

### Step 4 — Generate the Cluster

Create the following directory structure and files:

```
.claude/skills/<cluster-name>/
  SKILL.md                          ← Root context skill (shared directives from Step 2)
  setup/
    SKILL.md                        ← Setup skill (optional, for plugin deployment)

.claude/agents/<cluster-name>/
  (empty — agents created separately via pam-agent-creator)

~/.claude/registries/<cluster-name>/
  knowledge.jsonl                   ← Empty, initialized
  errors.jsonl                      ← Empty, initialized
  scps.jsonl                        ← Empty, initialized
  events.jsonl                      ← Empty, initialized

<project-root>/domain.yaml          ← Or ~/.claude/domains/<cluster-name>.yaml
```

**Root context skill** (SKILL.md): populated with the shared directives from Step 2, structured as a preloadable skill that every agent in the cluster lists in their `skills:` frontmatter.

**domain.yaml**: populated with the domain boundaries from Step 3, machine-readable for PreToolUse hooks and the PAM linter.

**Registry files**: empty JSONL files, initialized with a schema_version header comment.

### Step 5 — Evaluate

Verify:
- Root context skill parses correctly with valid frontmatter
- domain.yaml parses as valid YAML with required fields (cluster_name, domain.includes, domain.excludes, responsibilities, non_goals)
- Registry directory exists with all expected files
- Agent namespace directory exists
- Setup skill (if generated) parses correctly
- No template placeholders remaining

### Step 6 — Next Steps

Present to the user:
- What was created (full file tree)
- Evaluation results
- **"Now create your agents using `/pam-agent-creator` — each agent should declare this cluster's root skill in its `skills:` frontmatter to inherit the shared context."**
- **"After creating agents, use `/pam-hook-creator` to create any domain-enforcement hooks for this cluster."**

## PAM Standards Enforced

1. **Root context skill** with shared directives (Layered Context principle — §3.11)
2. **domain.yaml** with machine-readable boundaries (Architect precision — §3.3)
3. **Registry directory** initialized with schema-versioned JSONL files (§3.4)
4. **Agent namespace directory** ready for cluster-namespaced agents (§3.1)
5. **Setup skill** for plugin deployment (§3.9, optional)
6. **Every decision documented** — the creation process IS the education process
