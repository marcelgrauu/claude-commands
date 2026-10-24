---
name: growth-chart
description: /growth-chart video effect for Claude Code. 3D bars climb behind you to a big final number. Use it when the user types /growth-chart or asks for this effect on their video.
---

# /growth-chart

3D bars climb behind you to a big final number.

Cut me out. Behind me, seven 3D bars (dark graphite, the last one #FFE500 with a glow) rise one after another with a springy overshoot from a baseline at chest height, while a FOLLOWERS label and a big number count from 0 to 250,000 at the top. A camera kick when the count lands.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `GrowthChart` in `commands-fx/src/fx/Grid4.tsx`. Register it in `src/Root.tsx`:
   `<GrowthChart {...c("input", 0)} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the bar values (BARS) and the final number.
6. Render: `npx remotion render fx-growth-chart out/growth-chart.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. Behind me, seven 3D bars (dark graphite, the last one #FFE500 with a glow) rise one after another with a springy overshoot from a baseline at chest height, while a FOLLOWERS label and a big number count from 0 to 250,000 at the top. A camera kick when the count lands. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
