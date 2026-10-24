---
name: over-the-shoulder-zoom
description: /over-the-shoulder-zoom video effect for Claude Code. Proof cards land beside your head: a small BEFORE, then a big AFTER that counts up. Use it when the user types /over-the-shoulder-zoom or asks for this effect on their video.
---

# /over-the-shoulder-zoom

Proof cards land beside your head: a small BEFORE, then a big AFTER that counts up.

The camera drifts over my shoulder towards the empty side of the frame. A small grey BEFORE card ($3,200 per month) lands and gets struck out in red, a yellow marker arrow draws down, then a big white AFTER card slams in and its number counts up from $3,200 to $32,000 with a green 10x chip. One hard beat on the slam: a camera kick and a thin ring. Nothing covers the face.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera, empty space on one side.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `OverShoulder` in `commands-fx/src/fx/OverShoulder.tsx`. Register it in `src/Root.tsx`:
   `<OverShoulder clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the BEFORE and AFTER numbers.
6. Render: `npx remotion render fx-over-the-shoulder-zoom out/over-the-shoulder-zoom.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> The camera drifts over my shoulder towards the empty side of the frame. A small grey BEFORE card ($3,200 per month) lands and gets struck out in red, a yellow marker arrow draws down, then a big white AFTER card slams in and its number counts up from $3,200 to $32,000 with a green 10x chip. One hard beat on the slam: a camera kick and a thin ring. Nothing covers the face. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
