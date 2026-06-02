# AI.md

Guidance for writing blog posts in my voice for **side-effects**, my personal blog.
When drafting or editing a post, follow this file unless I say otherwise in the prompt.

## About me

I'm Richard, a developer who works close to the systems layer and lives in a
declarative, reproducible world. I run NixOS and reach for a `shell.nix` / flake for
basically every project — embedded firmware, music tooling, presentations, web stuff —
so that environments are pinned and disposable rather than hand-installed and fragile.

I'm a polyglot and a language tinkerer: comfortable across Rust, Python, C/C++, and
Haskell, and curious about the long tail (I'll happily go read about Forth or uLisp on
a microcontroller). I lean functional in how I think about problems — I like pure
transformations of data, modular designs, and treating config and rules as
human-readable, version-controllable files rather than code baked into a binary. When I
start a project the brief is usually some version of "make it easy to read, easy to
maintain, and reproducible."

I'm a comparative thinker. Before committing to a tool or language I want to see the
option space and the tradeoffs — "compare and contrast X vs Y," "what else could I use
here." Posts should reflect that instinct: lay out the alternatives honestly, then make
a call.

My interests are wide and I write about whatever I'm currently nerding out on: NixOS and
declarative environments, embedded/hardware projects (ESP8266, Arduino), creative coding
and live-coding music (TidalCycles), AI-assisted development and tooling, and the
general craft of keeping a system reproducible and maintainable.

The blog name **side-effects** is a deliberate pun — side effects in the
functional-programming sense, and the fact that the interesting stuff often happens at
the edges of what you set out to do. Keep that wry, slightly self-aware spirit; don't
explain the joke in posts.

## What the blog is about

Practical, hands-on writing about tools and systems I actually use, usually born from a
real problem I hit rather than an abstract think-piece. Typical shapes: "here's how I
got X working on NixOS and what tripped me up," "X vs Y and which I picked," "a small
project and the design thinking behind it." The reproducibility angle and the
design-for-maintainability angle are recurring themes.

## Voice and tone

- **Direct and concise.** Lead with the point. No throat-clearing intros, no "in
  today's fast-paced world." If a sentence isn't carrying weight, cut it. (My own
  prompts are terse and to-the-point — posts should be too.)
- **Conversational, not corporate.** Write like I'm explaining something to a competent
  friend over a terminal session. Contractions are fine; a dry aside is welcome.
- **Pragmatic and opinionated, but not dogmatic.** I hold real positions — pin your
  versions, prefer human-readable config, reproducibility is worth the upfront cost —
  but I show the tradeoffs and how others do it rather than declaring one true way.
- **Comparative.** When there's a choice of tools/languages, map the options and their
  tradeoffs before landing on a recommendation. Don't pretend the chosen path is the
  only one.
- **Show the why, not just the recipe.** When I give steps, explain what each one is
  doing and which ones bite people. The gotchas are the most valuable part.
- **Lightly self-deprecating.** Comfortable saying when I got something wrong or learned
  the hard way. Never arrogant, never falsely modest.

## How to write the posts

- Open with the concrete problem or goal, not a definition. Substance in the first
  couple of sentences.
- Use code blocks generously; make them copy-pasteable and correct. Annotate the
  non-obvious lines. Nix snippets, shell commands, and config files are all fair game
  and expected.
- Prefer prose with targeted code/commands over wall-to-wall bullet lists. Use a short
  list only when steps are genuinely sequential.
- Call out failure modes explicitly — the "this is the part that catches everyone"
  notes.
- Assume a technically literate reader. Don't explain what a terminal is, but do explain
  anything tool-specific, NixOS-specific, or recently-changed.
- When relevant, note the reproducible/declarative way to do something, since that's how
  I actually work and what readers come here for.
- Reasonable length: long enough to solve the problem, short enough that there's no
  padding. Quality over word count.

## Don't

- Don't use hype or marketing language ("game-changing," "revolutionary," "unleash").
- Don't pad with filler, restated headers, or summary paragraphs that add nothing.
- Don't over-format — no emoji, no bold scattered through every sentence, no excessive
  headers on a short post.
- Don't claim certainty I don't have. If something's version-dependent or might have
  changed, say so.
- Don't write in a generic "tech blog" voice that could be anyone's. If a paragraph
  could appear unchanged on a corporate dev-rel blog, rewrite it.

## Audience

Other developers — people comfortable in a terminal, running or curious about NixOS, or
who just landed here from a search because they hit the same problem I did. Often
language-curious tinkerers like me. Write for the person trying to get something
working, not for an algorithm.

# Side Effects

A personal blog/website built with [Zola](https://www.getzola.org/), a fast static site generator written in Rust.

## Site details

- **Base URL:** https://sideeffects.kelly.ws
- **Syntax highlighting theme:** catppuccin-mocha
- **Taxonomy:** tags

## Project structure

```
zola.toml              # Site configuration
content/
  _index.md            # Homepage
  blog/
    _index.md          # Blog section (sorted by date, paginated by 10)
    test-post.md       # Example post
templates/
  index.html           # Homepage template
  page.html            # Single post template
  section.html         # Blog listing template
  taxonomy_list.html   # All tags listing
  taxonomy_single.html # Single tag listing
```

## Workflow

Zola is not installed on this machine. Changes are committed and pushed to the Gitea remote at `http://gitforge.home.arpa/richard/side-effects`. A build/deploy pipeline on that server handles `zola build`.

## Writing a new post

Create a Markdown file under `content/blog/` with this frontmatter:

```markdown
+++
title = "Post Title"
date = YYYY-MM-DD
description = "Short description."
[taxonomies]
tags = ["tag1", "tag2"]
+++

Post body. Use `<!-- more -->` to set the summary break.
```

## Templates

Templates use the [Tera](https://keats.github.io/tera/) templating engine. The current templates are minimal/unstyled HTML. Sass files can be placed in a `sass/` directory and will be compiled automatically (`compile_sass = true` in zola.toml).

