# Winter Cabin

Full-screen animated winter scene: photographic mountain-cabin backgrounds with
falling snow, wind gusts and a fully procedural ambient soundtrack.

Open `index.html` in a browser. No build step, no external libraries.

## How it works

The backdrop is a set of eight photographs (in `img/`) — two series, each
covering four times of day: **napkelte** (sunrise), **dél** (noon),
**délután** (afternoon) and **este** (evening). The *Napszak* slider — or the
Auto cycle (~90 s per full day) — crossfades smoothly between the four photos,
so the day passes over the same valley. All animation is drawn live on a canvas
above the photo:

- **Falling snow** — three depth layers (~350–500 flakes on desktop, stored in
  flat typed arrays): tiny slow far flakes, swaying mid flakes, and fast
  soft-edged near flakes. Over the dark evening photo the flakes glow a touch
  brighter.
- **Wind gusts** — every 8–15 seconds a gust rises, holds and eases off; the
  flakes drift sideways in proportion to their size and a faint veil of blowing
  snow sweeps across the lower slopes.
- **Snowfall intensity** — from light flurry to snowstorm, with matching flake
  counts.

## Sound

Everything is synthesized live with the Web Audio API — no audio files:

- **Wind** — looped pink-ish noise through a swept band-pass filter; its gain
  and frequency follow the actual wind simulation, so gusts audibly howl.
- **Fire** — high-passed noise driven by tiny random gain spikes: a fireplace
  crackle for the cabin's chimney smoke.
- **Ambience** — a very quiet detuned warm chord that breathes, a touch louder
  in the evening.

A master volume slider plus individual toggles; sound starts only when switched
on (browser autoplay rules).

## Controls

- **Language** — a four-cell table: HU / EN / RO / GE. The whole panel, the
  time-of-day names and the snowfall levels switch instantly. The choice is
  remembered in `localStorage`; on a first visit the browser language decides,
  falling back to Hungarian.
- **Photo series** — switches between the two photo series.
- **Snowfall** — snowfall intensity.
- **Time of day + Auto** — parks the day at any point or runs the full cycle.
- **Wind** — calm / breezy / stormy.
- **Sound** — master volume and per-layer toggles.

Below 700px the flake count drops and the control panel collapses into a bottom
drawer. With `prefers-reduced-motion: reduce` the scene shows the calm evening
photo with no falling snow.

## The procedural version

The previous, fully code-drawn edition of this project — four procedurally
painted scenes with snow accumulation, clickable trees, a day/night cycle and
the same sound engine — is preserved as `procedural.html`.
