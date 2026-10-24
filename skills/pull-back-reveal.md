---
name: pull-back-reveal
description: /pull-back-reveal video effect for Claude Code. Tight on you, then the camera pulls back and you are inside Claude Code. Use it when the user types /pull-back-reveal or asks for this effect on their video.
---

# /pull-back-reveal

Tight on you, then the camera pulls back and you are inside Claude Code.

Start full screen on my clip. The camera pulls back and my video shrinks into a panel inside a Claude Code desktop window (macOS dots, light theme), with a task list on the left that ticks itself green: Transcribed 1 clip, Picked the best take, Cut 3 silences, Added captions, Rendered 4K, and a Reply to Claude box at the bottom.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `PullBackReveal` in `commands-fx/src/fx/Grid5.tsx`. Register it in `src/Root.tsx`:
   `<PullBackReveal clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the task list (TASKS).
6. Render: `npx remotion render fx-pull-back-reveal out/pull-back-reveal.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Start full screen on my clip. The camera pulls back and my video shrinks into a panel inside a Claude Code desktop window (macOS dots, light theme), with a task list on the left that ticks itself green: Transcribed 1 clip, Picked the best take, Cut 3 silences, Added captions, Rendered 4K, and a Reply to Claude box at the bottom. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
