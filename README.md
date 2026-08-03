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
- **Photo series** — a 2×2 table of four series; switching starts a fresh snow cover.
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

Snow builds up while you watch: every landed flake raises a ground drift along
the bottom of the frame, and flakes that brush dark shapes — pines, log walls —
stick and slowly whiten them. A darkness map of the current photo decides where
snow can settle. Switching series starts the cover afresh.

## Deployment

The site is plain static files, so it needs no build step. It is published to
**Cloudflare Pages** on every push to `main` by
`.github/workflows/deploy-cloudflare.yml`, which collects `index.html`,
`procedural.html`, `img/` and `_headers` into a `public/` folder and uploads it
with `wrangler pages deploy`.

Two things have to exist before the first run:

1. A Cloudflare Pages project named **`wintercabin`** (Workers & Pages → Create →
   Pages → *Direct Upload*). It only has to be created once.
2. Two repository secrets (Settings → Secrets and variables → Actions):
   - `CLOUDFLARE_API_TOKEN` — an API token with the **Cloudflare Pages: Edit**
     permission.
   - `CLOUDFLARE_ACCOUNT_ID` — the account ID from the Cloudflare dashboard
     sidebar.

Alternatively, skip the workflow entirely and connect the GitHub repository in
the Cloudflare Pages dashboard (*Connect to Git*): with **no build command** and
the **repository root** as the output directory, Cloudflare builds and deploys
on every push by itself. `_headers` works the same way in both setups — the
photos are cached for a year, the HTML always revalidates.

## The procedural version

The previous, fully code-drawn edition of this project — four procedurally
painted scenes with snow accumulation, clickable trees, a day/night cycle and
the same sound engine — is preserved as `procedural.html`.
