# Direct technical outreach

**Templates only.** They contain no names and make no claim of familiarity with anyone's work.
Fill in the bracketed parts yourself after actually reading the person's work — a template sent
unfilled is worse than no message.

## Rules

- **Never "please check out my project."** It asks for a favour and offers nothing.
- Lead with the architectural overlap, then ask **one** concrete question. One is answerable in
  ten minutes; three is a research request and gets ignored.
- Ask for the thing you actually want: criticism of a specific claim.
- Keep it under 150 words. If it needs scrolling, it will not be read.
- Do not follow up more than once.
- Do not imply endorsement, affiliation, or that you have collaborated before.

---

## A. AI agent / memory researcher

> Subject: continuity that survives model replacement — one question
>
> I published a limited architecture release for BRAIN, a persistent reality, evidence,
> authority and verification layer intended to survive model or agent replacement. It is not a
> memory store: belief is resolved on read rather than retrieved, and a model's output is never
> recorded as evidence.
>
> Your work on [AREA — e.g. long-horizon agent memory / retrieval grounding] overlaps with the
> boundary I am least sure about: whether "the intelligence is replaceable" is meaningful when
> a model still has to interpret the world schema it reads.
>
> If you have ten minutes, I would especially value criticism of that specific claim — that
> continuity is preserved across model replacement rather than merely relocated into the
> schema.
>
> Architecture only, not the implementation. Classification GENERAL_ARCHITECTURE_CANDIDATE;
> validation is internal and the repository says so.
>
> https://github.com/rogeliolopez2405-cloud/BRAIN-public

---

## B. Distributed systems / reliability engineer

> Subject: at-most-once across a partition — a limitation I can't close
>
> I published a limited architecture release for BRAIN, a continuity, evidence, authority and
> verification layer for AI systems. Two properties may be in your territory: rejoin merges by
> record identity so arrival order cannot decide truth, and a lost acknowledgement is kept as
> its own state rather than collapsed into success or failure.
>
> Your work on [AREA — e.g. consensus / exactly-once semantics / failure detection] overlaps
> with the limitation I have documented and cannot currently solve: a one-time grant consumed
> on both sides of a partition is **detected after rejoin, not prevented**. I claim prevention
> requires coordination established before the partition.
>
> If you have ten minutes, I would value criticism of that claim — particularly whether the
> detection-only position is as forced as I think, or whether I have missed an option that
> doesn't trade away availability.
>
> https://github.com/rogeliolopez2405-cloud/BRAIN-public

---

## C. Robotics / physical AI engineer

> Subject: "not seen" vs "gone" — asking whether this survives contact with real sensors
>
> I published a limited architecture release for BRAIN, a persistent world, evidence and
> verification layer intended to survive replacement of the intelligence above it. Two things
> may be relevant to your work: absence of an observation is never treated as observation of
> absence, and an action is not successful until something observes the result by a route that
> did not perform it.
>
> Your work on [AREA — e.g. state estimation / sensor fusion / autonomy safety] overlaps with
> the assumption I am least confident about: that verification can be kept genuinely separate
> from execution when the only sensor available is the one attached to the actuator.
>
> If you have ten minutes, I would value criticism of that separation — whether it holds in
> physical systems or quietly degrades into self-reporting.
>
> https://github.com/rogeliolopez2405-cloud/BRAIN-public

---

## After sending

Log it in [DISTRIBUTION-LOG.md](DISTRIBUTION-LOG.md). Direct outreach is usually the lowest
volume and the highest quality channel; if one reply produces a real architectural objection,
that outperforms a thousand views.
