# Raspberry Pi Multitrack USB Recorder

Records from all available ALSA capture devices into one WAV file per device.

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

Each USB sound card gets its own WAV file.

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
