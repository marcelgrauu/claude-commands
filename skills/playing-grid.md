---
name: playing-grid
description: /playing-grid video effect for Claude Code. You shrink into one tile of a grid of playing videos. Use it when the user types /playing-grid or asks for this effect on their video.
---

# /playing-grid

You shrink into one tile of a grid of playing videos.

My shot shrinks into the centre tile of a 3 by 3 grid of playing vertical videos, the other tiles popping in around me, some with big bold labels A SINGLE, OF THIS, SECOND, BIGGER.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
   This effect shows small playing reels. For each reel video the user gives you, make stills: `ffmpeg -ss 0.5 -i <reel.mp4> -vf fps=7.5,scale=270:480 -frames:v 18 -q:v 3 commands-fx/public/tiles/<name>/%02d.jpg` and pass `fxs/<name>.mp4` in `reels` (the Tile component reads the stills).
3. This effect needs no cutout.
4. The component is `PlayingGrid` in `commands-fx/src/fx/Grid6.tsx`. Register it in `src/Root.tsx`:
   `<PlayingGrid clip={c("input", 0).clip} reels={["fxs/a.mp4","fxs/b.mp4","fxs/c.mp4","fxs/d.mp4","fxs/e.mp4","fxs/f.mp4","fxs/g.mp4"]} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the labels and reels.
6. Render: `npx remotion render fx-playing-grid out/playing-grid.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> My shot shrinks into the centre tile of a 3 by 3 grid of playing vertical videos, the other tiles popping in around me, some with big bold labels A SINGLE, OF THIS, SECOND, BIGGER. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
