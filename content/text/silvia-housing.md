+++
title = "rancilio silva part 2"
description = "going down the rabbit hole and installing a PID controller on my espresso machine"
date = 2026-01-21
draft = true
[extra]
weight = 10
[taxonomies]
tags=["coffee", "diy"]
+++

So I've started my decent into ~~madness~~ espresso and went and bought a
secondhand Rancilio Silva:

![An image of a Rancilio Silva espresso machine in stainless steel](/espresso1b.jpg)

It's a simple machine. A 3-way valve. Vibration pump. Single boiler. Brew and
steam thermostats. I honestly think that the most complex part is the
EU-mandated energy saving module (but don't ask me about long-lasting boiler
design).

## the possibly hallucinated problem of temperature control

Most espresso ~~bullshit~~ science can be reduced to one thing: _reproducability_.
The end goal is to pull a good shot, and to make the process itself _repeatable_.
The general recommendation is to follow a list of steps ranked by impact, and check items off it as you go.

- __Beans:__ start with good quality, fresh beans
- __Recipe:__ dosage > ratio > grind size > brew time; dial it in as you go
- __Puck prep:__ it's very possible to fuck this up; keep it simple
- __Brewing pressure:__ pressure at the group head should be 9 bar
- __Water temp:__ 92°C according to the italian espresso council
- __Pre-infusion:__ well, manufacturers need something to sell people new machines, right?

But _this_ story is about brewing temperature, and my Rancilio Silva has a single,
insulated, ~800W boiler fed by a vibration pump. There is a little bit of
wiring to control the brewer (on/off) with a thermostat (a kind of
heat-sensitive controller) which heats the boiler until it reaches 80°C.

If you haven't been introduced to the world of regulators, let's ask
ourselves, how can the boiler heat water to temperature exceeding 80°C when
the thermostat switches off at 80°C?

- The heating element is not limited to 80°C. While turning it off will
  prevent the element from getting hotter, any residual heat will still
  affect the water.
- The thermostat measures the temperature of _the boiler_ (from the outside),
  perhaps simplest thought of as a delayed average temperature of the water inside
  the boiler.

All of this is a lot of text to basically say that it is impossible to reliably pull shots with this machine at 92°C.
How large effect will this temperature variance have, you ask. Not sure, I answer warily.

## how to surf on temperate internets

What do people on ~~reddit~~ internet recommend?

> <em>"Just temp. surf!"</em>

What is this temp. surf you ask? I cannot fully explain the level of cope or madness with this approach.
_Temperature surfing_ means pushing the brew button, causing hot water to flow into the group head and cold water into the boiler, and hoping that by repeating this process you'll luck out and eventually start your show at the correct temperature. _THATS IT_.

People are arguing that this works, _reliably_. I shit you not.

Luckily we have [ze germans](https://github.com/clevercoffee) who are too pragmatic for voodoo shit and solved this problem with some engineering and electronics.
The gist?
Replace the brew thermostat with a [Proportional Integral Derivative controller](https://en.wikipedia.org/wiki/Proportional%E2%80%93integral%E2%80%93derivative_controller).


## it's time to u-u-upgrade

About 6 months post-purchase it was finally time to tackle the problem, imagined or not, of water temperature.
The first task was to solder, assemble and dryrun the system. Here's how it looks on my ~~coffee table~~ workbench:

[]

A few mis-wirings, blown sensors and incidents of money-not-well-spent later, it's good to go:

- Firmware upload works via USB
- Firmware upload works via Wi-Fi (horay!)
- Web interface works ~20% of the time (horay?)

Next up, disassembly of the outer shell: a few screws are all that secure the outer chassis to the inner skeleton.
Pictured is the insulated boiler with its two thermostats (brown knobs) on the right.

![An image of a Rancilio Silva espresso machine in stainless steel](/espresso2b.jpg)

There are two thermostats; one for brew and one for steam. Rancilio's own manual has the schematics and I actually read it, somewhat appreciative of the similarity with open source software.
Replacing the brew thermostat is simple once I find a correctly-sized screw to thumb down the sensor to keep it in place. Snapped a picture before wrapping with some electro tape:

![A close up view of the newly installed temperature sensor where a thermostat used to be](/espresso3b.jpg)

Mounting the solid state relay, power supply and PCB is a little finicky but with the help of glue I manage a decent job. Finding a good place for the OLED screen, however, turns out to be quite the problem:

![An image of an OLED display showing the current boiler temperature](/espresso4b.jpg)

I have some plans on 3D printing a display holder and attach it to the chassis using magnets. But that's for next time.


