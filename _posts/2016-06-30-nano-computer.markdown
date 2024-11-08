---
layout:     post
title:      "How do I choose the right nano computer?"
date:       "2016-06-30"
author:     "Pierre Verkest"
header-img: "img/rpi.jpg"
---

Thanks to the [Open-source hardware](
https://en.wikipedia.org/wiki/Open-source_hardware) movement, last few
years there are a lot of nano computer that was created and sold to the
general public. So know we have to choose whare fit our requirements!!

You can have a look to [this comparator](http://socialcompare.com/fr/
comparison/raspberrypi-vs-banana-pi-vs-chip-vs-arduino-uno-vs-adafruit-metro-size-comparison)
that give a good an idea of the large choice offer to you.

Here I'm gonna explain how I choose it for the [Quadcopter fleet](
{% post_url 2016-06-29-autonomous-quadcopter-fleet %}) project.

First I've listed my requirement that would fit my needs, I would like
my quadcopter to be able to:

* communicate with each other, as I expect to fly indoor *wifi*
  capability appear a good starting point
* The *weight* should be very low as I expect to make it fly at some
  time
* The board should be evolutive in the sense that I could add sensors
* Make sure at least someone already managed to have this board fly
  on the same kind of [UAV](https://en.wikipedia.org/wiki/
  Unmanned_aerial_vehicle), Vincent Jaunet has shared his [quadcopter](
  https://github.com/vjaunet/QUADCOPTER_V2) project based on a Raspberry
  Pi, a lot of projects are detailed on the [instructables web site](
  http://www.instructables.com/id/DIY-Drones)
* Consider the community and support I can fiund over the internet
* Performances consideration: I guess a lot of thing will run to make
  it fly.
* Low battery consumption, many of those boards are based on ARM
  processors so that won't be the matter! It also depends on how many
  features there are on it but I expect that we could disable some through the
  software.
* Based on Open-source hardware

I've chosen to start with the [Raspberry Pi 3](
https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) which
embrace most of my criteria, it probably lack in term of performance
over the  [Orange Pi Plus 2](http://www.orangepi.org/orangepiplus2/)
but the first is cheaper to start with, smaller and lighter to embed in a
quadcopter!

other resources:

* ARM board [Benchmarks](http://www.phoronix.com/
  scan.php?page=article&item=raspberry-pi-3&num=1)
