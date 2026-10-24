---
name: song-card
description: /song-card video effect for Claude Code. A now playing card on a clean stage, then the shot wipes in with one word. Use it when the user types /song-card or asks for this effect on their video.
---

# /song-card

A now playing card on a clean stage, then the shot wipes in with one word.

A now playing card on a clean light stage: big cover, title, artist, a progress bar that moves and the times. Then the card lifts away, my shot wipes up behind it and one word punches in big on my chest.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `SongCard` in `commands-fx/src/fx/Grid9.tsx`. Register it in `src/Root.tsx`:
   `<SongCard clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the cover, title and artist.
6. Render: `npx remotion render fx-song-card out/song-card.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> A now playing card on a clean light stage: big cover, title, artist, a progress bar that moves and the times. Then the card lifts away, my shot wipes up behind it and one word punches in big on my chest. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
