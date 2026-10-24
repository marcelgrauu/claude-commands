---
name: before-after
description: /before-after video effect for Claude Code. A seam of light turns your room into a white studio. Use it when the user types /before-after or asks for this effect on their video.
---

# /before-after

A seam of light turns your room into a white studio.

Cut me out. A seam of white light with a yellow glow crosses the frame from left to right and, as it passes, my room turns into an endless white studio with a soft floor shadow, while I stay the same. A label switches from BEFORE to AFTER.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `BeforeAfter` in `commands-fx/src/fx/Grid2.tsx`. Register it in `src/Root.tsx`:
   `<BeforeAfter {...c("input", 0)} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the BEFORE and AFTER labels.
6. Render: `npx remotion render fx-before-after out/before-after.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. A seam of white light with a yellow glow crosses the frame from left to right and, as it passes, my room turns into an endless white studio with a soft floor shadow, while I stay the same. A label switches from BEFORE to AFTER. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
