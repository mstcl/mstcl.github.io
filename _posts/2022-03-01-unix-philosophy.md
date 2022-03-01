---
layout: post
title: "UNIX philosophy, modularity and software harmony"
date: 2022-03-01 08:54
tag:
  - essay
  - philosophy
  - linux
  - foss
---

## Do One Thing and Do It Well

If you want a thorough summary of all the different "schools of thought" that
concerns the term _UNIX philosophy_, I recommend the Wikipedia entry. From
there, you can probably find books and articles that will explain what it is
better than I will be able to.

From here onwards, I will focus on one of
[/g/](https://boards.4channel.org/g/)'s most quoted component of UNIX
philosophy: "Do One Thing and Do It Well". This principle suggests that
software designers and developers must strive to avoid adding "bloat" to their
software, a term all [suckless](https://suckless.org/) users abhor. To quote
Doug Mcllroy (1978):

    Make each program do one thing well. To do a new job, build afresh rather than complicate old programs by adding new "features".

But what makes a software "bloated", exactly? Here, "bloat" here does _not_
mean an operating system or userspace that comes with many different software
installed, this definition of "bloat" applies on a software-design level. Of
course, there are definitely overlaps, but the nuance is here to refrain
elitists from saying something along the line of "Ubuntu is bloat, install
Gentoo".

Like many other subjective qualities, you can often find the term "bloated"
predicated to any software which comes pre-packaged with too many built-in
functionalities, often times derailing from its main job (even worse, it might
be lacking in any true singular functionality). This symptom of a software
developing bloat is appropriately termed "feature-creep". This leads onto my
first point: software bloat is synonymous to exhibiting "feature-creep",
anything else boils down to subjective user-preference.

Take the example of a music player. Let's say you want to make a music player,
and you decide to make it a graphical user interface (GUI). We reach our first
junction, is this the right decision according to the principle? First of all,
an interface is a necessary feature, thus, whatever the choice may be, it is
not bloat. Otherwise, your software might as well be delivered in the form of
assembly carved onto the walls of a cave in Morse code, ready to be interpreted
by aliens.

However, one might wonder whether a GUI is more bloated than a command line
interface (CLI) or terminal user interface (TUI). My answer: while it depends
on the software, ultimately, it is _your_ preference (our first appearance of
subjectivity in UNIX philosophy). I'm sure if you're reading this, you're doing
so from hardware with a graphics processing unit and an operating system which
natively comes installed with drivers that can display graphical components
effortlessly. Therefore, there are no limiting factors in which interface you
choose. You may prefer to have text-based, static software remain CLI only, and
dynamic, graphical ones to be GUI for maximum usability. Our music player could
be a mixture of both. Things like the control the currently playing track is
dynamic as long as you're changing songs and displaying timestamps, or even
when browsing a library or playlist. Despite all that, these things could
_technically_ be done on a CLI. You can design it so that `$ music_player /path/to/track.ext` will simply play the track. It can display the playing
track or progress or list playable tracks with built-in commands. You would
call this a music player, would you? I would, and a TUI or GUI which does the
same thing would also have the same level of features. So please, don't go
calling GUIs bloat, just because you like to do everything through your
terminal. Again, it is _preference_. A GUI music player can definitely "do
one thing and do it well".

    TL;DR: Choosing how you want your software interface to be delivered does not, and should not affect its inherent level of bloat, as long as the functions are carried out in the same way.

This same argument applies to the choice of programming language. While
choosing a low level and highly optimised language might introduce less
latency, this does not have anything to do with software bloat. The language
itself might be bloated, but this does not _transfer_ onto the software it
compiles to simply because it is a tool, not the final product.

Thus, the only way to introduce bloat to your music player would be through a
spaghettifying your codebase and adding a Jenga tower chock full of features
that deviate from things functionalities I listed above. Here's an example, I'm
using `ncmpcpp` right now, which is a TUI `mpd` client, do I ever use its tag
editor? No. Musicbrainz Picard can do that. How about the clock? Absolutely
not, I have a watch. The visualiser? Not something I look at while working or
studying. Is `ncmpcpp` bloated then? Yes. Despite that, I like its TUI, and
it's a trade-off I'm willing to make insofar as time is concerned (more on this
below). It is one of the most popular clients, and if I can reduce the
installed size of about 2.6 MiB to something smaller to avoid the features I
don't need, I am obliged to do it in adherence to the principle.

## Plugins and scripts

_"My music player is too basic! How do make it more attractive?! Or even better,
how do I get others to contribute to it without it getting bloated?"_

The solution is to add a singular functionality to your software—a plugins
framework. Now, you can add to your software in the form of scripts or patches.
These introduce _modularity_, and with a nicely written application programming
interface (API) or interprocess communication (IPC), no one really needs to
read the entire source code in order to expand the functionality.

Now, one might ask: how would you apply this principle when it comes to
extensions and plugins? Plugins, extensions, add-ons, add-ins, however you wish
to call them, inherently follow UNIX philosophy. The modular nature of software
is what the principle "Do One Thing and Do It Well" alludes to. Good examples
include `vim`, `mpv`, and any suckless software. `i3` is very feature-packed
(but I use almost all features of it), and my only complain about bloat is
`i3bar`, which could be its own package like `i3status`. Despite that, `i3` has
a IPC framework in Python that allows scripts like `autotiling` to work.

The simplest expansion of functionality is interface customisation. Making a
framework for changing the theme of your software, for example. Instead of
hard-coding a Light and Dark theme with your own predefined colours, allow your
users to pick from a set of themes, or add in their own custom themes in form
of a file that is formatted properly and which your software can parse. On
Linux, this applies to only TUI and CLI, GUI program normally are written in
GTK or QT, which means they follow the theme you set your system to display
globally, or by changing a environmental variable when launching. Nevertheless,
the option is technically still there in form of the GTK and QT frameworks.

Presumably, you have now added a plugin framework with an easy-to-implement API
to the music player. Users can change their system theme and expect the
software to follow suit. If you went with a TUI instead, they can modify the
configuration or add in a theme file to change the colours of certain elements
and format how lists are displayed, for example.

The next step would be to allow scripts to be written in _[any language(s) of your
choice]_ which might allow you to edit tags, display a visualiser, a clock, etc.
At this point, you are so knee-deep in UNIX philosophy you can't stop accepting
pull requests for new scripts and patches.

    TL;DR: Plugins are not bloat and adhere to UNIX philosophy. Instead of force-feeding users to install 100% of the features you plan, they can pick and choose their own suite of functionalities. This is modularity and it is a good thing in software design. Thus, it is always better to make a framework that allow external scripts to expand a software's functionalities, than to include every feature.

## Software utopia and why harmony is so difficult to achieve

_"I follow UNIX philosophy to write my software, now what? I have to make sure it works with other software and utilities too. This is too much work."_

Well, don't take advice from me, I'm no expert. But as a power user, I have
spent more than half my life on the computer, and I share some sentiment when
it comes to integration with other tools people wrote. It is a lot easier to
write cohesive and perhaps harmonious software with a group of people with
strict guidelines, but this is not the case with non-Big-Tech, 'non-pozzed'
software; small developers often work alone or with a small list of
contributors. It is a lot more work to open source and modularise your software
than to hard code everything, and maybe even close source it so others don't
laugh at your spaghetti code.

But here's a secret: most users won't care about bloat. I've been to both
parties. I used to make sure almost every I use is terminal-based. I love
curses programs and CLI alternatives. But I slowly notice the downside of it;
when something is a bit bloated, you have the tendency to try and purge it,
since it doesn't belong with the rest. This is tiring. I want a solution that
"just works".

After all the configuration and exploration, I still find overlaps in
functionalities; I can't fully compartmentalise and isolate everything, it's
not possible. Communication apps and game launchers are a nightmare. To this
day, I still can't get PDF thumbnailers to work in PCmanFM without having to
install the whole Evince package even though I use Zathura. If you've gone
through something similar to this, you might've entertained that it's actually
better to have maybe less software, yet bigger ones that contain more bloat,
but there will be less overlap and therefore less headache. Well, that's how
the majority defaults to, besides just using what comes installed on their
system. The fact is, because most users won't care about bloat, they will
almost always pick the bloated option.

But this option often comes with severe side effects that might not be causally
related, but definitely correlated. Things like more overhead and lack of
optimisation due to bigger codebase, a big backlog of bugs to fix, data
collection and telemetry, lack of user customisation and companies that don't
listen to their users but participate in feature-creeping and constant UI
overhaul to attract more and more users in order to either monetise more of
their collected telemetry. What's more, software that do follow UNIX philosophy
often come open source and free (as in beer AND speech). Many software that
don't do this are closed source, absolutely propriety, free (as in beer AND not
speech). The truth is, most people will not care and they will buy into this.

In other news, if you share my practices and beliefs, don't let the slight
overlapping functionalities of your suite of applications get to you. If it's
the difference of about 2 MiB, it's still a world of difference compared to a
monolithic software that does everything and do them poorly, consumes > 1 GB of
RAM and is probably written in Electron.

Back to your music player. This means you shouldn't add a scrobbling feature.
You will now have to deal with Last.fm or Listenbrainz or whatever service's
API, handle authentication and token storing, run a daemon to upload it to the
internet, etc.. This is a red flag, you are _not_ writing a scrobbling
software, you are outputting music files.

Instead, learn to harmonise. Allow your program to be detected by something
like MPRIS, and let someone's scrobbling software pick the metadata up. You
will lose most of your users when they don't see a scrobbling feature. But let
them go if they don't want to run something like rescrobbled in the background
instead. If you feel generous, maybe you can recommend a scrobbling service
which works with your player in the manual or documentation.
