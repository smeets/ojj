+++
title = "rancilio silva part 1"
description = "going down the rabbit hole and installing a PID controller on my espresso machine"
date = 2025-04-25
[extra]
weight = 20
[taxonomies]
tags=["coffee", "diy"]
+++

So I've started my decent into ~~madness~~ espresso and went and bought a
secondhand Rancilio Silva:

![An image of a Rancilio Silva espresso machine in stainless steel](/silvia-pid/espresso1b.jpg)

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

How large effect will this temperature variance have on the taste, you ask.
Not sure, I answer warily.

## how to surf on temperate internets

What do people on ~~reddit~~ internet recommend?

> <em>"Just temp. surf!"</em>

What is this temp. surf you ask?
_Temperature surfing_ means pushing the brew button, causing hot water to flow into the group head and cold water into the boiler and then waiting for specific amount of time.
People hope that by doing this procedure (and presumably also by praying to every diety known to man) they'll luck out and eventually start the show at the correct temperature.

And people argue that this works, _reliably_.

Well, luckily we have [ze germans](https://github.com/clevercoffee) who don't believe in voodoo shit and are pragmatic enough to solve this problem with some engineering and electronics, with some open source software sprinkled on top.
The gist?
Replace the brew thermostat with a [Proportional Integral Derivative controller](https://en.wikipedia.org/wiki/Proportional%E2%80%93integral%E2%80%93derivative_controller).


## it's time to u-u-upgrade

About 6 months post-purchase it was finally time to tackle the problem, imagined or not, of water temperature.
The first task was to solder, assemble and dryrun the system. Here's how it looks on my ~~coffee table~~ workbench:

![How not to dry run your assembly](/silvia-pid/espresso6b.jpg)

A few mis-wirings, blown sensors and a few incidents of money-not-well-spent later, it's good to go:

- Firmware upload works via USB
- Firmware upload works via Wi-Fi (horay!)
- Web interface works ~20% of the time (horay?)

Next up, disassembly of the outer shell: a few screws are all that secure the outer chassis to the inner skeleton.
Pictured is the insulated boiler with its two thermostats (brown knobs) on the right side, partially obscured by cabling.

![Close-up image of the boiler inside a Rancilio Silva](/silvia-pid/espresso2b.jpg)

The two thermostats are for controlling brew and steam temperature separately.
Rancilio's own manual has the schematics and I actually read it, somewhat appreciative of the similarity with open source software.
Replacing the brew thermostat is simple once I find a correctly-sized screw to thumb down the sensor to keep it in place.

![A close up view of the newly installed temperature sensor where a thermostat used to be](/silvia-pid/espresso3b.jpg)

Mounting the solid state relay, power supply and PCB is a little finicky.
Here's me doing a final dry-run before ~~breaking any remaining warranty~~ committing myself to gluing the parts to the chassis.

![How not to dry run your assembly](/silvia-pid/espresso7b.jpg)

Several hours later... it's done.
Finding a good place for the OLED screen, however, turns out to be quite the problem:

![An image of an OLED display showing the current boiler temperature](/silvia-pid/espresso4b.jpg)

Oh well, I guess I need something to do next time. ¯\\\_(ツ)\_/¯
