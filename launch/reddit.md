# Reddit

**Status:** draft, not posted.

> **Check the rules of the specific subreddit immediately before posting.** Many technical
> subreddits restrict self-promotion to particular days, particular flairs, or a dedicated
> thread, and some ban repository links outright. Rules change. This document deliberately
> does not name subreddits, because a list written today is a list that gets someone banned in
> six months.
>
> Wherever it goes, post as the creator. Do not present it as a neutral discovery.

---

## Title

> I built an architecture for AI continuity where model replacement does not imply world-state
> loss. Looking for technical criticism.

---

## Body

> **Disclosure: I'm the creator.** This is my own project and I'm posting it to get it
> attacked, not upvoted.
>
> ### The problem
>
> AI systems can be intelligent and still have no continuity. The continuity usually lives in
> the conversation, the model, the agent, or the process — so when any of those ends, the world
> ends with it. The practical result is that a human becomes the memory and re-explains the
> same situation every session.
>
> ### How this differs from agent memory and context windows
>
> This is the distinction that matters most, so I'll be specific.
>
> **Context windows** are working memory for one session. **Agent memory** is usually retrieval:
> store text, embed it, fetch something similar later. Both answer "what do I recall?"
>
> BRAIN is trying to answer different questions, and keeps them separate on purpose:
>
> - **Evidence** — not "what was said" but *who looked, when, and what did they actually
>   observe*. A model's suggestion is never recorded as a fact.
> - **Continuity** — records are append-only and superseded, never edited. A correction is a
>   new record that outranks an old one; the old one stays readable. Belief is resolved on read
>   rather than stored, so an assertion can't outlive the evidence behind it.
> - **Authority** — who is entitled to decide a given kind of fact, scoped by domain. There is
>   no global ranking where one source always wins. Evidence establishes what a system *does*;
>   it does not decide what may happen.
> - **Verification** — an action isn't successful because it returned 200. Something has to be
>   observed afterwards, by a route that didn't perform the action.
>
> A retrieval layer will happily return a stale answer with confidence. The interesting part
> here is the machinery for *refusing* to — conflicts stay conflicts rather than being averaged
> into a plausible number, and "I don't know" is a first-class answer rather than a failure.
>
> ### Some concrete behaviour
>
> - A crash mid-action leaves a readable intention, because the journal is written before the
>   act.
> - A lost acknowledgement is its own state. A timeout is not "it didn't happen", so a
>   consequential action isn't retried on that assumption.
> - Two worlds that diverge and rejoin merge by record identity, so arrival order can't decide
>   truth — a rule you could otherwise game by disconnecting.
>
> ### Status, stated precisely
>
> Classification is GENERAL_ARCHITECTURE_CANDIDATE, internal architecture gate 9 of 9. That
> means it passed a gate I wrote, measured by tests I wrote, on one machine, with one owner.
> **The validation results are internally produced and nobody outside the project has
> reproduced them.** The public repository is an architecture release — the implementation and
> test suite aren't in it, so you can't independently reproduce the validation from what's
> published. Saying that up front rather than letting someone find it.
>
> Five known limitations are published deliberately, including ones I can't currently solve:
> no content classification; no containment of hostile in-process code (that needs an OS
> boundary); reuse isn't sensitive to losing one of several supporting roots; at-most-once
> authority across a partition is detected but not prevented; multi-owner is untested.
>
> Not open source — source-available for review, no license granted yet.
>
> ### What I'm asking for
>
> Specifically:
>
> 1. Does the four-way separation (evidence / continuity / authority / verification) hold up,
>    or is it a distinction without a difference in practice?
> 2. Is "survives model replacement" meaningful when a model still has to interpret the world
>    schema?
> 3. Is any of the five limitations broader than I've stated?
> 4. Is GENERAL_ARCHITECTURE_CANDIDATE too strong for evidence only I have produced?
>
> https://github.com/rogeliolopez2405-cloud/BRAIN-public

---

## Notes before posting

- Read the subreddit rules and the last week of posts before submitting. Match the register of
  what already does well there.
- The disclosure line stays at the top. Reddit reacts far worse to discovering undisclosed
  self-promotion than to the promotion itself.
- Answer criticism in the comments the same day. A post left unattended reads as a drop-and-run
  and gets treated accordingly.
- Do not cross-post the same text to several subreddits at once. That is the fastest route to a
  spam filter.
