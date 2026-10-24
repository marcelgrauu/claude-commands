---
name: comment-rain
description: /comment-rain video effect for Claude Code. Comments with your keyword rain all over the screen. Use it when the user types /comment-rain or asks for this effect on their video.
---

# /comment-rain

Comments with your keyword rain all over the screen.

Dark comment bubbles with avatars and usernames, all saying the keyword, pop in all over the screen one after another with a bounce and drift slowly upward.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `CommentRain` in `commands-fx/src/fx/Grid7.tsx`. Register it in `src/Root.tsx`:
   `<CommentRain clip={c("input", 0).clip} keyword="COMMANDS" />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the usernames and the keyword.
6. Render: `npx remotion render fx-comment-rain out/comment-rain.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Dark comment bubbles with avatars and usernames, all saying the keyword, pop in all over the screen one after another with a bounce and drift slowly upward. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
