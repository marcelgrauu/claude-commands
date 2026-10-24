---
name: cursor-click
description: /cursor-click video effect for Claude Code. A cursor clicks Generate and the result appears. Use it when the user types /cursor-click or asks for this effect on their video.
---

# /cursor-click

A cursor clicks Generate and the result appears.

At chest height a glass Generate button appears. A cursor arcs in from the corner and presses it (the button sinks and turns yellow, a yellow ripple goes out) and the result image pops in above it with a bounce and a 4K tag.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `CursorClick` in `commands-fx/src/fx/Grid2.tsx`. Register it in `src/Root.tsx`:
   `<CursorClick clip={c("input", 0).clip} result="thumbs/result.jpg" />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the button text and the result image.
6. Render: `npx remotion render fx-cursor-click out/cursor-click.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> At chest height a glass Generate button appears. A cursor arcs in from the corner and presses it (the button sinks and turns yellow, a yellow ripple goes out) and the result image pops in above it with a bounce and a 4K tag. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
