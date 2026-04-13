---
name: {{CLUSTER_NAME}}
description: {{DESCRIPTION}} — root skill for the {{CLUSTER_NAME}} cluster. Provides the primary workflow interface and shared context for all agents in this cluster.
---

# {{CLUSTER_NAME}}

USE WHEN: {{TRIGGER_PHRASES}}

## Shared Context

<!-- This section provides cluster-wide directives inherited by every agent that preloads this skill -->

- **Domain:** {{DOMAIN_DESCRIPTION}}
- **Cluster agents:** {{LIST_OF_AGENTS}}
<!-- Add environment references, tool locations, credentials variable names here -->
<!-- Example: GitHub personal access token: `$GH_PAT_OPS` (defined in operator shell env) -->

## Workflows

### {{WORKFLOW_1_NAME}}

<!-- Step-by-step procedure. Be specific enough that the agent follows deterministically. -->

1. {{STEP_1}}
2. {{STEP_2}}
3. {{STEP_3}}

### {{WORKFLOW_2_NAME}}

1. {{STEP_1}}
2. {{STEP_2}}

## Examples

<!-- Concrete demonstrations of correct output that calibrate the agent's behavior -->

### Example: {{EXAMPLE_NAME}}

**Input:** {{EXAMPLE_INPUT}}
**Expected output:** {{EXAMPLE_OUTPUT}}

## Sub-skills

This root skill contains the following sub-skills:

- `{{CLUSTER_NAME}}/{{SUB_SKILL_1}}/SKILL.md` — {{SUB_SKILL_1_DESCRIPTION}}
<!-- Add more sub-skills as needed -->

---

*Why this matters: {{WHY_THIS_MATTERS_ONE_SENTENCE}}*
