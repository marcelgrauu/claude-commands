---
name: stamp
description: /stamp video effect for Claude Code. A stamp slams in and the shot shakes. Use it when the user types /stamp or asks for this effect on their video.
---

# /stamp

A stamp slams in and the shot shakes.

A big yellow 100% AI stamp (thick border, glow) slams in from 3x, rotated -12 degrees, and the whole shot shakes for a few frames on impact.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `Stamp` in `commands-fx/src/fx/Grid7.tsx`. Register it in `src/Root.tsx`:
   `<Stamp clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the stamp text.
6. Render: `npx remotion render fx-stamp out/stamp.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> A big yellow 100% AI stamp (thick border, glow) slams in from 3x, rotated -12 degrees, and the whole shot shakes for a few frames on impact. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
