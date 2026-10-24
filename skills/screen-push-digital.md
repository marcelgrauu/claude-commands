---
name: screen-push-digital
description: /screen-push-digital video effect for Claude Code. Words light up on a macro LED wall and the camera flies through them. Use it when the user types /screen-push-digital or asks for this effect on their video.
---

# /screen-push-digital

Words light up on a macro LED wall and the camera flies through them.

Make a macro shot of a real LED wall: a pixel grid with visible red, green and blue sub-pixels and the unlit ones faintly grey. The words light up one by one with bloom and a subtle flicker, and each word pushes the next: the camera flies through it (it grows huge and fades) and the next word comes out of the depth. The wall is slightly tilted in 3D, with defocus at the top and bottom and a vignette.

**What you need:** Nothing. It is a text and graphics only effect.
**What works best in the clip:** no clip needed.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. No clip needed.
3. This effect needs no cutout.
4. The component is `ScreenPushDigital` in `commands-fx/src/fx/ScreenPushDigital.tsx`. Register it in `src/Root.tsx`:
   `<ScreenPushDigital />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the words (WORDS) and when each one lands (SEG).
6. Render: `npx remotion render fx-screen-push-digital out/screen-push-digital.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Make a macro shot of a real LED wall: a pixel grid with visible red, green and blue sub-pixels and the unlit ones faintly grey. The words light up one by one with bloom and a subtle flicker, and each word pushes the next: the camera flies through it (it grows huge and fades) and the next word comes out of the depth. The wall is slightly tilted in 3D, with defocus at the top and bottom and a vignette. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
