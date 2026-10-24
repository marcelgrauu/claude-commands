---
name: jiggle-delete
description: /jiggle-delete video effect for Claude Code. Your editing apps jiggle and get deleted until only Claude is left. Use it when the user types /jiggle-delete or asks for this effect on their video.
---

# /jiggle-delete

Your editing apps jiggle and get deleted until only Claude is left.

At the bottom of the frame, a white iPhone edit mode card shows CapCut, Edits, InShot and Premiere jiggling like when you delete apps, each with its minus badge. They get deleted one by one with a small yellow pop and the rest slide over to fill the gap. At the end only the Claude icon appears, with a bounce and a yellow ring.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `JiggleDelete` in `commands-fx/src/fx/Grid3.tsx`. Register it in `src/Root.tsx`:
   `<JiggleDelete clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the apps (APPS) and when each one is deleted (DEL).
6. Render: `npx remotion render fx-jiggle-delete out/jiggle-delete.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> At the bottom of the frame, a white iPhone edit mode card shows CapCut, Edits, InShot and Premiere jiggling like when you delete apps, each with its minus badge. They get deleted one by one with a small yellow pop and the rest slide over to fill the gap. At the end only the Claude icon appears, with a bounce and a yellow ring. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
