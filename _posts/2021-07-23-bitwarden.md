---
layout: post
title: "Bitwarden—Why I decided against self-hosting"
date: 2021-07-23 03:56:00
tags: tech
---

## Why I did this in the first place

I am not in the position to self-host anything over the internet. I live in
a rented property and the router isn't mine. Heck, I can't even log into
the router dashboard. But I was getting into self-hosting, privacy, all
that jazz, and thought I could read through Bitwarden docs and gave it a
go locally. What could go wrong? The best case scenario is that it's
hassle-free enough that I keep an offline copy when I leave the house and
come back to sync. Very similar to Syncthing, which I'm using between all
my devices already.

## Doing it

Very simple, I RTFM'd and set up a docker container to host an instance of
Bitwarden locally. I've also read about Vaultwarden, but since it was an
experiment, I didn't want more trouble _deciding_ which one I should test out.

Everything worked out fine. But here are some realisations. My passwords now
contain a single point of failure, and that is my physical computer, if
anything were to happen to the data on desktop, then my passwords are gone.
This entails a good back up of my physical drives, but I'm yuropoor so ehhh
after moments of deliberation, I decided to _not_ deactivate my current
Bitwarden account hosted on their centralised server.

## Post Host Clarity: the Morning after

After a good night sleep, a bit of Searx-fu to read up about self-hosting a
password manager, I conclude that doing so is bad opsec. Yes, you have control
over your data, but I'm dumber than a bric. If you compare my sysadmin
abilities to a Bitwarden employee, I think I know who's going back to passwords
on a post-it note first. One day, maybe one day, I will have the resource and
knowledge to self-host many, many things. Maybe not the bloated IoT projects I
see floating around (look, they're cool, but go and touch some grass please)
though. At the moment, I'm just too poor.

## Resources

Alongside the official documents, I used
[this](https://theselfhostingblog.com/posts/how-to-self-host-bitwarden-on-ubuntu-server/)
to setup the local server (obviously skipping the Nginx public access part).
I'm sure there are plenty of guides out there. I'm not a good example, so this
isn't a guide, just a ramble.
