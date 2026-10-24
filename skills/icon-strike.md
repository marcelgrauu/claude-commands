---
name: icon-strike
description: /icon-strike video effect for Claude Code. Your old editing apps get struck out. Use it when the user types /icon-strike or asks for this effect on their video.
---

# /icon-strike

Your old editing apps get struck out.

Cut me out. Three app icons (CapCut, Edits, Premiere) pop in above my head, then a red line strikes through each one in turn and it fades back, with no more apps in white serif italic, apps in yellow, appearing above them.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `IconStrike` in `commands-fx/src/fx/Grid6.tsx`. Register it in `src/Root.tsx`:
   `<IconStrike {...c("input", 0)} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the three icons.
6. Render: `npx remotion render fx-icon-strike out/icon-strike.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. Three app icons (CapCut, Edits, Premiere) pop in above my head, then a red line strikes through each one in turn and it fades back, with no more apps in white serif italic, apps in yellow, appearing above them. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
