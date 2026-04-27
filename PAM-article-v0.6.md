# The Professional Agent Model: What Real Adoption Changed

**By Jonathan Reyes** | Ethion Consulting
**Companion specification:** [PAM v0.6 Full Specification](./PAM-framework.md)

---

## A Quick Handshake

When I published the Professional Agent Model a few weeks ago, the central claim was this: AI agents are not tools. They are professionals. And the way you build them (the memory, the constraints, the domain ownership, the organizational structure around them) determines whether you get a capable professional or a capable mess.

That argument has not changed. What has changed is what I understand about building one when it meets real conditions.

This article covers three things: what real adoption taught us, what changed in v0.6 as a result, and where the framework goes next. If you read the first article, think of this as the follow-up. If you are new here, this one stands on its own, though I recommend going back to the first when you have time.

---

## What Happens When Someone Actually Builds It

Within days of publishing, a team in the security industry was already implementing PAM. Not as a side project. As a live system for a real organization with real compliance requirements. They were not asking for permission or guidance. They were building.

That was the signal I had been hoping for, and it arrived faster than I expected.

What you learn when someone else builds something you designed is different from what you learn building it yourself. You learn where the blueprint assumed context it never stated. You learn where the prose was clear enough to understand but not clear enough to implement. And you learn where the design held up under conditions you did not anticipate, and where it did not.

Three things surfaced from that adoption. They shaped v0.6.

---

## What v0.6 Addresses

### The Trust Boundary Is Not Optional

Before I describe what changed, I want to be explicit about something. **I am not a security professional.** The security model that underlies PAM was not invented by me. It was adopted from work that has been foundational in this space for years, work generously released into the open by someone who has been thinking clearly about where AI systems are headed long before most of the industry caught up.

Specifically, PAM's security model builds directly on **PAI (Personal AI Infrastructure)** by **Daniel Miessler** ([github.com/danielmiessler/PAI](https://github.com/danielmiessler/PAI)). The Trust Hierarchy concept, the `patterns.yaml` single-source-of-truth pattern, the protection-level taxonomy (`blocked / confirm / alert / zeroAccess / readOnly / confirmWrite / noDelete`), and the prompt-injection defense protocol are all PAI originals. Where PAM's security guidance earns trust, PAI is the reason.

Daniel has been a visionary in understanding where AI systems are going, practically and conceptually, long before these topics became mainstream conversation. His contributions have been invaluable to me, and to many others building seriously with these systems. v0.6 is the first version of PAM that formally names PAI as the foundation of its security model, both in the Foreword and inside the security architecture itself. That credit was overdue. It is now part of the spec.

With that named: the first gap adoption surfaced was the boundary between an agent that consumed external content and an internal agent that is about to act on the result.

Every practical agent system processes external content. Email. Web pages. User-pasted text. Third-party API responses. That content can contain instructions that look like agent commands. It can claim to be from a trusted source when it is not. It can redirect agent behavior in ways the human principal never authorized. This is not a theoretical threat. It is a documented, reproducible attack class.

To borrow the framing from the framework itself: **PAI secured the harness. PAM names the inter-agent boundary mechanism that a multi-agent system built on that foundation still needs.** When a reviewer agent hands a verdict to an acting agent across a trust boundary, how do you guarantee that verdict was not tampered with in transit? That is the question v0.6 answers.

v0.6 introduces a dedicated Hardening Tier (§3.13) that addresses this boundary explicitly. It defines four named attack surfaces (command injection, identity injection, definition-level tampering, and transit tampering) and provides five reference implementations for protecting against them. They range from the minimal, a structured-schema-only reviewer that strips free-text from the trust path, to the strongest, nonce-signed verdicts where the signing happens outside the LLM conversation context entirely. You do not use all five. You choose based on your threat model. That is the point: the spec now gives you the vocabulary and the options instead of leaving the design to you.

One framing worth being clear about: the Hardening Tier applies at trust boundaries, not everywhere. Internal agents calling other internal agents in the same trust zone do not need this overhead. Protection is proportional to blast radius. Where untrusted content crosses into trusted processing, you harden. Everywhere else, you build normally.

To repeat the credit because it matters: I did not invent the security philosophy underneath the Hardening Tier. Daniel did. What PAM adds is the specific mechanism a multi-agent framework needs on top of that philosophy: the named handoff between agents at a trust boundary, where one agent's output must be trusted by another agent's input. That addition is mine. The foundation is his.

