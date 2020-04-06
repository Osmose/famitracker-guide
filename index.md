# A Slightly Better FamiTracker Guide

You want to make chiptunes? Played Shovel Knight and think you're the next virt? Do you still play emulated NES games out of more than a misdirected nostalgia for a time when someone chose your clothes for you?

Whatever your answer, we have great news: You can be a chiptune musician! We're glad you're here.

This guide describes how to use FamiTracker, a program for making NES-compatible music. We'll cover the basics of FamiTracker itself, plus terminology and techniques that have been developed by the community around it. We'll _also_ cover non-FamiTracker parts of releasing and sharing chiptunes, like creating and uploading videos to YouTube to reach a wider audience.

What we won't be covering is how to _compose_ music; we're not here to dictate your creative process or give you the secret to making soul-shattering bangers. But the path to those starts here!

[toc]

## Trackers and Music Basics

We're not going to teach you how to write music, but there are some basic concepts about music and trackers that are worth going over. Feel free to skip this if you're comfortable with music fundamentals and are familiar with the idea of a tracker.

### Music

Music is (more or less) made by playing a series of notes in sequence. [Notes](https://en.wikipedia.org/wiki/Musical_note) correspond to sound frequencies and are arranged into octaves, each octave having 12 notes in them:

>  C C# D D# E F F# G G# A A# B

The notes above are arranged from lowest to highest in pitch. The `#` is read out loud as "sharp" (FamiTracker does not support flats). The notation wraps back to `A` after `G#` and each octave starts with `C`.

Octaves are numbered, with `0` being the lowest octave, and higher octaves being a higher pitch. FamiTracker uses the format `C-0` to refer to the `C` note in octave `0`. Sharp notes are formatted as `C#0`.

### Trackers

A tracker is a type of music sequencer that lets you arrange a series of notes to be played on a timeline. Trackers usually display their timelines vertically and use text to specify what note to play, what volume to use, effects, etc.

Trackers often split up a song into patterns of notes that can be reused multiple times throughout a song. Different trackers may support different sets of instruments or samples.

## FamiTracker Overview

FamiTracker is a tracker originally created by jsr that replicates the sound of the sound chip used in the Nintendo Entertainment System / Famicom. It also supports expansion chips that were sometimes included in game cartridges to supply extra sound capabilities. In other words, it lets you make NES music!

### 2A03

The 2A03 is the sound chip included within the NES. All FamiTracker songs support the basic set of channels included on the 2A03 chip:

- 2 pulse wave channels
- 1 triangle wave channel
- 1 noise channel
- 1 DPCM channel

### Expansion Chips

#### FDS

The FDS (Famicom Disk System) expansion adds one new channel that supports writing a custom sound wave for making unique sounds not possible with standard pulse wave or triangle wave channels.

#### MMC5

The MMC5 expansion adds 2 more pulse wave channels that are nearly identical to the default pulse wave channels.

#### VRC6

The Konami VRC6 (Virtual ROM Controler 6) expansion adds several new channels:

- 2 pulse wave channels with a wider range of available duty cycles than the standard pulse waves
- 1 sawtooth wave channel

This is the one used by Shovel Knight.

#### VRC7

The Konami VRC7 (Virtual ROM Controller 7) adds 6 channels of FM sound. It has 15 built-in sounds and allows for 64 custom instruments (but only one can be used at a time).

#### Namco 163

The Namco 163 adds up to 8 channels that can play custom sound waves of limited sizes and complexity. There are many caveats to using it properly.

### Channels and Instruments

Depending on the expansion chips in use, a FamiTracker song has a set of __channels__ available. Each channel can play a sound independently of another sound, and the number of channels available limits how many simultaneous notes you can play. 

> Example: With no expansion chips, a 2A03-only track can play 2 notes on the pulse wave channels, 1 on the triangle, and potentially one on the DPCM channel if you have a sample for it, for a total of 4 simultaneous notes. If you want to play more notes than that at once, you will either need to add an expansion chip with more channels, or resort to tricks.

Every note in a channel has an associated __instrument__. Each instrument is tied to a certain expansion chip (sort of) and may contain __patterns__ that control the volume, pitch, or duty cycle of the note being played over time.

> Example: A pulse wave instrument may define a volume pattern that starts at a low volume and grows louder quicky to make the notes sound software, like a wind instrument.

### Effects

__Effects__ modify notes playing on a channel in various ways. Examples including automatically sliding the pitch of a note up or down, muting the note after a delay, or adding vibrato (slightly shifting the pitch of a note up and down rapidly). Some effects are global and may affect the entire song, such as marking where the track should loop back to the start.

### Patterns, Rows, and Frames

A __pattern__ is a sequence of notes (with associated volume, instrument, and effect) for a single channel. A single line in a pattern is a __row__, and number of rows in each pattern is global to the entire song (unless a pattern contains an effect that cuts it off early). Each channel has its own set of patterns that are not shared with other channels.

A row has several columns, all of which can be blank:

- A __note__, controlling the pitch of the channel, or a __release__ (see volume patterns for more info) or __stop__ (which halts an in-progress note).
- A __volume__, controlling how loud the channel is.
- An __instrument__, controlling which instrument to use when playing this row's note.
- One or more __effects__.

A __frame__ is a single row of patterns for every available channel in a module.

### Songs and Modules

A __song__ is a set of frames that are played in order to create music. Songs have metadata attached to them, such as the song title, author, and copyright. A __module__ is the file (typically ending in `.ftm`) that FamiTracker opens and edits, and contains several songs that share certain settings, such as which expansion chips to use.

### Playback

FamiTracker begins playback on the first row of the first frame, and moves down the patterns at a speed determined by the current song's __speed__ and __tempo__. 

Notes don't have a _length_ per se; instead, you can imagine them being turned __on__ when the playhead hits a row with the note specified, and turned __off__ when the playhead hits a note stop or the channel volume is set to 0.