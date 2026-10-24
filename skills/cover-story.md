---
name: cover-story
description: /cover-story video effect for Claude Code. You are the cover of a magazine. Use it when the user types /cover-story or asks for this effect on their video.
---

# /cover-story

You are the cover of a magazine.

Cut me out and make me a magazine cover: a giant serif masthead EDIT behind my head, ISSUE 01 and the month small at the top, three coverlines with yellow rules sliding in on the left, a barcode in the corner.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `CoverStory` in `commands-fx/src/fx/Grid5.tsx`. Register it in `src/Root.tsx`:
   `<CoverStory {...c("input", 0)} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the masthead, coverlines and issue.
6. Render: `npx remotion render fx-cover-story out/cover-story.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Cut me out and make me a magazine cover: a giant serif masthead EDIT behind my head, ISSUE 01 and the month small at the top, three coverlines with yellow rules sliding in on the left, a barcode in the corner. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
