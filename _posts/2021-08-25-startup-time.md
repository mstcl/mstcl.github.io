---
layout: post
title: "My obsession with startup time"
date: 2021-08-25T17:26:05+01:00
draft: false
tags:
  - linux
---

## Neovim

At first, I tried to configure Neovim's plugins using Packer as opposed to using Vim-plug. I found out that it is easier to lazy-load plugins, so I tried to lazy-load as many as I can. Right now, I have ~~38~~ ~~41~~ 44 plugins, and so far only two of them are not lazy-loaded, as doing so messed things up no matter what I tried. By doing `#!bash nvim --startuptime /path/to/file`, it is reported that my Neovim takes ~~71~~ 64 ms to load the screen, pretty good considering I have a lot of plugins.

## ZSH

I removed `miniconda` because it was taking a while to load; I wasn't bothered about autoloading it, and I might try using pyenv instead because whatever conda tried to initialise in my `zshrc` takes up 60 ms. I removed those lines added by conda and when doing `time zsh -i -c exit`, I only see a total of 20 ms now. Pretty magical, I'm also not using any bloated zsh _configuration_ like oh-my-zsh, because I used to, and it added a plethora of aliases I never use and get confused by. I took my time to configure my `zshrc` properly, and now it feels much faster! Instead of using `compinit`, I'm now using `fzf` instead, my setup includes fzf fuzzy searching + syntax-highlighting + auto-suggest + zoxide as an autojump replacement. I can tell you that my shell feels pretty and powerful.

_Update Jan 06 2022:_
## systemd

Moved to systemd-networkd on my desktop because I don't change network frequently. Network Manager and dhcpcd are both quite slow. Doing this makes booting up a lot faster. I also use systemd-boot, and it's not very customisable but I'm not going to rice my bootloader. No shade for those who do. Say all you want about systemd and ew it's bloatware, it werks.

