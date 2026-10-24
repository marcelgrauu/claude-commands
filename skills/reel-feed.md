---
name: reel-feed
description: /reel-feed video effect for Claude Code. Your video becomes an Instagram Reels feed scrolled by thumb. Use it when the user types /reel-feed or asks for this effect on their video.
---

# /reel-feed

Your video becomes an Instagram Reels feed scrolled by thumb.

Turn the shot into a real Instagram Reels feed: status bar, Reels with a chevron and the camera on top, the tab bar at the bottom, and on every reel the heart, comment, repost and share counts, the three dots and the audio square on the right, the profile photo, username, Follow, caption and audio line at the bottom left, and a thin playback line. Scroll it like a thumb: always upwards, each swipe drags the page a quarter of the way while speeding up, lets go, and the page keeps that speed and eases onto the next reel. Six other reels pass and every swipe comes sooner and faster than the last, exponentially, until the last one lands on my reel again.

**What you need:** A vertical 9:16 clip shot on a locked camera (tripod).
**What works best in the clip:** any clip; it is your reel in the feed. Put the other reels at public/fx/<name>.mp4, your profile photo at public/pfp_me.jpg and six profile photos at public/art/a1.jpg to a6.jpg.

## Steps (Claude does them)

1. If there is no `commands-fx/` folder here, copy `~/.claude/skills/commands-base/project` to `./commands-fx` and run `npm install` inside it.
2. Copy the user's clip to `commands-fx/public/clips/input.mp4`, conformed to 30 fps: `ffmpeg -i <clip> -vf fps=30 -c:v libx264 -crf 16 commands-fx/public/clips/input.mp4`.
3. This effect needs no cutout.
4. The component is `ReelFeed` in `commands-fx/src/fx/RefMatch.tsx`. Register it in `src/Root.tsx`:
   `<ReelFeed clip={c("input", 0).clip} />`
   (`c("input", 0)` is the Root.tsx helper pointing to `clips/input.mp4` and its cutout `clips/input-alpha.mov`.)
5. Pull one frame of the clip with ffmpeg and look at it: fit the effect to the face and hands of THIS clip. Things you can change: the posts in the feed (POSTS: reel, user, caption, counts) and the swipe frames (SWIPES).
6. Render: `npx remotion render fx-reel-feed out/reel-feed.mp4` and check a sheet of 8 frames, transitions included. If anything covers the eyes, gets cut at the edge or fights the gesture, fix it and render again.
7. Hand over the mp4 and say in one line what you adjusted.

## Direct prompt (if you do not use the skill)

Paste this into Claude Code with your clip in the folder:

> Turn the shot into a real Instagram Reels feed: status bar, Reels with a chevron and the camera on top, the tab bar at the bottom, and on every reel the heart, comment, repost and share counts, the three dots and the audio square on the right, the profile photo, username, Follow, caption and audio line at the bottom left, and a thin playback line. Scroll it like a thumb: always upwards, each swipe drags the page a quarter of the way while speeding up, lets go, and the page keeps that speed and eases onto the next reel. Six other reels pass and every swipe comes sooner and faster than the last, exponentially, until the last one lands on my reel again. Build it in Remotion at 1080x1920 and 30 fps with professional motion (smooth curves, nothing static, the camera always slightly alive), and hold the last frame. Check the frames before calling it done.
