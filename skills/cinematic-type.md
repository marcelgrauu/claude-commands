---
name: cinematic-type
description: /cinematic-type video effect for Claude Code. Full colour cards with a big serif italic phrase and a line that comes in word by word. Use it when the user types /cinematic-type or asks for this effect on their video.
---

# /cinematic-type

Full colour cards with a big serif italic phrase and a line that comes in word by word.

Three full screen colour cards (burnt orange, near black, yellow). Each new card pushes the last one up with a fast eased move and vertical motion blur. On each, a big Apple Garamond italic phrase arrives letter by letter out of a blur while its tracking tightens, then a small Helvetica bold line comes in word by word underneath with its last words dimmed. Slow push in, a soft light in the centre and fine grain.

**What you need:** Nothing. It is a text and graphics only effect.
**What works best in the clip:** no clip needed.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. No clip needed.
3. This effect needs no cutout.
4. The component is `CinematicType` in `commands-fx/src/fx/Grid9.tsx`. Register it in `src/Root.tsx`:
   `<CinematicType />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the three cards (CT).
6. Render: `npx remotion render fx-cinematic-type out/cinematic-type.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Three full screen colour cards (burnt orange, near black, yellow). Each new card pushes the last one up with a fast eased move and vertical motion blur. On each, a big Apple Garamond italic phrase arrives letter by letter out of a blur while its tracking tightens, then a small Helvetica bold line comes in word by word underneath with its last words dimmed. Slow push in, a soft light in the centre and fine grain. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
