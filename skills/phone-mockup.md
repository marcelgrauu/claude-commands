---
name: phone-mockup
description: /phone-mockup video effect for Claude Code. A real iPhone slides out from behind you and a finger taps Approve. Use it when the user types /phone-mockup or asks for this effect on their video.
---

# /phone-mockup

A real iPhone slides out from behind you and a finger taps Approve.

From behind my head and shoulders, a real iPhone slides out to my side and settles slightly turned, with a soft bob. On its screen an assistant app shows one recommendation card with three small bars and an Approve button. A soft fingertip comes in, taps Approve and it turns into a green Applied.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod). This effect cuts you out to put things behind you.
**What works best in the clip:** talking to camera on a locked camera, space on one side.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. Cut the person out with a transparent background:
   `python commands-fx/cutout.py commands-fx/public/clips/input.mp4 commands-fx/public/clips/input-alpha.mov <temp_folder> <fps>`
   (install `pip install rembg onnxruntime pillow numpy` first if missing). Delete the temp folder afterwards.
4. The component is `PhoneMockup` in `commands-fx/src/fx/RefMatch.tsx`. Register it in `src/Root.tsx`:
   `<PhoneMockup {...c("input", 0)} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the app on the screen (Edit Assistant, the recommendation) and the tap frame (TAP).
6. Render: `npx remotion render fx-phone-mockup out/phone-mockup.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> From behind my head and shoulders, a real iPhone slides out to my side and settles slightly turned, with a soft bob. On its screen an assistant app shows one recommendation card with three small bars and an Approve button. A soft fingertip comes in, taps Approve and it turns into a green Applied. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
