# Thall/Djent song generator
- [What is it?](#what-is-it)
- [How does it work?](#how-does-it-work)
- [How to use it?](#how-to-use-it)
    - [Player](#player)
    - [Section](#section)
    - [Instruments](#instruments)
        - [Drums](#drums)
        - [Bass](#bass)
        - [Overdrive guitar](#overdrive-guitar)
        - [Clean guitar](#clean-guitar)
        - [Synth](#synth)
- [Download results](#download-results)
    - [Midi](#midi)
        - [Midi mapping](#midi-mapping)
    - [Audio files](#audio-files)
- [Samples](#samples)
    - [Overdrive guitar](#overdrive-guitar-1)
    - [Bass](#bass-1)
    - [Clean guitar](#clean-guitar-1)
    - [Synth](#synth-1)
    - [Drums](#drums-1)
- [Supported browsers](#supported-browsers)
## What is it?
This is a script for generating songs in djent/thall genre by predefined rules that can be tuned up by the user. The general idea is that every song consists of sections and every section contains instruments that play at the same time within the current section. Every instrument playbacks own part generated by patterns that loop within the section. More on that below.
## How does it work?
It's written using [Tone.js](https://tonejs.github.io/) Web Audio framework.
## How to use it?
### Player
![](/help/player.png)

Script represents a simple sequencer that puts notes to the `grid` and plays them step by step. Set up the preferred tempo and hit `play` button. This will playback a predefined demo song just to show how things work.

`Plus` button adds the new section to the end of the current song. `Reset` button resets the song to the default demo one.

All sections and their properties are saved between page reloads.
### Section
![](/help/section.png)

Every section can be `muted` by a checkbox near the section title, so it will not be played by a sequencer. Also, it can be removed from the current song using `X` button (it can't be removed if there is only one section in the song).

Section settings:
- `Bars count`: the length of the sections in bars.
- `Time signature`: time signature that defined one bar.
- `Scale`: instruments will pick up notes from the chosen scale. Each scale has a root of `A`. The lowest note in samples is `A0`, highest sampled note - `G#3`. Also, it must be possible to use notes higher that, but it will be pitched by the sampler, it won't be a real sample.
- `Grid resolution`: resolution of the sequencer's grid. By default, it's 1/16 of the note length.

Each section has instruments that can be enabled/disabled so that they won't be played:
- Drums
- Bass
- Overdrive guitar
- Clean guitar
- Synth
### Instruments
Every instrument follows defined patterns. Each pattern loops inside the current section until the section's end (length defined by the `Bars count` setting). This idea allows thinking in patterns and kind of visualizes them. It was created this way just to ease composing polymeters.
#### Drums
![](/help/drums.png)

Settings:
- `Kick pattern`: dots are kicks (`T` for triplet kicks), dashes are places without notes. Dot and dash have a length of the chosen section's grid resolution, in this particular example it's 1/16 of the note. For instance: this `.-..-.--.` pattern will look like a `.-..-.--..-..-.--..-..-.--..-..-` if section has 2 bars of 4/4 time signature which results in 32 1/16 notes.
- `Snare pattern`: the same idea here. Dots are snares.
- `Rhythm cymbal pattern`: dots are randomly picked rhythm cymbals.
- `Accent cymbal patterns`: every time kick pattern contains a defined accent pattern it will layer on the accent cymbal note. For example: if the accent pattern is `-.--` and the kick pattern is `.-..-.--` then the accent cymbal note will be layered on here `.-..-A--`. Dots are randomly picked crash cymbals (crashes, chinas, splashes). Multiple patterns can be specified, divide them by `,` symbol.
- `Accent choked cymbals`: enables for choking accent cymbals.
- `Fill patterns`: same rules here, but symbols are used instead of dots: `k` kick, `s` snare, `h` high tom, `m` middle tom, `l` low tome, `r` - one randomly picked form mentioned before notes. Capitalized symbols stand for triplet notes: `K`, `S`, `H`, `M`, `L` and `R`.
- `Snare ghost notes`: puts ghost snare notes if enabled.
- `Cymbals ghost notes`: puts "ghost" notes on cymbals (adds some groovy vibe to the drums track). Snare ghost notes take precedence if both are enabled.
- `Intense rhythm cymbals`: crash cymbals are used for rhythm hits defined in `Rhythm cymbal pattern` if enabled. High hats are used otherwise.
- `Change rhythm cymbal note every`: changes randomly picked rhythm cymbal note every `N` hits. For example: if this setting is 8 then currently playing left crash rhythm cymbal will be changed to another random cymbal note after 8 left crash hits.
- `Add accent after changing rhythm cymbal note`: adds accent crash cymbal note after changing rhythm cymbal note if enabled (and if there is a kick being played at the same time to sound more realistic).
- `Put fills after every`: layers on fills every `N` defined bars in the section using `Fill patterns`.
#### Bass
![](/help/bass.png)

Settings:
- `Note length`: every note in the pattern will have this defined length.
- `Dynamic note length`: every note in the pattern will sound until the next note hits.
- `Scale degree notes pattern`: pattern to play. Every note is encoded with a 2-digit number: 1st digit is a note degree in the scale (1st, 2nd etc. and if no note is available then the latest note in the scale will be used), 2nd digit is an octave. For example, 31 is a 3rd note in the scale of the 1st octave.
- `Stick to kick pattern (ignore pattern above)`: instrument will pick notes from the pattern and will layer on those notes to the kick pattern instead of `Scale degree notes pattern` if enabled.
- `Dead/muted notes patterns`: every time note pattern (or kick pattern if `Stick to kick pattern (ignore pattern above)` is enabled) contains a defined dead/muted notes pattern it will layer on dead/muted notes. Available notations are: `m` muted `A0` note, `d` dead `A0` note. Multiple patterns can be specified, divide them by `,` symbol.
#### Overdrive guitar
![](/help/overdrive_guitar.png)

Settings:
- `Note length`: every note in the pattern will have this defined length.
- `Dynamic note length`: every note in the pattern will sound until the next note hits.
- `Scale degree notes pattern left`: pattern to play in the left channel. Every note is encoded with a 2-digit number: 1st digit is a note degree in the scale (1st, 2nd etc. and if no note is available then the latest note in the scale will be used), 2nd digit is an octave. For example, 31 is a 3rd note in the scale of the 1st octave.
- `Scale degree notes pattern right`: pattern to play in the right channel. The same rules are applied here.
- `Stick to kick pattern (ignore pattern above)`: instrument will pick notes from the pattern and will layer on those notes to the kick pattern instead of `Scale degree notes pattern left/right` if enabled.
- `Dead/muted/harmonic notes patterns left`: every time note pattern (or kick pattern if `Stick to kick pattern (ignore pattern above)` is enabled) contains a defined dead/muted/harmonic notes pattern it will layer on dead/muted/harmonic notes for the left channel. Available notations are: `m` muted `A0` note, `d` dead `A0` note, `h` harmonic `A0` note. Multiple patterns can be specified, divide them by `,` symbol.
- `Dead/muted/harmonic notes patterns right`: the same for the right channel.
#### Clean guitar
![](/help/clean_guitar.png)

Settings:
- `Note length`: every note in the pattern will have this defined length.
- `Dynamic note length`: every note in the pattern will sound until the next note hits.
- `Scale degree notes pattern left`: pattern to play in the left channel. Every note is encoded with a 2-digit number: 1st digit is a note degree in the scale (1st, 2nd etc. and if no note is available then the latest note in the scale will be used), 2nd digit is an octave. For example, 31 is a 3rd note in the scale of the 1st octave.
- `Scale degree notes pattern right`: pattern to play in the right channel. The same rules are applied here.
#### Synth
![](/help/synth.png)

Settings:
- `Note length`: every note in the pattern will have this defined length.
- `Dynamic note length`: every note in the pattern will sound until the next note hits.
- `Scale degree notes pattern`: pattern to play. Every note is encoded with a 2-digit number: 1st digit is a note degree in the scale (1st, 2nd etc. and if no note is available then the latest note in the scale will be used), 2nd digit is an octave. For example, 31 is a 3rd note in the scale of the 1st octave.
## Download results
### Midi
Once `play` is clicked and the sequencer started playing the song it means that the midi file is already generated and can be downloaded right away. It's also possible to download the complete audio file after recording it.

![](/help/recording.png)
#### Midi mapping
Channels:
- Drums: 0
- Guitar overdrive left: 1
- Guitar overdrive right: 2
- Guitar clean left: 3
- Guitar clean right: 4
- Synth: 5
- Bass: 6

Drum notes:
- Kick: C1
- SnareHit: D1
- SnareCrossStick: D#1
- Tom1Hit: F1
- Tom2Hit: G1
- Tom3Hit: A1
- HighHatLeftTipTight: C2
- HighHatLeftEdgeTight: C#2
- HighHatLeftTipClosed: D2
- HighHatLeftEdgeClosed: D#2
- HighHatLeftOpen1: E2
- HighHatLeftOpen2: F2
- HighHatLeftOpen3: G2
- HighHatLeftPedal: G#2
- HighHatLeftOpen4: A2
- HighHatRightOpen1: B2
- CrashMainLeftHit: D3
- CrashMainLeftChoke: D#3
- CrashMainRightHit: F3
- CrashMainRightChoke: F#3
- CrashWideRightHit: A3
- CrashWideRightChoke: A#3
- ChinaHit: C4
- ChinaChoke: C#4
- SplashHit: F4
- SplashChoke: F#4
- MiniChinaHit: A4
- MiniChinaChoke: A#4
- RideBell: C5
- RideBow: D5
- RideCrash: F5
- RideChoke: F#5
### Audio files
![](/help/recorded.png)

Once the song is recorded it's possible to download audio files (in ogg format): file per instrument track + master track.
## Samples
### Overdrive guitar
I sampled 3 octaves of my Cort X3 guitar tunned down to Drop A1 with 10-80 strings on it. Samples are processed using Helix Native software and include two different variations (for left and right channels) with 3 articulations - dead/muted/harmony.
### Bass
Also, sampled 3 octaves of my Cort Action V Plus 5 strings bass tunned down to Drop A0. Samples are processed using Helix Native software. Only A0 samples have articulation for dead and muted notes.
### Clean guitar
These are the same samples as for distorted guitar, just processed with different preset for clean tone.
### Synth
The same samples but processed with another Helix Native preset.
### Drums
GGD: "Architects" + Perfect Drums Player "Restrains" kits mixed together.
## Supported browsers
In theory, every modern browser is supported, but I only tested it in Firefox and Chrome on macOS (should work on Windows and Linux machines as well). Works in `Firefox 112.0.1` and `Chrome 111.0.5563.64` (there might be some glitches in Chrome).

