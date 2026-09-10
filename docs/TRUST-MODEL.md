> **PUBLIC ARCHITECTURE RELEASE.** This document is part of a controlled export of the
> private BRAIN development repository. The implementation it describes is **not** published
> here — see [PUBLIC-SCOPE.md](../PUBLIC-SCOPE.md). Paths beginning `brain/kernel/` or
> `tests/` refer to files in the private repository and are given so the description can be
> checked against something specific, not because they are included.
>
> No open-source license has been granted. See [COPYRIGHT.md](../COPYRIGHT.md).

# Trust model

Stated as data in `TRUST` in `brain/kernel/lane.mjs`, so a test can assert that this document
and the code say the same thing. If they ever disagree, the code is canonical and the test fails.

## Who is trusted with what

| Party | What it is | Enforced against? | Why |
|---|---|---|---|
| **Trusted core** | `brain/kernel/*` — BRAIN's own execution paths | **N/A — it *is* the enforcement.** | The gate is code in this set. Structural tests assert it imports no network module and calls no `fetch`: it decides, it does not act. |
| **Trusted adapter** | Adapter code that uses the provided send path | **Yes** | It asks the gate, and the gate answers before a socket opens. |
| **Buggy adapter** | Trusted-but-wrong: asks the gate and mishandles the answer | **Yes** | `guardedSend` refuses *first*. The send function is never invoked on a denied decision, so a mishandled denial still sends nothing. This is the interesting case, and it holds. |
| **Malicious in-process code** | An adapter that ignores the gate entirely | **NO** | It does not have to ask. It can `import('node:http')` and open its own socket. Run `malicious-adapter` (attack id; tooling not published — see [VALIDATION-RESULTS.md](../VALIDATION-RESULTS.md)) and watch it succeed. |
| **Separate OS process** | Another process on the same machine | **NO** | BRAIN is not the operating system and does not claim to police it. |
| **External provider** | Whatever is on the far end of a permitted call | **NO** | Enforcement governs *whether bytes leave*, never what the recipient does with them afterwards. Sending to a permitted provider is a decision about that provider, not a property the bytes carry. |

## Where enforcement ends, in one sentence

> Application-level policy governs code that asks it. It cannot govern code that does not, and
> no amount of checking inside the same process changes that. The boundary is real and it is
> not the operating system.

— `TRUST.honest_summary`, `brain/kernel/lane.mjs`

## What would move the boundary

Containing hostile in-process code is an operating-system property. Any of these would do it,
and **none is implemented**:

- a separate OS process with no network access — namespace, container, or firewall rule
- a capability-based runtime where the network is not ambiently reachable

Naming them is not a plan to build them. It is the honest answer to "so how *would* you do it".

## Two things people expect that are not here

**Content classification.** Enforcement runs on *declared* provenance: a class carried by the
data and a policy carried by the objective. Nothing reads bytes and decides they are sensitive.
An unlabelled payload is treated as PRIVATE — absence of a label is not permission — but a
payload *mislabelled* PUBLIC is released, correctly, per its label.
`mislabelled` (attack id; tooling not published — see [VALIDATION-RESULTS.md](../VALIDATION-RESULTS.md))

**Multi-tenant isolation.** Lanes separate kinds of work for one owner. They were never asked to
separate two mutually distrusting owners, and a mechanism is only proven against the question it
was asked. There is no tenant model, no per-tenant authority arbitration, and no test.

## What *is* enforced, precisely

Within the trusted set above, at the point where bytes would leave:

- No policy denies. A revoked policy denies. Failure is closed in both directions.
- `LOCAL_ONLY` cannot route remote, and sole availability of a remote provider does not widen it.
- The payload's class must be in the policy's allow-list; the provider must be permitted **and
  observably present**, because a manifest saying "available" is a declaration, not an
  observation.
- A transformation must be *named* by policy. A class cannot separate a hash from a summary when
  both are `DERIVED`, so "it was derived" is not a reason it may leave.
- Derived data inherits the **most restrictive** root. An aggregate of private records is not
  public for being an aggregate.
- The decision is re-taken at send time, not carried from an earlier check.
- Denials are audited with the destination's **origin only** — no payload, no path, no query
  string. Auditing a leak must not be one.

Each line has a test in `tests/laneenforcement.test.mjs` or `tests/laneintegration.test.mjs`,
and a mutation that removes it fails those tests.

---

`SECURITY.md` at the repository root states the same boundary for the repository as a whole.
