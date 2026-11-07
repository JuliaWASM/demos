# Tyler-like map

An interactive slippy map with live [OpenStreetMap](https://www.openstreetmap.org/) tiles, written in Julia and compiled to WebAssembly with WasmCodegen.jl.

Live at <https://juliawasm.github.io/demos/tyler-map/>.

## What runs in wasm

All of the following is Julia, compiled to wasm under `verify=:error`:

- the map camera (drag, inertia, pinch, wheel, double tap and zoom snapping),
- tile choice (which tiles cover the viewport at the current zoom),
- the tile download (`Downloads.request`, which the browser runs with `fetch`),
- PNG decoding of the fetched tiles, in pure Julia,
- the Makie plots (`image!` plots that WGLMakie's three.js client draws).

The page's JavaScript only forwards raw pointer and wheel events, runs the animation frames, and provides the WASI and `fetch` host functions.

## Browser requirements

- **JSPI** (JavaScript Promise Integration), which means **Chrome 137 or newer**. Other browsers may not work yet.
- Network access to `tile.openstreetmap.org` for the map tiles and to `cdn.jsdelivr.net`, which serves one pinned script (`@bjorn3/browser_wasi_shim@0.4.2`).
- The page is a single self-contained file of about 2.3 MB (the wasm modules and WGLMakie client are inlined), so the first load takes a moment.

## OpenStreetMap tile usage

This demo uses the public OSM tile servers, so please follow the [Tile Usage Policy](https://operations.osmfoundation.org/policies/tiles/):

- Attribution ("© OpenStreetMap contributors", linking to the copyright page) stays visible on the map.
- This is a light-use demo. Don't use it for heavy traffic or as a tile source for an application.
- No bulk prefetching or scraping. After a gesture ends, the page requests only the visible tiles (at most 14) and eight lower-zoom background tiles. Nothing is requested during a gesture, failed tiles aren't retried, and tiles seen again come from the browser's HTTP cache.
- Requests are made by the browser's own `fetch`, so they carry the browser's User-Agent and the page's Referer.

Map data © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright), available under the ODbL.

## How it was built

The source lives in the makie-wasm spike (commit `75eeb1b`, on a branch that hasn't been pushed yet). Build scripts to be added.
