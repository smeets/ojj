+++
title = "rancilio silva pt. 1"
description = "going down the rabbit hole and installing a PID controller on my espresso machine"
date = 2025-01-15
draft = true
[taxonomies]
tags=["coffee", "diy"]
+++

So I've started my decent into ~~madness~~ espresso and went and bought a
secondhand Rancilio Silva:

![An image of a Rancilio Silva espresso machine in stainless steel]()

It's a simple machine. A 3-way valve. Vibration pump. Single boiler. Brew and
steam thermostats. I honestly think that the most complex part is the
EU-mandated energy saving module.

## temp surfing woes

Most espresso bullshit boil down to one thing: reproducability.

Thermostats are sawtooth controllers.

What do people on ~~reddit~~ internet recommend?

> <em>"Just temp. surf bro!"</em>
>
> -- some random dude on the internet, probably

:facepalm:

Luckily we have [ze germans](https://github.com/clevercoffee) who solved this
problem with a custom PCB, ESP32, solid state relay, power supply, temp.
sensor and some wire. The idea? Replace the brew thermostat with a good old
[PID controller](https://en.wikipedia.org/wiki/Proportional%E2%80%93integral%E2%80%93derivative_controller).

## temp sensor woes


