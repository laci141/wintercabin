# Winter Cabin

A full-screen animated winter scene: a snowy mountain landscape with falling snow,
spruce trees and a log cabin, drawn entirely with code on an HTML canvas.

Open `index.html` in a browser. No build step, no image files, no external
libraries — one self-contained file.

## The scene

Painted back to front: a day/night-driven sky with stars and a haloed moon, two
overlapping jagged mountain ridges, a blurred distant tree line, procedurally
drawn mid-ground spruces, a log cabin with warm glowing windows, and smooth
foreground snowdrifts.

## Falling snow

Three depth layers with distinct personalities, interleaved through the scene so
the depth reads:

| Layer | Share | Size | Behaviour | Drawn |
|---|---|---|---|---|
| Far | ~45% | 1–1.5px | slow, transparent, almost straight down | behind the mid-ground trees |
| Mid | ~35% | 2–3px | gentle sine sway, own phase and amplitude | in front of trees, behind the drifts |
| Near | ~20% | 4–6px | fast, soft-edged, strongest sway | on top of everything |

Roughly 350–500 flakes on desktop, stored in flat typed arrays. Every 8–15
seconds a gust arrives: wind eases up over ~1.5s, holds, then eases back down
over ~3s. Flakes gain horizontal velocity in proportion to their size, the tree
tops lean and spring back, the smoke column bends downwind, and a faint veil of
blowing snow sweeps across the drift tops.

## Snow accumulation

The world gets snowier as you watch. Landed flakes raise a ground heightmap that
is smoothed each pass so the drifts stay soft and rounded, and the blankets on
the tree tiers, the cabin roof and the fence rail thicken over minutes up to a
maximum.

- **Click a tree** to shake it: its snow slides off in soft clumps that burst
  into powder on landing, the branches spring back, and that tree starts
  accumulating again from thin.
- **Click the cabin roof** for a mini-avalanche off the eaves.
- **Reset snow** gently melts all accumulation back to the base state.

## Day/night cycle

About 90 seconds per full day, interpolated smoothly so the scene is composed at
any point in the cycle: golden sunset, blue hour, night (full starfield, moon,
and the window light pool as the brightest thing in the frame), then dawn. The
Auto toggle runs the cycle; the slider parks it anywhere.

## Rendering

Static geometry is drawn once to offscreen canvases per depth group and
re-composited with `drawImage` at integer coordinates, re-rendering a layer only
when it actually changes — accumulation growth every ~2s, a shaken tree, a melt.
The day/night tint is a `globalCompositeOperation` overlay pass rather than a
geometry redraw. Animated elements (snow, smoke, sparkles, window flicker) draw
on top each frame. `devicePixelRatio` is honoured, capped at 2.

## Responsiveness and motion

Below 700px the flake count drops and the control panel collapses into a bottom
drawer. With `prefers-reduced-motion: reduce` the scene starts still and fully
snow-covered at night with the windows glowing, and no falling snow.
