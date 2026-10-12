# Research Assistant Application: Daksh Teckchandani

An interactive presentation built for an interview for the Undergraduate Research Assistant position in the Department of Surgical Sciences, UW–Madison School of Veterinary Medicine. It runs as an ordinary website, and it is designed to be presented in person, one section at a time.

The whole site is `raWebsite.html`. There is no build step, no framework and no package manager. The only external request is for two Google Fonts. If that request fails, the page falls back to system fonts.

---

## Contents

1. [Presenting the site](#1-presenting-the-site)
2. [Deploying to GitHub Pages](#2-deploying-to-github-pages)
3. [Before you publish](#3-before-you-publish)
4. [How the file is organized](#4-how-the-file-is-organized)
5. [The nine chapters](#5-the-nine-chapters)
6. [CSS sections](#6-css-sections)
7. [JavaScript modules](#7-javascript-modules)
8. [3D designs and exports](#8-3d-designs-and-exports)
9. [Accessibility, reduced motion and print](#9-accessibility-reduced-motion-and-print)
10. [Customizing](#10-customizing)

---

## 1. Presenting the site

| Control | Action |
| --- | --- |
| `→` `↓` `Space` `Page Down` | Next. If the current chapter extends below the screen, it scrolls within the chapter first, then advances to the next one. |
| `←` `↑` `Shift+Space` `Page Up` | Previous, following the same logic in reverse |
| `Home` / `End` | First / last chapter |
| `F` | Toggle full screen and enable scroll snapping |
| Bottom control bar | Chapter counter, chapter title, previous/next buttons |
| Dots on the right (wide screens) | Jump to any chapter. Hover over a dot to see its title. |

Suggested setup for an in-person interview:

- Open the site in Chrome on a laptop.
- Press `F`.
- Advance with the arrow keys.
- Every animation starts when its chapter comes into view, so each section opens with its own sequence.

## 2. Deploying to GitHub Pages

1. Create a public repository, for example `lab-application`.
2. Upload `raWebsite.html` and this `README.md` to the root of the `main` branch.
3. Open **Settings → Pages**, choose **Deploy from a branch**, then select `main` and `/ (root)`.
4. The site goes live at `https://<username>.github.io/<repository>/raWebsite.html`, usually within a minute or two. GitHub Pages only serves a page at the bare repository URL if it is named `index.html`; to keep the shorter link, add a one-line `index.html` containing `<meta http-equiv="refresh" content="0; url=raWebsite.html">`.

Every later commit to `main` redeploys automatically. The repository history gives you a full record of every change and a one-click rollback.

## 3. Before you publish

Search `raWebsite.html` for `YOUR-` and replace both placeholders in the closing chapter:

- `https://www.linkedin.com/in/YOUR-LINKEDIN-HANDLE`
- `https://github.com/YOUR-GITHUB-USERNAME`

## 4. How the file is organized

`raWebsite.html` has three parts, in this order:

```
<head>
  meta, fonts
  <style>    CSS sections 1–14, each headed by a numbered comment block
<body>
  progress bar, top navigation, chapter dots, presentation control bar
  <main>     nine <section class="chapter"> elements
  <script>   JavaScript modules JS 1–JS 10, each headed by a numbered comment block
```

Every chapter is a `<section class="chapter">` with an `id` and a `data-title`. The `data-title` feeds the control bar and the dot tooltips. To add a chapter, add another section with both attributes. Navigation picks it up automatically.

## 5. The nine chapters

| # | `id` | Title | What it shows | Animation |
| --- | --- | --- | --- | --- |
| 1 | `intro` | Introduction | Headline, summary, education, cumulative GPA, current role | Interactive 2D finite-element mesh that deforms under the cursor and is colored by strain; staggered headline reveal |
| 2 | `why` | Why this lab | Motivation drawn from the cover letter; three working principles | Word-by-word heading reveal; icons that draw themselves; cards that tilt with the cursor |
| 3 | `website` | The lab website | Production work at CMIT NYCE and the four-step plan for the lab site | Live editor demo: fields type themselves, **Publish** is pressed, and a new lab member appears on the preview |
| 4 | `pipeline` | Report automation | The in-development report pipeline, with key figures | Counting statistics; files flow from scattered raw output into a validated grid and then into a report that is written and stamped "Reviewed" |
| 5 | `framework` | Validation framework | What the pipeline does and its scale, what version one reported, the audit (checking that each test can fail, invariant checks across modules, other findings), and what version two is designed around | Counting simulation total; grade-distribution bars built from the real counts; a permutation-versus-bootstrap demo; invariant bars; version-two items revealed in sequence |
| 6 | `research` | Research interests | Each defect from the audit mapped to the general research principle it violates | Connectors draw between columns with a travelling marker |
| 7 | `fit` | Qualifications | Candid assessment against each requirement in the posting | Level meters fill segment by segment |
| 8 | `areas` | Research areas | The lab's three research areas and the engineering contribution to each | Segmentation scan, interactive 3D bone mesh, rotating 3D double helix |
| 9 | `thanks` | Thank you | Closing note and contact links | Word-by-word heading reveal |

All numbers in chapter 5 come from an audit of the version-one code and result files, except the permutation demo, which runs on a synthetic trade list and is labeled as illustrative. The chapter 8 figures are illustrative.

## 6. CSS sections

| Section | Purpose |
| --- | --- |
| **1. Design tokens** | Colors, fonts, gutter and easing curve, all as custom properties on `:root`. `--v0` to `--v4` are the five stops of the viridis colormap used by every scientific figure. `--red` is UW red; `--night` tokens drive the dark chapters. |
| **2. Base** | Reset, body type, focus ring, `.wrap` container, `kbd` style, and `html.presenting` scroll snapping |
| **3. Navigation, deck, dots** | Top bar with the name mark, the floating control bar (`.deck`) and the chapter dots. `body.on-dark` restyles these whenever a dark chapter is active. |
| **4. Chapter scaffold and reveal system** | Full-height chapters, the large parallax numerals (`.bgnum`), chapter labels, titles and lead text. Also the reveal system: anything marked `data-r` fades and slides in when it scrolls into view (`data-r="left"`, `"right"` or `"scale"` change the direction). `.split` headings reveal word by word. |
| **5–13. Chapters** | One block per chapter, in page order, containing that chapter's layout and keyframe animations |
| **14. Reduced motion and print** | Final states for everything that animates, and a clean print stylesheet |

## 7. JavaScript modules

All script runs inside one IIFE. Every module is independent, so one can be removed without breaking the others.

| Module | What it does |
| --- | --- |
| **JS 1. Shared helpers** | `viridis(t)` maps 0–1 to the viridis colormap. `fitCanvas()` sizes a canvas for the device pixel ratio. `visibility(el)` returns an object whose `wait()` resolves when the element is on screen. `animateWhileVisible(el, frame)` runs a render loop only while the element is visible and the tab is active, so off-screen animations cost nothing. |
| **JS 2. Load, headings, reveals, counters** | Adds `loaded` to start the hero sequence, splits `.split` headings into word spans, staggers `data-r` reveals, and counts up `data-count` numbers (`data-prefix` and `data-suffix` are supported) with an IntersectionObserver |
| **JS 3. Presentation controls** | Builds the dots, tracks the active chapter, implements next/previous logic (scroll within a tall chapter before advancing), keyboard shortcuts and full screen. One throttled scroll handler drives the progress bar, the nav background and the parallax numerals. |
| **JS 4. Hero mesh** | A jittered triangular lattice. Each node is pulled back toward its rest position by a spring and pushed away by a moving load. The load follows the cursor, or drifts on its own when idle. Triangle strain is the mean relative change in edge length, colored with viridis. A click adds an impulse wave. |
| **JS 5. Card tilt** | Subtle perspective tilt that follows the cursor on `.tilt` cards. Only active for mouse and trackpad users. |
| **JS 6. Editor demo** | An async loop that types three example lab members into the form, presses Publish, adds a card to the preview, shows a toast, then resets. It pauses whenever the demo is off screen. |
| **JS 7. Pipeline particles** | Builds 36 file icons. Each has three positions stored as CSS custom properties (scattered, gridded, report). A single CSS keyframe moves them through all three. |
| **JS 8. Permutation vs bootstrap demo** | Generates a synthetic list of 60 trades with a positive total, split into three regime blocks of 20. **Pass A** shuffles trades within each block 60 times: every equity path passes through the same point at each block boundary and ends at the same total, so the share of profitable runs is always 0% or 100% and the 5th–95th percentile spread of final P&L is $0. **Pass B** resamples trades with replacement, so final P&L spreads out. The two passes alternate, with readouts updated after each. |
| **JS 9. 3D bone** | Procedural mesh, renderer, drag controls and OBJ export (see below) |
| **JS 10. 3D helix** | Rotating double helix with highlighted markers (see below) |

## 8. 3D designs and exports

Both 3D figures are drawn on a 2D canvas with a small hand-written renderer. There is no WebGL and no library. Each frame does three things:

1. Rotates every vertex with plain rotation matrices.
2. Applies a perspective divide: `f = D / (D − z)`.
3. Depth-sorts the faces back to front (painter's algorithm) and fills them.

### Bone finite-element mesh (`#bone`, JS 9)

**Geometry.** `buildBone()` creates a surface of revolution along the x-axis from −1 to 1:

- 60 rings along the length (`NU`) × 22 vertices per ring (`NV`) = 1,320 vertices and 1,298 quad faces.
- Ring radius = a slender shaft (`0.115 + 0.04x²`) plus two Gaussian bulges for the epiphyses, at x = −0.86 and x = 0.84.
- A taper term closes both ends.
- A slight bow (`0.06(1 − x²)`) and an elliptical flare near the ends make the shape read as a long bone rather than a cylinder.

**Stress shading.** The model is a beam loaded at a single point:

- The bending moment falls off linearly from the load position `L`, which sweeps back and forth along the shaft.
- Each vertex's stress is the moment times its distance from the neutral axis, so the top and bottom fibers carry the most.
- Faces are colored with viridis, low to high, and shaded by the angle between their normal and a fixed light.
- The yellow arrow marks the load. The two triangles mark the supports.

This is a visual model of beam bending, not a solved FE analysis.

**Interaction.**
- Drag horizontally to turn the bone and vertically to tilt it.
- When idle, it spins slowly about its own long axis and sways gently.
- The stress pattern moves with the spin, because the load direction stays fixed in the world.

**Export.** **Download mesh (.obj)** saves `long-bone-mesh.obj`, a Wavefront OBJ of the rest-pose mesh:

- Plain text: `v x y z` lines, followed by 1-indexed quad faces `f a b c d`.
- Units are normalized; the bone spans −1 to 1 on the x-axis.
- It opens directly in Blender (**File → Import → Wavefront**), MeshLab, ParaView and most CAD and FE pre-processors.
- To use it as an FE input, triangulate or remesh it first. Most meshing tools do this on import.
- To export a finer mesh, raise `NU` and `NV` at the top of `buildBone()`.

### Double helix (`#helix`, JS 10)

**Geometry.**
- 30 base pairs along the x-axis.
- Each strand is a helix with radius `R = 0.3` and a twist of `0.62` rad per base pair.
- The second strand is offset by `2.3` rad, giving the visible major and minor grooves.
- The whole figure rotates about its long axis and is tilted slightly for depth.

**Rendering.**
- Backbone segments, nucleotide beads and base-pair rungs are collected into one list, depth-sorted and drawn back to front.
- Beads scale with perspective and fade with depth.

**Markers.**
- The base pairs listed in `marked` light up in red or yellow once a scanning band passes over them.
- They are decorative, not real data.
- Edit the `marked` set to change which rungs light up.

### Segmentation scan (imaging card)

This one is pure SVG and CSS, with no script:

- Two blurred paths form a joint radiograph.
- `feTurbulence` adds film grain.
- A gradient beam sweeps downward while the segmentation contour draws itself (`stroke-dashoffset` on a path with `pathLength="1"`).
- Once the contour closes, the fill and bounding box fade in.

All of these keyframes share a single 4.8-second cycle (`xrscan`, `contour`, `xrfill`, `xrbox`), so they stay synchronized.

### Saving still images of any figure

- Every canvas figure (hero mesh, permutation demo, bone, helix) can be saved as a PNG. Right-click it and choose **Save image as…**.
- For a higher-resolution capture, zoom the browser to 200% first. Canvases render at up to 2× device pixel ratio.
- The SVG segmentation figure can be copied out of the page source as a standalone `.svg`.

## 9. Accessibility, reduced motion and print

- **Keyboard.** All navigation is available from the keyboard, and focus states are visible.
- **Labels.** Decorative figures are hidden from screen readers. Interactive and informative figures carry `aria-label` descriptions.
- **Reduced motion.** When the operating system requests it (`prefers-reduced-motion: reduce`):
  - every animation and transition is disabled;
  - canvases render a single static frame;
  - the editor demo shows its finished state;
  - bars and lists appear in their final state.
- **Small screens.** Layouts collapse to a single column down to 360 px wide. The chapter dots hide below 1,100 px.
- **Print.** Printing removes navigation and animated figures, switches dark chapters to white, and keeps the text content.

## 10. Customizing

| To change | Edit |
| --- | --- |
| Colors or fonts | CSS section 1, plus the Google Fonts `<link>` in `<head>` |
| Chapter order or titles | The `<section class="chapter">` elements, their `data-title` attributes, and the `.kicker` numbers and `.bgnum` numerals |
| Editor demo entries | The `entries` array in JS 6 |
| Permutation demo | `NT`, `NP`, `BLOCK` and the trade distribution in `makeTrades()` (JS 8) |
| Animation speed | Durations in each chapter's CSS block, `DUR`/`HOLD` in JS 8, and the `sleep()` values in JS 6 |
| Bone shape or resolution | `NU`, `NV` and the radius terms in `buildBone()` (JS 9) |
