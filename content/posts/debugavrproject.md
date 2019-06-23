---
title: "Debugging AVR projects"
description: "Practical guide to troubleshoot problems with your AVR microcontroller projects."
date: "2013-03-24"
categories: 
    - "AVR"
    - "Maker"
    - "Debug"
    - "Electronics"
---

This is a distilation of my last weeks of experience building an AVR curcuit on a breadboard and prototype board and programming it in assembly. Look at one of my previous post to see how you get up to speed with the assembler, avrdude for transfering programs etc.

### Continuity Buzzer
When I wanted to check whether to points on my circuit were connected before I turned my multimeter on measuring resistance (power turned off of course). If it measured around 0 ohm I knew I had a connection. Only recently I got a much nicere multimeter and I discovered how much faster you can do this with a continuity buzzer. It works pretty much the same as my old approach. I can still see the resistance in ohms. However it also gives off a beep each time resistance is zero. I didn't realize how usefull that was until I started checking my circuits for problems. You can just move your test probes along quickly without every moving your eyes away from the circuit and just listen for the buzz. I was surprised by how much faster I could check lots of lines on my circuit.

On my prototype board were I just built a development board for 8 pin AVR microcontrollers (see previous post) I was often not sure if my soldering was good or if I had connected everything correctly. The board was a mess of cables so it was easy to forget connections which I did. With the continuity buzzer my approach was to first place one test probe on ground and then move the second one along every point of the circuit which is supposed to be connected to ground and listen for a beep. Then I repeat the same procedure for Vcc.

### Printf type debugging on a Microcontroller
When developing software I would use one of my LEDs for debugging. So e.g. if I wrote a program where you could adjust the frequency of the blinking of the green LED, then I would use the yellow LED for debugging. I would write:

    sbi PINB, PB1 ;flip yellow LED
    
To flip the LED from on to off or off to on. Writing 1 to the input register, as opposed to the output register does that. I would move this line around in the program to make sure that I hit the appropriate section of code when I expected. A limiting factor is that you can not keep this line in a lot of places, because you can not distinguish between them. This is not as problematic as it sounds, since the programs you write on a ATtiny13 in assembly will probably be quit small. If you write larger programs I recommend using the Arudino instead or investing in a proper development board from ATMEL. I did all my development using TextMate 2.0 with an AVR assembly bundle. I then had a hotkey for building and one for uploading. It worked quite well.

### Clock signal mistakes
When creating the hardware, by far the mistake which cost me the most time was mixing up CLKI and SCK (pin 2 and 7) on the AVR. I thought I should use CLKI for the clock pin 3 on the ISP6 connector. Then nothing works and you get the same error message from avrdude as you would have gotten if no cable was connected. Actually avrdude gives the same error for almost anything it seems. Other than that most of my mistakes were in software.

### Messing up unsigned arithmetic
You really ought to get a grasp of unsigned aritmetic. Since you typically work with 8bit values you often have to use unsigned numbers to be able to have numbers in the range 0 to 255 and not just -128 to 127. When branching you should then not use instructions such as:
<ul>
	<li><span style="line-height:13px;">BRLT (<strong>BR</strong>anch if <strong>L</strong>ower <strong>T</strong>han)</span></li>
	<li>BRGE (<strong>BR</strong>anch if <b>G</b>reater or<b> E</b>qual)</li>
	<li>BRMI (<strong>BR</strong>anch if <b>MI</b>nus)</li>
</ul>
These instructions will typically treat the numbers in the comparisons or arithmetic instructions preceding them as signed numbers with range -128 to 127. The problem with that is that if you write:

    ldi r16, 128
    cpi r16, 127
    brge r16Greatest
    
You might expect this code to branch to <em>r16Greatest, </em>since 128 > 127. But it wont because BRGE is treating the numbers as signed. Then 128 is -128, and -128 < 127, so there will be no branching.

### Understanding timers
My second big misunderstanding was the timer/counter system. Register TCNT0 counts upwards constantly and you can use it as a sort of timer or to generate PWM signals on the output. One way of using it is to toggle an output pin OC0B (PB1) or OC0A (PB0) whenever I/O register TCNT0 is equal to I/O register OCR0B or OCR0A. You can configure this in one of the timer controll registers. So far so good. The problem arose when using something called CTC mode. When using CTC, the TCNT0 register will be reset to 0 each time TCNT0 equals OCR0A. The last part is the imporant. You can only use OCR0A for this purpose on ATtiny13. When TCNT0 equals OCR0B it will toggle the OC0B pin if you have enabled that but it will never reset TCNT0. There is no way of configuring that.

The implication of that is also that you can not blink your LEDs at different frequency, but you may blink them out of phase, since OCR0B need not be the same as OCR0A. When I first set this up I did not understand this difference so I wrote a value into OCR0B thinking that would affect the frequency of LED blinking. I did not get any blinking at all. Just a steady light because OCR0A was 0, since it was never set. Thus when TCNT0 got reset it immediatly got the same values as OCR0A again. When turning on interrupts, this had the effect of making the interrupt only fire once.