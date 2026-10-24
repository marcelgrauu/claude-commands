---
name: checklist
description: /checklist video effect for Claude Code. An iOS Reminders style list that ticks itself off. Use it when the user types /checklist or asks for this effect on their video.
---

# /checklist

An iOS Reminders style list that ticks itself off.

In front of my chest, an iOS Reminders style card (soft white, big rounded corners): a Today title in orange and a 0 of 3 counter, three rows Drop your clips, Type the command, Post the reel. Each circle fills orange with a white check, the text gets struck through and greys out, the counter goes up, a small camera kick on each tick.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `Checklist` in `commands-fx/src/fx/Grid4.tsx`. Register it in `src/Root.tsx`:
   `<Checklist clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the steps (STEPS) and the tick frames (TICKS).
6. Render: `npx remotion render fx-checklist out/checklist.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> In front of my chest, an iOS Reminders style card (soft white, big rounded corners): a Today title in orange and a 0 of 3 counter, three rows Drop your clips, Type the command, Post the reel. Each circle fills orange with a white check, the text gets struck through and greys out, the counter goes up, a small camera kick on each tick. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
