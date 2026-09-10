# BRAIN

**Belief · Reality · Authority · Intelligence · Navigation**

One persistent world. Many intelligences.

The AI can be replaced. The BRAIN stays.

> **PUBLIC ARCHITECTURE RELEASE**
> No open-source license has been granted. See [COPYRIGHT.md](COPYRIGHT.md).

---

## What this is

BRAIN is a **provider-independent persistent reality, continuity, evidence, authority and
verification layer** for AI and machine systems.

Intelligences forget. A context window ends, a model is replaced, a process dies — and whatever
was learned goes with it. The usual result is that a human becomes the memory, re-explaining the
same situation every session. BRAIN is the part that does not forget, and it is deliberately not
a model: truth resolution, provenance, freshness and enforcement are structurally incapable of
calling one. Remove every adapter and you lose capability, not world.

Three failures it exists to stop, all of which come from state living in a conversation:

1. An approach that already failed gets retried, because nothing recorded that it failed.
2. A decision gets contradicted, because the reason was only ever in a chat.
3. A fix is claimed without proof, because nothing distinguishes *tried* from *verified*.

## What BRAIN is not

- **Not another LLM.** It contains no model and trains nothing.
- **Not a chatbot.**
- **Not MCP.** It is not a tool-calling protocol or transport.
- **Not an agent framework.** It does not orchestrate agents or own a control loop.
- **Not a robot**, and not embodied.
- **Not a claim of production readiness.** See [LIMITATIONS.md](LIMITATIONS.md).

## The core principle

```
Intelligence proposes.
Evidence establishes.
Authority decides.
Reality verifies.
```

Four separate steps, kept separate on purpose. A model proposing something is not evidence for
it. Evidence establishing what a system *does* is not a decision about what may happen or what
the system is *called*. A decision is not a verification — something has to actually happen and
be observed. Collapsing any two of them is the failure this project is built to prevent.

The full set of architectural laws is published as data in
[`policy/laws.json`](policy/laws.json).

## The architecture loop

```
   Reality
      ↓
   Evidence          someone or something LOOKED, and said when
      ↓
   Continuity        append-only; superseded, never edited
      ↓
   World Model       KNOWN · CONFLICTING · ASSUMED · UNKNOWN — all four are answers
      ↓
   Intelligence      proposes; nothing it says is evidence
      ↓
   Authority         decides; domain-scoped, never global
      ↓
   Action            journalled BEFORE it happens
      ↓
   Verification      observed by a route that did not perform it
      ↓
   Reality
```

Detail: [docs/DIAGRAM.md](docs/DIAGRAM.md) · [docs/SPECIFICATION.md](docs/SPECIFICATION.md)

## Current classification

```
GENERAL_ARCHITECTURE_CANDIDATE
```

Architecture gate: **9 of 9** criteria established.

Read that as: *passed this project's own domain-independent architecture gate, and is ready to
be validated somewhere other than where it was built.* **Candidate** is the operative word.

It does **not** mean production platform, commercially hardened, enterprise ready, multi-tenant
proven, hostile-code sandboxed, universally secure, distributed consensus, or finished.

The classification is derived from recorded evidence rather than asserted — withdraw a
criterion's evidence in the private repository and the derived level falls on its own. See
[VALIDATION-RESULTS.md](VALIDATION-RESULTS.md).

## Known limitations

Five boundaries are currently **unproven** and are published rather than buried:

1. **Content classification** — enforcement runs on declared provenance; BRAIN does not inspect
   bytes to decide they are sensitive.
2. **Hostile in-process containment** — application-level policy cannot contain malicious code
   in the same process. That needs an OS boundary.
3. **Corroboration-strength reuse sensitivity** — a belief that keeps its value while losing one
   of several independent supporting roots still reports as valid.
4. **Prevention of partitioned at-most-once authority** — double consumption across a partition
   is detected after rejoin, not prevented.
5. **Multi-owner / multi-tenant operation** — untested. One owner, one machine, is the whole of
   what was measured.

Each is stated in full, with what would be required to prove it, in
[LIMITATIONS.md](LIMITATIONS.md) and as data in
[`policy/limitations.json`](policy/limitations.json).

## What is in this repository

This is a **controlled export**, not the complete development repository. The implementation,
the test suite and the validation tooling are not published. What is here is enough to
understand, critique and challenge the architecture.

| | |
|---|---|
| [docs/SPECIFICATION.md](docs/SPECIFICATION.md) | each part, split IMPLEMENTED / ESTABLISHED / UNPROVEN / OUT OF SCOPE |
| [docs/DIAGRAM.md](docs/DIAGRAM.md) | the loop, and which parts are replaceable |
| [docs/FAILURE-MODES.md](docs/FAILURE-MODES.md) | 17 failure modes, the mechanism for each, and its remaining limit |
| [docs/TRUST-MODEL.md](docs/TRUST-MODEL.md) | who is trusted with what, and where enforcement ends |
| [LIMITATIONS.md](LIMITATIONS.md) | the five unproven boundaries |
| [VALIDATION-RESULTS.md](VALIDATION-RESULTS.md) | frozen internal validation results, and what they do not mean |
| [`policy/laws.json`](policy/laws.json) | the architectural laws, canonical |
| [`policy/limitations.json`](policy/limitations.json) | the limitations, canonical |
| [PUBLIC-SCOPE.md](PUBLIC-SCOPE.md) | what is included, what is excluded, and why |
| [PROVENANCE.md](PROVENANCE.md) | frozen commits, version, authorship |
| [COPYRIGHT.md](COPYRIGHT.md) | rights status |

## Challenge it

This release exists to invite technical challenge, not agreement. The most useful thing a
reader can do is disagree with something specific:

- Read a claim in [docs/SPECIFICATION.md](docs/SPECIFICATION.md) and argue the evidence does not
  support it.
- Find a failure mode in [docs/FAILURE-MODES.md](docs/FAILURE-MODES.md) whose "remaining limit"
  is understated.
- Show that a limitation in [LIMITATIONS.md](LIMITATIONS.md) is broader than stated — for
  example, that the egress gate can be bypassed by *trusted* code that does ask it.
- Argue that `GENERAL_ARCHITECTURE_CANDIDATE` is too strong for the evidence shown.

Open an issue. Being wrong in public is the point of publishing.

## Authorship

Created and architected by **Rogelio Lopez** —
[github.com/rogeliolopez2405-cloud](https://github.com/rogeliolopez2405-cloud).

See [PROVENANCE.md](PROVENANCE.md) and [CITATION.cff](CITATION.cff).

---

Copyright © 2026 Rogelio Lopez. All rights reserved. Publicly viewable for evaluation,
research discussion, technical review and provenance. No open-source license is granted at this
time — see [COPYRIGHT.md](COPYRIGHT.md).
