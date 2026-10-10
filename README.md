# RF Bench - S-Parameter & Antenna Analysis Suite

[![License: CC BY-NC-SA 4.0](https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
![Maintained: Yes](https://img.shields.io/badge/Maintained%3F-yes-green.svg)
![Vanilla JS](https://img.shields.io/badge/JavaScript-Vanilla-yellow?logo=javascript)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)

🌐 **Try it Live**: [**nezarati.github.io/rf-bench**](https://nezarati.github.io/rf-bench/index.html)

A professional vector-network-analyser post-processor that lives in **one HTML file**.
Drop in Touchstone data and get an interactive RF lab: multi-file comparison, Smith chart,
TDR, stability/gain/noise/mixed-mode analysis, matching-network synthesis and journal-grade
figure export — **no dependencies, no build step, no server, nothing leaves your browser**.


---

## ⚡ Quick start

```bash
open index.html        # macOS      (or just double-click it)
xdg-open index.html    # Linux
start index.html       # Windows
```

Then **drag & drop** Touchstone files (`.s1p … .s8p`, v1.0 & v2.0) anywhere on the page —
or press **Demo data** for three synthetic datasets (dual-band antenna `.s2p`,
4-port MIMO array `.s4p`, 1-port monopole `.s1p`).

## 🧭 Feature tour

| Area | Highlights |
|---|---|
| **Loading** | Touchstone v1/v2 · S/Y/Z/H/G · MA/DB/RI · any frequency unit · per-file Z₀ · v2 noise blocks & `[Information]` · multi-file overlay · rename / recolour / solo / hide |
| **Summary** | min return loss · interpolated −10 dB & threshold bands · fractional BW · resonance (X = 0) · Z@res · VSWR · η / realized gain / mismatch · MSG & MGA · Rollett K · μ₁/μ₂ · σmax(S) passivity · reciprocity |
| **Diagrams** | \|S\| dB with band shading & hover detail cards · phase & group delay · **Smith chart** (Z/Y grids, VSWR / stability / gain / NF circles) · VSWR & R/X · gain & efficiency · stability · 4-port mixed-mode (Sdd/Scc/Scd, CMRR) · noise figure + circles · integrity (passivity / reciprocity) |
| **TDR** | low-pass inverse-DFT · DC extrapolation (none / const / linear) · Kaiser / Hann / Hamming / Blackman / rect · distance or time axis · step ρ(t) & step impedance Z(t) · fault-event table · range-resolution readout |
| **Viewer UX** | per-diagram toolbar (fit, fit-Y, ±, ↑↓, maximise-alone, reset) · wheel = X+Y zoom · gutter wheel = single axis · Shift-drag box zoom · `A` autoscale · `F` fit current view · click-legend-to-hide · live `f =` hover chip · free (unlocked) Y axes |
| **Markers** | **per diagram — one click = one point** on the clicked curve · M1/M2 + unlimited M3… (Alt+click, `+`, `+M`) · interpolated (x, y) labels · collision-free placement + leader lines · sidebar ownership dropdown · in-app **self-test** |
| **Band report** | sidebar tree of every −10 dB band: f_low / f_centre / f_high / BW / FBW % (interpolated edges) · click a row → band highlighted on the diagrams · selectable CSV export |
| **Tools** | L-section matching (both topologies + swept result) · quarter-wave transformer · single-shunt-stub matcher with predicted-RL sweep · band analyser · fixture de-embedding (delay / √f loss / ABCD cascade) · cascade-networks builder (plot or download .s2p) · antenna efficiency · wavelength calculator |
| **Exports** | **IEEE figures per diagram**: true-vector SVG & 600 dpi PNG at 3.5 × 2.75 in (journal column) or 7.16 × 3.10 in (two-column span) · white ground, outward ticks, 10 pt Arial numerals, print-safe palette, auto curve symbols · CSV of any table · JSON summary of all metrics |

### 🖱️ Mouse & keyboard

| Input | Action |
|---|---|
| Click / Shift+click diagram | M1 / M2 on **that** diagram (single point on the clicked curve) |
| Alt+click · `+` · toolbar `+M` | extra marker M3… on that diagram |
| Wheel · Shift+wheel · Alt+wheel | zoom X+Y · X only · Y only (around cursor) |
| Wheel over an axis gutter | zoom that axis only |
| Drag · Shift+drag | pan · box-zoom |
| Double-click · `↺` | reset view |
| `⛶` · `⇕` · `+ − ↑ ↓` · `⤢` | fit X+Y · fit Y to current view · vertical zoom/pan · maximise alone (Esc restores) |
| `A` · `F` | autoscale every diagram on the tab · fit hovered diagram to current view |
| `1…9` · `0` · `O` · `Esc` | tabs · Tools · open files · clear markers |
| Legend row click | hide / show that trace |
| Hover a band shade | start / stop / centre / BW / FBW / min detail card |
| **Touch:** 1 finger drag | pan the diagram (X+Y) |
| **Touch:** 2 fingers pinch | zoom around the pinch centre |
| **Touch:** tap | M1 on the tapped curve of that diagram |
| **Touch:** double-tap | reset the view |

## 📄 Touchstone support

| | v1.0 | v2.0 |
|---|---|---|
| `.s1p … .s8p` (ports from extension or row length) | ✅ | ✅ |
| Option line `# GHz S MA R 50` — any unit, S/Y/Z/H/G, MA/DB/RI | ✅ | ✅ |
| `[Version] [Reference] [Two-Port Order] [Number of …]` | — | ✅ |
| `[Begin/End Information]` (kept & displayed) | — | ✅ |
| Noise data `NFmin Γopt Rn` (interleaved v1-style & trailing v2) | ✅ | ✅ |
| n-port reduction (Sᵢ with other ports terminated in Z₀) | ✅ | ✅ |

## 🗂 Repository layout

```
rf-bench/
├── index.html                 ← the entire application (one file, zero dependencies)
├── README.md
├── LICENSE                    ← CC BY-NC-SA 4.0
├── .github/workflows/ci.yml   ← GitHub Actions: runs the test suite on push / PR
├── tests/units.js             ← zero-dependency Node suite (parser, RF math, bands, TDR)
└── assets/                    ← sample IEEE exports shown above
```

## 🧪 Tests & CI

Needs **nothing but Node ≥ 16** (the suite extracts the app's script block and exercises it directly):

```bash
node tests/units.js     # → "23 passed, 0 failed"
```

Covers: Touchstone v1/v2 parsing (formats, option lines, port order, noise) · Y/Z/H/G → S conversions ·
Γ↔Z round trips · VSWR · Hermitian eigensolver vs analytic spectra · σmax/SVD · gain & stability factors
vs hand values · ABCD cascades · interpolated band edges · synthetic-antenna metrics · TDR fault location
(0 / 1.2 / 3.0 m on a modelled cable). GitHub Actions runs it on every push; an optional jsdom + node-canvas
end-to-end harness (all tabs, exports, interactions) is used during development.

## 🚀 Go live in 2 minutes

The app is a single static file, so *any* static host works.

**GitHub Pages (recommended)**

```bash
git init && git add . && git commit -m "RF Bench: single-file S-parameter & antenna analysis suite"
git branch -M main
git remote add origin https://github.com/nezarati/rf-bench.git
git push -u origin main
```

Then: **repo → Settings → Pages → Source: *Deploy from a branch* → `main` / `(root)` → Save.**
Live at `https://nezarati.github.io/rf-bench/index.html` within a minute.

**Other hosts** — Netlify Drop (drag the folder on [app.netlify.com/drop](https://app.netlify.com/drop)),
`npx vercel`, Cloudflare Pages (empty build command, output `/`), or any web server
(`python3 -m http.server`). Uploading `index.html` alone is enough — it is fully self-contained.

## 📱 Mobile friendly

Below 900 px the layout reflows: the sidebar becomes a **☰ drawer** (backdrop tap closes it, it also
auto-closes when you switch tabs), the top bar scrolls horizontally, toolbars/tabs/inputs grow to
finger size, grids collapse to one column and the marker readout becomes a **bottom sheet** that also
reports which −10 dB band the marker sits in. Diagrams are fully touch-driven (pan / pinch / tap-marker /
double-tap reset, `touch-action` scoped so the page still scrolls normally), and everything else —
including the IEEE SVG/PNG exporters and the TDR — works unchanged. Uses `100dvh` so iOS Safari
toolbars don't clip the layout.

## 🌍 Browser support

Any evergreen browser (Chrome / Edge / Firefox / Safari) with Canvas 2D — a `roundRect` polyfill is bundled.
Works from `file://`, behind proxies, and completely offline.

## ⚠️ Caveats

* Figures are only as good as the data — passivity/reciprocity checks are provided so you can judge quality.
* TDR range resolution is bounded by the measured span (≈ vf·c / 2·Δf); the default view hides the wrap-around half of the time record.
* The IEEE exporter targets journal quality (600 dpi, pt-accurate type); print symbols are added automatically when a panel holds more than two curves.

## 📜 License

Copyright (c) 2026 nezarati.
This work is licensed under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0
International License** (CC BY-NC-SA 4.0).
To view a copy, visit <https://creativecommons.org/licenses/by-nc-sa/4.0/> or send a letter to
Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.

*Built with vanilla JS + Canvas 2D. If you find it useful, a star ⭐ and a citation in your
measurement reports are appreciated.*
