---
layout: post
title: "From Vimwiki to plain Markdown"
date: 2021-08-24T21:47:30+01:00
draft: false
tags:
  - tech
  - writing
---

## Why I'm changing

I have been using a combination of vimwiki and Mkdocs inside [neovim]({{< relref vim >}}) for a while now, but I'm not really satisfied with the outcome. Although Mkdocs with the material theme is extremely feature-packed (python markdown extensions are greatly useful), I feel like rejecting bloat and returning to simplicity is better for blogging/journalling/organising in general. Mkdocs was made with slick documentions in mind so the javascript was annoying me quite a bit.

## Hugo

I looked up SSG and found Hugo to be easy to use, I have yet to deploy a GitHub page with it and archive my Mkdocs repository, but once I do it, it will feel like a proper personal blog!

In terms of neovim, I will replace vimwiki with a good Markdown syntax plugin because the built-in one is hot garbage. This means I won't need vim-zettel as well, and instead of using any plugins, I will just manage my page with `hugo <command>` instead. But good syntax-highlighting and font-formatting is what I'm looking for. Following links can be done with `gf` and fuzzy search or `live_grep` for it with telescope.nvim.

Anyway, this would be my first post created with Hugo's built-in commands.

### Update: Aug 25 2021

I found [this](https://github.com/plasticboy/vim-markdown) to be the best vim Markdown plugin. I can edit some of its highlight groups to add `gui=strikethrough` for strikethrough support. While following links don't quite work because I have to declare cross-referenced links in a weird way, which the plugin cannot understand. But if I hover my mouse over text.md, and use `gf`, I follow the link like magic.
