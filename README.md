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

## Start and stop

```bash
./recordctl start
./recordctl status
./recordctl stop
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

Each matched capture device gets its own WAV file.

## Merge last session into one multichannel WAV

```bash
./recordctl merge-last
```

This creates:

```text
recordings/session_YYYYMMDD_HHMMSS/multitrack.wav
```

## Notes

Separate USB sound cards are not sample-synchronous. For long recordings, tracks may drift. For exact synchronization, use a single multichannel audio interface.
