---
name: ui-world
description: /ui-world video effect for Claude Code. Claude's interface becomes the ceiling and you sit under it. Use it when the user types /ui-world or asks for this effect on their video.
---

# /ui-world

Claude's interface becomes the ceiling and you sit under it.

Cut me out. A huge Claude window unfolds in 3D above and behind my head (my head overlaps its bottom edge), it spills warm light down on me, four of my clips fly up from the desk in arcs and drop into the composer one by one, a small grey 4 clips · 0:48 count fades in, the prompt Turn these into a reel types itself, the send button pulses with a camera kick and an Editing your reel bar fills.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera, room above the head.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `UiWorld` in `commands-fx/src/fx/UiWorld.tsx`. Register it in `src/Root.tsx`:
   `<UiWorld {...c("input", 0)} thumbs={["thumbs/a.jpg","thumbs/b.jpg","thumbs/c.jpg","thumbs/d.jpg"]} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the prompt text and the four thumbnails.
6. Render: `npx remotion render fx-ui-world out/ui-world.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out. A huge Claude window unfolds in 3D above and behind my head (my head overlaps its bottom edge), it spills warm light down on me, four of my clips fly up from the desk in arcs and drop into the composer one by one, a small grey 4 clips · 0:48 count fades in, the prompt Turn these into a reel types itself, the send button pulses with a camera kick and an Editing your reel bar fills. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
