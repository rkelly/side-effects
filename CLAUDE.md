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
