---
name: nle-timeline
description: /nle-timeline video effect for Claude Code. Claude cuts the dead air on a real editing timeline. Use it when the user types /nle-timeline or asks for this effect on their video.
---

# /nle-timeline

Claude cuts the dead air on a real editing timeline.

A dark glass editing timeline slides up across my chest: timecode and a Claude status pill on top, a ruler, a yellow caption track, a filmstrip of my clip, my voice as a waveform and a yellow music bed. Two silences on the voice get flagged as dead air in red, drop out, and everything ripples left to close the gaps. Then a yellow playhead runs and the timecode counts with it.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera; Claude saves one frame of the clip as public/thumbs/input.jpg for the filmstrip.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `NleTimeline` in `commands-fx/src/fx/Grid6.tsx`. Register it in `src/Root.tsx`:
   `<NleTimeline clip={c("input", 0).clip} still="thumbs/input.jpg" />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the captions, the silences (NLE_SIL) and the status lines.
6. Render: `npx remotion render fx-nle-timeline out/nle-timeline.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> A dark glass editing timeline slides up across my chest: timecode and a Claude status pill on top, a ruler, a yellow caption track, a filmstrip of my clip, my voice as a waveform and a yellow music bed. Two silences on the voice get flagged as dead air in red, drop out, and everything ripples left to close the gaps. Then a yellow playhead runs and the timecode counts with it. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
