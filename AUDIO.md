# AUDIO - Freeze video
Track: "Adding the Sun" - Kevin MacLeod.
License: CC0 1.0 (public domain) - no copyright, no attribution required, commercial use OK. Sourced from FreePD, mirrored on GitHub SoundSafari/CC0-1.0-Music.
Direct URL: https://raw.githubusercontent.com/SoundSafari/CC0-1.0-Music/main/freepd.com/Adding%20the%20Sun.mp3

## How it was edited in
- 13 s section starting at 0:08 of the track.
- Fade in 0.6 s / fade out 1.6 s.
- lowpass=15500 to soften harsh highs.
- loudnorm=I=-16:TP=-1.5 -> verified mean -17.9 dB / peak -1.5 dB (present, not blaring).
- Muxed as AAC 192k into the finished video.

## History / why
The first pass used a synthesized bed (pad + air + hum). Too loud and the timbre was annoying, so it was replaced with this real CC0 track at a lower, tasteful level.

## To change the audio
- Different track: edit TRACK_URL in build_freeze_video.sh to any file in the SoundSafari CC0 repo, or drop your own .mp3 next to the script and point to it, then re-run.
- Max reach: on IG/TikTok post the silent master and add a trending in-app sound (free + platform-licensed) instead.
- Caveat: audio isn't auditioned inside the build tool - it's verified by loudness levels.
