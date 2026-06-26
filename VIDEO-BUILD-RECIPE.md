# Freeze Video - Build Recipe (how it was made)
Companion to build_freeze_video.sh (auto script) and AUDIO.md (audio detail). 2026-06-26.

## Inputs & facts
- Source: 20260626_152506.mp4 - Google Drive id 1s0eE-ORIxlLaYp1lGpuL62gZYKygZSmq, pulled with curl (~66 MB).
- Source properties: landscape 1920x1080, HEVC, 18 s, NO audio track.
- Tools: ffmpeg + curl. Font: Anton (premium display). Music: CC0 public-domain track. No paid services needed.
- Compliance: no prices, no "% cheaper"; CTA = comment FREEZE -> DM pricing.

## The 6 stages
1. Vertical + grade. Landscape -> 9:16 1080x1920 (blur-fill so nothing is cropped). Grade: brighter, cooler/blue, +vibrance, light contrast + sharpen.
2. Speed-ramp. 0-2 s real-time hook, 2-14 s at 2x (the freeze), 14-18 s at 0.8x slow-mo payoff. Output ~13 s.
3. Depth-of-field. Radial gaussian mask keeps the machine sharp; background softened (gblur 9) + mild vignette.
4. Captions (Anton). Two-line, centred, safe-zone, shadow + outline. Accents: FREEZES gold, SERVICED IN SYDNEY cyan, FREEZE pink. Sequence: hook -> "3 flavours. one machine." -> "Aussie-made / serviced in Sydney" -> end card "Comment FREEZE for pricing / Snow Flow Sydney - since 2007".
5. Audio (real, CC0). "Adding the Sun" (Kevin MacLeod, FreePD via GitHub SoundSafari/CC0-1.0-Music, CC0 - no copyright). 13 s section, softened highs + fades, normalised to -16 LUFS (verified mean -17.9 / peak -1.5 dB). See AUDIO.md.
6. Mux + compress. Master SFX3-freeze-master_9x16.mp4 (~9 MB, H.264 + AAC). Backup -sm.mp4 (<2 MB). Both +faststart.

## Re-run / tweak
- Run: bash build_freeze_video.sh -> outputs in ./snowflow_freeze_build/.
- Words: edit the t_*.txt lines. New clip: change DRIVE_ID. New music: change TRACK_URL or drop your own mp3.
- Background blur: gblur sigma=9 (lower = sharper bg).
- Hook A/B: see SFX3-Freeze-VIRAL-content-pack-v2.md.

## Known limits
- Audio is a CC0 track at -16 LUFS, not auditioned in-tool; swap a trending in-app sound for max reach if preferred.
- ffmpeg captions are clean but static-timed; for kinetic captions finish in Descript/CapCut/Premiere using this as the base.
