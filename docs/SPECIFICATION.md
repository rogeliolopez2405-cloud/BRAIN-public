> **PUBLIC ARCHITECTURE RELEASE.** This document is part of a controlled export of the
> private BRAIN development repository. The implementation it describes is **not** published
> here — see [PUBLIC-SCOPE.md](../PUBLIC-SCOPE.md). Paths beginning `brain/kernel/` or
> `tests/` refer to files in the private repository and are given so the description can be
> checked against something specific, not because they are included.
>
> No open-source license has been granted. See [COPYRIGHT.md](../COPYRIGHT.md).

# BRAIN — technical specification

Current implementation terminology. Nothing aspirational is described as if it exists.

Each section is split four ways:

- **IMPLEMENTED** — the code is there and does this.
- **ESTABLISHED** — a gate criterion covers it, with a claim record behind it. `validate` (tooling not published — see [VALIDATION-RESULTS.md](../VALIDATION-RESULTS.md)) names the record.
- **UNPROVEN** — known gap. Not a plan.
- **OUT OF SCOPE** — deliberately not attempted, so it will not be found missing later.

Zero npm dependencies. Node ≥ 18. All logs are append-only JSONL.

---

## Identity

**IMPLEMENTED.** `brain/kernel/recordid.mjs`. A record id is minted as
`PREFIX-<14 digits UTC>-<12 hex>`. It is a *name*: nothing reads a count, a type, an order or a
position out of it. `isRecordId` is deliberately permissive — an unfamiliar id is UNFAMILIAR,
not MALFORMED, because a stricter reader would reject records written by a future version.
Legacy counter-derived ids are recognised as a distinct generation and still resolve.

**ESTABLISHED.** `durable_identity`. Unique without coordination across concurrent writers,
restarts, hostile clocks and independent machines.

**UNPROVEN.** Nothing claimed about ordering by id. Ids are not sortable into world order — use
event time.

**OUT OF SCOPE.** Content-addressed ids, cryptographic signing, global registry.

## World / instance

**IMPLEMENTED.** `brain/kernel/instance.mjs`. A world has a minted `instance_id` stored in
`brain/state/instance.json`, separate from where it happens to live. Directory, host and port
are *observed*, never identity — a world that moves keeps its name, and one that is copied does
not become two worlds with one name by accident.

**ESTABLISHED.** `continuity_across_model_replacement`. A process with no prior memory recovers
the world, the classification and the open blockers from durable artifacts alone.

**UNPROVEN.** Multi-owner. One `instance_id` names one world for one owner.

## Evidence and claims

**IMPLEMENTED.** `brain/kernel/truth.mjs`. A claim is `subject_ref` + `claim_key` +
`claim_value` with an actor, a source, an epistemic state and an optional `authority_ref`.
Resolution is a pure derivation over a supplied corpus: agreement, supersession within an
origin, and conflict between origins are separate outcomes. `KNOWN`, `CONFLICTING`, `ASSUMED`
and `UNKNOWN` are distinct answers and none of them is a value.

Records are superseded, never edited. An outranked claim is preserved as
`non_controlling_conflicting_evidence` and is explicitly *not marked false*.

**ESTABLISHED.** `concurrent_write_integrity`. 16 concurrent writers, zero lost records, zero
corrupt lines, no duplicate identities.

**UNPROVEN.** Corroboration strength — see LIMITATIONS.

## Authority

**IMPLEMENTED.** `brain/authority/authorities.json`. Authority is **domain-scoped**: an entry
speaks for a dotted claim-key prefix and for nothing else. The most specific matching domain
forms the controlling tier; precedence orders *within* that tier only. There is no global
ranking in which one source always outranks another.

Where authorities are registered for a key and no claim cites an applicable one, BRAIN caps its
own confidence at `ASSUMED` rather than `KNOWN` — a source claiming to know is not BRAIN
knowing, and absence of disagreement is not authority.

**ESTABLISHED.** `one_time_authority`. A grant is consumed exactly once within a world, via
exclusive create (`flag: 'wx'`), proven from 2 to 64 concurrent consumers.

**UNPROVEN.** At-most-once across a partition — see LIMITATIONS.

## Continuity and resumption

