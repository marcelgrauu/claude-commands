---
name: paper-cut
description: /paper-cut video effect for Claude Code. You become a paper sticker on torn kraft paper. Use it when the user types /paper-cut or asks for this effect on their video.
---

# /paper-cut

You become a paper sticker on torn kraft paper.

Cut me out and turn me into a paper sticker: a thick white border all around my cutout, a lifted shadow, a slight wobble, popping up onto a kraft paper background with a torn cream paper strip and a big black CUT IT headline behind me.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `PaperCut` in `commands-fx/src/fx/Grid5.tsx`. Register it in `src/Root.tsx`:
   `<PaperCut cut={c("input", 0).cut} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the headline (CUT IT).
6. Render: `npx remotion render fx-paper-cut out/paper-cut.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out and turn me into a paper sticker: a thick white border all around my cutout, a lifted shadow, a slight wobble, popping up onto a kraft paper background with a torn cream paper strip and a big black CUT IT headline behind me. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
