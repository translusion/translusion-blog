---
title: "AVR Assembly programming on Mac OS X"
date: "2013-03-05"
description: "Finding out which assembler you should use for AVR programming can be tricky. This is a short guide to the software you should use and recommended text editor plugins."
categories: 
    - "AVR"
    - "Assembly"
---

When starting with out programming the [AVR][avr] microcontroller on OS X, you will probably start by using [CrossPack][crosspack]. However the problem with that is it doesn't come with a compiler meant to be used directly by you. It is mainly there for the C compiler. [CrossPack][crosspack] installs the AVR GNU assembler <strong>avr-as</strong> described [here][avr-as]. The assembly files typically end with <strong>.S</strong>. You can invoke it through gcc with e.g.

    avr-gcc -Wall -Os -DF_CPU=8000000 -mmcu=attiny13 -x assembler-with-cpp -c ledflash.S
  
This will assemble the file <em>ledflash.S </em>for the AVR microcontroller unit (MCU) ATtiny13. The problem with this assembler is that it does not use the same directives as the the official ATMEL  assembler. Most example code and tutorials you can find online is written according to the ATMEL assembler.

Fortunatly if you look around you can find the open source [avra][avra] assembler. A problem when first googling is that an older assembler <strong>tavrasm</strong> will more likely pop up first in your search. Ignore this one. It does not seem to be maintained.

[avra][avra] is newer and is very  easy to compile yourself. There are no dependencies. You just have to give a list of files to gcc or clang to compile it. To check that everything has been assembled correctly you can use the <a title="vAVRdiasm" href="https://github.com/vsergeev/vAVRdisasm">vAVRdiasm</a> to diassemble the .hex file and see that you get back something that looks like what you put in ;-)

There is also a TextMate <a title="avr-assembly bundle" href="https://github.com/mherb/avr-assembly.tmbundle">bundle avr-assembly</a> which you can install which makes it possible to have syntax highlight for your AVR assemble programming.

As a final note, be aware that you have to fiddle a little bit with all of this software. A lot of stuff did not work exactly as the manual said. It is usually just very minor adjustments to make it work. E.g. the TextMate bundle instructions gave the wrong path to bundles for TextMate2.

[crosspack]: http://www.obdev.at/products/crosspack/index.html
[avr]: http://en.wikipedia.org/wiki/Atmel_AVR
[avr-as]: http://www.nongnu.org/avr-libc/user-manual/assembler.html
[avra]: http://avra.sourceforge.net