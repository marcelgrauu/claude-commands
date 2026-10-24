---
name: gravity-drop
description: /gravity-drop video effect for Claude Code. Letters fall with gravity and bounce into place. Use it when the user types /gravity-drop or asks for this effect on their video.
---

# /gravity-drop

Letters fall with gravity and bounce into place.

Cut me out. A small yellow italic gravity sits above my head, then the letters of DROP fall from above one by one with real gravity, squash on impact, bounce twice and settle, in a big cream serif behind my head.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `GravityDrop` in `commands-fx/src/fx/Grid6.tsx`. Register it in `src/Root.tsx`:
   `<GravityDrop {...c("input", 0)} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the word.
6. Render: `npx remotion render fx-gravity-drop out/gravity-drop.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. A small yellow italic gravity sits above my head, then the letters of DROP fall from above one by one with real gravity, squash on impact, bounce twice and settle, in a big cream serif behind my head. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
