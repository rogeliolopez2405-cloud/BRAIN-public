> **PUBLIC ARCHITECTURE RELEASE.** This document is part of a controlled export of the
> private BRAIN development repository. The implementation it describes is **not** published
> here — see [PUBLIC-SCOPE.md](../PUBLIC-SCOPE.md). Paths beginning `brain/kernel/` or
> `tests/` refer to files in the private repository and are given so the description can be
> checked against something specific, not because they are included.
>
> No open-source license has been granted. See [COPYRIGHT.md](../COPYRIGHT.md).

# The loop

```
                                    ┌───────────────────────────────────────┐
                                    │                                       │
                                    ▼                                       │
                              ┌───────────┐                                 │
                              │  REALITY  │  the world, which does not      │
                              └─────┬─────┘  care what anyone believes      │
                                    │                                       │
                                    ▼                                       │
                              ┌───────────┐                                 │
                              │ EVIDENCE  │  someone or something LOOKED,   │
                              └─────┬─────┘  and said when                  │
                                    │                                       │
                                    ▼                                       │
                        ┌───────────────────────┐                           │
                        │  WORLD / CONTINUITY   │  append-only, superseded  │
                        │  claims · provenance  │  never edited, survives   │
                        │  instance · journal   │  the process that wrote   │
                        └───────────┬───────────┘  it                       │
                                    │                                       │
                                    ▼                                       │
                        ┌───────────────────────┐                           │
                        │  KNOWLEDGE / BELIEF   │  KNOWN · CONFLICTING ·    │
                        │  resolved, not stored │  ASSUMED · UNKNOWN        │
                        └───────────┬───────────┘  all four are answers     │
                                    │                                       │
                                    ▼                                       │
                        ┌───────────────────────┐                           │
                        │     INTELLIGENCE      │  PROPOSES. Nothing it     │
                        │   (model, replaceable)│  says is evidence.        │
                        └───────────┬───────────┘                           │
                                    │                                       │
                                    ▼                                       │
                        ┌───────────────────────┐                           │
                        │       AUTHORITY       │  DECIDES. Domain-scoped.  │
                        │  may this happen?     │  Never global.            │
                        └───────────┬───────────┘                           │
                                    │                                       │
                                    ▼                                       │
                        ┌───────────────────────┐                           │
                        │   CAPABILITY / LANE   │  CAN != MAY.              │
                        │  can it? may it leave?│  Denial = zero bytes.     │
                        └───────────┬───────────┘                           │
                                    │                                       │
                                    ▼                                       │
                        ┌───────────────────────┐                           │
                        │        ACTION         │  journalled BEFORE it     │
                        │  (tool, replaceable)  │  happens                  │
                        └───────────┬───────────┘                           │
                                    │                                       │
                                    ▼                                       │
                        ┌───────────────────────┐                           │
                        │     VERIFICATION      │  observed by a route that │
                        │  did it actually      │  did not perform it       │
                        │  happen?              │                           │
                        └───────────┬───────────┘                           │
                                    │                                       │
                                    └───────────────────────────────────────┘
                                         back to REALITY, and around again
```

## What is replaceable, and what is not

```
   REPLACEABLE                                    PERSISTENT
   ─────────────────────────────────────────      ──────────────────────────────
   models          swap one for another           ┌────────────────────────┐
   providers       remove them all                │        BRAIN           │
   tools           different implementations      │                        │
   bodies          CLI, viewer, mobile, agent     │  world · claims        │
   processes       crash them                     │  provenance · journal  │
   machines        move the directory             │  authority · lanes     │
                                                  │  classification        │
   ── none of these is where the world lives ──   └────────────────────────┘
```

**Losing any box on the left loses capability. None of them loses world.**

- Swap the model → the world is unchanged; the new one reads it from disk.
- Remove every provider → BRAIN can observe less and knows exactly the same things it knew.
  It cannot *learn* new external facts, and it says so rather than guessing.
- Kill the process mid-action → the journal was written first; recovery reads an intention.
- Move the directory to another machine → the world keeps its `instance_id`.

The kernel is structurally incapable of calling a model or a provider. That is asserted against
the import graph, not against anyone's intention.

## Where the arrows must not be short-circuited

Each of these is a real failure that has a test:

| Short-circuit | What it looks like | What it produces |
|---|---|---|
| INTELLIGENCE → WORLD | a model's suggestion recorded as a fact | confident fiction, indistinguishable from evidence |
| EVIDENCE → AUTHORITY | "the gate passed, so promote" | a system that grades itself |
| AUTHORITY → LANE | "the owner said do it, so it may leave" | exfiltration on a correctly granted permission |
| ACTION → REALITY | "it returned 200, so it happened" | a success claim nobody observed |
| WORLD → BELIEF (stored) | caching the resolved answer | an assertion that outlives its evidence |

The last one is why belief is **resolved on every read** rather than stored.

---

Laws: `policy/laws.json` — one canonical source.
