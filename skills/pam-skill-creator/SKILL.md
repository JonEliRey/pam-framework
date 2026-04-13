---
name: pam-skill-creator
description: Creates PAM-compliant skills with correct structure, frontmatter, and domain binding. Walks users through decisions about each part of the skill and evaluates the result. Use when creating new skills that follow PAM framework standards.
---

# PAM Skill Creator

USE WHEN: create pam skill, new pam skill, pam skill creator, build skill for pam, create skill following pam, pam-compliant skill

## Overview

This skill is a **PAM-standards wrapper around `/skill-creator`** — the official, vetted Claude Code skill creation tool. It does NOT duplicate `/skill-creator`'s work. Instead, it:

1. Gathers PAM-specific context (cluster membership, skill type, domain binding) BEFORE invoking `/skill-creator`
2. Invokes `/skill-creator` to do the actual skill generation (leveraging its full, tested functionality)
3. Applies PAM-specific post-processing (add "Why this matters" callout, verify cluster placement, ensure PAM frontmatter conventions)
4. Runs the PAM evaluation sub-skill to verify compliance

**Before starting:** If Claude Code skills documentation hasn't been checked in the last 7 days, fetch `code.claude.com/docs/en/skills` and note any changes. Present changes to the user for awareness before proceeding.

## Interactive Workflow

### Step 1 — PAM Context Gathering (before invoking /skill-creator)

Ask the user using the AskUserQuestion tool:

1. **Which cluster does this skill belong to?** (determines filesystem placement and shared context)
2. **What type of skill is this?**
   - **Root skill** — the cluster's primary workflow interface (lives at `.claude/skills/<cluster>/SKILL.md`)
   - **Sub-skill** — a reusable procedure under a root skill (lives at `.claude/skills/<cluster>/<sub-skill>/SKILL.md`)
   - **Standalone** — independent of any cluster (lives at `.claude/skills/<name>/SKILL.md`)
3. **Will this skill be eagerly preloaded into agents via `skills:` frontmatter, or discovered progressively?**
   - Eager = part of the agent's persona, loaded at startup
   - Progressive = discovered by description match, loaded on invocation

### Step 2 — Invoke /skill-creator

Now invoke the official `/skill-creator` skill with the additional context gathered in Step 1. Pass along:
- The cluster context and placement requirements
- The PAM template structure as a reference (from `~/.claude/skills/pam-skill-creator/templates/`)
- Any PAM-specific requirements (e.g., "include a 'Why this matters' callout at the end")

Let `/skill-creator` handle the core generation, evaluation, and iteration — it's the vetted tool for this job.

### Step 3 — PAM Post-Processing

After `/skill-creator` completes, verify and enhance:
- Is the file in the correct PAM cluster location? If not, move it.
- Does it have a "Why this matters" callout? If not, add one.
- Does it reference the cluster root context skill (if part of a cluster)? If not, note this for the user.
- Are PAM convention fields present in frontmatter comments? If not, add them.

### Step 4 — PAM Evaluation

Run the PAM-specific evaluation sub-skill:
- Invoke `~/.claude/skills/pam-skill-creator/evaluation/SKILL.md`
- This verifies PAM-specific compliance: cluster placement, "Why this matters" present, no template markers, connection to cluster context

### Step 5 — Report

Present to the user:
- What was created (file path, type, cluster)
- `/skill-creator` results + PAM evaluation results
- Next steps (e.g., "add this skill to your agent's `skills:` frontmatter to preload it")

## PAM Standards Enforced

Every skill created by this tool must have:

1. **YAML frontmatter** with at minimum: `name`, `description`
2. **USE WHEN line** with trigger phrases
3. **Structured workflow sections** (not just prose — step-by-step procedures)
4. **"Why this matters" callout** at the end (one sentence explaining the skill's value)
5. **allowed-tools** restrictions where appropriate (for predictability)
6. **Connection to cluster root context** (if part of a cluster — reference the shared context skill)
