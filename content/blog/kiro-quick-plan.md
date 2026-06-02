+++
title = "Kiro 0.12 Introduces Quick Plan: Spec-Driven Development Without the Wait"
date = 2026-06-02
description = "Kiro's new Quick Plan session type generates requirements, design, and tasks in a single pass — no approval gates, no back-and-forth."
[taxonomies]
tags = ["kiro", "ai", "tooling", "ide"]
+++

Kiro, the spec-driven AI IDE, shipped version 0.12 on May 6th with a feature that directly addresses one of the main friction points in its workflow: the approval gates between specification phases. The new **Quick Plan** session type generates requirements, design, and implementation tasks in a single uninterrupted pass.

<!-- more -->

## The problem Quick Plan solves

Kiro's core idea is that AI should plan before it codes. When you start a feature spec, Kiro walks through three phases — requirements (user stories in EARS notation), technical design, and a task breakdown — producing artifacts saved to `.kiro/specs/`. Each phase traditionally requires your review and approval before the next one begins.

That's valuable when you're exploring unfamiliar territory. But when you already know what you want — a variant of something you've built before, or a feature with well-understood requirements — those gates slow you down without adding much.

## How Quick Plan works

Quick Plan collapses the three-phase cycle into a single pass:

1. You select **Quick Plan** from the session type picker (alongside Feature Specs and Bugfix Specs).
2. Kiro asks clarifying questions *upfront* about scope, constraints, and edge cases.
3. You answer those questions once.
4. Kiro autonomously generates all three artifacts — `requirements.md`, `design.md`, and `tasks.md` — without stopping for approval between them.
5. You land directly on the task list, ready to start implementation.

The output is identical in structure to what a standard Feature Spec produces. You get the same EARS-notation user stories, the same architectural design docs, the same linked task list. The artifacts are still editable after generation, so you can refine anything that needs adjustment. The difference is purely in the workflow: context gathered upfront, artifacts delivered together.

## What else shipped in 0.12

Two other features round out the release:

**Parallel task execution.** Kiro now analyzes task dependencies and groups independent tasks into parallel waves instead of running them sequentially. For specs with four or more independent tasks, this can cut execution time significantly.

**Analyze Requirements.** A new analysis step that identifies logical inconsistencies, ambiguities, conflicting constraints, and gaps in your requirements before design begins. This catches the kind of problems that otherwise surface mid-implementation.

## When to use Quick Plan (and when not to)

Quick Plan is a good fit when:

- You have clear requirements and trust Kiro to fill in the details
- You're prototyping and want to prioritize speed
- You've built something similar before

Stick with standard Feature Specs when:

- You're working in an unfamiliar domain where iterative refinement matters
- Compliance or safety requirements demand stakeholder review at each phase
- The feature is complex enough that reviewing the design before generating tasks will save rework

The key insight is that Quick Plan doesn't sacrifice rigor for speed — the same artifacts get produced either way. It just removes the waiting when you don't need it.
