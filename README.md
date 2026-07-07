# Raspberry Pi Multitrack USB Recorder

Records from all matching ALSA capture devices into one WAV file per device.

The default command is a toggle:

```bash
./recordctl
```

First run starts recording. Second run stops recording.

## Install dependencies

```bash
sudo apt update
sudo apt install -y alsa-utils ffmpeg
```

## Check devices

```bash
arecord -l
./recordctl devices
```

By default, `./recordctl devices` only includes devices whose ALSA card/device
description matches `DEVICE_REGEX` from `recordctl.conf`.

Set `DEVICE_REGEX=""` or `DEVICE_REGEX=".*"` in `recordctl.conf` to use all
detected capture devices.

## Start and stop

```bash
./recordctl start
./recordctl status
./recordctl stop
```

## Quality settings

Settings live in `recordctl.conf`.

- `CHANNELS="2"` enables stereo recording if the device supports it.
- `OUTPUT_FORMAT="wav"` keeps the highest compatibility and easiest editing.
- `OUTPUT_FORMAT="mp3"` writes smaller files using ffmpeg/libmp3lame.
- `MP3_BITRATE="192k"` controls MP3 quality.
- `RATE="48000"` is a good default for USB audio devices.

Example stereo MP3 setup:

```bash
CHANNELS="2"
OUTPUT_FORMAT="mp3"
MP3_BITRATE="192k"
```

Or use one-button mode:

```bash
./recordctl
```

## Output

Recordings are saved into:

```text
recordings/session_YYYYMMDD_HHMMSS/
```

Each matched capture device gets its own audio file.

## Merge last WAV session into one multichannel WAV

```bash
./recordctl merge-last
```

This creates:

```text
recordings/session_YYYYMMDD_HHMMSS/multitrack.wav
```

## Notes

Separate USB sound cards are not sample-synchronous. For long recordings, tracks may drift. For exact synchronization, use a single multichannel audio interface.

If you hear a constant hum or buzz, the cause is usually hardware or gain staging rather than the recording script: USB power noise, ground loops, cheap audio adapters, overloaded mic input, or analog cabling. Test one device at a time, reduce input gain, try a powered USB hub, and compare WAV vs MP3 before assuming software compression is the cause.
