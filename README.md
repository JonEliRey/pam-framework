# Professional Agent Model (PAM)

**A framework for building AI agent systems as professionals, not tools.**

By [Jonathan Reyes](https://ethion.io) / Ethion Consulting

---

## What Is PAM?

PAM is a blueprint for building AI agent systems. Not a ready-made system. A design philosophy and set of tools for creating your own, purpose-built for you.

PAM assumes you already have a brain (Claude, ChatGPT, Gemini, Mistral, or any LLM). What it provides is everything else: the harness you build around that brain to make it useful, reliable, and yours.

**Read the article:** [The Professional Agent Model: A Blueprint for Building Your Own AI System](./PAM-article.md)

**Read the full specification:** [PAM v0.5.2](./PAM-framework.md) (25,900 words)

---

## Quick Start

### Install as a Claude Code Plugin

Clone this repo into your project or a shared location:

```bash
git clone https://github.com/JonEliRey/pam-framework.git
```

Claude Code will automatically discover the plugin and its skills. You'll have access to:

- `/pam-skill-creator`: create PAM-compliant skills (wraps `/skill-creator` with PAM standards)
- `/pam-hook-creator`: create PAM-compliant hooks (wraps `/hookify` with criticality tiers)
- `/pam-agent-creator`: create PAM-compliant agents with memory, cluster namespace, and skill preloading
- `/pam-cluster-creator`: create complete PAM clusters with root context, domain.yaml, and registries

### Build Your First Cluster

```
/pam-cluster-creator
```

The skill walks you through every decision: domain, shared context, agents, boundaries. When done, you have a working PAM cluster with all the pieces in place.

---

## What's in This Repo

```
pam-framework/
  README.md                         ← you are here
  PAM-framework.md                  ← full specification (25,900 words)
  PAM-article.md                    ← companion article (3,500 words)
  .claude-plugin/
    plugin.json                     ← makes this a Claude Code plugin
  skills/
    pam-skill-creator/              ← skill creation with PAM standards
    pam-hook-creator/               ← hook creation with criticality tiers
    pam-agent-creator/              ← agent creation with memory + namespace
    pam-cluster-creator/            ← cluster creation with full structure
```

---

## Core Ideas

1. **Agents are professionals, not tools.** Think about potential, not fixed capabilities.
2. **The harness is everything around the brain.** Skills, hooks, memory, context, guard rails.
3. **Clusters organize teams.** Shared context, bounded domains, no information bloat.
4. **Memory makes it real.** Without memory, every session starts from zero.
5. **Four executive seats govern the system.** Chief of Staff (user advocacy), COO (operations), CTO (innovation), CISO (security).
6. **Start simple, grow on evidence.** Don't build the full stack on day one.

---

## License

MIT

---

## Author

Jonathan Reyes · [Ethion Consulting](https://ethion.io)

*PAM is a living framework. This document is the starting point, not the destination.*