**IMPLEMENTED.** `brain/kernel/resumption.mjs`. A `contextBasis` records what an intelligence
relied on. `reconcile` reports, per item, whether the world still establishes it —
`SUPERSEDED`, `CONFLICTED`, `KNOWN_BUT_STALE`, `NOT_OBSERVED`, `REVOKED`, `SPENT`. It calls no
model and no provider, and writes nothing: deciding what to do about a superseded assumption is
not its business.

**ESTABLISHED.** Covered by `continuity_across_model_replacement`.

## Knowledge reuse

**IMPLEMENTED.** `brain/kernel/knowledge.mjs`. A `sufficiencyBasis` fingerprints the semantic
outcome — belief, epistemic state, controlling authority — scoped to one subject and key.
`revalidate` re-derives at use time rather than trusting an earlier check. `KNOW`, `CURRENT`,
`MAY` and `CAN` are answered separately, so a revoked grant blocks the action while leaving the
fact intact.

**ESTABLISHED.** `knowledge_reuse` and `concurrent_knowledge_continuity`. Scoped: 5,000
unrelated writes leave a basis untouched, and revalidation considers 1 record at a world of
1,000,000.

**UNPROVEN.** Corroboration strength is not in the fingerprint.

## Action journal and crash recovery

**IMPLEMENTED.** `brain/kernel/actionjournal.mjs`, `commitment.mjs`. Write-ahead: the intention
is journalled before the act, so a dead process leaves a readable intention rather than
silence. `ACTION_STAGE` runs AVAILABLE → AUTHORIZED → REQUESTED → SENT → EXECUTED → RESULT →
VERIFIED. `recoverAction` returns a verdict, including `SENT_UNKNOWN`, `EXECUTED_UNVERIFIED`,
`ALREADY_VERIFIED` and `EFFECT_WITHOUT_RECORD`.

**ESTABLISHED.** `crash_recovery`. At-most-once for a consequential action across real OS
processes, against a far side that counts and is deliberately not idempotent.

**UNPROVEN.** Nothing is claimed about *exactly* once. At-most-once is the guarantee; a
dispatch that fails on the wire leaves the action unperformed and says so.

## Partition and rejoin

**IMPLEMENTED.** `brain/kernel/rejoin.mjs`. `exportBranch` / `mergeBranches` — union keyed by
durable record identity, so order-invariance is structural rather than defended. The merge
decides nothing: a structural test asserts it imports no resolver, and every verdict comes from
the ordinary readers over the merged corpus. Two records under one identity are quarantined
rather than chosen between.

**ESTABLISHED.** `partition_rejoin`.

**UNPROVEN.** A one-time grant consumed on both sides is detected after rejoin, not prevented.

## Lane enforcement

**IMPLEMENTED.** `brain/kernel/lane.mjs`. `egress()` decides before any connection opens;
`guardedSend()` invokes the send only on a permitted decision. Fails closed with no policy and
on a revoked policy. Derived data inherits the most restrictive root. Unlabelled data is
PRIVATE. A transformation must be *named* by policy, because a class cannot separate a hash
from a summary when both are DERIVED. The decision is re-taken at send time. Denials are
audited without the payload, the full URL or the query string.

**ESTABLISHED.** `lane_enforcement`, within the trust model in `TRUST`.

**UNPROVEN.** Content classification; hostile in-process containment. See LIMITATIONS.

**OUT OF SCOPE.** Egress filtering at the OS or network layer. BRAIN is an application.

## Classification

**IMPLEMENTED.** `brain/kernel/classification.mjs` + `brain/policy/promotion.json`. Two answers
computed independently: DERIVED from criteria, ASSERTED from a claim under
`authority:owner.classification`. Absence of a criterion claim is `UNPROVEN`, never a pass. A
disagreement is reported and never resolved automatically.

**ESTABLISHED.** All nine criteria; `brain classify` (private CLI; not published) shows the current state.

**OUT OF SCOPE.** Any level above `GENERAL_ARCHITECTURE_CANDIDATE`. The gate does not define one.

---

## What BRAIN is not

Not a model. Not an agent. Not a scheduler, a queue, a database, or a consensus system. It has
no opinion about which model you use, and it will not become less correct if you change one.
The kernel is structurally incapable of calling a provider — a test asserts the import graph,
not the intention.
