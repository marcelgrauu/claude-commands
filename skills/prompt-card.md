---
name: prompt-card
description: /prompt-card video effect for Claude Code. The prompt types itself live and the button switches to Copied. Use it when the user types /prompt-card or asks for this effect on their video.
---

# /prompt-card

The prompt types itself live and the button switches to Copied.

Over my head a dark card with a yellow PROMPT label: three lines of prompt type themselves in mono with a yellow caret, then the Copy button turns yellow and switches to Copied with a small pop and a camera kick.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `PromptCard` in `commands-fx/src/fx/Grid7.tsx`. Register it in `src/Root.tsx`:
   `<PromptCard clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the prompt lines.
6. Render: `npx remotion render fx-prompt-card out/prompt-card.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Over my head a dark card with a yellow PROMPT label: three lines of prompt type themselves in mono with a yellow caret, then the Copy button turns yellow and switches to Copied with a small pop and a camera kick. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
