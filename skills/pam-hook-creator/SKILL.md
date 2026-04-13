---
name: pam-hook-creator
description: Creates PAM-compliant hooks with correct criticality tiers, native exit-code implementation, and event-capability verification. Walks users through hook design decisions and evaluates the result.
---

# PAM Hook Creator

USE WHEN: create pam hook, new pam hook, pam hook creator, build hook for pam, create hook following pam

## Overview

This skill is a **PAM-standards wrapper around `/hookify`** — the official, vetted Claude Code hook creation tool. It does NOT duplicate `/hookify`'s work. Instead, it:

1. Gathers PAM-specific context (criticality tier, event-capability verification, cluster membership) BEFORE invoking `/hookify`
2. Invokes `/hookify` to do the actual hook generation (leveraging its full, tested functionality including rule syntax and pattern matching)
3. Applies PAM-specific post-processing (add `on_failure:` metadata, verify tier/event compatibility, add auto-SCP on Tier 1)
4. Runs PAM-specific evaluation to verify compliance

**Before starting:** If Claude Code hooks documentation hasn't been checked in the last 7 days, fetch `code.claude.com/docs/en/hooks` and note any changes to the 26 hook events or their blocking capabilities. Present changes to the user.

## Interactive Workflow

### Step 1 — Hook Purpose

Ask the user:

1. **What does this hook do?** (one sentence)
2. **Which hook event does it attach to?** Present the 26 Claude Code events grouped by blocking capability:
   - **Hard-block events** (can prevent actions via exit 2): PreToolUse, PermissionRequest, UserPromptSubmit, Stop, SubagentStop, TaskCreated, TaskCompleted, ConfigChange, Elicitation, ElicitationResult, WorktreeCreate, TeammateIdle
   - **Soft-block events** (can provide feedback but not prevent): PostToolUse, PostToolUseFailure
   - **Observational events** (no blocking): SessionStart, SessionEnd, Notification, SubagentStart, PreCompact, PostCompact, StopFailure, PermissionDenied, InstructionsLoaded, CwdChanged, FileChanged, WorktreeRemove
3. **Is this a protective hook (fail-closed) or a continuity hook (fail-open)?**
   - Protective (Tier 1): blocks the operation if the hook fails — only valid on hard-block events
   - Continuity (Tier 2): session continues if the hook fails — valid on any event

### Step 2 — Event-Capability Verification

**CRITICAL CHECK:** If the user chose Tier 1 (fail-closed) on a non-hard-block event, WARN:
> "The event you chose ({{EVENT}}) does not support native blocking via exit code 2. A Tier 1 hook on this event cannot actually prevent the operation. Options: (a) switch to a hard-block event, (b) switch to Tier 2, or (c) use `decision: block` JSON feedback (soft-block only)."

### Step 3 — Hook Implementation

Ask the user:

4. **What language?** (bash, TypeScript/Bun, Python — generates appropriate handler)
5. **What tool patterns should it match?** (for PreToolUse: specific tool names via `tool_name` matcher)
6. **What cluster does this hook belong to?** (for the `on_failure:` PAM metadata)

### Step 4 — Generate the Hook

Generate the hook file with:
- Correct PAM `on_failure:` metadata in comments or frontmatter
- Native exit-code implementation:
  - Tier 1: `exit 2` with stderr reason on blocking condition; structured JSON `{"decision":"block","reason":"..."}` for richer response
  - Tier 2: always `exit 0`; log errors to stderr
- Try/catch error handling appropriate to the tier
- Structured JSON output for events that support it (PreToolUse → `hookSpecificOutput.permissionDecision`)

### Step 5 — Auto-SCP on Tier 1 (if applicable)

If the hook is Tier 1, include the `pam_write_draft_scp()` library call that generates a draft SCP when the hook blocks an operation. The SCP proposes a scope correction or constraint revision.

### Step 6 — Evaluate

Verify:
- Hook file parses correctly
- Event name matches a real Claude Code event
- Tier assignment is compatible with event blocking capability
- Exit codes are correctly implemented for the declared tier
- No template placeholders remaining

### Step 7 — Report

Present: file created, event, tier, evaluation results, how to register in settings.json or agent frontmatter.

## PAM Standards Enforced

1. **`on_failure:` metadata** declared (PAM convention, not runtime-parsed)
2. **Native exit-code implementation** matching the declared tier
3. **Event-capability verified** — Tier 1 only on hard-block events
4. **Auto-SCP on Tier 1 blocks** (self-healing)
5. **Error handling** appropriate to tier (fail-closed or fail-open)
