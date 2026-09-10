> **PUBLIC ARCHITECTURE RELEASE.** This document is part of a controlled export of the
> private BRAIN development repository. The implementation it describes is **not** published
> here — see [PUBLIC-SCOPE.md](PUBLIC-SCOPE.md). Paths beginning `brain/kernel/` or
> `tests/` refer to files in the private repository and are given so the description can be
> checked against something specific, not because they are included.
>
> No open-source license has been granted. See [COPYRIGHT.md](COPYRIGHT.md).

# What BRAIN does not claim

<!-- Rendered from the canonical policy JSON in the private repository. The canonical copy is published here as policy/limitations.json. -->

Source: `policy/limitations.json`. Five limitations travel with the
`GENERAL_ARCHITECTURE_CANDIDATE` classification, recorded in the
classification claim's own basis and in the promotion decision under `docs/decisions/`,
so they cannot be dropped by editing a document.

**These are not a roadmap.** "What would prove it" says what evidence would be required, not
what anyone intends to build.

---

## 1. content classification

**Not proven.** BRAIN cannot look at arbitrary bytes and determine that they are sensitive.

**Why.** Every enforcement decision runs on DECLARED provenance -- a class carried by the data and a policy carried by the objective. Nothing inspects content. This was a deliberate choice in round 115: building a classifier would have produced a mechanism whose failures are invisible, and a lane rule that is right most of the time is not a boundary.

**Current behaviour.** Unlabelled data is treated as PRIVATE, and a payload labelled PUBLIC is released according to that label whether or not the label is correct. Mislabelled data is released correctly per its label, which is the same thing as being released wrongly.

**What would prove it.** A classifier with a measured false-negative rate on a corpus BRAIN did not label itself, and an enforcement path that consults it. Neither exists.

**Demonstrated by attack** `mislabelled` (attack id; tooling not published — see [VALIDATION-RESULTS.md](VALIDATION-RESULTS.md))

---

## 2. hostile in process containment

**Not proven.** Application-level lane enforcement cannot contain malicious code running inside the same process.

**Why.** The gate governs code that asks it. Hostile in-process code does not have to ask: it can import node:http and open its own socket. No amount of checking inside the same process changes that, because the check and the bypass have equal privilege.

**Current behaviour.** Enforced against BRAIN's own paths, against trusted adapters using the provided send path, and against buggy adapters that ask and mishandle the answer -- the gate refuses first, so a mishandled denial still sends nothing. A test in tests/laneenforcement.test.mjs performs the bypass deliberately and succeeds.

**What would prove it.** An external runtime boundary: a separate OS process with no network access (namespace, container, or firewall rule), or a capability-based runtime where the network is not ambiently reachable. That is an operating-system property, not an application one.

**Demonstrated by attack** `malicious-adapter` (attack id; tooling not published — see [VALIDATION-RESULTS.md](VALIDATION-RESULTS.md))

---

## 3. corroboration strength

**Not proven.** Reuse is not sensitive to how many independent roots support a belief.

**Why.** The sufficiency basis fingerprints the semantic outcome -- belief, epistemic state, controlling authority. Adding supporting records to the fingerprint was tried in round 114 and made every reread invalidate every reuse, so the fingerprint was narrowed and the gap documented rather than hidden.

**Current behaviour.** A belief that keeps its value while losing one of two independent supporting roots reports STILL_VALID. The answer is unchanged and genuinely weaker, and BRAIN does not say so.

**What would prove it.** A strength measure in the basis that distinguishes losing a root from gaining an unrelated record, with tests showing it does not invalidate on ordinary rereads.

**Demonstrated by attack** `corroboration-loss` (attack id; tooling not published — see [VALIDATION-RESULTS.md](VALIDATION-RESULTS.md))

---

## 4. partitioned at most once

**Not proven.** A one-time grant consumed on both sides of a partition cannot be prevented from being consumed twice.

**Why.** At-most-once across disconnected worlds is not establishable by merging afterwards. The two consumptions both really happened, in worlds that could not see each other; no merge rule invents a winner without inventing a story.

**Current behaviour.** DETECTION, NOT PREVENTION. Rejoin reports both consumptions intact, names the conflict, and states that coordination before the partition is what would have prevented it.

**What would prove it.** Coordination established BEFORE the partition -- a lease, a quorum, or a single-writer token. Every one of those trades availability for the guarantee, and none is implemented.

**Demonstrated by attack** `split-grant` (attack id; tooling not published — see [VALIDATION-RESULTS.md](VALIDATION-RESULTS.md))

---

## 5. multi owner

**Not proven.** Nothing about multi-owner or multi-tenant operation has been tested.

**Why.** Every round measured one owner on one machine. Lanes separate kinds of work for one owner; they were never asked to separate two owners from each other, and a mechanism is only proven against the question it was asked.

**Current behaviour.** Single owner. `authority:owner.classification` names one owner and nothing arbitrates between two. Lane enforcement runs on declared provenance, which no adversarial tenant was ever invited to declare.

**What would prove it.** An adversarial tenant model: two owners with conflicting authority over the same subject, and tests showing neither can read, write, or release the other's records.

**No attack published.** No reproduction: there is no implementation to reproduce against.

---

Every claim BRAIN *does* make is in `validate` (tooling not published — see [VALIDATION-RESULTS.md](VALIDATION-RESULTS.md)), and every one of them names
the record that established it.
