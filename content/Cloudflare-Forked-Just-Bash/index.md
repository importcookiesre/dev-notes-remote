---
title: Cloudflare Forked Just-Bash
description: Cloudflare recently forked just-bash (an open-source project from Vercel Labs designed for safe, sandboxed bash-like execution in AI agents) and published their version as @cloudflare/shell. What started as a simple fork turned into a weekend Twitter storm about open-source etiquette, security, and big-tech rivalries.
draft: false
tags:
modified: 2026-03-16
created: 2026-03-16
---

# G’day everyone – AbhikrX here

Welcome to my little spot on the web. This is my first post, so bear with me if it’s a bit rough around the edges. I’ve been keeping an eye on dev tools and AI agent stuff lately, and last weekend there was some proper juicy drama in the open-source scene between Vercel and Cloudflare. Thought it’d be worth a quick write-up.

---

## What’s just-bash all about?

**just-bash** is a clever little MIT-licensed library cooked up by Vercel Labs. The main idea is dead simple but bloody useful: let AI agents run bash-style commands in JavaScript environments without stuffing up the whole machine.

It packs some serious security muscle:

- Blocks dodgy JS tricks like `eval` and `Function` constructors  
- Stops prototype pollution attacks in their tracks  
- Runs the lot in a QuickJS/WASM sandbox  
- Uses AST transforms to check commands before they even run  
- Had optional (but risky) Python bits via pyodide at first, but they pulled back or ditched it cos proper isolation was a nightmare  

It’s still early days (beta feel in the README), but it’s built for agents that need to mess with filesystems, run CLIs, or do light scripting without needing a full VM. Low latency, no extra services – perfect for edge or local agent setups.

---

## Then Cloudflare had a crack at it

One of the Cloudflare blokes (Sunil Pai, @threepointone) forked the repo and chucked it up on npm as `@cloudflare/shell`.

The changes?

- Ditched the beta/experimental warnings  
- Brought back Python support (via pyodide) without the original safety nets  
- Stripped out a bunch of the defence-in-depth stuff, prototype pollution guards, and other hardening bits  

Vercel’s CTO, Malte Ubl (@cramforce), clocked it pretty quick and dropped a solid rundown (looked like an article-style thread on X). He called it a **“slop-fork”** – basically a half-arsed copy that made things less secure and yanked important warnings, which could trick people into thinking it’s better or official.

Vercel boss Guillermo Rauch (@rauchg) jumped in too, saying it fits a bigger pattern: Cloudflare forking ecosystem projects (like that Vinext/Next.js stuff earlier) to nudge devs towards their own edge runtime instead of sticking with open Node.js standards.

---

## How it played out

Sunil fired back publicly: nah mate, it was just a personal experiment to play with runtime hooks for Cloudflare Workers. No official company push. He owned up that skipping the “experimental” label was a stuff-up and said he’d hit up the Vercel crew on Monday to chat about contributing properly.

There was a bit of back-and-forth – some spicy quotes, memes, and jabs about “forking the whole bloody ecosystem” – but the main players had a private yarn, sorted it, and the whole thing blew over in less than two days. Fair dinkum quick resolution.

---

## Why it actually matters, eh?

Legally? All good. MIT licence lets you fork, tweak, and republish as long as you give credit.

But open source isn’t just about licences – it’s about trust and how we do things:

- Forking security-sensitive early projects is risky  
- You’re taking a snapshot that might miss later fixes or the full story  
- Yanking hardening features (especially if you don’t get why they’re there) can cause real headaches  
- Slapping a big-company name like `@cloudflare/` on it makes it look mature and endorsed when it’s not  
- When the big players fork small projects without a heads-up or upstream PRs, it can feel a bit extractive  

We’ve seen tension between Vercel and Cloudflare before (Vinext, runtime fights, etc.), but they handled this one like grown-ups in the end.

---

## A few takeaways

- **Maintainers:** Spell out your security reasons clearly. The “why we did X” history is gold  
- **Users:** Always diff the forks yourself. A fancy brand doesn’t mean it’s safer or better  
- **Experimenters:** Chuck an “experimental” tag on it, give folks a yell before you publish a branded fork – especially on security stuff  

Open source works best when we share, but also when we have a quick chat first. A DM can save everyone a weekend headache.

---

## What do you reckon?

Is forking always fair go under permissive licences, or should there be stronger unwritten rules for security-sensitive libs?


---

**Cheers for reading my first go at this.**  
More to come on AI agents, dev tools, and whatever madness the ecosystem throws up next.

**AbhikrX**  
@importcookiesre  
Ranchi, March 2026