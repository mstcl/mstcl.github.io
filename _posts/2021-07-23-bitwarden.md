---
layout: post
title: "Bitwarden - I tried self-hosting, then pussied out"
date: 2021-07-23 03:56:00
tags: tech
---

## Self-hosting

- I sucesfully setup a docker container that is self-hosting bitwarden locally. I cannot expose the server so locally should be ideal still since I'm not going out as much or doing anything which requires instantaneous syncing.
- my passwords now contain a single point of failure, and that is my physical computer, if anything were to happen to the data on my nvme, then my passwords are screwed
- but hey, at least my passwords are not lingering somewhere on a centralised server that is very attractive to those with malicious intents
- I'm not important anyway, who would want to attack my local data? 😁

## Update (the next day)

- after some consideration and reading up about this decision, self-hosting a **password** manager is probably too risky for my liking, I'm back to the main server for now.

## resources

- I used [this](https://theselfhostingblog.com/posts/how-to-self-host-bitwarden-on-ubuntu-server/) to setup the local server (obviously skipping the nginx public access part)
