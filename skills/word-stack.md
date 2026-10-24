---
name: word-stack
description: /word-stack video effect for Claude Code. Giant words stack up next to your head. Use it when the user types /word-stack or asks for this effect on their video.
---

# /word-stack

Giant words stack up next to your head.

In the empty wall next to my head, stack three giant words: PASTE, THE, COMMAND. The first two in Inter Tight 800 white and the last in weight 300 with a white to yellow gradient. Each one rises out of a mask with a resolving blur and a small skew.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** any clip with space on one side.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `WordStack` in `commands-fx/src/fx/Grid2.tsx`. Register it in `src/Root.tsx`:
   `<WordStack clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the three words.
6. Render: `npx remotion render fx-word-stack out/word-stack.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> In the empty wall next to my head, stack three giant words: PASTE, THE, COMMAND. The first two in Inter Tight 800 white and the last in weight 300 with a white to yellow gradient. Each one rises out of a mask with a resolving blur and a small skew. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
