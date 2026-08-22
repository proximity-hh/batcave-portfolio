# Batcave Portfolio — Howard Huang

An interactive pixel-art Batcave landing page. One HTML file plus assets, no build step
and no dependencies. Open `index.html` in a browser, or drop the folder on any static host
(GitHub Pages, Netlify, Vercel, S3).

## Structure

```
batcave-portfolio/
├── index.html              all markup, styles and script
├── index-standalone.html   same site, everything embedded in one file
└── assets/
    ├── images/
    │   ├── overview.jpg        poster frame + title-screen backdrop
    │   ├── computer.jpg        Batcomputer area
    │   ├── armory.jpg          Armory area
    │   ├── batmobile.jpg       Batmobile bay area
    │   ├── computer_zoom.jpg   monitor wall close-up
    │   ├── armory_zoom.jpg     suit corridor close-up
    │   └── batmobile_zoom.jpg  cockpit close-up
    ├── video/
    │   └── overview.mp4        animated cave (replaces the still on the landing scene)
    ├── audio/
    │   ├── background.mp3      cave ambience (seamless 80s loop)
    │   ├── theme.mp3           title theme
    │   └── sfx.mp3             UI click
    └── ui/
        ├── screen-frame.png    monitor bezel, used as a 9-slice border-image
        ├── cursor-logo.png     default cursor sprite
        └── cursor-cowl.png     hover cursor sprite
```

## Navigation model

```
START SCREEN
   └── OVERVIEW ─── cyan outlines select an area
         ├── EXPERIENCE   (Batcomputer) ── amber marker ── monitor close-up + text panel
         ├── PAST WORK    (Batmobile)   ── amber marker ── cockpit close-up + text panel
         └── ABOUT ME     (Armory)      ── amber marker ── suits close-up + text panel
```

Back steps up one level (button, top right) and Escape does the same from anywhere.

## Editing your content

All copy lives in the `CONTENT` object near the top of the `<script>` in `index.html`.
Each entry takes HTML, so:

- `<h3>` — job title or project name
- `<p class="meta">` — employer, location, dates
- `<p>` — body text
- `<p class="tags">` — tech stack or short lists
- wrap each block in `<div class="entry">` to get the divider and bullet

Two links are still placeholders — search for `data-link="linkedin"` and drop in the real
URLs.

## Tuning the visuals

| What | Where |
|---|---|
| Colours | CSS variables at the top of `<style>` (`--cyan`, `--marker`, `--amber`) |
| Ambient FX | the `SCENE_FX` object — glows, scanlines, LEDs, bats, dust, per scene |
| Page backdrop | the backdrop IIFE — blur amount, bat count, dust density |
| Landing video | `assets/video/overview.mp4`; swap the file, keep the framing |
| Area outlines | the `<svg class="zones">` block inside the overview scene |
| Drill-in markers | the `<g class="zone">` groups inside each area scene |
| Audio levels | the `LVL` object in the audio module |

FX and hotspot coordinates are percentages of each artwork, measured against the source
images, so they stay aligned at any window size.

## Resolution

Artwork is exported at **2x its native size with nearest-neighbour scaling**, so the browser
scales *down* to fit a 1080p screen rather than up. Upscaling pixel art past 1:1 is what makes
it look soft and blocky; downscaling stays crisp. If you replace any artwork, run the same 2x
nearest step first — a smooth (bicubic) upscale will undo the benefit.

The scene is capped by `--stage-max` (top of the `<style>` block, default `1400px`) and
centred, so on a 1080p desktop the art renders *below* its asset resolution instead of being
stretched to fill. Raise it toward `1920px` for a bigger picture, lower it toward `1200px`
for maximum crispness. Setting it to `100vw` restores full-bleed.

The landing video is kept at its native 1920x1088. The parallax layer scales the scene by
3.5%, chosen so the pan never exposes an edge while adding as little upscale as possible.

## Notes

- Sized for 16:9 and fills the viewport. Artwork uses `cover`, and the SVG overlays use
  `preserveAspectRatio="xMidYMid slice"` to match it exactly, so hotspots never drift.
- The custom cursor and all animation are disabled on touch devices and under
  `prefers-reduced-motion`.
- Audio starts on the START press, since browsers block autoplay before a user gesture.
- The landing scene plays a muted, looping video. It carries its own animation, so the
  ambient FX layer is not applied there — the hover outlines and markers still are.

## Licensing

The Batman theme, the bat logo and the cowl silhouette are third-party copyrighted works,
fine for a local demo or coursework but a risk on a public site. Swap them for licensed or
original assets before publishing. The cowl source also carries a stock watermark.
