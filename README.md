# Introduction

`cdj_mode` is a set of bash functions that prepare music for DJ'ing.
A input file (anything that can be read by ffmpeg) is converted to a wav file.
That wav file is split into pieces.
The bpm of the wav file is established (tap tempo or specified).
The pieces are copied to any number of target directories.

# Intended Use Case

Over many years of DJ'ing, a hard learned lesson is that there is very little you can rely on with respect to any club DJ setup.
You cannot expect CDJs that support rekordbox. You cannot expect CDJs with hot cue buttons or particular looping features. You cannot expect that the link jacks will work.
Hell, sometimes they aren't even Pioneers. Or it's only Serato.

Having a system for organizing, prepping, and performing that is completely agnostic of any performance technology, while maintaining the most basic features a DJ needs like BPM information, cue points, and library organization, is the problem that `cdj_mode` was intended to solve.

`cdj_mode` will work with any performance setup that supports:

- USB media (FAT file system)
- Gapless playback

This is known to include CDJs (turn off autocue), Serato, "other" brand CDJs, Traktor and Mixxx and probably any other "in the box" DJ performance software.

# Prerequisites

- sox
- ffmpeg
- [tapbpm](https://github.com/theaugy/tapbpm)

# Configuration

`cdj_mode` will copy the prepared tracks to multiple target directories. I will plug in both of my performance USB sticks, mount them at `/mnt/rb3` and `/mnt/rb4`, and ensure that `cdj_target_dir` is `/mnt/rb3,/mnt/rb4`.
To change these, find the line near the top of the `cdj_mode` file and update the `cdj_target_dir=` line to point at the directory or directories you want. Comma-separate multiple directories.

# Workflow

This is intended to be run from a bash shell. For macOS, Linux and similar operating systems, this is easy. Windows users should consider Windows Subsystem for Linux.

First, put your shell into music mode:

```
$ source path/to/cdj_mode/cdj_mode
```

Load a track (called `some_song.mp3` in this case)

```
$ cdj_track music/some_song.mp3
```

Set the bpm (`120` in this case):
```
$ cdj_bpm 120
```

Find your cue points by scrubbing to a point in the song (`28` seconds in this example):

```
$ cdj_scrub 28
```

This will play 1 second of the song starting at the offset. If you're off by a little bit, you can nudge it forward or backward by changing the offset. You can use decimal places to get it exactly right (you rarely need more than the first decimal place):

```
$ cdj_scrub 28.14
```

If you need to hear more than 1 second of context, pass the number of seconds to play as the second argument (we'll listen to `5` seconds to be sure we got it right):

```
$ cdj_scrub 28.14 5
```


When it sounds right, add a split:

```
$ cdj_split
```

You can scrub anywhere else and add another split. You can add any number of splits. Don't worry about the order; when you finalize the track, your splits will always be created in chronological order.

To hear all the splits you've created so far:

```
$ cdj_split_preview
```

When you're done with the track:

```
$ cdj_done
```

`cdj_done` will break the track into pieces and copy it to the target directories. So if your target directory is `/mnt/cdj_usb_key`, and you created splits at `28` and `64`, and the bpm is `120`, then `cdj_done` will generate the following files:

```
/mnt/cdj_usb_key/120.00_some_song [000].wav
/mnt/cdj_usb_key/120.00_some_song [028].wav
/mnt/cdj_usb_key/120.00_some_song [120].wav
```

Each piece starts at the exact offset where you put its split.
