# YouTube Niche Validator

Decide whether a YouTube niche is worth entering before you spend months on it.

The failure it prevents: searching a keyword, looking at the top 20 results, seeing millions of views and concluding the niche is open. That is survivorship bias — you sampled the winners and never saw the hundreds of videos on the same topic that died.

This skill replaces it with a design that has a **denominator**: dual-slice sampling (a popularity head plus a relevance denominator across a fixed window), a hit rate computed against the denominator rather than the head, a **channel-concentration check** that catches matrix-account monopolies, and big-versus-small channel stratification so you only copy what a new channel can replicate. It also carries a mandatory Made-for-Kids (COPPA) economics fork: comments are forced off, monetisation surfaces are disabled, and only contextual ads run — an ad-revenue-only model does not hold there.

## What is in this repository

This is the **free edition**: `SKILL.md`, the complete method write-up — the part that actually does the work.

| File | What it is |
|---|---|
| [`SKILL.md`](SKILL.md) | the full method, written to be read by an AI assistant |
| [`LICENSE.txt`](LICENSE.txt) | licence for the free edition |

## How to use it

It works with any AI client that reads a skill file — WorkBuddy, Claude Code, Codex, Cursor — or with no
client at all:

1. **As a skill.** Put `SKILL.md` where your client looks for skills (usually a folder named after the
   skill, containing `SKILL.md`).
2. **As a prompt.** Paste `SKILL.md` into a conversation with any capable model and then ask your question.

## What the paid editions add

Deeper reference files (scoring rubrics, checklists, platform heuristics) and, where applicable, runnable
scripts. The hosted versions need no setup at all.

| Where | What you get |
|---|---|
| [Agensi](https://agensi.io/creators/alpha-lay) | the full package, including source files |
| [Poe](https://poe.com/YouTubeNicheCheck) | hosted — no install, just talk to it |
| [PromptBase](https://promptbase.com) | selected tools |
| [aishifu.shop](https://aishifu.shop/) | free downloads and the research notes behind these tools |

The same free edition is also available as a zip at [aishifu.shop/downloads](https://aishifu.shop/downloads/).

## Licence

See [LICENSE.txt](LICENSE.txt). In short: use it in your own work, personal or commercial, and modify it
freely — but do not resell it or pass it off as your own product.

---

Built by **Alpha Lay**. More tools, and a 1,274-case study of AI video that these methods were tested
against: <https://aishifu.shop/>

## Official site

This free edition, the research notes behind it, and downloads for every tool in this repository live on the official site:

- **Free downloads:**&#8203; <https://aishifu.shop/downloads/>
- **Original research & docs:**&#8203; <https://aishifu.shop/>

