+++
title = "SPF: Choosing Software by Fit, Not by Ladder"
date = 2026-06-04
description = "Automation maturity models tell you to climb one rung at a time. But rungs aren't stations you have to stop at — and while you wait, the market keeps shipping new entrants that can carry you further, with better documentation, than anything you'd build yourself. SPF — Stack, Paradigm, Fit — is a fit-first way to choose software."

[taxonomies]
tags = ["architecture", "tooling", "buy-vs-build", "decision-making"]

[extra]
toc = true
+++

The decision my team actually faces, over and over, isn't "how mature is our automation?" Nobody has a budget line for maturity. The decision is concrete and recurring: *we have this need — what should we go get to meet it?* Build a script? Adopt a declarative tool? Buy the platform with the AI features? Wait?

For a long time we answered that question with the same instrument everyone reaches for: an automation maturity ladder. Manual at the bottom, scripted above it, declarative above that, something autonomous and self-healing at the top. The ladder gives you a comfortable story — you're *here*, you should get to *there*, one rung at a time. And it's quietly wrong in a way that costs money, because it treats the rungs as stations you have to stop at, and it treats "up" as the goal.

This post is about a better way to make that call. It reframes the maturity ladder as a selection map, swaps "how high have we climbed?" for "what does this need actually require?", and then does the thing the ladder forbids: it tells you when to **skip a rung entirely** — and when to wait, because the market is about to hand you a better one.

We started calling it **SPF**, for the three things it makes you look at: the **Stack** (where in your architecture the need lives), the **Paradigm** (how a solution works, from manual to autonomous), and the **Fit** (how close a candidate sits to what the need actually requires). The name is a half-joke — it's protection against over-exposure, except the thing you're guarding against is over-engineering — but the three letters really are the whole method.

## The ladder lie: rungs aren't stations

The ladder mindset smuggles in an assumption nobody defends out loud: that you progress through the levels in order. Script it now, declare it later, get smart about it eventually. Each rung is a place you pause before climbing to the next.

But the rungs aren't stations. They're just positions describing how a solution works — done by hand, by imperative steps, by declared state, by gated AI assistance, or by ungated autonomy. There is no law that says you must occupy the imperative rung before you're allowed to stand on the declarative one. And when a mature declarative solution already exists for your problem, deliberately building the imperative version first is one of the most common, most expensive mistakes a team can make. You pay the setup cost to build it, you pay the operational cost to run it, and then — because it was always a stopgap — you pay a *migration* cost to replace it with the thing you could have adopted on day one.

So the real question isn't "what's the next rung up?" It's "what rung does this need actually call for, and what's the cheapest way to land directly on it?"

## A selection map, not a ladder

To answer that, you need to locate the need on a map rather than a line. The map has two axes.

The **vertical axis** is where in the stack the need lives — provisioning, host configuration, service configuration, schema, orchestration, data products. These are ordered by dependency: each layer is built on the one beneath it, and the test is simply "could this layer work if the one below it didn't exist?" Orchestration needs a configured database; the database doesn't need the orchestrator. So you read the stack bottom-up, and you tend to solve foundational needs first because everything above them depends on it.

The **horizontal axis** is *how* a solution works, scored 0 to 4: manual, imperative, declarative, intelligent-assisted (AI proposes, a deterministic gate disposes), and autonomous (the system decides and acts at runtime). Moving right hands more of the work to the machine and makes your input more abstract — you go from doing the steps, to writing the steps, to declaring the end state, to stating a goal.

A need is a point on this map: a layer, and a level that the layer's problem requires. Choosing software is the act of going and getting a solution that sits on that point. The vertical axis is the **S** in SPF, the horizontal axis is the **P**, and the **F** — Fit — is the score you compute once you know where the need sits and where a candidate solution would land.

## Pick the level the need requires — not the highest one available

The engine underneath the whole method is a single principle: **choose the least powerful paradigm that fully fits the problem.** More automation is not better in the abstract. Moving right buys delegation and adaptivity but spends determinism, transparency, and control, and it raises the up-front cost. The cost-and-risk of operating at each level is U-shaped: high on the left (manual toil, drift, errors), high on the right (waste, complexity, fragility), and lowest at the level the problem actually needs.

