---
name: cutout-grid
description: /cutout-grid video effect for Claude Code. A 2x2 grid of images fills the frame behind you and changes with each idea. Use it when the user types /cutout-grid or asks for this effect on their video.
---

# /cutout-grid

A 2x2 grid of images fills the frame behind you and changes with each idea.

Cut me out. Behind me the frame fills with a 2 by 2 grid of four images that scale in one by one, then all four swap together for the next idea, with a centred caption on my chest.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `CutoutGrid` in `commands-fx/src/fx/Grid9.tsx`. Register it in `src/Root.tsx`:
   `<CutoutGrid {...c("input", 0)} (images in public/art) />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the two sets of four images and the captions.
6. Render: `npx remotion render fx-cutout-grid out/cutout-grid.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. Behind me the frame fills with a 2 by 2 grid of four images that scale in one by one, then all four swap together for the next idea, with a centred caption on my chest. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
