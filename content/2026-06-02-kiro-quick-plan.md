+++
title = "Kiro 0.12 Introduces Quick Plan: Spec-Driven Development Without the Wait"
date = 2026-06-02
description = "Kiro's new Quick Plan session type generates requirements, design, and tasks in a single pass — no approval gates, no back-and-forth."
[taxonomies]
tags = ["kiro", "ai", "tooling", "ide"]
+++

Kiro, the spec-driven AI IDE from AWS, shipped version 0.12 on May 6th[^1] with a feature that directly addresses one of the main friction points in its workflow: the approval gates between specification phases. The new **Quick Plan** session type generates requirements, design, and implementation tasks in a single uninterrupted pass.

<!-- more -->

## The problem Quick Plan solves

Kiro's core idea is that AI should plan before it codes. When you start a feature spec, Kiro walks through three phases — requirements written in EARS (Easy Approach to Requirements Syntax) notation[^2], technical design, and a task breakdown — producing artifacts saved to `.kiro/specs/`. Each phase traditionally requires your review and approval before the next one begins[^3].

That's valuable when you're exploring unfamiliar territory. But when you already know what you want — a variant of something you've built before, or a feature with well-understood requirements — those gates slow you down without adding much.

## How Quick Plan works

Quick Plan collapses the three-phase cycle into a single pass[^4]:

1. You select **Quick Plan** from the session type picker (alongside Feature Specs and Bugfix Specs).
2. Kiro asks 2–4 targeted clarifying questions upfront about scope, constraints, and edge cases — tailored to your detected tech stack[^5].
3. You answer those questions once.
4. Kiro autonomously generates all three artifacts — `requirements.md`, `design.md`, and `tasks.md` — without stopping for approval between them.
5. You land directly on the task list, ready to start implementation.

The output is identical in structure to what a standard Feature Spec produces. You get the same EARS-notation user stories, the same architectural design docs, the same linked task list. The artifacts are still editable after generation, and changes only trigger regeneration of the affected artifacts rather than rebuilding the entire spec[^5]. The difference is purely in the workflow: context gathered upfront, artifacts delivered together.

## What else shipped in 0.12

Two other features round out the release[^1]:

**Parallel task execution.** Kiro builds a dependency graph from your task list, identifies which tasks write to the same files and which are truly independent, then groups independent tasks into parallel waves. Each task runs in its own isolated context with no state leaking between them — if one fails, the others keep running[^5].

**Analyze Requirements.** A new analysis step powered by what Kiro calls neurosymbolic AI — combining LLMs with automated reasoning. It samples multiple interpretations of each requirement to find ambiguities and uses automated reasoning to validate logical consistency across the full set[^5]. This catches the kind of problems that otherwise surface mid-implementation.

## When to use Quick Plan (and when not to)

Quick Plan is a good fit when:

- You have clear requirements and trust Kiro to fill in the details
- You're prototyping and want to prioritize speed
- You've built something similar before

Stick with standard Feature Specs when:

- You're working in an unfamiliar domain where iterative refinement matters
- Compliance or safety requirements demand stakeholder review at each phase
- The feature is complex enough that reviewing the design before generating tasks will save rework

It's worth noting that Feature Specs themselves come in two workflow variants — requirements-first and design-first[^3] — so Quick Plan is really a third option on a spectrum from most to least human oversight. The key insight is that it doesn't sacrifice rigor for speed; the same artifacts get produced either way. It just removes the waiting when you don't need it.

[^1]: Kiro Team, "Kiro IDE 0.12 Changelog," kiro.dev, May 6, 2026. <https://kiro.dev/changelog/ide/0-12/>
[^2]: Kiro Team, "Introducing Kiro," kiro.dev, July 14, 2025. <https://kiro.dev/blog/introducing-kiro/>
[^3]: Kiro Team, "Feature Specs," Kiro Documentation. <https://kiro.dev/docs/specs/feature-specs/>
[^4]: Kiro Team, "Quick Plan," Kiro Documentation. <https://kiro.dev/docs/specs/quick-plan/>
[^5]: Ankit Sharma, "Specs just got faster (and smarter)," kiro.dev, May 12, 2026. <https://kiro.dev/blog/faster-smarter-specs/>
