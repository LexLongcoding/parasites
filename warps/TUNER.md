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

## Building from source

Tested on Ubuntu (WSL) with the Arm GCC 7-2018-q2 toolchain and Python 2.7.

```bash
# Tools
sudo apt install -y build-essential bzip2 curl
cd ~
curl -L -o gcc-arm.tar.bz2 https://developer.arm.com/-/media/Files/downloads/gnu-rm/7-2018q2/gcc-arm-none-eabi-7-2018-q2-update-linux.tar.bz2
tar xjf gcc-arm.tar.bz2

# Python 2.7 + numpy (needed by the .wav encoder)
"${SHELL}" <(curl -L micro.mamba.pm/install.sh)
source ~/.bashrc
micromamba create -y -n py27 -c conda-forge python=2.7 numpy
micromamba activate py27

# Source
git clone <this repo> parasites
cd parasites
git submodule update --init stmlib stm_audio_bootloader

# Two fixes inside the submodules:
#  - don't fail the build on warnings from the newer compiler
sed -i 's/ -Werror//' stmlib/makefile.inc
#  - the encoder passes a float array size, which newer numpy rejects
sed -i 's/numpy.zeros((1.0 \* self._sr, 1))/numpy.zeros((int(self._sr), 1))/' stm_audio_bootloader/qpsk/encoder.py

# Build
touch warps/resources.cc
mkdir -p build/warps
PYTHONPATH=. make -f warps/makefile TOOLCHAIN_PATH=$HOME/gcc-arm-none-eabi-7-2018-q2-update/ wav
```

The firmware update file is `build/warps/warps.wav` (about 12 MB).

## Flashing

1. Unpatch the module and connect your computer's audio output to **IN 1**.
2. Power on while holding **Int Osc**; its LED blinks orange.
3. Turn off system sounds, disconnect speakers, set the volume to maximum.
4. Play the `.wav`. The Algorithm LED shows the level: green/yellow is good,
   red is too hot (adjust with the Level 1 knob).
5. Let it play to the end; the module restarts with the new firmware.
