# Winter Cabin

Full-screen animated winter scene: photographic mountain-cabin backgrounds with
falling snow, wind gusts and a fully procedural ambient soundtrack.

Open `index.html` in a browser. No build step, no external libraries.

## How it works

The backdrop is a set of sixteen photographs (in `img/`) — four series, each
covering four times of day: **napkelte** (sunrise), **dél** (noon),
**délután** (afternoon) and **este** (evening). The *Napszak* slider — or the
Auto cycle (~90 s per full day at 1×, adjustable 0.2×–4×) — crossfades smoothly
between the four photos, so the day passes over the same valley. All animation
is drawn live on a canvas above the photo:

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

- **Language** — a four-cell table: HU / EN / RO / DE. The whole panel, the
  time-of-day names and the snowfall levels switch instantly. The choice is
  remembered in `localStorage`; on a first visit the browser language decides,
  falling back to Hungarian.
- **Photo series** — a 2×2 table of four series.
- **Speed** — the auto day cycle runs at 0.2×–4×.
- **Snowfall** — snowfall intensity.
- **Time of day + Auto** — parks the day at any point or runs the full cycle.
- **Wind** — calm / breezy / stormy.
- **Sound** — master volume and per-layer toggles.

On desktop the compact panel sits in the top-right corner; the ✕ in its corner
closes it and a small glass button in the top-right corner of the screen brings
it back. Below 700px the flake count drops and the panel becomes a bottom
drawer with full-size, thumb-friendly controls, every row 5px from both screen
edges. With `prefers-reduced-motion: reduce` the scene shows the calm evening
photo with no falling snow.

The snow only falls — nothing settles. An earlier version grew a drift along
the bottom of the frame and whitened the pines, but it veiled the photograph
and read as haze, so the flakes now simply pass through the scene.

## Deployment

The site is plain static files, so it needs no build step. The repository is
connected to **Cloudflare Workers** (Workers & Pages → the `wintercabin`
project), which rebuilds and redeploys on every push to `main` and serves the
result from `https://wintercabin.laszlokasa6.workers.dev`.

Two committed files shape that deployment:

- `wrangler.jsonc` — names the Worker and points its static assets at the
  repository root. Committing it stops the build from generating a throwaway
  config on every run.
- `.assetsignore` — excludes everything that is not part of the site. Without
  it the build uploads the repository plumbing (`.git`, `.github`) and the docs
  as publicly readable files; only `index.html`, `procedural.html`, `img/` and
  `_headers` should be served.

`_headers` caches the photos for a year and keeps the HTML revalidating.

## The procedural version

The previous, fully code-drawn edition of this project — four procedurally
painted scenes with snow accumulation, clickable trees, a day/night cycle and
the same sound engine — is preserved as `procedural.html`.
