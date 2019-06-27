---
weight: "1"
title: "Prototyping"
description: "Systems which can be used for prototyping such as Lego Mindstorm, Meccano, Fischertechnik, MakerBeam, MakeBlock and OpenBuilds"
illustration: "fischertechnik-differential.jpg"
---

Lets say you want to test out an idea for a robot car, DVD changing robot arm, automatic card sorter or something similar. Machining your own parts would be time consuming. So what are the options for rapid prototyping?

    
## Lego Mindstorm
The best known option and a huge community

![alt text](/images/maker/lego-rover.jpg "Lego")

Easy to find help and wide variety of parts. You can get hold of lego mindstorm in almost any toystore. The downside is that lego is its own world. Lego parts don't mesh naturally with regular industrial components. You are typically limited to using parts made specifically for lego. 
<hr>
    
## Meccano

Readily available and works quite well with non meccano parts from industry

![alt text](/images/maker/meccano-crane.jpg "Meccano")

In Norway you can find Meccano at:

* [Clas Ohlson][clas]
* [Riktige Leker][riktigleker]
* [Teknisk Museum](http://www.tekniskmuseum.no/nettbutikk)

It is slower to put together things with Meccano than with Lego but the benefit is that the same approach for connecting things as "real" objects: screws and nuts. So you can more easily connect it to standard industrial components.

It also give a more solid construction, which you could keep as the final version if you want to.
<hr>
    
## [Fischertechnik](/prototyping/fischertechnik)

Plastic parts like lego, but designed mainly for prototyping machines and engineering structures

![Fischertechnik Rover](/images/maker/fischertechnik-rover.jpg)

The way pieces are fitted together is very different from  Lego. It is similar to how industrial prototyping systems such as [T-slots][tnut] works. In fact it can be combined with [OpenBeam][openbeam] aluminium T-slots. This makes the system one of the most flexible ones covered on this page. Unlike Lego and Meccano pieces do not need to be fitted together at fixed points. The main downside [fischertechnik][fischertechnik] is that:

* Largely unknown outside of Germany.
* Can't buy kits in your average toystore.
* Much smaller community than Lego Mindstorm
<hr>
    
## MakerBeam and OpenBeam

A [T-slot][tnut] system for small hobby sized constructions

![alt text](/images/maker/makerbeam.jpg "MakerBeam")


[T-slots][tnut] used in the industry is usually 20x20mm, which is often impractical for a hobbyist. However this size dominates for 3D printer designs.

[MakerBeam][makerbeam] sells a variety of T-slots which are smaller than 20x20mmm. Their original beams simply called [MakerBeam][mbeamsmall] are 10x10 mm which is pratical for more detailed work.

They have two types of beams which are 15x15mm, called [OpenBeam][openbeam] and [MakerBeamXL][mbeamlarge]. This is a very convenient size because it is exactly the same as standard [fischertechnik][fischertechnik] building blocks. That means one can combine with the extensive number of parts existing for the [fischertechnik][fischertechnik] building system. It means you could prototype something really fast without using any screws in [fischertechnik][fischertechnik], and once you get it working, start replacing plastic T-slots with aluminium ones.

The fact that they have two types of 15x15mm slots requires some explanation. [OpenBeam][openbeam] was a separate competing product previously. While I have a bunch of [OpenBeam][openbeam] T-slots I think [MakerBeamXL][mbeamlarge] is probably a better choice today. They have threaded holes at the end so you can easily attach the ends. You don't have to tap threaded holes yourself.  

What is practical with the 15x15mm T-slots [MakerBeam][makerbeam] sells is that they can be used with standard M3 nuts and bolts. A lot of T-slot systems require specialized nuts and bolts. E.g. the 10x10mm beams require special screw heads or nuts.

MakerBeams are not sold with ready made kits, including instructions such as Lego, Meccano and Fischertechnik. Using it requires more knowledge. [MakerBeams][makerbeam] are really just beams, brackets and screws. You have to buy industry standard motors, gears etc to make something usefull. That is both a benefit and a downside. It means you can use parts made for the industry which is often cheaper than specialzed stuff from e.g. Lego.
<hr>
    

## MakeBlock

[MakeBlock][makeblock] is the the closest thing to a modern incarnation of Meccano sets. They sell whole kits like Lego, Meccano and Fischertechnik, but which are based on combining aluminium parts with screws. 

![MakeBlock](/images/maker/mblock-ranger.jpeg)

One advantage of [MakeBlock][makeblock] is that they seem to be available a lot of places. There are many stores in Norway which sells them, unlike e.g. [fischertechnik][fischertechnik] which is generally quite unknown outside of Germany.

It is hard to overstate how impressive [MakeBlock][makeblock] is though. They came out of nowhere and built up an impressive ecosystem in short time. They offer a very wide and varied selection of mechanical components making it possible to build anything from rovers, XY plotters to robot arms. To go with their mechanical parts they have a well thought of array of electronic components and microcontrollers which are Arduino compatible. In addition they have built an impressive array of software to support this. For instance you can program their robots with a visual programming language based on [Scratch][scratch].

The industruction manuals for their kits are of very high quality. Among the best I have seen.

### Pros and Cons
You could call this a modern version of Meccano, or Meccano done right. Meccano simply feels quite cumbersome to build and screws easily loosen. With [MakeBlock][makeblock] you build much faster and get solid structures where screws don't easily loosen.

The feel when building [MakeBlock][makeblock] is something in between Meccano and T-slot systems like [MakeBlock][makeblock]. A lot of their mechanical parts have holes drilled at fixed intervals just like Meccano. However reather than just flat plates like Meccano most of their standard beams allow you attach things at the ends, not just the top and bottom. The image below gives some ideas what is possible.

![MakeBlock Beams](/images/maker/mblock-beams.jpg)

Notice how brackets for motors are attached at the end of the beams. Next look at how the top beam is attached with screws going through a rail running through the centre of the bottom beam. Much like T-slots, this allows you to attach something anywhere rather than being limited to where holes have been drilled.

This gives you the T-slot advantage of attaching stuff whereever you want, while you also retain the Meccano advantage of being able to run beams or screws straight through the beams if desired at fixed locations.

MakeBlock handily beats Meccano. But I still have a softness for T-slot based systems. Makeblock may be a more versatile solution, but it has the disadvantage that nobody else is building blocks like that, while T-slots are used everywhere in industry. It is not just [fischertechnik][fischertechnik] and [MakeBlock][makeblock] but also companies such as [RatRig][ratrig] catering to people buidling custom 3D printers and routers use T-slots. Thus there is some benefits in getting used to using such a system.
<hr>

## OpenBuilds & RatRig
This is the Lego of 3D printers. [OpenBuilds][openbuilds] started like a Kickstarter project just like [MakeBlock][makeblock]. 

![V-Core](/images/maker/corexy4.png)

However like [MakeBlock][makeblock] it is an open source kind of design. Which is probably why you see a large ecosystem having been built up around [OpenBuilds][openbuilds]. In Europe [RatRig][ratrig] is the lead provider for OpenBuilds parts. It is not clear to me whether they actually manufacture the parts themselves or get them from OpenBuilds. 

Anyway what is great about [RatRig][ratrig] is that you easily buy them in Norway through [Kjell & Company][kjell]. [Kjell][kjell] has physical stores all over Norway. Alternative you can use [Creative 3D][creative3d] which sells mostly stuff for 3D printers.

[OpenBuilds][openbuilds] is a T-slot like system which they call V-slot, which is a nice design for supporting linear rails. You can put wheels which fit in where the screws and bolts would go, to easily create linear rails.

### OpenBuilds vs MakerBeam
These two systems are naturally to compare. [MakerBeam][makerbeam] has the advantage of selling 15x15mm beams which fits [Fischertechnik][fischertechnik] parts. [OpenBuilds][openbuilds] V-slot beams in contrast are 20x20mm using M5 screws. That could be a bit big for some hobby projects. However it has the advantage of being the industry standard dimension for T-slots.

[MakerBeam][makerbeam] is quite bare bones. They really just focus on selling aluminium beams with brackets to connect them. [OpenBuilds][openbuilds] in contrast is aiming for a whole system to build 3D printers, CNC machines and laser cutters. Thus the part list e.g. at RatRig is considerably larger. You will find a large variety of ways to connect V-slots in different ways as well as wheels, pulleys, lead screws, plates with drilled holes etc.

It seems to me that the OpenBuild system is very good if you aim to build some kind of project which requires linear movements along an axis. That is useful for camera sliders, 3D printers, plotters, laser cutters, CNC machines which you want to rigid and have high precision.

When comparing my [MakeBlock][makeblock] XY-plotter kit with the setup that OpenBuild uses, their system seems simpler and more elegant. Every axis is essentially built the same way and more stable than my [MakeBlock][makeblock] design. However [MakeBlock][makeblock] seems better suited at making robot arms, rovers and similar.
<hr>

## Lynx Motion
[Lynx Motion][lynxmotion] offers a very different kind of build system to the ones I have discussed above. It is designed primarily for making what you typically thing of as robots. You can build walking robots, robot arms or insect like robots with [Lynx Motion][lynxmotion] parts.

![V-Core](/images/maker/hexapod.jpg)

I always like systems that try to utilze industry standards of some sort. [Lynx Motion][lynxmotion] does just that. Everything is built around standar servos used in radio controlled cars, boats and planes. What Lynx Motion does, is to provide various aluminium brackets compatible with standard servos, so you can create whole robot assembles with servos and brackets. 

[clas]: http://www.clasohlson.no/Product/StartPageProducts.aspx?kb=kb

[riktigleker]: http://www.riktigeleker.no/?page=312
[fischertechnik]: http://www.fischertechnik.de/en/Home.aspx
[tnut]: http://en.wikipedia.org/wiki/T-slot_nut
[makerbeam]: https://www.makerbeam.com
[mbeamsmall]: https://www.makerbeam.com/makerbeam/
[mbeamlarge]: https://www.makerbeam.com/makerbeamxl/
[openbeam]: https://www.makerbeam.com/openbeam/
[makeblock]: https://www.makeblock.com
[scratch]: https://scratch.mit.edu
[ratrig]: http://www.ratrig.com
[openbuilds]: https://openbuilds.com
[kjell]: https://www.kjell.com/no/produkter/elektro-og-verktoy/elektronikk/aluminiumprofiler
[creative3d]: https://www.creative3d.no
[lynxmotion]: http://www.lynxmotion.com