# cdj_mode

## Installation

To set up a shell for cdj mode, run:
```
$ source path/to/cdj_mode/cdj_mode
```

## Dependencies

- bash
- ffmpeg
- tapbpm
- sox

## Configuration

Set the comma-separated target directories with:

```
$ export cdj_target_dir=/mnt/usb1,/mnt/usb2
```

## Usage

### Select A Track

cdj_mode is track-oriented. This means that you prepare one track at a time. Select the track with:

```
$ cdj_track path/to/foo.flac
```

### Set BPM

Once a track has been selected with `cdj_track`, you are required to specify the bpm with `cdj_bpm`:

```
$ cdj_bpm 120
```

If you have `tapbpm` in your path, then you can call `cdj_bpm` with no arguments to play 5 seconds of the song in the background while `tapbpm` is running.

### Set Split Points (optional)

cdj_mode supports splitting the file into pieces based on split points that you specify. This is particularly handy if you use CDJs auto-cue turned *off*. The pieces will play together seamlessly, so you can achieve "cue points" by splitting the track up.

cdj_mode has a "scrub point", which is modified with:

```
$ cdj_scrub 29.42
```

This will set the scrub point at 29.42 seconds into the track, and it will play one second of the song starting at the scrub point.

By repeatedly setting the scrub point, you can zero in on the exact spot you want to split the track.

When you have found the right scrub point, you can split the track at the scrub point with:

```
$ cdj_split
```

At any time, you can preview all split points with:

```
$ cdj_split_preview
```

Which will play all your split points in the order you created them (one second each)

You can remove split points by modifying the `cdj_current_splits` environment variable directly. The format is space-separated real numbers.

### Finalizing

When you are finished setting the bpm and the split points, you can copy to the target directories with:

```
$ cdj_done
```

If you specified split points, you will copy one file for each split point to your target directories.

Split points are included in the file name, which are in this format:

```
123.45_Original Filename [000.00].wav
123.45_Original Filename [045.21].wav
```

Where `123.45` is the BPM, `Original Filename` is the filename of the track specified with `cdj_track 'Original Filename.flac'` with extension removed, and `45.21` is the split point.

### Next Track

Invoking `cdj_track` will reset BPM and split points for you.
