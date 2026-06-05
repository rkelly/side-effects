+++
title = "Introducing SPF: An Assessment for Choosing Software by Fit"
date = 2026-06-04
description = "SPF — Stack, Paradigm, Fit — is a lightweight assessment method for deciding what software to adopt for a given need. It borrows a layered-architecture model for one axis and braids two automation standards for the other, then scores a solution by how closely it fits the need rather than how advanced it is."

[taxonomies]
tags = ["architecture", "tooling", "automation", "decision-making"]

[extra]
toc = true
+++

The recurring decision this method was built for is mundane to state and surprisingly hard to settle: given a need, what software should we adopt to meet it, and in what order relative to everything else on the roadmap? The instance that prompted it was a sequencing question — whether to put declarative configuration of our database environment ahead of building pipeline orchestration. Both were defensible. What was missing wasn't an opinion; it was an honest way to *score* the choice, so the answer rested on something more durable than whoever argued longest.

<!-- more -->

The instrument most teams reach for is an automation maturity ladder: manual at the bottom, scripted above it, declarative above that, autonomous at the top, with an implied instruction to climb. The trouble is that the ladder encodes a claim that doesn't survive scrutiny — that more automation is always better. It isn't. Over-automating a problem is a real and expensive failure mode, and a ladder is structurally incapable of seeing it, because it only measures height.

So the intention behind SPF was narrow and practical: a way to choose software that is matched to what a need actually requires, and that treats over-engineering as the failure it is. That intention has good precedent. The levels-of-automation literature has long framed its central question not as *how high can we go* but as which functions should be automated *and to what extent* — an explicitly bidirectional question (Parasuraman, Sheridan & Wickens, 2000). SPF takes that same posture and points it at software selection.

## What SPF is

SPF assesses a need, and the candidate solutions that could meet it, and produces a recommendation you can defend rather than merely assert. The name is the method — it makes you look at three things:

- **Stack** — where in your architecture the need lives.
- **Paradigm** — how a candidate solution works, from manual to autonomous.
- **Fit** — how closely a solution sits to what the need actually requires.

The first two are axes you locate a need on; the third is the score you compute from them. (The name is lifted from sunscreen on purpose: it's protection against over-exposure — except the exposure you're guarding against is over-engineering.)

Each axis deliberately borrows its shape from an existing, well-understood standard, so the method isn't asking you to trust something invented from scratch.

## The Stack axis: a layered-architecture model

The vertical axis is an abstraction stack, and it's the same shape as the layered models software has used for decades — most familiarly the OSI networking model, where each layer is built on the one beneath it and consumes the abstractions the lower layer exposes without needing to know its internals. Read bottom to top: infrastructure provisioning, host and OS configuration, service configuration, schema and data model, orchestration, and finally data products with their lineage and observability.

The ordering is by dependency, and there's a falsifiable test for it: *could this layer operate correctly if the one beneath it did not exist?* Orchestration needs a configured, reachable database; a configured database doesn't need an orchestrator. So configuration sits genuinely beneath orchestration — a fact about the dependency graph, not a matter of taste.

That observation is what settled the sequencing question that started this. Orchestration is a coordinator: it acts on servers, accounts, and permissions that have to exist first, and the configuration layer is what produces them. You can't orchestrate against infrastructure you haven't defined yet. "Configuration first" stopped being a preference and became a consequence of where the two needs sit on the stack.

Like every layered model, the axis is unambiguous for distant layers and a little blurry for adjacent ones (service configuration and schema bleed into each other). That's a property of the genre, not a defect, and you use the axis for the distinctions it draws cleanly.

## The Paradigm axis: two standards, braided

The horizontal axis is where SPF does something less conventional: it braids *two* existing standards that each capture half of what "how a solution works" means.

The first strand is the **levels-of-automation** scale — how much of the work is delegated to the machine. This is old, well-studied ground, beginning with Sheridan and Verplank's levels of automation (1978) and refined by Parasuraman, Sheridan and Wickens (2000); its most familiar descendant is SAE J3016, the levels of driving automation from 0 to 5 that anchor "Level 0" at no automation. These scales measure *who decides and acts* — human, machine, or some split.

The second strand is the **imperative-versus-declarative** distinction — *how the behaviour is specified*. This comes from a different tradition: the desired-state configuration lineage, where you declare an end state and an engine converges to it, formalized in Mark Burgess's work on convergent configuration and promise theory (1995 onward). The design principle that governs it is the **rule of least power** (Berners-Lee & Mendelsohn, 2006): choose the least powerful language sufficient for a purpose, because less power is more predictable and more reusable.

SPF's five paradigm levels are what you get when you lay those two strands along one line:

- **0 — Manual.** You do it by hand; nothing is encoded.
- **1 — Imperative.** You write the steps; a machine runs them. You own the *how*.
- **2 — Declarative.** You declare the desired *state*; an engine reaches it and corrects drift. You own the *what*.
- **3 — Intelligent-assisted.** You give a goal; an AI or heuristic proposes a change, but a deterministic gate stays in charge.
- **4 — Autonomous.** You give a goal; the system senses, decides, and acts at runtime, ungated.

