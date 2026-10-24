---
name: orbit
description: /orbit video effect for Claude Code. Small reel cards orbit around your body while the line lands in two voices above your head. Use it when the user types /orbit or asks for this effect on their video.
---

# /orbit

Small reel cards orbit around your body while the line lands in two voices above your head.

Cut me out. Eight small vertical reel cards (white rim, each with a black caption label burned in) orbit around my body at chest height, facing the camera: the near half passes in front of me, bigger and sharp; the far half passes behind my cutout, smaller and softer. They spin in fast and settle into a steady spin. Above my head the line lands in two voices: and you can get the in small Helvetica, word by word, then main character in a big Apple Garamond italic in pale gold with a soft glow and two twinkling sparkles.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera, chest and head in frame.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
   This effect shows small playing reels. For each reel video the user gives you, make stills: `ffmpeg -ss 0.5 -i <reel.mp4> -vf fps=7.5,scale=270:480 -frames:v 18 -q:v 3 commands-fx/public/tiles/<name>/%02d.jpg` and pass `fxs/<name>.mp4` in `reels` (the Tile component reads the stills).
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `Orbit` in `commands-fx/src/fx/Orbit.tsx`. Register it in `src/Root.tsx`:
   `<Orbit {...c("input", 0)} reels={["fxs/a.mp4","fxs/b.mp4","fxs/c.mp4","fxs/d.mp4","fxs/e.mp4","fxs/f.mp4","fxs/g.mp4","fxs/h.mp4"]} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the reels, their captions (CAPS) and the title words.
6. Render: `npx remotion render fx-orbit out/orbit.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. Eight small vertical reel cards (white rim, each with a black caption label burned in) orbit around my body at chest height, facing the camera: the near half passes in front of me, bigger and sharp; the far half passes behind my cutout, smaller and softer. They spin in fast and settle into a steady spin. Above my head the line lands in two voices: and you can get the in small Helvetica, word by word, then main character in a big Apple Garamond italic in pale gold with a soft glow and two twinkling sparkles. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
