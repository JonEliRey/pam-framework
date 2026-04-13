---
name: pam-skill-evaluator
description: Evaluates a newly created PAM skill for compliance with PAM framework standards. Runs automatically after skill creation.
---

# PAM Skill Evaluator

USE WHEN: evaluate pam skill, check skill compliance, verify skill, pam skill evaluation

## Evaluation Checklist

Given a skill file path, run each check and report pass/fail:

### Check 1 — File exists at the expected path
- Read the file. If it doesn't exist, FAIL with the expected path.

### Check 2 — YAML frontmatter parses correctly
- The file must start with `---` and contain valid YAML frontmatter
- Required fields: `name`, `description`
- FAIL if either field is missing or empty

### Check 3 — Description is discoverable
- The `description` field must be a meaningful sentence (not just the skill name repeated)
- It should contain keywords that Claude Code's skill discovery would match
- WARN if description is under 10 words

### Check 4 — USE WHEN line present
- Search for a line starting with `USE WHEN:` in the body
- FAIL if missing — the skill won't be discoverable by trigger phrases

### Check 5 — Workflow sections exist
- Search for at least one `## Workflow` or `### Step` or numbered list (1. 2. 3.)
- WARN if no structured workflow found — skill may be too prose-heavy

### Check 6 — "Why this matters" callout present
- Search for "Why this matters" (case-insensitive) near the end of the file
- WARN if missing — PAM convention for every skill

### Check 7 — Correct filesystem location
- If the skill is a root skill: verify it's at `.claude/skills/<cluster>/SKILL.md`
- If a sub-skill: verify it's under a root skill directory
- If standalone: verify it's at `.claude/skills/<name>/SKILL.md`
- WARN if location doesn't match the expected pattern for the skill type

### Check 8 — No leftover template markers
- Search for `{{` and `}}` in the file body
- FAIL if found — template placeholders were not replaced

## Report Format

```
PAM Skill Evaluation: {{SKILL_NAME}}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ File exists
✅ Frontmatter valid (name: X, description: Y)
✅ Description discoverable (N words)
✅ USE WHEN present
✅ Workflow sections found
✅ "Why this matters" present
✅ Correct location for [type]
✅ No template markers remaining

Result: PASS (8/8) | WARN (list) | FAIL (list)
```

If any check FAILs, present the specific issue and suggest a fix.
