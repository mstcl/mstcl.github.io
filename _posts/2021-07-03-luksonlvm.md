---
layout: post
title: "How I set up my daily driver: Luks on LVM"
date: 2021-07-03 01:26:00
tags:
  - tech
  - linux
---

## LUKS setup

I wanted to protect my desktop PC with LUKS2, I decided to use LVM after reading not so good things about BTRFS. I settled on LUKS on LVM instead of LVM on LUKS because I wanted to have partition separation for easier control if I do end up with a corrupted root partition. While chain unlocking is not as secure as entering two passwords to decrypt two partitions, the [guide](https://gist.github.com/ddnomad/57c2ed9cc8372d0079fc280e7459bbeb) I used worked and it is a very good guide which explains everything. Mind that I have used the official Arch installation guide wiki page a few times when I was still clueless and reinstalled Arch every time something minor failed.

Of course, being a great wiki, it also contains the information from the gist I used, [this](https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LUKS_on_LVM) is the page with basically the same information, however the gist did a great job explaining nevertheless.

### A visualisation of my partitioning scheme

```
+---------+-------------+-------------+-------------+
|   EFI   |   lv root   |   lv swap   |   lv home   |
|  /boot  |      /      |             |    /home    |
|  fat32  |     ext     |             |     ext     |
|  /d/n1  |  /d/m/root  |  /d/m/swap  |  /d/m/home  |
|         |    LUKS2    |   dmcrypt   |    LUKS2    |
|         |-------------+-------------+-------------+
|         |                  vg                     |
|         |-----------------------------------------|
|         |                pv /d/n2                 |
+---------------------------------------------------+
```

## Note for future needs

This is an easy [guide](https://wiki.hackzine.org/sysadmin/linux-lvm-luks-resize.html) to resize a logical volume, if I need to do so in the future.
