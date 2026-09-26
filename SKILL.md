---
name: youtube-niche-validator
description: Decide whether a YouTube niche is worth entering before you spend months on it. Replaces "look at the top videos" research with a sampling design that has a real denominator: dual-slice sampling (popularity head + relevance denominator) to kill survivorship bias, a hit rate computed against the denominator, a channel-concentration check that catches matrix-account monopolies, and big-vs-small channel stratification so you only copy what a new channel can actually replicate. Includes a mandatory Made-for-Kids (COPPA) economics fork. Use when validating a YouTube niche, topic, or keyword; comparing candidate niches; or auditing why a niche looks easier than it is.
---

# YouTube Niche Validator

## The failure this prevents

The standard way to research a niche is to search a keyword, look at the top 20 results, see millions of views, and conclude the niche is open. **That is survivorship bias.** You sampled the winners and never saw the hundreds of videos on the same topic that died. Every niche looks easy from the top of the search page.

This skill replaces that with a design that has a **denominator**. The single most important idea: a hit rate is only meaningful when the denominator is *everything published in the window*, not *the stuff the algorithm showed you*.

## Hard gate: three priors (verify before you scale sampling)

Do not jump to a 300-video pull. Spend one cheap pass on each of these first. Each takes one or two spot checks.

1. **Caliber** — What does the keyword actually mean on this platform? Long-form, Shorts, or both? Run one term at a small sample (around 10 results) and eyeball relevance before committing to a term list. A keyword that mixes Shorts and long-form will silently poison your denominator.
2. **Extractability** — Are the fields you need actually obtainable? Confirm you can get publish date, channel follower count, and (where allowed) comments. If a field is always missing, the method that depends on it is not usable and you must substitute.
3. **Risk control** — Does the collection method survive repeated calls? Frequent automated detail requests start failing; pacing requests with a delay of about 3 seconds has been measured at 8/8 success where rapid-fire requests failed after 5-6 calls. **Do not trade delay for speed** — a broken pull costs more than a slow one.

If any prior fails, stop and fix it. Scaling a broken pipeline just produces a confident wrong answer faster.

## Workflow (5 steps)

### Step 1 — Dual-slice sampling

Collect **two** slices per keyword, not one:

- **Head slice**: sorted by popularity. This is what everyone looks at.
- **Denominator slice**: sorted by relevance, across a fixed time window. This is your true population.

Parameters that matter:
- **Window** (`all` / `today` / `week` / `month` / `year`) defines the denominator. Pick it deliberately — "is this niche open *right now*" and "has this niche ever been open" are different questions with different windows.
- **Type**: long-form video, Shorts, or channel. Do not mix them in one slice.
- **Volume**: you need roughly 150+ results per keyword before a hit rate is stable. Paginate to get there.

> Why two slices: the head slice tells you what winning looks like; the denominator slice tells you how often winning happens. You need both, and they answer different questions.

### Step 2 — Enrichment

The search index gives you rough data. Before scoring, fill in:
- exact publish date (search often returns only relative text like "5 days ago")
- channel follower count
- likes
- tags (useful for mining demand language)

Enrichment is what makes the time-window filter and the size stratification in Step 4 possible. Make it resumable — long pulls will be interrupted.

### Step 3 — Channel concentration check (YouTube-specific, do not skip)

Compute what share of the **head slice** comes from a single channel.

- **If one channel accounts for more than 50% of the head slice, flag the niche as matrix-account-dominated.**
- When flagged, **recompute the hit rate deduplicated by channel**: multiple hits from the same channel count once.

Why this exists and why it is not optional: a single operator running a matrix of channels can occupy most of the visible top results. If you read the head slice at face value, you will overstate the opportunity by roughly an order of magnitude. In one measured case, 73% of the head slice (11 of 15) came from one matrix operator.

This step has no analogue in short-video-platform methods. It is required specifically because YouTube channel-level concentration is common and invisible unless you measure it.

