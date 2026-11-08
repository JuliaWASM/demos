# tyler-map layout fix

## Cause

`tyler-map/index.html` had no `<!doctype html>`, so browsers rendered it in quirks mode. In quirks mode the
inline `height: 100%` on WGLMakie's wrapper `<div>` (inside `.tw-map`) resolves against the viewport instead of
the auto-height parent, so the wrapper became `innerHeight - 16px` tall (884 px at 1440x900, 1064 at 1920x1080,
828 at 390x844). The canvas stays 512x384 (aspect-locked by `height: auto !important`), and the `.tw-map`
`background: #ddd` showed through below it: the gray box. On a landscape phone the wrapper was shorter than the
canvas, so the canvas overflowed instead.

The missing document head also meant no charset, so the `±` in the intro rendered as `Â±` when served without a
charset header (as `python -m http.server` does; GitHub Pages sends one).

## Fix

- `tyler-map/index.html`: added `<!doctype html>`, `<html lang="en">`, `<head>`, `<meta charset="utf-8">`,
  `<body>` and the matching closing tags. No CSS changed: with standards mode the wrapper is `height: auto`
  and `.tw-map` is exactly the canvas plus its 1 px border.
- Intro rewritten as three short paragraphs (what it is and how to use it, what runs where, browser
  requirement). The old bottom "Requirements" paragraph is merged into the third; the tile-usage note is kept,
  shortened.
- `index.html` (landing): lead, card description and meta description trimmed ("entirely", "slippy map", "all").

## Checks

Playwright + Chromium 158 (JSPI available), pages served locally with `python3 -m http.server`. Live OSM tiles
did load in the sandbox (14 tiles at z12, `missing 0`), so the check used the real map.

Measured `#twmap` box vs canvas (before -> after):

| size | before (map h / canvas h) | after |
| --- | --- | --- |
| 1440x900 | 886 / 384 | 386 / 384 |
| 1920x1080 | 1066 / 384 | 386 / 384 |
| 768x1024 | 1010 / 384 | 386 / 384 |
| 1024x768 | 754 / 384 | 386 / 384 |
| 390x844 portrait | 830 / 261 | 263 / 261 |
| 844x390 landscape | 376 / 384 (overflow) | 386 / 384 |

(The 2 px difference is the border.) After the change, on desktop: drag pan, wheel zoom, +/- buttons and double
click all update the view (zoom 12 -> 13 -> 12 -> 13, tiles fetched, no missing). On a touch phone context:
+ button, double tap, two-finger pinch (zoom 14 -> 16) and touch pan all work.

Not changed: the map is a fixed 512x384 Makie scene scaled down on narrow screens, so on wide screens it is
514 px wide in a 760 px column. Making it larger needs a change on the Julia side.

Screenshots are in `shots/`, named `<size>-before.png` / `<size>-after.png` (full-page), plus `landing-*`.
