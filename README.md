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
    │   ├── overview.jpg        wide cave shot (landing scene) + title backdrop
    │   ├── computer.jpg        Batcomputer area
    │   ├── armory.jpg          Armory area
    │   ├── batmobile.jpg       Batmobile bay area
    │   ├── computer_zoom.jpg   monitor wall close-up
    │   ├── armory_zoom.jpg     suit corridor close-up
    │   └── batmobile_zoom.jpg  cockpit close-up
    ├── logos/                  company logo chips (128px square)
    ├── work/                   two previews per entry, named <key>-1 / <key>-2
    │                           experience tiles are real; the three project
    │                           tiles are still generated placeholders
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

## Previews and case studies

Each experience and project shows two images on hover (always visible on touch). They live in
`assets/work/` and are named after the entry key:

```
specsavers-1.jpg   illuminate-1.jpg   orbit-1.jpg
props-assist-1.jpg ai-tictactoe-1.jpg npo-connect-1.jpg
howard-1.jpg                                           (and -2 for each)
```

Drop your own screenshots in at those filenames and nothing else needs to change. Tiles are
16:10 (1200x750). Source images of any shape are normalised onto that tile by placing the
image uncropped over a blurred, dimmed copy of itself, so portrait photos and wide slides sit
side by side without distortion or lost content.

A tile can be video instead of an image - see `orbit-2.mp4`, which uses a `<video>` with a
poster frame and the same styling as the stills.

The three projects also open a full case study. That copy is in the `CASE_STUDIES` object in
`index.html` — each has a title, kicker, three facts, two shots and a list of sections. Add a
new one by giving its entry `data-case="your-id"` and adding a matching key.

## Tuning the visuals

| What | Where |
|---|---|
| Colours | CSS variables at the top of `<style>` (`--cyan`, `--marker`, `--amber`) |
| Ambient FX | the `SCENE_FX` object — glows, scanlines, LEDs, bats, dust, per scene |
| Page backdrop | the backdrop IIFE — blur amount, bat count, dust density |
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

The landing scene is a still with the ambient FX layer over it — monitor flicker, scanlines,
rack LEDs, the armory strip lights, the waterfall and splash, drifting bats and their red
eyes. All of it is configured in `SCENE_FX.overview`.

The scene is capped by `--stage-max` (top of the `<style>` block, default `1400px`) and
centred, so on a 1080p desktop the art renders *below* its asset resolution instead of being
stretched to fill. Raise it toward `1920px` for a bigger picture, lower it toward `1200px`
for maximum crispness. Setting it to `100vw` restores full-bleed.

The parallax layer scales the scene by
3.5%, chosen so the pan never exposes an edge while adding as little upscale as possible.

## Mobile

On a portrait phone the 16:9 scene sits as a band near the top and a stacked button rail
appears underneath it — tapping a tiny outline on a 390px screen is unreliable, so the rail
gives full-width targets instead. The rail is contextual: three sections on the overview, one
"OPEN FILE" button inside an area, and nothing while a readout is open. Landscape and desktop
use the outlines as before and never show the rail. Because the artwork uses
`cover` and the SVG overlays use `slice`, the hotspots stay locked to the art at every size —
nothing needs re-measuring per breakpoint.

On touch devices the area labels are always visible (there is no hover to reveal them), tap
targets are padded out, the readout panel goes near-fullscreen, the custom cursor is off, and
the backdrop dust is dropped.

## Notes

- Sized for 16:9 and fills the viewport. Artwork uses `cover`, and the SVG overlays use
  `preserveAspectRatio="xMidYMid slice"` to match it exactly, so hotspots never drift.
- The custom cursor and all animation are disabled on touch devices and under
  `prefers-reduced-motion`.
- Audio starts on the START press, since browsers block autoplay before a user gesture.

## Licensing

The Batman theme, the bat logo and the cowl silhouette are third-party copyrighted works,
fine for a local demo or coursework but a risk on a public site. Swap them for licensed or
original assets before publishing. The cowl source also carries a stock watermark.
