---
layout: post
title: "Trying out dotfiles automation"
date: 2021-07-30 00:53:00
draft: false
tags:
  - tech
  - writing
  - linux
---
_Updated 01/02/2022_

## Motivation

I wanted a simple and creative way to manage my dotfiles, one which would allow
more automation than my previous method. I stumbled upon many existing tools
that can be utilised to manage dotfiles, such as [GNU
Stow](https://www.gnu.org/software/stow/) and
[chezmoi](https://github.com/twpayne/chezmoi). But they all seem to be too
overkill for me, and their management method do not float my boat quite that
well. Here's what these tools might envision a `~/dotfiles` or `~/.dotfiles`
directory:

```
dotfiles
|- package-x
    |- file.conf
    |- init.ini
    |- .x
|- package-y
    |- config.json
```

However, I prefer to have **all** my dotfiles (and important files from
`~/.config`) that I would like to keep in a remote Github repository in the
directory level, with easily identifiable filenames. This way, when directory
convention changes, or when I want to quickly edit a theme instead of the
settings of a certain program `abc`, I can edit `~/.dotfiles/abc-theme.conf`
instead of having to navigate to the directory `abc` and then editing
`theme.conf`, for example. This way, my fuzzy searches with `fzf` also works
both in `zsh` and `neovim`.

## Folder name

I decided to go with `~/dotfiles` instead `~/.dotfiles` because I want to be able to access and see all my configs without toggling 'show hidden' inside TUI and GUI file managers, that way, my home directory always looks clean. Plus, I like matching the names of my git repositories with the local repository.

## Preferred structure

As mentioned before, I would like my `~/dotfiles` directory to be:

```
dotfiles
|- x-file.conf
|- x-init.ini
|- x
|- y-config.json
```

The mentioned helper packages wouldn't jive so well with this setup.

## Script

I'm not that good with bash and its syntax since I just moved to using linux
full-time for only about 2 months, but I managed to chalk up a script that goes
through all the files in the directory (with exceptions) and symlinking them to
their respective location within `~/.config` or elsewhere. My method is to set
the first line of each file to a comment with a delimiter `:`, so that
everything after `:` is the respective `/path/to/file` relative to `$HOME`.
This way, I can change the name of the symlinked file.

## Caveats

- `fontconfig` files are in `xml` formats and require the first line to be
  version number. Thus, I have to add exception to these files and read the 2nd
  line instead.
- Due to the way I wrote my script, if the comment system requires commenting
  characters to be wrapped around the comment itself, I have to close the
  comment on the next line (like `/**/` of `css` files)
- It cannot _yet_ create the required directories for the symlink if they don't
  exist. This is probably an easy addition but I'm lazy, and this works well
  enough, once I have to set up another box, I will try to implement some sort
  of `mkdir -p` command so I don't have to create a billion folders
- It can detect if a file of the same name as the symlink already exists, and
  if so, rename it to `*.old` and then perform the symlinking, however, if the
  conflicted file is another symlink, this does not work.
  - The workaround might be to detect if it is a symlink, and then simply
    removing the symlink instead.

## Outcome and update

I have for a long time moved on from this shell script and file structure, my
folder remains, but now it has the same file structure as my home directory,
imagine it as a tiny home directory. The main files I want to push to git
remain in `dotfiles/` and I symlink the folders I want manually. I think this
is faster, and plus, I am not deploying this particular setup on anything
else other than my desktop, so I don't see the point of doing this a second
time. Yes, that means I might have vouched not to reinstall my OS for at
least another 5 years.
