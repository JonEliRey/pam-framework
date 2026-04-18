# CLAUDE.md — PAM Framework Repo

This file provides guidance to Claude Code when working in this repository.

## What This Is

The **Professional Agent Model (PAM)** framework — a blueprint and set of meta-skills for building AI agent systems as professionals, not tools.

This repo is:
- A **Claude Code plugin** (`.claude-plugin/plugin.json`) auto-discovered by any Claude Code installation that clones it
- The **public spec source** (`PAM-framework.md`) — the full specification, currently at v0.5.2
- The **companion article** (`PAM-article.md`) — Ethion-flavored narrative introduction
- The **creator skills** (`skills/pam-*-creator/`) — the four scaffolding tools (skill, hook, agent, cluster)
- A **specs directory** (`specs/`) for in-progress version planning (v0.6 in progress)

This is NOT an Ethion consulting artifact. It is a standalone framework. Articles about PAM may appear on ethion.io, but the framework itself lives here and is meant to be adopted freely.

## Repo Structure

```
pam-framework/
├── README.md                      ← public overview
├── PAM-framework.md               ← full published spec (v0.5.2)
├── PAM-article.md                 ← companion narrative article
├── CLAUDE.md                      ← this file
├── .claude-plugin/
│   └── plugin.json                ← Claude Code plugin manifest
├── skills/
│   ├── pam-skill-creator/
│   ├── pam-hook-creator/
│   ├── pam-agent-creator/
│   └── pam-cluster-creator/
└── specs/
    └── v0.6/                      ← in-progress v0.6 planning
        ├── PLAN.md                ← v0.6 scope, tracks, status
        ├── DECISIONS.md           ← decisions log (D1–D24 and continuing)
        └── HANDOFF.md             ← session transition pointer
```

## Current State

- **Published version:** v0.5.2 (in `PAM-framework.md` and `.claude-plugin/plugin.json`)
- **In progress:** v0.6 (see `specs/v0.6/PLAN.md`)
- **External adopter:** Frank Cotto (security-industry company) — case study pending

## Version Conventions

- Published versions are committed and tagged. `PAM-framework.md` always reflects the latest published version.
- In-progress work lives in `specs/vX.Y/`. Never edit `PAM-framework.md` directly until a version is being cut.
- Plugin version in `.claude-plugin/plugin.json` matches the published spec version.

## Core Framework Principles (settled, do not re-litigate)

1. **Agents are professionals, not tools** — designed for potential, not fixed capabilities
2. **The harness is everything around the brain** — skills, hooks, memory, context, guardrails
3. **Clusters organize teams** — shared context, bounded domains, no information bloat
4. **Memory makes it real** — without memory, every session starts from zero
5. **Four executive seats govern the system** — Chief of Staff, COO, CTO, CISO
6. **Start simple, grow on evidence** — don't build the full stack on day one
7. **Intent over Prescription** (v0.6) — Sutton's Bitter Lesson applied: specify outcomes, let capability find the path
8. **Adaptive, not Constraining** (v0.6) — the framework evolves with the user, not the reverse
9. **Trust is a runtime property, not a privilege state** (v0.6) — building on PAI's runtime-enforcement model
10. **PAM improves through adopter conversations, not designer speculation** (v0.6)

## Attribution

PAM's security model builds directly on **PAI (Personal AI Infrastructure)** by **Daniel Miessler** (https://github.com/danielmiessler/PAI). The Trust Hierarchy concept, the `patterns.yaml` single-source-of-truth pattern, the protection-level taxonomy (`blocked / confirm / alert / zeroAccess / readOnly / confirmWrite / noDelete`), and the prompt-injection defense protocol are PAI originals. PAM v0.6 extends these with per-cluster inventory, formal Layer 1/2/3 reviewer separation, and tamper-evident verdict handshakes.

Any spec text referencing the runtime-trust model must credit PAI.

## Cross-Repo Context

| Related location | What it is |
|---|---|
| `~/.claude/` | PAI system — security reference implementation, live example |
| `~/.claude/MEMORY/WORK/20260417-172857_pam-v06-framework-evolution/PRD.md` | The active session PRD driving v0.6 design dialogue (Algorithm ISC tracking) |
| `~/dev/ethion-consulting/Articles/pam-framework/` | Article-focused workspace (the PAM launch article lives there, separate from framework spec) |
| `~/dev/ethion-consulting/` | Ethion brand/strategy — PAM articles may appear on ethion.io but are distribution, not framework |

## Development Workflow

When proposing changes to PAM:

1. Read `specs/v0.6/PLAN.md` first to understand current version scope
2. Check `specs/v0.6/DECISIONS.md` — if something has been decided, don't re-open it without naming why
3. Propose changes in the specs directory; never edit the published `PAM-framework.md` directly mid-version
4. When a version is cut, copy the finalized spec from specs to `PAM-framework.md` and update plugin.json

## GitHub

- **Owner:** JonEliRey
- **Repo:** https://github.com/JonEliRey/pam-framework
- **Visibility:** Public
- **License:** MIT