### Intent Is More Powerful Than Prescription

The second gap was subtler. It showed up in how people implemented skills and configured agents.

When you describe a system thoroughly enough, there is a risk the reader treats the description as a prescription. They do not ask what outcome they are trying to achieve. They implement what the document describes. The result is a system that looks like PAM but behaves like a template: all the structure, none of the adaptability.

v0.6 adds three principles to the Core Philosophy that address this directly.

**Intent over Prescription** is the most important. It borrows from Richard Sutton's *Bitter Lesson*, the observation that approaches built on general methods and computation consistently outperform approaches that encode human knowledge into systems. Applied to agent design: specify the outcome, not the steps. When you tell an agent what you want to achieve, it finds a path. When you tell it exactly what steps to follow, you have pre-emptively constrained its effectiveness to what you could imagine when you wrote the instruction. Specify the result. Let the professional figure out how.

**Adaptive, not Constraining** follows from that. The framework should evolve with you, not hold you to decisions you made on day one. If your system is getting more rigid over time, if the harness is becoming a cage, something is wrong. A PAM system should get better at serving you as it learns what you actually need, not more brittle as it accumulates configuration.

**Trust is a runtime property, not a privilege state.** This one matters most for security. Trust is not a label you assign at configuration time and never revisit. It is earned, continuously, at the boundary where content crosses from external to internal. The Hardening Tier implements this principle mechanically. The principle names why the implementation is designed the way it is.

### The Onboarding Prompt Is the Entry Point

The third thing adoption taught us is that the bootstrap matters more than the specification.

Nobody reads a framework document from start to finish before building. They look for the entry point. They ask: where do I start? What is the first thing I build? How do I know if I am doing it right?

For v0.6, we updated `PAM-ONBOARD.md`, the portable bootstrap prompt you paste into any frontier AI, to be that entry point without caveats. The stale version references are gone. The document now cleanly represents where the framework actually is. You paste it in, and the AI interviews you about your context, your role, and what you need, then produces a tailored starting point for your PAM implementation.

This is the test I care most about: can someone with no existing harness, given the framework document and the onboarding prompt, build a working PAM-compliant system? v0.6 moves that test closer to passing.

---

## Where We Are Taking This

PAM was built on Claude Code because Claude Code currently provides the richest set of native primitives for the kind of agent architecture PAM describes: hooks with blocking semantics, persistent memory, progressive and eager skill loading, a strong plugin model. That decision was deliberate, and it remains correct for the reference implementation.

But the principles do not belong to any one platform.

The next question we are working on is this: how does PAM adapt when you are building somewhere else? When your organization runs on a different AI provider, or on an open-source agent framework, what does a PAM implementation look like? Where are the gaps between what PAM requires and what that platform provides? And, the question I find most interesting, where does a different platform do something *better* than the reference implementation?

We are exploring a protocol for that adaptation. It would research the target platform's actual capability surface, map PAM's primitives against what exists, and surface both the gaps and the opportunities. Where the platform falls short, you get a workaround or a maturity note. Where it exceeds the reference, you get a recommendation to exploit that advantage. The framework should not assume its own implementation is the ceiling.

More on that in subsequent articles as the work develops.

---

## The Feedback Loop Is Now Named

One thing I formalized in v0.6 deserves explicit mention: the principle that **PAM improves through adopter conversations, not designer speculation.**

This is not new behavior; the early adoption above already reflects it. But naming it as a principle changes how the framework is supposed to work. The specification is not a finished artifact. It is an evolving document shaped by what people actually run into when they build. The adopter is not just a user. They are a contributor to the next version.

If you are building with PAM, I want to hear what breaks. What the spec left underspecified. Where the design assumed context you did not have. Where something worked better than you expected. That feedback is what shapes the next version, and right now, the loop is still small enough that every signal matters.

---

## Where to Start

If you have not read the original article, start there. It covers the foundational argument, why the agent-as-professional model produces better outcomes than the agent-as-tool model, in a way I will not repeat here.

If you are ready to build, the full v0.6 specification is at [PAM-framework.md](./PAM-framework.md) in this repository. The onboarding prompt is at `PAM-ONBOARD.md`. Paste it into any capable AI and begin.

And if you build something, especially if you run into something the spec did not anticipate, I want to know about it.

---

*PAM is a living framework. This document is the starting point, not the destination.*
