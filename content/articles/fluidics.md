---
title: "Digital and Analog Fluidics Computers"
date: 2019-12-29T20:17:40+01:00
categories: 
    - "Fluidics"
---

Electricity can be thought of as a fluid flowing through cables. Hence it is possible to mimic the operations of a computer with fluids.

In fact there is a whole engineering discipline devoted to this called fluidics and micro fluidics when dealing with extremely miniaturized devices. These devices work by using vacuum or pressure to pull or push a fluid through an intricate set of tiny pipes or tunnels.
Presently the field of fluidics is mainly concerned with making devices for chemical and biological laboratories. One actual has various chemicals going through the micro channels not just generic gas or liquid.

However it is quite possible to use this technology for computing devices.

## FLODAC Fluid Computer
Most notable is the [FLODAC][flodac] fluid computer built in 1964 as a proof of concept. It was made up of 250 fluid NOR gates. It had 4 bit word sizes and a memory of 4 words as well as 4 different instructions:

- Move
- Add
- Jump
- Halt

This was to demonstrate that a program made with the 4 most fundamental instructions for any computer. FLODAC ran at 10 cycles per second. It has however been [theorized][pneumatic-computers] that clock rates of 10 to 100 kHz is possible. That sounds extremely low compared to an electronic computer which operates at arond 3 GHz today (2019). However that isn't necessarily as limiting as it sounds. The human brain operates at a measly 30 Hz. Still the human brain outperforms almost every computer. It has been [calculated][humanbrain] that the human brain has a processing power of 6 peta flops. That is six million billion calculations per second. Which [compares favorably][humanbrain] to the worlds fastest super computer:

 > the world’s fastest supercomputer is actually about 30 petaflops. Of course, it cost half a month of China’s GDP to build, and requires 24 megawatts to run and cool, which is about the output of a mid-sized solar power station. 

The human does roughly the same with just 20 watts.

How does the human brain achieve this while operating at such low frequency? Due to massive parallelism. Fluidics systems could likewise gain processing power from parallelism. Since you can build them in 3D using simple plastics, you can build quite a lot of channels in small area.

So as a rule of thumb we could say a fluidics system should be able to run at 300 - 3000 times faster clock cycle than the human brain.

[flodac]: https://www.semanticscholar.org/paper/FLODAC%3A-a-pure-fluid-digital-computer-Gluskin-Jacoby/4533b85cbd4759864440a263c4bc1cf961b1fac7

[pneumatic-computers]: http://www.electronicdesign.com/archive/pneumatic-logic-devices-pushed

[humanbrain]: https://patrickjuli.us/2016/04/06/what-is-the-processing-power-of-the-human-brain/
