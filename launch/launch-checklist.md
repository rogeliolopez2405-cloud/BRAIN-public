# Launch checklist

Work top to bottom. The pre-flight checks exist because every one of them is cheap now and
expensive after a few hundred people have looked.

## Pre-flight — before any post goes out

- [ ] **1. Public repo is public and correct**
      `gh api repos/rogeliolopez2405-cloud/BRAIN-public --jq '.visibility'` → `public`
      README loads, links work, `v0.1.0` release is visible.

- [ ] **2. Authorship is right**
      README, `PROVENANCE.md`, `CITATION.cff` and `COPYRIGHT.md` all say **Rogelio Lopez**.

- [ ] **3. No personal email anywhere**
      In files *and* in commit metadata:
      `git log --format='%ae' | sort -u` → only the GitHub noreply address.
      Once a post drives traffic, commit metadata gets read.

- [ ] **4. Copyright status unchanged**
      `COPYRIGHT.md` still says All rights reserved, no open-source license granted.
      Nothing anywhere calls it "open source".

- [ ] **5. Social preview is set**
      Uploaded via Settings → General → Social preview. **This is a manual step; there is no
      API for it.** Verify by pasting the repo URL into a private message on the platform you
      are about to post to and looking at the card that renders.

- [ ] **6. Issue template works**
      Open `https://github.com/rogeliolopez2405-cloud/BRAIN-public/issues/new/choose` and
      confirm "Architecture challenge" appears and pre-fills.

- [ ] **7. Read your own README as a stranger**
      Give yourself 30 seconds. Can you answer: what is it, why does it exist, how is it
      different from an LLM or agent framework, what is its status, what is proven vs unproven,
      who made it, how do I challenge it? If any answer requires scrolling to the end, fix that
      before posting rather than after.

## Distribution — in this order

The order is deliberate. Each channel is slower-burning and higher-signal than the last, and
the early ones surface embarrassing mistakes while the audience is small.

- [ ] **8. LinkedIn** — [linkedin.md](linkedin.md). Warmest audience, most forgiving of a first
      post. Record the timestamp.
- [ ] **9. X / Twitter** — [x-twitter.md](x-twitter.md). Single post or thread.
- [ ] **10. Show HN** — [hacker-news.md](hacker-news.md). **Read the title note first** — there
      is a real convention question about whether a docs-only repo should use "Show HN".
      Weekday morning US Eastern. **Block out several hours to answer the thread.**
- [ ] **11. Reddit** — [reddit.md](reddit.md). **Check the subreddit's self-promotion rules
      immediately before posting.** Disclose that you are the creator, in the first line.
- [ ] **12. Direct outreach** — [direct-outreach.md](direct-outreach.md). Fill the brackets in
      properly. Send a handful, not a hundred.

## After each distribution event

- [ ] **13. Record the timestamp** in [DISTRIBUTION-LOG.md](DISTRIBUTION-LOG.md), immediately.
      Reconstructing it later never happens.

- [ ] **14. Check GitHub Insights → Traffic** — Insights tab → Traffic. Record:
      views · unique visitors · clones · unique cloners · stars · forks · issues.
      Traffic data covers the **last 14 days only** and is not retained beyond that, so if you
      do not write it down it is gone.

- [ ] **15. Do not read raw views as technical validation.**
      Views measure distribution. A thousand views and no issues means the post travelled and
      the argument did not land — that is a real result, and it is not a good one. **One issue
      that identifies a genuine architectural contradiction is worth more than every view
      number on this page.** Judge the launch on the quality of objections received.

## The honest success criterion

This release exists to be attacked. Rank the outcomes:

1. Someone files a challenge that forces a documented limitation to widen, or shows
   `GENERAL_ARCHITECTURE_CANDIDATE` is unsupported. **Best possible outcome.**
2. Someone with relevant expertise engages seriously and finds nothing wrong yet.
3. Stars and traffic with no technical engagement. **This looks like success and is not.**

If the result is 3, the problem is the argument or the audience, not the reach. Adding more
channels will not fix it.
