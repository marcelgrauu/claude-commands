---
name: giant-captions
description: /giant-captions video effect for Claude Code. One giant caption word behind you, the small caption under it. Use it when the user types /giant-captions or asks for this effect on their video.
---

# /giant-captions

One giant caption word behind you, the small caption under it.

Cut me out. Each spoken word appears giant behind my head (Inter Tight 900, alternating white and yellow, snapping in from 1.25x) while the normal caption builds up word by word on a dark pill under me.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `GiantCaptions` in `commands-fx/src/fx/Grid6.tsx`. Register it in `src/Root.tsx`:
   `<GiantCaptions {...c("input", 0)} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the caption words (CAPS).
6. Render: `npx remotion render fx-giant-captions out/giant-captions.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. Each spoken word appears giant behind my head (Inter Tight 900, alternating white and yellow, snapping in from 1.25x) while the normal caption builds up word by word on a dark pill under me. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
