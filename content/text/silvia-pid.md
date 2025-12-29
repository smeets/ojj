+++
title = "rancilio silva"
description = "going down the rabbit hole and installing a PID controller on my espresso machine"
date = 2025-04-25
[taxonomies]
tags=["coffee", "diy"]
+++

So I've started my decent into ~~madness~~ espresso and went and bought a
secondhand Rancilio Silva:

![An image of a Rancilio Silva espresso machine in stainless steel]()

It's a simple machine. A 3-way valve. Vibration pump. Single boiler. Brew and
steam thermostats. I honestly think that the most complex part is the
EU-mandated energy saving module (but don't ask me about long-lasting boiler
design).

## temp surfing woes

Most espresso ~~bullshit~~ science can be reduced to one
thing--_reproducability_. The end goal is to make a good cup, and try to make
that simple _and_ easy. The general recommendation is to follow a list of
steps ranked by impact, and check items off it as you go.

- __Beans:__ good quality & fresh beans
- __Dosage:__ grind size x roast level x weight = volume in portafilter
- __Puck prep:__ it's very possible to fuck this up
- __Brewing pressure:__ pressure at the group head should be 9 bar
- __Water temp:__ 92°C according to the italian espresso council
- __Pre-infusion:__ well, manufacturers need something to sell people new machines, right?

This story is about brewing temperature. The Rancilio Silva has a single,
insulated, ~800W boiler fed by a vibration pump. There is a little bit of
wiring to control the brewer (on/off) with a thermostat (a kind of
heat-sensitive controller) such that the boiler is only powered when <80°C.

If you haven't been introduced to the world of regulators, let's ask
ourselves, how can the boiler heat water to temperature at-or-above 80°C when
the thermostat cuts power at 80°C?

- The heating element is probably way hotter than 80°C. While turning it off
  will prevent the element from getting hotter, any residual heat will still
  affect the water.
- The thermostat measures the temperature of _the boiler_ (on the outside,
  even), perhaps simplest thought of as the average temperature of the water
  inside the boiler with a bit of delay.

All of this is a lot of text to basically say, thermostats are shit and it is
impossible get accurate (92°C) and reliable (for every shot) brewing
temperature.

What do people on ~~reddit~~ internet recommend?

> <em>"Just temp. surf!"</em>

What is this temp. surf you ask? I cannot fully explain the level of cope or
madness with this approach. _Temperature surfing_ means pushing the brew
button, causing hot water to flow into the group head and cold water into the
boiler, and hoping for the best. _THATS IT_.

People are _arguing_ that this works, _reliably_. I shit you not.

Luckily we have [ze germans](https://github.com/clevercoffee) who are too
pragmatic for voodoo shit and solved this problem with a custom PCB, ESP32,
solid state relay, power supply, temp. sensor and some wire. The idea?
Replace the brew thermostat with a good old[PID controller]
(https://en.wikipedia.org/wiki/Proportional%E2%80%93integral%E2%80%93derivative_controller).

About 6 months after buying the Rancilio Silva it was finally time to tackle
the problem, imagined or not, of water temperature.

## it's time to u-u-upgrade



