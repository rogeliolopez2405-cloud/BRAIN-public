# Hacker News

**Status:** draft, not submitted.

---

## Title

Suggested:

> Show HN: BRAIN – a persistent reality/authority layer designed to survive model replacement

**Verified against the published material.** Every part of that title is supported:

- *"persistent reality/authority layer"* — the README's own description is "persistent reality,
  continuity, evidence, authority and verification layer".
- *"designed to survive model replacement"* — `continuity_across_model_replacement` is one of
  the nine gate criteria and is recorded as established. "Designed to" is the right strength:
  it claims intent and internal evidence, not independent proof.

**One convention problem, worth deciding before you post.** "Show HN" is conventionally for
work others can run and play with. This repository is documentation, policy data and frozen
results — there is nothing to execute. Some readers will say so, and they will be right on the
convention even if the content is substantial.

Two options:

1. **Keep "Show HN"** and pre-empt it in the first line of the body (the draft below does
   this). Honest, and it puts the objection where you control the framing.
2. **Drop "Show HN"** and post as a normal submission:
   > BRAIN – a persistent reality/authority layer designed to survive model replacement

Option 2 avoids the argument entirely. Option 1 gets more visibility if it lands. Your call.

---

## Body

> This is an architecture release, not runnable software — I'm flagging that up front because
> Show HN usually means something you can execute, and there's nothing here to run.
>
> The problem I kept hitting: AI systems can be intelligent and still have no continuity,
> because the continuity lives in the conversation, the model, the agent, or the process. When
> any of those ends, the world ends with it. In practice a human becomes the memory and
> re-explains the same situation every session.
>
> BRAIN is an attempt at a different separation — the intelligence is replaceable, the
> persistent world is not. Concretely it's a layer holding evidence, provenance, continuity,
> domain-scoped authority, and verification, with the constraint that it never calls a model or
> a provider itself.
>
> The principle it's built on is that four things usually collapsed into one are kept apart:
> intelligence proposes, evidence establishes, authority decides, reality verifies. A model
> suggesting something isn't evidence for it. Evidence establishing what a system does isn't a
> decision about what may happen. A decision isn't a verification — something has to actually
> happen and be observed by a route that didn't perform it.
>
> Some things that fall out of that, which I think are the interesting part:
>
> - A crash mid-action leaves a readable intention, because the journal is written before the
>   act. Recovery returns a verdict, not a guess.
> - A lost acknowledgement is its own state. A timeout is not recorded as "didn't happen" — you
>   can't retry a consequential action on that assumption.
> - Two worlds that diverge and rejoin merge by record identity, so arrival order can't decide
>   truth. Conflicts survive the merge instead of being resolved by whoever reconnected first.
> - Belief is resolved on read rather than stored, so an assertion can't outlive the evidence
>   that justified it.
> - Authority is domain-scoped. There's no global ranking where one source always wins.
>
> What it is not: not a model, not a chatbot, not MCP, not an agent framework, not embodied. It
> orchestrates nothing.
>
> Current classification is GENERAL_ARCHITECTURE_CANDIDATE with an internal architecture gate
> at 9 of 9. I want to be precise about what that does and doesn't mean: it passed a gate I
> wrote, measured by tests I wrote, on one machine, with one owner. **The validation results in
> the repository were produced internally and nobody outside the project has reproduced them.**
> The public repository deliberately does not include the implementation or the test suite, so
> you cannot independently reproduce the validation from what's published. I'd rather say that
> plainly than have someone discover it.
>
> Why the implementation isn't public: the attack/validation tooling's transitive import
> closure is 17 of 54 kernel modules — about a third of the implementation — so publishing a
> runnable version would publish the thing I'm not ready to publish. The alternative was
> shipping a knowingly broken tool. I chose static results plus an explanation, and the
> repository says what that costs you as a reader.
>
> Five known limitations are published on purpose, including ones I can't currently solve:
> no content classification (enforcement runs on declared provenance, so mislabelled data is
> released correctly per its label); no containment of hostile in-process code (that needs an
> OS boundary, not application policy); reuse isn't sensitive to losing one of several
> supporting roots; at-most-once authority across a partition is detected after rejoin, not
> prevented; and multi-owner/multi-tenant is entirely untested.
>
> No open-source license — it's source-available for review. I'd rather defer that decision
> than grant broad rights by dropping in a default file.
>
> The criticism I actually want: whether the four-step separation holds up or is a distinction
> without a difference; whether "survives model replacement" is meaningful when the world
> schema is still something a model has to interpret; whether any of the five limitations is
> broader than I've stated; and whether GENERAL_ARCHITECTURE_CANDIDATE is too strong for
> evidence that only I have produced.
>
> https://github.com/rogeliolopez2405-cloud/BRAIN-public

---

## Notes before posting

- **Post it and then stay in the thread.** HN punishes drive-by submissions and rewards authors
  who answer hard questions directly. Budget a few hours.
- The two paragraphs admitting internal-only validation are the most important ones in the
  post. Do not trim them for length. If someone finds that admission themselves after reading
  a confident post, the thread turns.
- Expect these specific challenges, and they are all fair:
  - "This is just a database with opinions." Answer with the resolved-on-read and
    domain-scoped-authority behaviour, not with adjectives.
  - "You can't claim continuity across model replacement without the implementation." Correct.
    Say so, and offer reviewer access to the private repository.
  - "9 of 9 on a gate you wrote yourself is not a result." Also correct, and the repository
    already says it. Agreeing quickly is better than defending.
- Best posting window is generally weekday mornings US Eastern. Do not repost if it doesn't
  land the first time.
- Do not editorialise the title. HN moderators rewrite titles that oversell.
