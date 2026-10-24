---
name: liquid-glass-graph
description: /liquid-glass-graph video effect for Claude Code. A thick liquid glass slab floats up in front of you with a graph that climbs. Use it when the user types /liquid-glass-graph or asks for this effect on their video.
---

# /liquid-glass-graph

A thick liquid glass slab floats up in front of you with a graph that climbs.

Float a thick liquid glass slab (820x540, 58 px corners) up in front of my chest. The glass must refract the real video: inside the slab draw the same clip magnified 12%, blurred 24 px and lifted, with a stronger magnification on a thin rim so it reads as a thick slab, a bright bevel on the top edge and a soft shadow cast on my shirt. Inside: a RETENTION label that tracks in, a big number counting from 19 to 68 percent, a dashed grey line for the last reel, and this reel's line drawing from left to right in a white to #FFE500 gradient with glow, data points popping as the tip passes them and a tooltip following the tip. When the line hits the top: a halo ring, a green +212% chip snaps in and the camera kicks. The slab flips up from the desk in 3D, then floats with a slow sway; the background softens slightly while it plays.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera, hands at chest height.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `LiquidGlassGraph` in `commands-fx/src/fx/LiquidGlassGraph.tsx`. Register it in `src/Root.tsx`:
   `<LiquidGlassGraph clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the label, the 19 to 68 values, the +212% chip and the time axis.
6. Render: `npx remotion render fx-liquid-glass-graph out/liquid-glass-graph.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Float a thick liquid glass slab (820x540, 58 px corners) up in front of my chest. The glass must refract the real video: inside the slab draw the same clip magnified 12%, blurred 24 px and lifted, with a stronger magnification on a thin rim so it reads as a thick slab, a bright bevel on the top edge and a soft shadow cast on my shirt. Inside: a RETENTION label that tracks in, a big number counting from 19 to 68 percent, a dashed grey line for the last reel, and this reel's line drawing from left to right in a white to #FFE500 gradient with glow, data points popping as the tip passes them and a tooltip following the tip. When the line hits the top: a halo ring, a green +212% chip snaps in and the camera kicks. The slab flips up from the desk in 3D, then floats with a slow sway; the background softens slightly while it plays. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