That needed level — call it the **fit level** — is set by demand, not aspiration. Roughly: a one-off needs level 0–1; a repeated, drift-prone, *specifiable* need wants declarative at level 2; reach for the AI-assisted level 3 only when gated automation adds real value; and the autonomous level 4 is justified only when the desired state genuinely can't be written down. Score a solution by its *signed* distance from that fit level, so that being over-built (a flashy tool for a trivial need) counts as a failure exactly the way being under-built does.

With the fit level in hand, selection becomes targeting practice: land on it directly.

## Leapfrogging: skip the rungs you'd only leave

Here's where the ladder mindset does the most damage and the map does the most good. If your fit level is declarative and a mature declarative tool exists, **skip the imperative rung.** Don't build the script you'd migrate away from. Adopt the tool that already sits where you're going.

The economics make this more than a tidiness preference. The reason "build it yourself" pays off only at scale is its high fixed cost — somebody has to model the desired state, learn the tooling, make everything idempotent. A packaged declarative product means *someone else already paid that fixed cost*. Adopting it drops your intercept dramatically, which pulls the break-even point sharply to the left: a bought declarative tool can be worth it at a handful of repetitions, where *building* your own wouldn't pay off until dozens. The existence of a mature product changes the buy-versus-build math, and it almost always argues for skipping the rung you were only going to stand on briefly.

One asymmetry keeps this honest, and it's the difference between leapfrogging and overshooting: **you skip up to the fit level, never past it.** A mature declarative tool is a reason to skip imperative. A glossy autonomous, AI-driven platform is *not* a reason to skip declarative, if declarative is what your problem needs — that's just over-service wearing a sales deck. Leapfrog to the fit level efficiently; don't let a vendor's roadmap talk you above it.

## Waiting is a move too: the market keeps shipping better rungs

The ladder mindset has one more blind spot, and it's the one I most want to flag: it treats *not adopting yet* as pure failure — a rung you haven't climbed. But waiting is a legitimate move with real upside, because the map isn't static. New entrants ship constantly, and they don't always land on the rung you expected.

If you resist the urge to build a stopgap at today's available level, you keep your options open for a tool that hasn't arrived yet — and new entrants increasingly compete not just on *doing the job* but on what they throw off *around* the job. The clearest example is artifacts. A solution doesn't only execute; it can leave behind durable, valuable side effects — documentation, lineage graphs, audit trails, a version-controlled record of intent. Those artifacts are a first-class selection criterion, not a nice-to-have, because they're what make a system reviewable, reproducible, and defensible to an auditor.

This is where the paradigm axis pays off again. A declarative solution gives you the artifact almost for free: the spec *is* the documentation, version-controlled and authoritative, because the system does what the spec says by construction. The behavior and the docs physically can't drift apart. So a team that waited out the imperative era and adopted a mature declarative entrant didn't just get a better execution model — it got a documentation and audit story it would never have gotten from the homegrown scripts it didn't build. The new entrant leapfrogged it forward *and* handed it richer artifacts as part of the deal.

