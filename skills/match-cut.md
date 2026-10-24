---
name: match-cut
description: /match-cut video effect for Claude Code. Macro shots of real printed pages cut faster and faster on one word that never moves. Use it when the user types /match-cut or asks for this effect on their video.
---

# /match-cut

Macro shots of real printed pages cut faster and faster on one word that never moves.

Make a fast macro match cut: 16 cuts that accelerate down to 2 frames each, across 8 different printed pages (cream newsprint, glossy magazine, tech blog, dark mode, typewriter, salmon financial paper, tabloid caps, cover story), each tilted in 3D like a macro lens on a desk, cut faster and faster. The word Claude sits in a #FFE500 highlight box in the exact same pixel on every page: every rotation, tilt and push is pivoted on the word. Real depth of field: a sharp copy masked to a small oval around the word over a copy blurred 13 px. Warm light pool on the word, falloff to the edges, a 1 frame flash on each cut, and the last page holds while the focus closes in.

**What you need:** Nothing. It is a text and graphics only effect.
**What works best in the clip:** no clip needed.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. No clip needed.
3. This effect needs no cutout.
4. The component is `MatchCut` in `commands-fx/src/fx/MatchCut.tsx`. Register it in `src/Root.tsx`:
   `<MatchCut />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the locked word (WORD) and each page (PAGES).
6. Render: `npx remotion render fx-match-cut out/match-cut.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Make a fast macro match cut: 16 cuts that accelerate down to 2 frames each, across 8 different printed pages (cream newsprint, glossy magazine, tech blog, dark mode, typewriter, salmon financial paper, tabloid caps, cover story), each tilted in 3D like a macro lens on a desk, cut faster and faster. The word Claude sits in a #FFE500 highlight box in the exact same pixel on every page: every rotation, tilt and push is pivoted on the word. Real depth of field: a sharp copy masked to a small oval around the word over a copy blurred 13 px. Warm light pool on the word, falloff to the edges, a 1 frame flash on each cut, and the last page holds while the focus closes in. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
