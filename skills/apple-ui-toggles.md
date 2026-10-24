---
name: apple-ui-toggles
description: /apple-ui-toggles video effect for Claude Code. An iOS settings group in liquid glass whose switches flip on one line at a time. Use it when the user types /apple-ui-toggles or asks for this effect on their video.
---

# /apple-ui-toggles

An iOS settings group in liquid glass whose switches flip on one line at a time.

In front of my chest, float an iOS 26 settings group in frosted liquid glass (it refracts the video) with three rows, each with a real Settings style icon: One prompt, Paste into Claude, Perfect result. The switches flip on one by one: the knob stretches while it travels, the track floods green #34C759 from the knob, the row lights up and the camera nudges towards that row. After the third: a green rim lights the whole card, a Done chip drops in under it and the camera pushes in. The card flips up in 3D at the start.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `AppleToggles` in `commands-fx/src/fx/AppleToggles.tsx`. Register it in `src/Root.tsx`:
   `<AppleToggles clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the three rows (ROWS) and the flip frames (FLIPS).
6. Render: `npx remotion render fx-apple-ui-toggles out/apple-ui-toggles.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> In front of my chest, float an iOS 26 settings group in frosted liquid glass (it refracts the video) with three rows, each with a real Settings style icon: One prompt, Paste into Claude, Perfect result. The switches flip on one by one: the knob stretches while it travels, the track floods green #34C759 from the knob, the row lights up and the camera nudges towards that row. After the third: a green rim lights the whole card, a Done chip drops in under it and the camera pushes in. The card flips up in 3D at the start. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
