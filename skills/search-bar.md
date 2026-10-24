---
name: search-bar
description: /search-bar video effect for Claude Code. A search bar types the question and the results drop. Use it when the user types /search-bar or asks for this effect on their video.
---

# /search-bar

A search bar types the question and the results drop.

Over my head a white iOS search bar types claude video effects, then three result cards drop in under it one by one, each with a bold title and a grey meta line.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** talking to camera.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `SearchBar` in `commands-fx/src/fx/Grid7.tsx`. Register it in `src/Root.tsx`:
   `<SearchBar clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the query and the results.
6. Render: `npx remotion render fx-search-bar out/search-bar.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Over my head a white iOS search bar types claude video effects, then three result cards drop in under it one by one, each with a bold title and a grey meta line. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