(One caution so the artifact argument stays grounded: this peaks at the declarative and gated-assisted levels. A fully autonomous system can *generate* unlimited documentation, but those documents describe its behavior after the fact rather than determine it — they're less authoritative, not more. So "more artifacts" is a reason to favor a strong declarative entrant, not a reason to chase the most autonomous one.)

Waiting isn't free, of course — while you wait you bear the manual toil and drift you haven't solved yet. So this is an option-value judgment, not a blanket "always wait." But the ladder never even puts the option on the table, and it should be on the table: sometimes the right selection is the tool that doesn't exist yet, and the right move today is a cheap interim that you can abandon without regret.

## SPF in one picture

Here is SPF reading three capabilities. Each row is a need located on the map — its stack layer and the paradigm level it requires — scored against where it sits today. The **Fit** column is signed: negative is under-served, positive is over-built, zero is matched.

| Capability | Stack layer | Needs | Today | Fit | The move |
|---|---|:---:|:---:|:---:|---|
| Database configuration | 3 · service config | 2 · declarative | 0 · manual | **−2** | Adopt a mature declarative tool — skip the imperative rung |
| One-off data backfill | 5 · orchestration | 1 · imperative | 4 · autonomous | **+3** | Over-built — replace the agent with a script |
| Schema migrations | 4 · schema | 2 · declarative | 2 · declarative | **0** | Matched — leave it alone |

Plotted on the map, the point is unmistakable: every recommended move is *toward* the fit level, from whichever direction the capability is off it.

<figure style="margin:2rem 0;">
<svg width="100%" viewBox="0 0 760 450" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="An SPF map plotting three capabilities. Database configuration sits at manual and should leapfrog rightward to declarative, skipping imperative. A one-off backfill sits at autonomous and should move left to imperative because it is over-built. Schema migrations already sit at declarative, their fit level, and should be left alone.">
<defs>
<marker id="ah-blue" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M1 1 L9 5 L1 9 Z" fill="#2E6FB5"/></marker>
<marker id="ah-amber" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M1 1 L9 5 L1 9 Z" fill="#C2702C"/></marker>
</defs>
<text x="20" y="26" font-family="system-ui, sans-serif" font-size="14" font-weight="600" fill="#24221E">An SPF read of three capabilities</text>
<circle cx="524" cy="22" r="5" fill="#ffffff" stroke="#6B655B" stroke-width="1.5"/>
<text x="536" y="26" font-family="system-ui, sans-serif" font-size="11" fill="#6B655B">current</text>
<circle cx="612" cy="22" r="5" fill="#6B655B"/>
<text x="624" y="26" font-family="system-ui, sans-serif" font-size="11" fill="#6B655B">fit target</text>
<line x1="227" y1="60" x2="227" y2="390" stroke="#E5E2DA"/>
<line x1="341" y1="60" x2="341" y2="390" stroke="#E5E2DA"/>
<line x1="455" y1="60" x2="455" y2="390" stroke="#E5E2DA"/>
<line x1="569" y1="60" x2="569" y2="390" stroke="#E5E2DA"/>
<line x1="683" y1="60" x2="683" y2="390" stroke="#E5E2DA"/>
<line x1="170" y1="87.5" x2="740" y2="87.5" stroke="#E5E2DA"/>
<line x1="170" y1="142.5" x2="740" y2="142.5" stroke="#E5E2DA"/>
<line x1="170" y1="197.5" x2="740" y2="197.5" stroke="#E5E2DA"/>
<line x1="170" y1="252.5" x2="740" y2="252.5" stroke="#E5E2DA"/>
<line x1="170" y1="307.5" x2="740" y2="307.5" stroke="#E5E2DA"/>
<line x1="170" y1="362.5" x2="740" y2="362.5" stroke="#E5E2DA"/>
<g font-family="system-ui, sans-serif" font-size="11.5" fill="#24221E" text-anchor="end">
<text x="158" y="87.5" dominant-baseline="central">6 · Data products</text>
<text x="158" y="142.5" dominant-baseline="central">5 · Orchestration</text>
<text x="158" y="197.5" dominant-baseline="central">4 · Schema</text>
<text x="158" y="252.5" dominant-baseline="central">3 · Service config</text>
<text x="158" y="307.5" dominant-baseline="central">2 · Host / OS</text>
<text x="158" y="362.5" dominant-baseline="central">1 · Provisioning</text>
</g>
<g font-family="system-ui, sans-serif" font-size="11" fill="#6B655B" text-anchor="middle">
<text x="227" y="410">0 manual</text>
<text x="341" y="410">1 imperative</text>
<text x="455" y="410">2 declarative</text>
<text x="569" y="410">3 assisted</text>
<text x="683" y="410">4 autonomous</text>
</g>
<text x="455" y="434" font-family="system-ui, sans-serif" font-size="10.5" fill="#9A948A" text-anchor="middle">paradigm  ·  more control ←→ more delegation</text>
<line x1="676" y1="142.5" x2="348" y2="142.5" stroke="#C2702C" stroke-width="2" marker-end="url(#ah-amber)"/>
<circle cx="683" cy="142.5" r="6" fill="#ffffff" stroke="#C2702C" stroke-width="2"/>
<circle cx="341" cy="142.5" r="6" fill="#C2702C"/>
<text x="512" y="131" font-family="system-ui, sans-serif" font-size="11" fill="#C2702C" text-anchor="middle">over-built — dial back</text>
<circle cx="455" cy="197.5" r="6" fill="#2F8E6B"/>
<text x="455" y="186" font-family="system-ui, sans-serif" font-size="11" fill="#2F8E6B" text-anchor="middle">matched — leave it</text>
<line x1="234" y1="252.5" x2="448" y2="252.5" stroke="#2E6FB5" stroke-width="2" marker-end="url(#ah-blue)"/>
<circle cx="227" cy="252.5" r="6" fill="#ffffff" stroke="#2E6FB5" stroke-width="2"/>
<circle cx="455" cy="252.5" r="6" fill="#2E6FB5"/>
<text x="341" y="241" font-family="system-ui, sans-serif" font-size="11" fill="#2E6FB5" text-anchor="middle">leapfrog — skip imperative</text>
</svg>
<figcaption style="font-size:0.9em; color:#6B655B; text-align:center; margin-top:0.5rem;">The same three capabilities on the SPF map. Vertical position is the stack layer; horizontal position is the paradigm. Every move points toward the fit level — rightward for the under-served database config (skipping the imperative rung), leftward for the over-built backfill, and nowhere at all for the already-matched migrations.</figcaption>
</figure>

The blue arrow is the leapfrog: configuration is a textbook declarative need sitting at manual, so the move is straight to a declarative tool, stepping over imperative entirely. The amber arrow is the correction the ladder can't see: a one-off backfill someone built as an autonomous agent is *over* its fit level, and the right move is down, not up. And schema migrations, already sitting exactly where they belong, score a fit of zero and earn the rarest recommendation in software — leave them alone.

## Selecting well: the diligence the map doesn't do for you

Skipping and waiting both depend on judgment the map can't make for you. Before you leapfrog onto a declarative tool, two things have to be true. It has to genuinely *cover* your problem — partial coverage is a trap, because the gaps drag you right back into imperative glue code, and now you're running a hybrid that's harder than either pure option. And your team has to be able to *operate* it — a convergence engine is a real skill cost, and a tool you can't run confidently is a worse fit than a humbler one you can. When those don't hold, the lower rung can be the correct place to rest after all.

## Where this needs more work

I want to be honest about the two soft spots, because they're exactly the inputs a real purchasing decision runs on.

The first is **assessing need.** Today the fit level comes from a four-line rubric — repetition, volatility, specifiability — which is directionally right but hand-wavy. To trust it for a spend decision you'd want structured, measurable inputs: runs per quarter, number of target environments, change rate, blast radius, regulatory exposure, whether the desired state is even expressible. Turning that into a defensible demand model is the next piece of work.

The second is **evidence of real gains.** The map tells you the *direction* of a good move but not its *magnitude*, and magnitude is what justifies budget. There's a usable evidence base to draw on — delivery and recovery metrics, studies on infrastructure-as-code adoption, measured reductions in drift incidents and audit-prep time — and pulling those numbers in is what turns a sensible-sounding reframe into an actual business case.

## The reframe

You're not climbing a ladder. You're choosing software to meet a need, and the goal is to land on the level that need requires — no lower, no higher.

That single shift changes the moves available to you. You can skip the rungs you'd only leave, adopting a mature solution directly instead of building a stopgap you'll migrate away from. You can refuse to overshoot, declining the autonomous platform when declarative is what the problem wants. And you can treat waiting as a real option, because the market keeps shipping new entrants — often with better artifacts, better documentation, a better audit story — than anything you'd assemble yourself.

The ladder asks "how high have we climbed?", and the answer is always "not high enough," forever. SPF asks "what does this need require, and what's the cheapest way to land exactly there?" — and that question has an answer you can actually finish.
