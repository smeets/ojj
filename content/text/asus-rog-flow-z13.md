+++
title = "asus rog flow z13"
description = "a new machine"
date = 2025-04-10
draft = false
[taxonomies]
tags=["work", "laptop", "dotfiles", "setup"]
+++

A while back, my until-then kind-of-trusty Dell XPS 13, stopped charging via
USB-C. After disassembly I hooked the 4-cell battery up to my lab UPS
overnight in the bathroom and got it to boot (yay for just-in-time backups).
I suppose a more hands-on hacker would call it a success and keep going, the
roughed-up chassis, caused by prying it open with a screwdriver, worn as a
token of honor from the field of battle. But not me.

We ordered a new machine, a fun and crazy laptop-tablet thing, called Asus ROG
Flow Z13. It comes with Windows 11 and some crap from Asus. Oh well, let's
put Linux on it and see how it goes.

## Arch pt. 1

My previous laptop ran Arch, so I "etched" a new USB with 2025.03.01. The first thing
the installer suggests is connecting to the internet, and well _fuck__:

No matter what I tried, `iw` did not manage to connect to my Wi-Fi. I no
longer have dmesg logs, but all connection attempts failed within 200ms,
including retries...

So said fuck this, searched around and found Bazzite. I'm no longer playing
videogames as much but a gaming-focused distro with patches for the Z13?
That'll do.

## Bazzite for software dev.

[Bazzite](https://bazzite.org) is built on Fedora and I have no opinions on
Fedora. After going through the unfamiliar motions of bootstrapping
an _atomic_ system (i.e. image-based so system upgrade is just an image swap,
but no clue if that's a useful definition of atomicity but w/e), I kind of
reached a decent status quo:

- Wi-Fi works, woho!
- Install all dev things in a [distrobox](https://distrobox.it/) (including sublime text)
- Bind `Mod+Enter` to run `distrobox enter dev`
- Install [Niri WM](https://github.com/YaLTeR/niri)

But the way there was quite painful, as it usually is when doing
trial-by-error w/o reading the fucking manual:

- Cool glitchy rendering artifacts (courtesy of amdgpu driver?) -- went away with Niri!
- Fighting a distro that said "no! that's against the law!" instead of "you brake it, you fix it"
- Read `fish` config loading order ~20 times ...
- Could not install Sublime Text for shit, until I tried it _inside_ a distrobox! Cool moment when it worked.
- Some flatpaks work, some not... and I have no idea why.

Oh well, finally done.

This worked quite okay for couple of weeks and system upgrades until I started
to get strange errors, probably due to my bad practice of not putting
everything dev-related inside the distrobox container.

Then April came and I said, let's try again with arch 2025.04.01.

## Arch pt. 2

Yeah, no. Wi-Fi was still broken.

So over the weekend I did what any self-respecting nerd without time
conscience does, and plugged in an ethernet cable and installed arch anyway.

After trying what feels like a bazzillion different things (but probably was
closer to a monkey performing the same 3 tricks in different order) I
installed NetworkManager.

And the heavens blessed me with Wi-Fi.

Spirits high I started doing my actual bootstrap routine, and after
installing [hyprland](https://hyprland.org/) et. al. I noticed my next
problem: I could not scroll using the trackpad. For the love of all that is
holy, what is going on with this machine? Is the trackpad registering itself
as a mouse? No. A tablet? No. A keyboard with a nipple? I mean, maybe?

I'll gloss over my fruitless search of the www for answers. Defeated and
depressed, I was ready to go to bed to the joyful sound of birds laughing at
me. Then, while brushing my teeth, I remberered that the touchpad had worked
fine on Bazzite, so they must do something right, right!? Turns out the swell
folks apply patches and compile the linux kernel [themselves](https://github.com/bazzite-org/kernel-bazzite) -- AND EVEN PROVIDE IT ON
AUR. Praise to the author [], blessed be their name.

Anyway, installation was simple. But how to try it out? __Some hours later__
after reading about crazy boot setups, all that was needed was essentially:

```sh
sudo kernel-install add-all
sudo bootctl update
```

Reboot. Enjoy the modern life that is scrolling.

