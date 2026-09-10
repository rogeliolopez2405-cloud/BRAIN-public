# Public scope

**BRAIN-public is not the complete BRAIN development repository.** It is a controlled export.
Read this before drawing conclusions from what you can or cannot find here.

## What is included

| | |
|---|---|
| `README.md` | what BRAIN is, what it is not, the core principle, the loop, the classification |
| `docs/SPECIFICATION.md` | each subsystem, split IMPLEMENTED / ESTABLISHED / UNPROVEN / OUT OF SCOPE |
| `docs/DIAGRAM.md` | the architecture loop and what is replaceable |
| `docs/FAILURE-MODES.md` | 17 failure modes, mechanism, proof reference, remaining limit |
| `docs/TRUST-MODEL.md` | who is trusted with what, and where enforcement ends |
| `LIMITATIONS.md` | the five unproven boundaries, in full |
| `VALIDATION-RESULTS.md` | frozen internal validation results, and what they do not mean |
| `policy/laws.json` | the architectural laws, canonical and machine-readable |
| `policy/limitations.json` | the limitations, canonical and machine-readable |
| `PROVENANCE.md`, `CITATION.cff`, `COPYRIGHT.md`, `MANIFEST.sha256` | provenance, citation, rights, integrity |

## What is intentionally excluded

**Implementation.** The kernel — truth resolution, provenance, authority, continuity, knowledge
reuse, the action journal, rejoin, lane enforcement — is not published. Documents here refer to
paths like `brain/kernel/lane.mjs` so a description can be tied to something specific; those
files are in the private repository.

**Tests.** The suite is not published. Test file names appear in `policy/laws.json` and in the
failure-mode map for the same reason.

**Validation and attack tooling.** Not published, and this was a decision with a measured
reason rather than a preference:

- The attack pack's transitive import closure is **17 of 54 kernel modules — 31% of the
  kernel**, including the truth, authority, lane, knowledge, rejoin, resumption and action-journal
  modules. Publishing a runnable attack tool would publish roughly a third of the implementation
  as a side effect.
- The validation tool needs fewer modules, but reads the recorded claim and development logs —
  the private world data — to derive anything at all.

The rule applied was: do not expose implementation merely to make a tool run, and do not publish
a tool that is knowingly broken. Both would have been worse than publishing the results
statically, which is what [VALIDATION-RESULTS.md](VALIDATION-RESULTS.md) does.

**Development history.** This repository's git history begins at v0.1.0. The private history is
not filtered, rewritten or included, so nothing private is recoverable from it.

**World data.** Recorded claims, development events, checkpoints, decisions and instance
identity are private-world records and are not published.

**Everything personal or operational.** No credentials, tokens, environment files, machine
paths, logs, correspondence, collaborator or reviewer information, personal financial data, or
material from any unrelated project.

## What this means for evaluating the claims

**Evaluate claims only against material actually published here.** Where a statement in these
documents rests on code or tests that are not included, you are being asked to take a
description on trust — and you are entitled to say so. That is a legitimate criticism of this
release and it is not hidden.

Specifically:

- The validation results in [VALIDATION-RESULTS.md](VALIDATION-RESULTS.md) were **produced
  internally**. Nobody outside the project has independently reproduced them, and they should
  not be read as if someone had.
- You cannot currently run the attack pack yourself from this repository. If reproducibility
  matters more to you than the current scope, that is worth saying in an issue.
- The architectural claims — how conflict is resolved, how authority is scoped, what happens
  after a crash — are described precisely enough to be argued with on their merits, and that is
  the intended level of engagement for this release.

## Why publish a partial package at all

Three reasons, stated plainly:

1. **Provenance.** A public, timestamped, hashed checkpoint of the architecture.
2. **Critique.** The architecture can be attacked on its design without the implementation.
3. **Not granting broad rights yet.** Publishing everything under an open-source license is a
   decision that cannot be walked back. See [COPYRIGHT.md](COPYRIGHT.md).

Wider access to the private repository, including the implementation and the runnable validation
tooling, is granted case by case to reviewers.
