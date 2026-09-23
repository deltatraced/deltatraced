---
status: done
---

# 1 Objective

Capturing any documentation on this project prior to this point of 2025-08-02 Wk 31 Sat - 10:39.

# 2 Journal

2025-08-02 Wk 31 Sat - 10:41

Text on IC: Mega32U4-MU 1726E PH A98WUA

[first use](https://www.pjrc.com/teensy/first_use.html). [gh PaulStoffregen/teensy_loader_cli](https://github.com/PaulStoffregen/teensy_loader_cli). [loader_win10](https://www.pjrc.com/teensy/loader_win10.html). [for debug printing](https://www.pjrc.com/teensy/hid_listen.html).

[td_pulse](https://www.pjrc.com/teensy/td_pulse.html).

This explains a little about using an RC filter to turn a PWM signal into analog

[desmos simulation 1](https://www.desmos.com/calculator/x5spbh3ar2).

A Formalism of PWM typical duty cycle to be varied with `a` from 0 to 1

[desmos simulation 2](https://www.desmos.com/calculator/2upmz8gsp4).

 > 
 > I've modeled embedding a load signal like a sin(t) into the PWM,
 > `a` can be modified for a fixed duty cycle shown in blue (slightly shifted up for clarity),
 > while `b` and `c` are parameters for the sin signal converted to the variable
 > PWM duty cycle

[desmos simulation 3](https://www.desmos.com/calculator/dpzwieksjr).

2025-08-02 Wk 31 Sat - 10:38

I found this entry:

April 12, 2024

I modeled a way to vary the percentage of how on or off a line is by changing a parameter a from 0 to 1: [desmos simulation 1](https://www.desmos.com/calculator/x5spbh3ar2).

This is called pulse width modulation in electronics.

Then, instead of giving each pulse the same width or percentage_on, I also modeled a way to translate another signal like a sin wave into different pulse widths. If you change parameters `b` and `c` and notice one red dot, you see that it corresponds to the pulse width above it, translating energy change in space to the length of time a wire is active essentially [desmos simulation 2](https://www.desmos.com/calculator/2upmz8gsp4).

I also kept the blue constant pulse width signal a little above it for reference

This is the pattern these red dots lie in at every pulse:  [desmos simulation 3](https://www.desmos.com/calculator/dpzwieksjr).

All this is because I've a little computer that can only output full power or nothing, and yet I want it to play music with a little cheap magnet speaker I salvaged with my siblings from some of their old toys. But full on full off doesn't play much. But if I vary the time it's on and off like this, I could charge a capacitor like a little battery just right when the pulse is active or discharge it when the pulse is inactive so that I can play a musical note

Desmos is really cool that I can share models where people vary parameters to play around with them

2025-08-02 Wk 31 Sat - 10:47

[RC circuit math document](https://www.cs.cmu.edu/~tdear/ee/rc_rl.pdf).

[desmos simulation 4](https://www.desmos.com/calculator/dk16kxtwws).

[discrete convolution visual](https://quincyaflint.weebly.com/academic-material/discrete-convolution).

[referenced convolution desmos simulation](https://www.desmos.com/calculator/ea96vohtuq?lang=fr).

April 29 2024

[Transistor 2N3906 (PNP)](https://www.onsemi.com/pdf/datasheet/2n3906-d.pdf).

For a PNP transistor to conduct, Vb \< Ve - 0.7V usually.

Facing the square side, Left to right: Emitter, Base, Collector

The symbol has an arrow on the emitter down pointing *towards* the base. This means PNP. If it was pointing *away*, it would be NPN. For PNP, current flows from emitter to collector, as indicated by the arrow on the emitter towards the base.

To satisfy the condition Vb \< Ve - 0.7V, we connect Ve to VCC. This lets it conduct on Vb=0 -> 0 \< VCC - 0.7V and not conduct on Vb=VCC -> VCC \</\< VCC - 0.7V

Judging by the amplitude of the output sound, I seem to get a louder signal. However, it is strange that VCC also outputs sound (and a kind of sinusoid), which might mean that this system ground is not attenuating the cap current.

2025-08-02 Wk 31 Sat - 11:38

May 27 2024

[Keyboard pinout illustration](https://circuitdigest.com/sites/default/files/inlineimages/u/4x4-Matrix-Keypad-Pinout.png) from [tutorial](https://learningmicro.wordpress.com/interfacing-4x4-matrix-keypad-with-arm-based-kl25z-freedom-development-board/).

[electronics stackexchange tri-state circuit answer](https://electronics.stackexchange.com/questions/509380/how-is-high-impedance-state-physically-different-from-a-logic-low-state/509386#509386).

2025-08-02 Wk 31 Sat - 11:55

[In-browser circuit simulator](https://www.falstad.com/circuit/)
