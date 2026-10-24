---
name: all-at-once
description: /all-at-once video effect for Claude Code. The orbit and the reel fan fire on the same beat. Use it when the user types /all-at-once or asks for this effect on their video.
---

# /all-at-once

The orbit and the reel fan fire on the same beat.

Cut me out. On the same beat, five reels fan out behind my head like a hand of cards while six more small reels orbit around my head on a tilted ring, the near ones in front of me and the far ones behind. The camera pushes in 7%.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
   This effect shows small playing reels. For each reel video the user gives you, make stills: `ffmpeg -ss 0.5 -i <reel.mp4> -vf fps=7.5,scale=270:480 -frames:v 18 -q:v 3 commands-fx/public/tiles/<name>/%02d.jpg` and pass `fxs/<name>.mp4` in `reels` (the Tile component reads the stills).
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `AllAtOnce` in `commands-fx/src/fx/Grid4.tsx`. Register it in `src/Root.tsx`:
   `<AllAtOnce {...c("input", 0)} reels={["fxs/a.mp4","fxs/b.mp4","fxs/c.mp4","fxs/d.mp4","fxs/e.mp4","fxs/f.mp4","fxs/g.mp4"]} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the reels.
6. Render: `npx remotion render fx-all-at-once out/all-at-once.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. On the same beat, five reels fan out behind my head like a hand of cards while six more small reels orbit around my head on a tilted ring, the near ones in front of me and the far ones behind. The camera pushes in 7%. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
