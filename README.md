# RF Bench — S-Parameter & Antenna Analysis Suite

**A professional, fully client-side VNA post-processor in a single HTML file.**
No build step, no dependencies, no server, no uploads — your Touchstone data never leaves the browser.

[![CI](https://github.com/USERNAME/rf-bench/actions/workflows/ci.yml/badge.svg)](https://github.com/USERNAME/rf-bench/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![No dependencies](https://img.shields.io/badge/dependencies-0-brightgreen)](package.json)
[![Single file](https://img.shields.io/badge/bundle-one%20HTML%20file-orange)](index.html)

> **Live demo:** https://USERNAME.github.io/rf-bench/ *(after publishing — see [Go live](#go-live-in-2-minutes))*

![Sample IEEE export](assets/sample_ieee_span.png)
*Two-column-span IEEE export (7.16 × 3.10 in, 600 dpi) produced by the built-in figure exporter from the synthetic demo data: vertical legend, 10 pt Arial numerals, centred axis titles, per-curve print symbols and two markers with interpolated (x, y) labels.*

---

## Quick start

```bash
# just open it — that's the whole install
open index.html            # macOS
xdg-open index.html        # Linux
start index.html           # Windows
```

Then either **drag & drop Touchstone files** (`.s1p … .s8p`, v1.0 & v2.0) anywhere on the page, or press
**Demo data** to load three synthetic datasets (dual-band antenna `.s2p`, 4-port MIMO array `.s4p`, 1-port monopole `.s1p`).

## Feature tour

| Area | Highlights |
|---|---|
| **Loading** | Touchstone v1/v2, S/Y/Z/H/G parameters, MA/DB/RI formats, any frequency unit, per-file reference impedance, v2 noise blocks & `[Information]`, multi-file overlay, rename/recolour/solo/hide per file |
| **Summary** | min return loss, interpolated −10 dB & threshold bands, fractional BW, resonance (X = 0), Z@res, VSWR, gain/efficiency/realized gain/mismatch, MSG/MGA, Rollett K, μ₁/μ₂, σmax(S) passivity, reciprocity |
| **Diagrams** | \|S\| dB (with band shading & hover detail cards), phase & group delay, **Smith chart** (Z/Y grids, VSWR/stability/gain/NF circles), VSWR & R/X, gain & efficiency, stability (K, \|Δ\|, μ + circles), mixed-mode Sdd/Scc/Scd + CMRR (4-port), noise figure + circles, integrity (passivity/reciprocity) |
| **TDR** | low-pass inverse-DFT reflectometry with DC extrapolation (none/const/linear), Kaiser/Hann/Hamming/Blackman/rect windows, distance or time axis, step ρ(t) and step impedance Z(t), reflection-event table, range-resolution readout |
| **Viewer UX** | per-diagram toolbar (fit X+Y, fit Y, ±zoom, ↑↓ pan, maximise-alone, reset), wheel = X+Y zoom, axis-gutter wheel = single axis, Shift-drag box zoom, `A` autoscale, `F` fit-current-view, click-legend-to-hide, live `f =` hover chip, unlocked Y axes |
| **Markers** | **per-diagram, one click = one point** on the clicked curve; M1/M2 + unlimited M3… (Alt+click, `+`, `+M`), interpolated (x, y) labels with collision-free placement & leader lines, sidebar ownership dropdown, in-app **self-test** button |
| **Band report** | sidebar tree of every −10 dB band: f_low / f_centre / f_high / BW / FBW % (interpolated edges); click a row to highlight the band on the diagrams; selectable CSV export |
| **Tools** | L-section matching synthesis (both topologies + swept result), quarter-wave transformer, single-shunt-stub matcher with predicted-RL sweep, band analyser, fixture de-embedding (delay / √f loss / ABCD cascade), cascade-networks builder (plot or download .s2p), antenna efficiency, wavelength calculator |
| **Exports** | **IEEE figures per diagram**: SVG (true vector) & PNG (600 dpi) at 3.5 × 2.75 in (journal column) or 7.16 × 3.10 in (two-column span), white ground, outward ticks, Arial 10 pt numerals, print-safe palette; CSV of any table; JSON summary of all metrics |

### Mouse & keyboard reference

| Input | Action |
|---|---|
| Click / Shift+click diagram | M1 / M2 on **that** diagram (single point on clicked curve) |
| Alt+click, `+`, toolbar `+M` | extra marker M3… on that diagram |
| Wheel / Shift+wheel / Alt+wheel | zoom X+Y / X / Y around cursor |
| Wheel over axis gutter | zoom that axis only |
| Drag / Shift+drag | pan / box-zoom |
| Double-click, `↺` | reset view |
| `⛶ fit`, `⇕ fit Y`, `+ − ↑ ↓` | fit both / fit current view / vertical zoom & pan |
| `⤢` | maximise diagram alone (Esc restores) |
| `A`, `F` | autoscale all diagrams / fit hovered diagram to current view |
| `1…9`, `0` | switch tabs / Tools |
| `O`, `Esc` | open files / clear markers |
| Legend row click | hide/show that trace |
| Band shade hover | start/stop/centre/BW/FBW/min detail card |

## Touchstone support matrix

| | v1.0 | v2.0 |
|---|---|---|
| `.s1p … .s8p` (ports inferred from extension or row length) | ✅ | ✅ |
| `# GHz S MA R 50` style option lines (any unit, S/Y/Z/H/G, MA/DB/RI) | ✅ | ✅ |
| `[Version]`, `[Reference]`, `[Two-Port Order] 12A21B/21A12B`, `[Number of …]` | — | ✅ |
| `[Begin/End Information]` blocks (kept & shown) | — | ✅ |
| Interleaved (v1-style) and trailing (v2) noise data `NFmin Γopt Rn` | ✅ | ✅ |
| Multi-port reduction (Sᵢ of an n-port with other ports terminated in Z₀) | ✅ | ✅ |

## Repository layout

```
rf-bench/
├── index.html            ← the entire application (≈240 KB, zero dependencies)
├── README.md
├── LICENSE               ← MIT
├── .github/workflows/ci.yml   ← runs the test suite on every push/PR
├── tests/units.js        ← zero-dependency Node suite (parser, RF math, bands, TDR)
└── assets/               ← sample IEEE exports used by this README
```

## Tests & CI

The suite needs **nothing but Node ≥ 16** (it extracts the app's script block and exercises it directly):

```bash
node tests/units.js     # → "23 passed, 0 failed"
```

It covers: Touchstone v1/v2 parsing (formats, option lines, port order, noise), Y/Z/H/G → S conversions,
Γ↔Z round trips, VSWR, Hermitian eigensolver vs analytic spectra, σmax/SVD, gain & stability factors vs
hand values, ABCD cascades, interpolated band edges, synthetic-antenna metrics and TDR fault location
(0 / 1.2 / 3.0 m on a modelled cable). GitHub Actions runs it on every push (`.github/workflows/ci.yml`).
An optional jsdom+node-canvas end-to-end harness (all tabs, exports, interactions) is used during development.

## Go live in 2 minutes

The app is one static file, so *any* static host works.

### Option A — GitHub Pages (recommended)

```bash
git init
git add .
git commit -m "RF Bench: single-file S-parameter & antenna analysis suite"
git branch -M main
git remote add origin https://github.com/USERNAME/rf-bench.git
git push -u origin main
```

Then in the browser: **repo → Settings → Pages → Build and deployment → Source: *Deploy from a branch* →
Branch: `main` / `(root)` → Save.**
Your demo is live at `https://USERNAME.github.io/rf-bench/` within a minute (the badge URL in this README
assumes exactly that — edit `USERNAME` everywhere).

### Option B — Netlify / Vercel / Cloudflare Pages

* **Netlify Drop:** drag the repo folder onto https://app.netlify.com/drop — instant URL, no account config.
* **Vercel:** `npx vercel` in the folder (static, zero config).
* **Cloudflare Pages:** connect the repo, build command *empty*, output directory */*.

### Option C — Any web server

`python3 -m http.server` in the folder, or upload `index.html` alone to any host — it is completely self-contained
(no CDN, no fonts, no external assets required at runtime).

## Browser support

Any evergreen browser (Chrome/Edge/Firefox/Safari) with Canvas 2D — including `roundRect`
(a polyfill is bundled). Works from `file://`, behind proxies, and fully offline.

## Caveats

* Figures are only as good as the data: passivity/reciprocity checks are provided so you can judge quality.
* TDR range resolution is bounded by the measured span (`≈ vf·c / 2·Δf`); the default view hides the
  wrap-around half of the time record.
* The IEEE exporter targets journal quality (600 dpi, pt-accurate type); always double-check colour-blind
  safety of your own trace palettes — print symbols are added automatically when a panel holds >2 curves.

## License

MIT © 2026 RF Bench contributors — see [LICENSE](LICENSE).

*Built with vanilla JS + Canvas 2D. If you find it useful, a star ⭐ and a citation in your measurement
reports are appreciated.*
