---
layout: post
title: "Cleaning $HOME"
date: 2021-07-31 20:33:00
draft: false
tags:
  - linux
---

## XDG_BASE_DIRECTORY crisis

I `ls -a` my home directory one morning and found that it is **littered** with `.dir` and `.file` from compilers and packages that do not comply with the XDG standards. I did realise that I did not export the variables in my `zshrc`, so I did that and emptied my home directory pretty well. However, there are quite a few that needed their separate variable, like `PACKAGE_XYZ_CONFIG_DIR=$HOME/.config/xyz/`, some even needed alias that points the config directory to the right place. Such atrocity!

Regardless, I followed [this](https://wiki.archlinux.org/title/XDG_Base_Directory) extremely good resource that allowed me to de-clutter the `~/` directory extensively.

## Things to remember

I have to edit LightDM root config according to [this](https://askubuntu.com/questions/960793/whats-the-right-place-to-set-the-xauthority-environment-variable/961459#961459) to get rid of `.Xauthority`
