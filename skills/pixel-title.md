---
name: pixel-title
description: /pixel-title video effect for Claude Code. Your title assembles from flying dots above your head. Use it when the user types /pixel-title or asks for this effect on their video.
---

# /pixel-title

Your title assembles from flying dots above your head.

Cut me out. Above my head, a word assembles out of hundreds of dots: each dot flies in from a random spot and settles into the letter shapes of the word in Inter Tight 900, mostly white with a few yellow, with a soft glow. The word sits behind my head.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera, room above the head.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `PixelTitle` in `commands-fx/src/fx/Grid4.tsx`. Register it in `src/Root.tsx`:
   `<PixelTitle {...c("input", 0)} word="COMMANDS" />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the word.
6. Render: `npx remotion render fx-pixel-title out/pixel-title.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. Above my head, a word assembles out of hundreds of dots: each dot flies in from a random spot and settles into the letter shapes of the word in Inter Tight 900, mostly white with a few yellow, with a soft glow. The word sits behind my head. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