It matters to be honest that this is a braid. The two strands co-vary in infrastructure tooling, which is why one axis is workable, but they are independent in principle — Terraform is fully declarative yet decides nothing at runtime, and a hand-coded control loop is highly autonomous yet thoroughly imperative. Neither levels-of-automation nor the imperative/declarative distinction can express the other on its own; the SAE-style scales say nothing about specification style, and the paradigm distinction says nothing about autonomy. For a tool that sits off the diagonal where the two strands part ways, score it by its autonomy and flag it for a closer look.

## Scoring fit: the heart of the method

This is what separates SPF from a relabeled ladder. Rather than scoring how high a solution climbs, you score how *close* it lands to what the need requires. Three numbers per capability.

**The needed level, N** — derived from demand, not aspiration. Repetition, volatility, blast radius, and scale push N up; specifiability and longevity cap it:

- One-off or rarely repeated → N = 0–1
- Repeated, drift-prone, *specifiable*, long-lived → N = 2
- The above, plus real value from AI-accelerated authoring behind a gate → N = 3
- Desired state genuinely *unspecifiable* and runtime adaptation essential → N = 4

The last line is load-bearing: an unspecifiable problem is the *only* legitimate reason to reach level 4. If you can write the desired state down, you almost certainly shouldn't be paying for runtime inference.

**The fit, which is Current minus N — and it is signed.** Zero is matched. Negative is under-served: you pay in manual toil, drift, and errors. Positive is over-served: you pay in setup cost, complexity, fragility, and lost authority. Both directions are failures, and the signed gap is exactly what a maturity ladder cannot represent — a ladder only ever reports "not high enough."

**The priority, which is the absolute fit times the stakes** (a 1–3 weight from risk and frequency), so a small misfit on a critical system outranks a large misfit on something trivial.

Underneath all three sits the rule of least power, and it has an economic shape worth drawing out, because it's the part that makes the scoring more than a slogan.

## The economics behind the U

Effort and cost along the paradigm axis split into a fixed part and a marginal part that move in opposite directions. The up-front cost *rises* as you move right: manual costs almost nothing to start, while declarative is expensive to begin — you model the desired state, learn the tooling, make operations idempotent. But the marginal cost — per run, per environment — *falls* as you move right: manual costs the same every time and never amortizes, while declarative re-applies and self-heals for almost nothing.

So total cost is a trade between a rising fixed cost and a falling marginal one, and where they cross depends on how many times you'll run the thing. Plotted against paradigm level, the total cost-and-risk of operating a capability is **U-shaped**: high on the left from toil and drift, high on the right from waste and fragility, lowest at the level the need actually requires. The job of an SPF score is to find the bottom of that U — not the top of the ladder.

This is also why a *bought* solution changes the maths. A packaged declarative tool means someone else already paid the fixed modeling cost; adopting it drops your intercept and pulls the break-even point sharply left, so it can be worth adopting at a fraction of the repetition that would justify building your own.

## SPF in one picture

To make it concrete, here is SPF reading three illustrative capabilities. Each row locates a need on the map — its stack layer and the paradigm level it requires — and scores it against where it sits today. The **Fit** column is signed: negative is under-served, positive is over-built, zero is matched.

| Capability | Stack layer | Needs | Today | Fit | The move |
|---|---|:---:|:---:|:---:|---|
| Database configuration | 3 · service config | 2 · declarative | 0 · manual | **−2** | Adopt a mature declarative tool |
| One-off data backfill | 5 · orchestration | 1 · imperative | 4 · autonomous | **+3** | Over-built — replace the agent with a script |
| Schema migrations | 4 · schema | 2 · declarative | 2 · declarative | **0** | Matched — leave it alone |

<figure style="margin:2rem 0;">
<svg width="100%" viewBox="0 0 760 450" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="An SPF map plotting three capabilities. Database configuration sits at manual and should move rightward to declarative. A one-off backfill sits at autonomous and should move left to imperative because it is over-built. Schema migrations already sit at declarative, their fit level, and should be left alone.">
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
<text x="341" y="241" font-family="system-ui, sans-serif" font-size="11" fill="#2E6FB5" text-anchor="middle">move up to declarative</text>
</svg>
<figcaption style="font-size:0.9em; color:#6B655B; text-align:center; margin-top:0.5rem;">Three capabilities on the SPF map. Vertical position is the stack layer; horizontal position is the paradigm. Every recommended move points toward the fit level — rightward for the under-served database config, leftward for the over-built backfill, and nowhere at all for the already-matched migrations.</figcaption>
</figure>

