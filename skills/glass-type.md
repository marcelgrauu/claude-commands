---
name: glass-type
description: /glass-type video effect for Claude Code. Giant 3D glass letters standing in the room, behind you. Use it when the user types /glass-type or asks for this effect on their video.
---

# /glass-type

Giant 3D glass letters standing in the room, behind you.

Cut me out. Behind me, on the wall, raise a giant word in 3D glass letters (Inter Tight 900, about 270 px). Each letter has translucent thickness (16 offset slices), a face that refracts the room (a blurred, lifted, shifted plate of the wall), a bright rim stronger at the top left and a moving specular band. The letters flip up one by one from below. My head covers the bottom of the word so it reads as behind me.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
   Also make the wall plate: one frame of the clip scaled to 1080x1920, blurred 14 px and brightened x1.45, saved as `public/plates/input_glass.jpg`.
4. The component is `GlassType` in `commands-fx/src/fx/GlassType.tsx`. Register it in `src/Root.tsx`:
   `<GlassType {...c("input", 0)} plate="plates/input_glass.jpg" />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the word (WORD), its size (SIZE) and height (TOP).
6. Render: `npx remotion render fx-glass-type out/glass-type.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. Behind me, on the wall, raise a giant word in 3D glass letters (Inter Tight 900, about 270 px). Each letter has translucent thickness (16 offset slices), a face that refracts the room (a blurred, lifted, shifted plate of the wall), a bright rim stronger at the top left and a moving specular band. The letters flip up one by one from below. My head covers the bottom of the word so it reads as behind me. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
