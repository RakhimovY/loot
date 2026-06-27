---
name: loot
description: Use when someone pastes a link (X/Twitter post or thread, article, blog, GitHub repo, Reddit or Hacker News thread, YouTube video) and wants the practical, actionable takeaways, what is worth taking and what to skip, with a before/after case for each. Triggers include /loot, "loot this", "what's worth taking", "break this down", "what should I take from this" followed by a URL. Optional lens preset (maker, engineer, marketer, researcher).
---

# loot

## Overview

Paste a link, get a short, opinionated read: what is worth taking (with a before/after case for each) and what to skip. No summary, no retelling, just a decision.

One job: kill the noise and hand back something you can act on.

## When to use

- Someone pastes a link plus a short ask: "loot this", "what's worth taking", "break this down", `/loot <url>`.
- Any link type in the fetch table below.

**When NOT to use:** they want a full, faithful summary of the source, or a deep archived write-up. This skill deliberately throws most of the source away.

## How to get the content (escalate, do not give up)

Your job is to actually read the source. Use the best tool you have, and if you lack the capability, **recommend the tool the user should add**, do not just refuse. A flat "paste it yourself" is the last resort, not the first answer.

**1. Static pages** (article, blog, docs, GitHub repo, Reddit, Hacker News): fetch the URL with your agent's web-fetch tool. (Claude Code calls it `WebFetch`; other agents have an equivalent.) For a GitHub repo, also pull the raw README and note what it does, stars, and recent activity; use the `gh` CLI if available. This works out of the box.

**2. JS-heavy or login-walled pages** (X/Twitter, LinkedIn, Instagram, Threads): a plain fetch returns an empty shell, so this needs a real browser.
- If you have a browser-automation tool or MCP connected, use it. One running with the user's logged-in session can read anything the user can see (needed for LinkedIn and most of X).
- If you do not have one, do NOT stop at "paste it". Tell the user a browser tool is needed and recommend adding one: for example Playwright MCP (commonly run as `npx @playwright/mcp@latest`) for public pages, or a Chrome/browser MCP that reuses their logged-in session for login-walled sites. Then offer the paste fallback so they are unblocked right now.

**3. Video** (YouTube and others): there is usually no built-in transcript.
- If you have a transcript tool or MCP, use it.
- If not, recommend installing one: for example `yt-dlp` (it can pull a video's captions/subtitles) or a YouTube-transcript MCP. Then offer to continue the moment they paste a transcript.

Whatever path you take: if you could not actually read the source, say so plainly and **never invent what it said**. Recommending the right tool is part of the job; a refusal is not.

## The lens (what separates signal from noise)

Everything is filtered through one lens. Default is **maker**. The user can switch it (`--lens engineer`, "as an engineer", etc.) or pass a custom one-line lens.

An item passes ONLY if both are true:
1. it changes how you do the thing the lens cares about;
2. you can actually act on it.

Personal stories, lifestyle, motivation, generic platitudes, hype, and tactics tied to a platform you do not use: all noise. There is no fixed cap on the number of items, the bar above is what keeps it short, not a counter.

| Lens | An item counts if it changes how you... | Before/after cases are framed as... |
|---|---|---|
| **maker** (default) | build OR sell your own product | time, money, users |
| **engineer** | design, write, test, or operate software | bugs, maintenance, performance, complexity, on-call pain |
| **marketer** | reach, convert, or retain an audience | reach, conversion, trust, churn |
| **researcher** | understand a topic well enough to act | understanding, better decisions, dead-ends avoided |

Custom lens: if the user passes a one-line lens, use it the same way (keep what changes how they do that thing; frame cases as consequences for it).

## Output contract (exactly this, in this order)

Respond in the reader's language. Default to English; if the user wrote in another language, answer in that language.

**1. Reality-check line (optional).** One line at the top, ONLY if there is something to ground: small numbers, obvious flexing, unverified claims. If the source is solid, skip this line.

**2. `What's worth taking`** , a numbered list. Each item has exactly three parts:

```
N. **Short bold title**
One line: what they did.
- Without it: what happens to your work if you don't do this (the lens's consequence domain).
- With it: what happens if you do.
```

Every item must have both "Without it" and "With it". No case, no item.

**3. `What to skip`** , 2 to 4 lines. Only two things: (a) risky or against-the-rules tactics, as a warning; (b) things that look useful but do not apply to the reader (wrong platform, wrong stack). Drop pure jokes and lifestyle silently, do not list them.

**4. `In one line:`** a compressed plan, steps joined by arrows: step -> step -> step.

## What usually counts as signal (priors, not a template)

These help recognize signal for the default **maker** lens. Take only what is actually in the source, never pad.

- copy what already sells (proven demand), do not invent it;
- aim at a strong human desire (money, health, status, fear of falling behind);
- measure everything, pour effort only where money already lands;
- kill dead projects faster, run more cheap attempts;
- early signal means sales in the first days, not applause;
- charge from day one, keep value behind payment;
- strip friction from the first steps, push completion up;
- notify only the people who actually engage;
- test the price and the payment screen, not the button color.

For other lenses, the priors shift (engineer: simpler designs, fewer moving parts, tests that catch real bugs, observability; marketer: one channel done well, a sharp offer, proof over claims; researcher: primary sources, reproducible methods, falsifiable claims). Use them as a filter, not a checklist.

## Example (copy the form, not the content)

This is what the final answer looks like, maker lens, source is a viral "how I hit $1k/mo" thread. No leading `>` markers, no "Example" heading.

```text
Reality check: $1k/mo is small, and half the thread is flexing (gym, supplements, diet). Filter hard.

**What's worth taking**

1. **Copy what already sells**
He didn't invent an idea, he took a product that already makes money for others.
- Without it: you spend months building something nobody asked for, hoping they'll want it.
- With it: demand is already proven, so you only fight execution.

2. **Pour effort only where money already lands**
He exported all sales, found which countries and keywords actually pay, and spent more there.
- Without it: you spread time and budget evenly, including onto dead things.
- With it: you find the two or three channels that work and feed only those.

3. **Charge from day one**
The core sits behind a hard paywall, nothing valuable is free.
- Without it: people use it for free and never pay.
- With it: whoever wants the value pays, and you see real demand.

**What to skip**
- Bidding on competitors' names and asking for a review during onboarding (the store "patched" his trick): on the edge of the rules.
- Apple Ads, RevenueCat, SwiftUI: iOS-app stack only, not for you if you build on the web.

**In one line:** copy what already sells -> launch cheap -> watch where money shows up -> feed only that -> charge immediately.
```

## Common mistakes

- An item without "Without it / With it". Every item needs both cases.
- Items labeled by source numbers (#1, #5) instead of a title plus a one-line "what they did".
- Extra sections (meta-pattern, red flags, a separate "ignore" dump). Keep the 4 contract parts only; warnings go inside "What to skip".
- Retelling the whole source instead of selecting. Take only what passes the lens.
- Inventing what the source said when a fetch failed. If you couldn't read it, say so and ask for a paste.