The signed fit is what makes the middle row legible. On a paradigm axis alone it scores a 4 — top of the ladder, apparently exemplary. SPF reads it as over-built: runtime autonomy spent on a job that runs once, where an imperative script is the better fit. And the matched migrations score a priority of zero and earn the rarest recommendation in software — leave them alone.

## What the scores tell you to do

A fit score isn't only a diagnosis; it points at an action, and the actions are more varied than "climb."

When a capability is under-served and a mature solution already exists at its fit level, **adopt it directly rather than building your way up one rung at a time.** Writing the imperative version first, then migrating to the declarative tool later, pays the setup cost twice; the buy-versus-build maths above is why skipping the intermediate rung is usually correct.

When a capability is over-served, the action is to **dial back** — the move no maturity ladder will suggest. Reducing capability, and the cost and fragility that came with it, is a legitimate and money-saving outcome of an assessment.

And sometimes the action is to **wait**, because the map isn't static and new tools increasingly compete on what they leave behind, not just what they execute. A declarative solution hands you the specification as a version-controlled, authoritative record of intent — Burgess's original point about convergent configuration was precisely that the automation code *is* a description of the desired end state. A strong new entrant may offer richer artifacts still: lineage, audit trails, reproducible documentation. Those artifacts are a real selection criterion because they make a system reviewable and defensible later. One caution keeps it honest: artifact authority peaks around the declarative and gated-assisted levels and *drops* at full autonomy, where generated documentation describes behaviour after the fact rather than determining it — so "better artifacts" argues for a strong declarative tool, not the most autonomous one. Waiting carries the cost of the toil you haven't yet solved, so it's an option-value call — but it's a call SPF puts on the table and the ladder never does.

## Where SPF still needs work

Two parts are softer than the rest, and they're the inputs a real spend decision depends on.

The first is **assessing need.** Today N comes from a short rubric — repetition, volatility, specifiability — which is directionally right but hand-wavy. To trust it for a purchasing decision you'd want structured, measurable inputs: runs per quarter, number of target environments, change rate, blast radius, regulatory exposure. Turning that into a defensible demand model is the next piece of work, and the levels-of-automation literature's function-by-function analysis is a reasonable starting frame for it.

The second is **evidence of real gains.** SPF tells you the *direction* of a good move but not its *magnitude*, and magnitude is what justifies budget. There's a usable evidence base — for example the DevOps delivery-and-recovery metrics popularized by the DORA research and *Accelerate* — and pulling those numbers in is what turns a sensible reframe into a business case.

## Fit, not altitude

That's SPF: locate a need on the **Stack**, judge a solution on the **Paradigm**, and score the **Fit** between them. It came out of a sequencing decision we had no honest way to score, and the move that unstuck it was giving up altitude as the goal.

A maturity ladder asks "how high have we climbed?", and the answer is always "not high enough," forever. SPF asks "what does this need require, and what's the closest, cheapest way to land exactly there?" — and that question has an answer you can reach, and then stop.

## References

The thinking here leans on a few existing standards and ideas, one set per axis:

- **Sheridan, T. B., & Verplank, W. L. (1978).** *Human and Computer Control of Undersea Teleoperators.* MIT Man-Machine Systems Laboratory. — The original levels-of-automation scale. [semanticscholar.org](https://www.semanticscholar.org/paper/Human-and-Computer-Control-of-Undersea-Sheridan-Verplank/d48b94e6af5093e7cc41e20fa6aca4f3a2d860bb)
- **Parasuraman, R., Sheridan, T. B., & Wickens, C. D. (2000).** "A Model for Types and Levels of Human Interaction with Automation." *IEEE Transactions on Systems, Man, and Cybernetics — Part A,* 30(3), 286–297. — Frames automation as a choice of *what to automate and to what extent*, the posture SPF adopts. [doi:10.1109/3468.844354](https://doi.org/10.1109/3468.844354)
- **SAE International, J3016.** *Taxonomy and Definitions for Terms Related to Driving Automation Systems for On-Road Motor Vehicles.* — The 0–5 driving-automation levels; the model for anchoring "Level 0" at no automation. [sae.org](https://www.sae.org/standards/content/j3016_202104/)
- **Berners-Lee, T., & Mendelsohn, N. (2006).** *The Rule of Least Power.* W3C Technical Architecture Group Finding. — Choose the least powerful tool sufficient for the purpose. [w3.org](https://www.w3.org/2001/tag/doc/leastPower.html)
- **Burgess, M. (1995).** "Cfengine: a site configuration engine." *USENIX Computing Systems,* 8(3). — The convergent, desired-state configuration model (later, promise theory) underpinning the declarative end of the paradigm axis. [markburgess.org](http://markburgess.org/cv.html)
- **ISO/IEC 7498-1.** *The Basic Reference Model (OSI).* — The layered-architecture template the Stack axis follows.
- **Forsgren, N., Humble, J., & Kim, G. (2018).** *Accelerate: The Science of Lean Software and DevOps.* — Delivery and recovery metrics, a starting point for sizing the gains an SPF-recommended move would produce.
