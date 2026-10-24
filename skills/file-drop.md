---
name: file-drop
description: /file-drop video effect for Claude Code. Your clip flies into a drop zone and uploads. Use it when the user types /file-drop or asks for this effect on their video.
---

# /file-drop

Your clip flies into a drop zone and uploads.

Above my head, a dashed Drop raw footage zone appears. A thumbnail of my clip flies in on an arc and drops in, the border turns green, raw_take_01.mov uploads with a green bar and a percentage, and Ready to edit pops in.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `FileDrop` in `commands-fx/src/fx/Grid6.tsx`. Register it in `src/Root.tsx`:
   `<FileDrop clip={c("input", 0).clip} thumb="thumbs/clip.jpg" />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the file name.
6. Render: `npx remotion render fx-file-drop out/file-drop.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Above my head, a dashed Drop raw footage zone appears. A thumbnail of my clip flies in on an arc and drops in, the border turns green, raw_take_01.mov uploads with a green bar and a percentage, and Ready to edit pops in. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
