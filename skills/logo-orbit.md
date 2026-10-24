---
name: logo-orbit
description: /logo-orbit video effect for Claude Code. Your tool logos orbit around your head on a tilted ring. Use it when the user types /logo-orbit or asks for this effect on their video.
---

# /logo-orbit

Your tool logos orbit around your head on a tilted ring.

Cut me out. Eight app logo tiles (112 px, 28 px corners) spin onto a tilted ellipse around my head (720 x 220 px, -6 degrees) with a thin blue line. The near half passes in front of me, sharp and bigger; the far half passes behind my cutout, smaller and softer. They spin in with motion blur, then keep turning slowly.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera, room around the head.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `LogoOrbit` in `commands-fx/src/fx/Grid8.tsx`. Register it in `src/Root.tsx`:
   `<LogoOrbit {...c("input", 0)} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the logo tiles (LOGOS) and the ring (LCX, LCY, LRX, LRY).
6. Render: `npx remotion render fx-logo-orbit out/logo-orbit.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. Eight app logo tiles (112 px, 28 px corners) spin onto a tilted ellipse around my head (720 x 220 px, -6 degrees) with a thin blue line. The near half passes in front of me, sharp and bigger; the far half passes behind my cutout, smaller and softer. They spin in with motion blur, then keep turning slowly. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
