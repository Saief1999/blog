# Saief's Personal Blog

## Overview

This repository contains my personal blog. Powered by [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

## Quick Start

Start by installing [Hugo](https://gohugo.io/installation/)

To add a new post, simply run

```bash
hugo new posts/my-new-post.md
```

This will create a new post based on `archetypes/default`

### Drafts

Each new post created based on the default archetype is a draft. **Drafts are built by default** and marked with `[draft]`. To change this behavior change `buildDrafts:false` in `config.yml`

After finishing the writing process we need to manually change `draft` to `false` in the header part of the markdown file.

## Cover Images

We can add cover images to our articles. To do that, paste this into the header part of the article then change as necessary.

```yaml
cover:
    image: "<image local path/remote URL>"
    alt: "<image alt text>"
    caption: "<image caption>" 
    relative: true/false # To use relative path for cover image, used in hugo Page-bundles 
```

### Starting the local server

```bash
hugo serve
```

## Quick References

- [Hugo Content Structure](https://gohugo.io/content-management/organization/)
- [PaperMod theme wiki](https://github.com/adityatelange/hugo-PaperMod/wiki)
