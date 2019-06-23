---
title: "Development board for 8 pin AVR microcontrollers"
description: "How to make your own board for testing out your AVR programs. It gives you LED's to blink buttons to push and an easy way to attach your AVR programmer."
date: "2013-03-24"
categories: 
    - "AVR"
    - "Maker"
    - "Electronics"
---

I got a bunch of ATtiny13 AVR microcontroller chips because I tought it would be fun to see what you can do with a tiny 8 pin microcontroller. The thought first popped into my mind when I looked at a project for creating motor controller for an electric spinning wheel.

The project used the same chip as in an arduino with an arduino bootloader. The ATmega328 is a lot more expensive than a ATtiny13. You can get a tiny for around 20 Kr at e.g. <a title="electrokit" href="http://www.electrokit.com">electrokit</a> in sweden.

Lots of projects such as motor controllers with buttons or a dial for controlling speed does not need anything more than a tiny13, and a small program. My first problem however hooking up the circuit below to program one of my tiny13s, is that it is easy to connect the cables to the programmer wrong.

<caption id="" align="alignnone" width="650"><a href="http://pluggerogkontakter.files.wordpress.com/2013/03/img_0648.jpg"><img id="i-179" class=" wp-image" title="ATtiny13 development breadboard" src="http://pluggerogkontakter.files.wordpress.com/2013/03/img_0648.jpg?w=650" alt="ATtiny13 development breadboard" width="650" height="365" /></a> Development board for AVR ATtiny13 microcontroller on breadboard, connected to a AVR-ISP500 programmer from Olimex. Pin 2 and 7 are connected to push buttons, which will pull the pins LOW when pushed (will connect to GND). Pin 6 and 5 drive a yellow and green LED through 2 transistors. The two transistors and 22K Ohm resistors are there to avoid interfering with the programmer, since it uses those pins for MISO and MOSI.</caption>

The example above is not too bad because I used colored wires with one female and one male end. Thus I could connect the female end directly to the output of the programmer and the male end into the breadboard. But if you have to connect into the connector of a ISP6 cable then it is easy to get the pin numbering wrong.

So that is why I build the above curcuit first on a breadboard, tested it and then built it on the prototype board below.

<caption id="" align="alignnone" width="650"><a href="http://pluggerogkontakter.files.wordpress.com/2013/03/img_0653.jpg"><img id="i-216" class=" wp-image" title="ATtiny13 development board" src="http://pluggerogkontakter.files.wordpress.com/2013/03/img_0653.jpg?w=650" alt="ATtiny13 development board" width="650" height="427" /></a> Pin 2 and 7 are connected to push buttons, which will pull the pins LOW when pushed (will connect to GND). Pin 6 and 5 drive a yellow and green LED through two 2N2222 transistors. Potentiometer is connected to pin 3. Red LED indicates that our LM7805 voltage regulator is delivering power. On the bottom right there is a ISP6 connector for in system programming.</caption>

It has a ISP6 connector (black 2x3 pins). With this there is only one way to connect the cable from your programmer, so it is very quick to connect your 8 pin AVR microcontroller to a programmer. A lot of devboards contains the bare minimum of components. I wanted to be able to test most of my ideas and programs directly on my development board without having the move the chip to another circuit to do actual work.

So my development board has the following inputs and outputs which I believe I will typically be using for most projects:
<ul>
	<li><span style="line-height: 14px;">2 outputs connected to a green and yellow LED. These are connect to pins which can do PWM, since I will need that for motor controller programs.</span></li>
	<li>2 digital inputs connected to push buttons. Assume pullup resistor is enabled.</li>
	<li>1 analog input connected to a potentiometer.</li>
</ul>
<caption id="" align="alignnone" width="650"><a href="http://pluggerogkontakter.files.wordpress.com/2013/03/tiny13-programmer_bb.png"><img id="i-236" class=" wp-image" title="Partlist for attiny13 devboard" src="http://pluggerogkontakter.files.wordpress.com/2013/03/tiny13-programmer_bb.png?w=650" alt="partlist" width="650" height="304" /></a> 10uF, 100uF capacitor handling 25V. 10K potentiometer. Two 2N2222 transistors. 3 push buttons (one for reset not shown). 3 regular LEDs as shown. 1 switch for power. LM7805 voltage regulator (+5V out). Restors as shown.</caption>

If figured I would to projects with blinking LEDs, dimmers or motor controllers. All that will require PWM. Hence pin 6 and 5 were selected for output.

In the design it is worth nothing that the outputs of the AVR chip do not power the green and yellow LED directly. The reason for that is that pin 6 and 5 are used for in system programming, and are thus connected to our programmer.

Connecting LEDs directly to these same pins as used by the programmer could interfere with its operation. <a title="Atmel AVR042: AVR Hardware Design Considerations" href="http://www.atmel.com/images/doc2521.pdf">ATMELs datasheets</a>(page 6) says you should put in a series resistor between the load and the AVR. 10K would be a good value. I put in 22K and a transistor so very little current or voltage drop will happen to drive the LEDs.

I can program the AVR without any external power because the AVR-ISP500 programmer I use delivers power to the AVR through pin 2 of ISP6 connector. With the transistors I am also able to program it while external power is connected and the circuit is running. This makes development a lot faster, because I do not need to unplug anything to test the circuit, or plug in anythin to transfer an updated version of my program.

Here is a very simple program I ran on it to make one of the LEDs blink. I will not explain the program here, because it is just something for you to try, to make sure it works.

      .include "tn13def.inc"
      .def a = r16
      .org 0000
       rjmp Reset

    Reset:
       ;define output
       sbi DDRB, PB0
       sbi DDRB, PB1
       cbi PORTB, PB0 ;turn off LED
       cbi PORTB, PB1

       ;set prescaler to divide clock freq by 1024. 
       ;each count is about 0.8ms at 1.2MHz
       ldi a, 1<<CS02 | 1<<CS00
       out TCCR0B, a

       ;set blink frequency
       ldi a, 250
       out OCR0A, a
       ldi a, 250
       out OCR0B, a

       ;set CTC mode (clear timer/counter on compare)
       ldi a, 1<<WGM01 | 1<<COM0B0 | 1<<COM0A0 
       out TCCR0A, a 
    loop:
       nop
       rjmp loop