### Step 4 — Hit rate + size stratification

**Hit rate = qualifying original videos ÷ denominator count.**

- The denominator is the **relevance slice**, never the head slice.
- Decide in advance what "qualifying" means for your niche (a view threshold relative to channel size is usually more honest than an absolute number).

Decision bands:

| Hit rate | Verdict |
|---|---|
| ≥ 25% | Adopt — the niche reliably produces hits |
| 15-25% | Keep — workable, but expect variance |
| < 15% | Kill — too few winners to plan around |

**Then split by channel size.** Separate large channels (roughly 100k+ followers) from small ones (roughly under 10k), and read the two groups separately.

> The small-channel hit rate is the one that matters for a new channel. A niche where only established channels break out is not an open niche, no matter how good the aggregate hit rate looks.

### Step 5 — Verdict

Write up: the window and keywords used, both slice sizes, the concentration flag and whether you deduplicated, the overall hit rate, the small-channel hit rate, and one of three calls — **adopt / keep / kill** — with the reason. Keep the write-up dated and comparable to future runs so you can re-measure the same niche later.

## Made-for-Kids (COPPA): a mandatory fork

Before entering any kids, picture-book, toy, or nursery-rhyme niche, confirm these three. If you skip them, the method above will mislead you.

1. **Comments are forced off by the platform** (COPPA). You cannot turn them on. Symptom: comment count is always unavailable.
   - Consequence: the usual "mine the comments for unmet demand" move **does not work**. Substitute: retailer reviews, book-community reviews, forum threads, and high-frequency words in competitor titles and tags.
   - **Confirm it is the platform, not your scraper**: run the identical command against a known non-kids video. If the control returns comments, collection is fine and the restriction is real.
2. **Monetization surfaces are off**: notification bell, end cards, info cards, memberships, super chats. The subscribe-and-return loop is broken; growth runs on algorithmic recommendation rather than subscriber revisits.
3. **Only contextual ads, no personalized ads.** RPM runs roughly $1-3 versus $5-15 for general audiences — **a 50-80% reduction**.
   - Consequence: **an ad-revenue-only model does not hold.** Model the business on brand sponsorship, content licensing, or off-platform products instead.
   - Legal note: mislabeling kids content carries real exposure. Under the amended COPPA rule (FTC, effective January 2025), the per-violation penalty ceiling is $53,088.

## Anti-hallucination constraints

These are hard rules. Violating them produces a confident, wrong verdict.

1. **Never compute a hit rate against the head slice.** Head ÷ head is not a rate; it is a tautology close to 100%.
2. **Never skip the concentration check** on YouTube. It is the step that catches the single largest source of over-optimism.
3. **Never report an aggregate hit rate without the small-channel split.** The aggregate is the least actionable number you have.
4. **If a field was unavailable, say so** and state which decision it blocks. Do not impute it and do not quietly drop it.
5. **If you did not run collection, do not invent numbers.** Present the method, the parameters to use, and what the output would look like — labeled explicitly as a plan, not a result.
6. **Quote the window.** A hit rate without a time window is not comparable to anything.

## Attribution

This skill is original work by aishifu. The method was designed and tested on YouTube first: dual-slice sampling, hit-rate banding and size stratification were extended for YouTube with the channel-concentration check and the Made-for-Kids economics fork. No part of it is reproduced from another skill or repository. Tooling guidance reflects verification performed in September 2026 — re-check tool status before relying on it, since unmaintained libraries are the most common silent failure in this workflow.

## Resources

- `references/niche-scoring-rubric.md` — hit-rate bands, concentration thresholds, stratification rules, and the worked verdict template
- `references/tooling-and-gotchas.md` — tool selection with verification dates, and the failure modes that cost the most time
- `references/kids-niche-playbook.md` — the full COPPA fork: what breaks, what to substitute, how to model revenue
