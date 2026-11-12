# Warps Parasite: Tuner mode (replaces Doppler panner)

The Doppler panner has been removed. Its slot (the first feature-mode position,
algorithm knob fully counter-clockwise, orange) now holds a **middle C tuner**.

## Entering / leaving the tuner

Same as any other Parasite feature mode: hold the **Int Osc** button, turn the
**Algorithm** knob fully counter-clockwise (orange), release. To leave, hold the
button and turn the knob to another mode.

If your module was saved in Doppler mode, it will boot into the tuner.

## Tuning

Patch the oscillator you want to tune into **IN 1** (IN 2 also works; both
inputs are summed, and unpatched inputs are ignored). The level knobs have no
effect on detection.

The **Algorithm knob LED** shows how far you are from middle C (C4, 261.63 Hz,
A4 = 440 Hz):

| LED                                | Meaning                                  |
|------------------------------------|------------------------------------------|
| Dim white, breathing               | No signal detected                       |
| Solid green                        | In tune (within ±3 cents)                |
| Green fading to **red**            | Flat (3–50 cents)                        |
| Green fading to **blue**           | Sharp (3–50 cents)                       |
| Red, blinking                      | More than 50 cents flat                  |
| Blue, blinking                     | More than 50 cents sharp                 |
| Blinking fast (red or blue)        | More than a semitone off                 |

The tuner only targets middle C, so an oscillator sitting on C3 or C5 reads as a
full octave flat or sharp (fast blink). Go in the direction the colour tells you
until it slows down and fades to green.

Sine, triangle, saw, square and pulse waves all work. Heavily wavefolded or
very bright, complex waveforms can confuse it; switch your VCO to a simpler
wave while tuning.

## Reference oscillator

In tuner mode, a short press of **Int Osc** turns a middle C reference sine on
or off (it no longer cycles the internal oscillator shapes while in this mode).
The **Int Osc LED** lights green when the reference is on. It always starts
off at power-up.

- **OUT**: your input passed through, mixed with the reference when it is on.
  Listen to it and tune until the beating slows down and stops.
- **AUX**: the reference alone (a pure sine at moderate level), silent when it is off.

Your saved internal-oscillator shape for the other modes is left untouched.
