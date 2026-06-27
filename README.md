# loot

**Paste a link, get only what's worth taking.**

`loot` is a skill for [Claude Code](https://claude.com/claude-code) and compatible agents. You give it a link (a tweet, a thread, an article, a GitHub repo, a Reddit/HN thread, a video), and it hands back a short, opinionated read: **what's worth taking**, with a concrete *before/after* case for each point, and **what to skip**. No summary. No retelling. Just a decision you can act on.

## Why

Most "how I did X" posts, threads, and repos are about 10% signal wrapped in 90% noise: flexing, lifestyle, motivation, platform-specific tricks, padding. `loot` strips that and leaves only what changes how *you* work, and for every point it gives a `Without it / With it` case so you know exactly why it matters.

It filters through a **lens** so the same link gives a founder, an engineer, and a marketer different (and relevant) takeaways.

## Example

Input: a viral "how I hit $1k/mo in 30 days" thread. Lens: `maker` (default).

```text
Reality check: $1k/mo is small, and half the thread is flexing (gym, supplements, diet). Filter hard.

What's worth taking

1. Copy what already sells
He didn't invent an idea, he took a product that already makes money for others.
- Without it: you spend months building something nobody asked for, hoping they'll want it.
- With it: demand is already proven, so you only fight execution.

2. Charge from day one
The core sits behind a hard paywall, nothing valuable is free.
- Without it: people use it for free and never pay.
- With it: whoever wants the value pays, and you see real demand.

What to skip
- Bidding on competitors' names, asking for a review during onboarding (the store "patched" it): on the edge of the rules.
- Apple Ads, RevenueCat, SwiftUI: iOS-app stack only, skip if you build on the web.

In one line: copy what already sells -> launch cheap -> watch where money shows up -> feed only that -> charge immediately.
```

## Install

**Manual (works everywhere):**

```bash
git clone https://github.com/RakhimovY/loot
mkdir -p ~/.claude/skills
cp -r loot/skills/loot ~/.claude/skills/loot
```

Then start a new session and the skill is available.

**Via the skills CLI** (if you use [agentskills.io](https://agentskills.io) tooling):

```bash
npx skills add RakhimovY/loot
```

## Usage

```
/loot https://example.com/some-post
/loot https://github.com/owner/repo --lens engineer
loot this: <url>
what's worth taking from <url>
```

- Default lens is **maker**. Switch with `--lens engineer`, `--lens marketer`, `--lens researcher`, or pass a custom one-line lens ("as a designer", "for a teacher", ...).
- Output language defaults to English and auto-matches the language you write in.

## Lenses

| Lens | Keeps what changes how you... | Cases framed as... |
|---|---|---|
| `maker` (default) | build **or** sell your own product | time, money, users |
| `engineer` | design, write, test, or operate software | bugs, maintenance, performance, complexity |
| `marketer` | reach, convert, or retain an audience | reach, conversion, trust, churn |
| `researcher` | understand a topic well enough to act | understanding, decisions, dead-ends avoided |

## What it can fetch

| Source | Works out of the box? |
|---|---|
| Article, blog, docs | yes (`WebFetch`) |
| GitHub repo | yes (page + README) |
| Reddit, Hacker News | usually |
| X/Twitter, LinkedIn, Instagram | often not (JS/login wall): paste the text, or use a connected browser MCP |
| YouTube / video | no built-in transcript: paste the captions, or use a transcript tool |

If a fetch fails or returns a login wall, `loot` tells you and asks you to paste the content. It never invents what the source said.

## Output contract

1. **Reality-check line** (only if there's hype or small/unverified numbers to flag).
2. **What's worth taking** , numbered, each with a one-line "what they did" plus `Without it:` / `With it:`.
3. **What to skip** , only risky/against-the-rules tactics and things that don't apply to you. Jokes and lifestyle are dropped silently.
4. **In one line** , a compressed plan: step -> step -> step.

## License

[MIT](./LICENSE) (c) 2026 Yerkebulan Rakhimov
