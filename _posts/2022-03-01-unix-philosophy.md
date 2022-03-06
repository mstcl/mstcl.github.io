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

Today, I want to talk about a small part of UNIX philosophy. If you want a
thorough summary of all the different "schools of thought" that concerns the
term UNIX philosophy, I recommend the Wikipedia entry. From there, you can
probably find books and articles that will explain what it is better than I
will be able to.

From here onwards, I will focus on one of
[/g/](https://boards.4channel.org/g/)'s most quoted component of UNIX
philosophy: "Do One Thing and Do It Well". This principle suggests that
software designers and developers must strive to avoid adding "bloat" to their
software, a term all [suckless](https://suckless.org/) users abhor. To quote
Doug Mcllroy (1978):

    Make each program do one thing well. To do a new job, build afresh rather than complicate old programs by adding new "features".

What makes a software "bloated", exactly? Here, "bloat" does _not_ mean an
operating system or user-space that comes with many different software
installed; today's definition of "bloat" applies on a software-design level. Of
course, there are definitely overlaps, but the nuance is here to refrain
elitists from saying something along the line of "Ubuntu is bloat, install
Gentoo". It's not about Candy Crush being installed as a default program on
your grandma's computer, it's about a self-contained software on its own.

Like many other subjective qualities, you can often find the term "bloated"
predicated to any software which comes pre-packaged with too many built-in
functionalities, often times derailing from its main job (even worse, it might
be lacking in any true singular functionality). This symptom of a software
developing bloat is appropriately termed "feature-creep". This leads onto my
first contention: software bloat is synonymous to "feature-creep", and any
other definition eventually reduces to user-preference, which is subjective.

### GUI vs CLI

The way a software is interactively displayed is one of its primary properties.
It affects the way functionalities are carried out, such as input, output,
configuration, integration with other software, along with its user
friendliness. But often, the _functions_ themselves are not gravely modified
through the choice of the interactive system. But since it is part of any
software, is causally related, if any, to a software's bloat?

The straightforward answer is: such a feature is a necessary feature, therefore
whatever the choice may be, it cannot be inherently bloated. 

However, one might contend through _observation_ that often times, software
that comes packaged with a GUI is more bloated than a command line interface
(CLI) or terminal user interface (TUI). Take Redshift, which is a utility that
allows you to make your monitor warmer. Whether or not it is scientifically
proven to help your eyes is another matter, some like me just find such a
screen more comfortable to look at. Now, Redshift can be configured through a
text file, and run via the command line or as a daemon, a systemd unit for
example. But there also exists GUI wrappers and similar options that come with
a GUI. Are they inherently more bloated because I can now drag sliders to
adjust the temperature of the screen? No. What it harms ever so slightly is
software harmony (I will get onto this), since I can use a text editor to edit
configuration files of _any_ software, unifying my user experience. I don't
want 3 different software with 3 different GUI libraries that obfuscate the
concept of tweaking options, which on its own does not need a GUI. However,
displaying a representation of the colours, one could say, is more easily done
with a GUI. Effectively, you want to stick to the best option for your software
(so that it does its one thing well). Otherwise, it is purely a matter of
preference. A rough guideline is that text-based and static software for CLI,
and dynamic, pictorial ones for GUI).

Let's take the concept of a music player. Its job is to perform tasks like
playing and changing track, display metadata, list your library or a playlist.
With a CLI, one can design it so that `$ music_player /path/to/track.ext` will
play a track, and through built-in commands, one can see what is being played
plus other things, even though it will not be dynamic. This is still a music
player, despite it being worlds different from foobar2000 or Winamp or iTunes.
If we go with a TUI or GUI, and the music player does the same thing (same =
functionally, and a function is conceptual, not visual), it would also have the
same level of features, and hence the same level of bloat. So if you're one of
those people, please don't call GUIs bloat because you like to do everything
through your terminal. Again, it is _preference_. A GUI music player can
definitely "do one thing and do it well".

    TL;DR: Choosing how you want your software interface to be delivered does not, and should not affect its inherent level of bloat, as long as the functions are carried out in the same way.

This same argument applies to the choice of programming language as well. While
choosing a low level and highly optimised language might introduce less
latency, this does not have anything to do with software bloat. The language
itself might be bloated, but this does not _transfer_ onto the software it
compiles. The language is a tool, not the feature.

The only way to introduce bloat to a music player would be add a Jenga tower
chock full of features. Features which deviate from tasks listed above. Here's
an example, I'm using `ncmpcpp` right now, which is a TUI `mpd` client, do I
ever use its tag editor? No. Musicbrainz Picard can do that. How about the
clock? Absolutely not, I have a watch. The visualiser? Not something I look at
while working or studying. Is `ncmpcpp` bloated then? Yes. Despite that, I like
its TUI, and it's a trade-off I'm willing to make insofar as time is concerned
(more on this below). It is one of the most popular clients and that's how I
found it. If I can reduce the installed size of about 2.6 MiB by switching to
something smaller to avoid the features I don't need, I am obliged to in
adherence to the principle.

## Plugins and scripts

_"My music player is too basic! How do make it more attractive?! Or even better,
how do I get others to contribute to it without it getting bloated?"_

The solution is to add a very special feature to your software, a plug-in
framework. Now, you can add to your software in the form of scripts or patches.
Doing so will introduce _modularity_, which is the ability to separate a
system's component in order to recombine and add variety and uniqueness to how
a user might choose to use it. One can add modularity easily by adding an
application programming interface (API) or a interprocess communication (IPC)
system. This way, no one really needs to read the entire source code in order
to expand the functionality.

Plug-ins, extensions, add-ons, add-ins, however you wish to call them,
inherently follow UNIX philosophy. The modular nature of software is what the
principle "Do One Thing and Do It Well" alludes to. Not only does it help the
developer focus on individual parts of a software to ensure it is optimised and
bug-free, it is also user-friendly because this results in a highly
customisable experience. Good examples include `vim`, `mpv`, and any suckless
software. I want to particularly highlight `i3`, which is a feature-packed
window manager. Even though it does quite few things, they all do what window
managers do. The only exception, i.e. my only complain is arguably `i3bar` (a
status bar), which could be its own package like `i3status` (allows
communication with the system to display the status). Despite that, `i3` has a
IPC framework in Python that allows scripts to extend its power.

The simplest expansion of functionality is interface customisation. Making a
framework to allow changing the theme of your software is one example. Instead
of hard-coding a light and dark theme with your own predefined colours, it
would be better to allow users to pick from a set of themes and ultimately edit
and add in their own custom themes. On Linux, this applies to only TUI and CLI,
GUI program normally are written in GTK or QT, which means they follow the
theme you set your system to display globally. Nevertheless, the option is
technically still there in form of tweaking GTK and QT.

To advance higher, one could introduce a plug-in framework which allows scripts
to be written in any popular languages such as Lua, Python, Perl, etc. For a
music player, this might be a tag editor, a visualiser, a clock, etc.
`Weechat`, an IRC chat client, is a good example of how plug-ins are
implemented. Although most of them are visual enhancements, some of them add in
new functionalities that improve on quality of life.

    TL;DR: Plug-ins are friends, not foes, when it comes to UNIX philosophy. Instead of force-feeding users to install 100% of the features you plan, they can pick and choose their own suite of functionalities. This is modularity and it is a good thing in software design. Thus, it is always better to make a framework that allow external scripts to expand a software's functionalities, than to include every feature.

## Software utopia and why harmony is so difficult to achieve (a more personal passage)

_"I follow UNIX philosophy to write my software, now what? I have to make sure it works with other software and utilities too. This is too much work."_

Well, don't take advice from me, I'm no expert. But as a power user, I have
spent more than half my life on the computer, and I share some sentiment when
it comes to integration with other tools people wrote. It is a lot easier to
write cohesive and perhaps harmonious software with a group of people with
strict guidelines, but this is not the case with non-Big-Tech, ''non-pozzed''
software; small developers often work alone or with a small list of
contributors. It is a lot more work to keep a clean code base for open
sourcing, write guidelines for contributing, while at the same time adding
modularity to your software.

Here's a secret: most users won't care about bloat. I've been to both parties.
I used to make sure almost every I use is terminal-based. I love curses
programs and CLI alternatives. But I slowly notice the downside of it; when
something seems bloated, there is the tendency to purge it and look for a
replacement. This is tiring. I want a solution that "just works".

On my path to reduce bloat, I realise it's not easy to fully compartmentalise
and isolate everything. Communication apps and game launchers are a nightmare.
To this day, I still can't get PDF thumbnailers to work in PCmanFM without
having to install the whole Evince package even though I use Zathura. If you've
gone through something similar to this, you might've entertained the thought
that it's actually easier to go with bigger software that, you know it,
contains a high dosage of bloat, for the sake of less headache. That's what the
majority defaults to, besides just using what comes installed on their system.
The fact is, because most users won't care about bloat, they will almost always
pick it, out of convenience and lack of interest for good software. Companies
will pay money for old, propriety software that are badly written and not
well-maintained because they think paying for software will result in better
products. Depends on how you define "better" when it comes to software, this is
often not the case.

The downsides of bloated software that might not be causally related, but
definitely correlated are what you might expect. Things like more overhead and
lack of optimisation due to bigger codebase, a big backlog of bugs to fix, data
collection and telemetry, lack of user customisation and companies that don't
listen to their users but only their investors, feature-creeping, constant UI
overhaul to attract more and more users in order to make a profit in monetising
collected telemetry. On the other hand, software that do follow UNIX philosophy
and stay this way often come open source and free (as in beer AND speech). Many
software that don't do this are closed source, absolutely propriety, free (as
in beer AND not speech). Again, most people will not care and they will buy
into this.

In other news, if you share my practices and beliefs, don't let the slight
overlapping functionalities of your suite of applications get to you. If it's
the difference of about 2 MiB, it's still a world of difference compared to a
monolithic software that does everything and do them poorly, consumes > 1 GB of
RAM and is probably written in Electron.

We end by returning to the music player, now in the context of harmony. A
scrobbling (logging tracks and storing it in a third-party service) feature
should not be included in a music player. Because one will now have to deal
with third-party service's API, then handle authentication and token
storing, and also upload it to the internet, etc.. Within UNIX philosophy, this
is a red flag, you are _not_ writing a scrobbling software, you are outputting
music files.

It's not lazy to let the music player be detected by something like MPRIS, and
let someone else's software pick the metadata up and scrobble the tracks (like
`rescrobbled`). Because their software focuses on this function alone, they
will do a better job than the music player's. Instead of excluding the part of
the user base that obsessively scrobbles, the developer can instead recommend a
software which works well and is tested with their music player. This will help
increase exposure of the FOSS community, and pass on the legacy of decades-old
philosophy.
