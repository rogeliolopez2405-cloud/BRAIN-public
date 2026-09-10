# Validation results (frozen)

> **These results were produced internally.** No party outside the project has independently
> reproduced them. Internal validation is not independent validation, and neither is production
> readiness. Read this section before reading the numbers.

Recorded from the private repository at the architecture freeze:

| | |
|---|---|
| Frozen architecture HEAD | `1c549cc6dce117f9d567bc155fe7fe58f56dd409` |
| Frozen validation package commit | `5116f351387cafe618c47b7ee74d386470a7f105` |
| Classification | `GENERAL_ARCHITECTURE_CANDIDATE` — derived and asserted, agreeing |
| Architecture gate | 9 of 9 criteria established |
| Full suite | **2,598 passing, 0 failing**, across 704 suites |
| Runtime | Node.js 24.17.0, win32 |

## The nine gate criteria

Each criterion is established by a recorded claim carrying its own written evidence, and is
traced to the modules that implement it and the tests that exercise it. Counts below are the
passing tests in each criterion's own test files, executed at the freeze.

| Criterion | Tests | Result |
|---|---|---|
| `durable_identity` | 50 | pass, 0 fail |
| `one_time_authority` | 11 | pass, 0 fail |
| `concurrent_write_integrity` | 8 | pass, 0 fail |
| `knowledge_reuse` | 23 | pass, 0 fail |
| `continuity_across_model_replacement` | 20 | pass, 0 fail |
| `crash_recovery` | 56 | pass, 0 fail |
| `partition_rejoin` | 30 | pass, 0 fail |
| `concurrent_knowledge_continuity` | 24 | pass, 0 fail |
| `lane_enforcement` | 62 | pass, 0 fail |

Criterion test counts overlap the full-suite total rather than summing to it — several files
cover more than one criterion, and the suite contains a great deal that no criterion claims.

## What each criterion asserts

- **durable_identity** — a record's name survives concurrent writers, hostile clocks, restarts
  and independent machines, and is never derived from record count or position.
- **one_time_authority** — a grant is consumed exactly once within a world, proven from 2 to 64
  concurrent consumers.
- **concurrent_write_integrity** — 16 concurrent OS processes writing one world: zero lost
  records, zero corrupt lines, no duplicate identities.
- **knowledge_reuse** — an established fact answers without re-establishing it, and reuse is
  refused when the answer is superseded, conflicted or no longer established.
- **continuity_across_model_replacement** — a process with no prior memory recovers the world,
  the classification and the open blockers from durable artifacts alone.
- **crash_recovery** — an action interrupted mid-flight is recoverable: SENT stays uncertain,
  EXECUTED never reads as VERIFIED, and a verified action survives a lost acknowledgement.
- **partition_rejoin** — two diverged worlds merge without arrival order deciding truth;
  order-invariance is structural, and conflicts are preserved rather than resolved.
- **concurrent_knowledge_continuity** — reuse revalidates against a dependency-scoped basis
  rather than being trusted after an earlier check, proven against a real cross-process race.
- **lane_enforcement** — outbound data crosses one gate that decides before any connection
  opens; a denial is zero bytes at a real socket, measured at a server outside the module under
  test.

## How the classification is derived

Two answers are computed independently and compared:

- **DERIVED** — what the nine criteria currently support, recomputed from records on every call.
- **ASSERTED** — what an authority recorded. The authority that can name the system is
  deliberately scoped so that it *cannot* reach the gate criteria: the owner decides what BRAIN
  is called, and does not decide whether a criterion is met.

When the two disagree, the disagreement is reported and not resolved automatically. That state
actually occurred during development — the gate reached 9 of 9 while the recorded classification
still said something lower — and both records remain in the private log.

Withdrawing a criterion's evidence causes the derived level to fall on its own. It is not
remembered.

## Adversarial testing

Fourteen attacks were run against the frozen package. Ten are required to hold; **four are
expected to succeed** and demonstrate the documented boundaries in
[LIMITATIONS.md](LIMITATIONS.md) rather than defects:

| Attack | Expectation | Frozen result |
|---|---|---|
| `identity-survival` | must hold | held |
| `crash-recovery` | must hold | held |
| `sent-unknown` | must hold | held |
| `resumed-context` | must hold | held |
| `partition-conflict` | must hold | held |
| `same-root` | must hold | held |
| `concurrent-correction` | must hold | held |
| `lane-zero-byte` | must hold | held |
| `transitive-deny` | must hold | held |
| `classification-derivation` | must hold | held |
| `mislabelled` | **expected to get through** | got through, as documented |
| `malicious-adapter` | **expected to get through** | got through, as documented |
| `corroboration-loss` | **expected to get through** | got through, as documented |
| `split-grant` | **expected to get through** | got through, as documented |

Mutation testing was used throughout development to confirm the tests fail when the code is
broken — for the final rounds, 24 of 24 and 7 of 7 planted defects were caught.

## Scale properties

Measured on the machine above. These describe **shape** — whether a cost grows with the world,
which is architectural — not tuned performance.

| Property | Result |
|---|---|
| Identity minting | 1.00× cost across a 1000× larger world |
| Lane decision | 0.37× across a 1000× world, considering **0** records; a naive scan for the same policy grew 443,956× |
| Knowledge reuse | dependency set stays at **1 record** at a world of 1,000,000 |
| Rejoin | cost follows what is *offered*, not what is held |

One honest exception, recorded rather than smoothed: knowledge reuse's median latency does grow
with world size, because a single filter pass finds the one relevant record. Removing that would
require an index — a real store — which is not built.

---

## Why you cannot run these yourself from this repository

The validation and attack tooling is not published. This was measured, not assumed: the attack
pack's transitive import closure is **17 of 54 kernel modules (31% of the kernel)**, and
publishing a runnable version would publish roughly a third of the implementation as a side
effect. The validation tool needs fewer modules but reads the private world records to derive
anything.

The choice was between exposing implementation to make a tool run, publishing a knowingly broken
tool, or publishing the results statically and saying so. This document is the third.

**What that costs you as a reader:** you are taking these numbers on trust. That is a fair thing
to hold against this release, and [PUBLIC-SCOPE.md](PUBLIC-SCOPE.md) says so too. If independent
reproducibility matters to your evaluation, open an issue — reviewer access to the private
repository is granted case by case.
