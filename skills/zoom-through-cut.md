---
name: zoom-through-cut
description: /zoom-through-cut video effect for Claude Code. The camera flies into a frame on the wall and comes out in the next shot. Use it when the user types /zoom-through-cut or asks for this effect on their video.
---

# /zoom-through-cut

The camera flies into a frame on the wall and comes out in the next shot.

The camera pushes faster and faster into the picture frame on the wall behind me (a 10x zoom with motion blur), a one frame flash, and it comes out of the zoom into the next shot, which settles from 2.4x to 1x. Vignette and grain on top.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera, a picture frame or object behind you.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `ZoomThroughCut` in `commands-fx/src/fx/Grid5.tsx`. Register it in `src/Root.tsx`:
   `<ZoomThroughCut clip={c("input", 0).clip} next="fx/next.mp4" />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the target point and the next shot.
6. Render: `npx remotion render fx-zoom-through-cut out/zoom-through-cut.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> The camera pushes faster and faster into the picture frame on the wall behind me (a 10x zoom with motion blur), a one frame flash, and it comes out of the zoom into the next shot, which settles from 2.4x to 1x. Vignette and grain on top. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
