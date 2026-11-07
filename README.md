# demos

Live demos of Julia compiled to WebAssembly with WasmCodegen.jl, hosted on GitHub Pages at <https://juliawasm.github.io/demos/>.

Each demo is a static folder served as-is. There is no build step in this repo.

## Demos

| Demo | Description | Requirements |
| --- | --- | --- |
| [`tyler-map/`](tyler-map/) ([live](https://juliawasm.github.io/demos/tyler-map/)) | A Tyler-like interactive map with live OpenStreetMap tiles. Camera, tile choice, PNG decoding and Makie plots run as Julia compiled to wasm. | JSPI (Chrome 137+), network access to tile.openstreetmap.org |

## Adding a demo

1. Create a folder `my-demo/` with an `index.html` (everything it needs should live in the folder, or be loaded from a pinned CDN URL).
2. Add a `my-demo/README.md` saying what runs in wasm, the browser requirements, how it was built, and any usage terms for external services it calls.
3. Add an entry to the demo list in the top-level `index.html` (copy an existing `<li class="demo">`) and a row to the table above.
4. Open a pull request. Merging to `main` deploys the whole repo root to Pages (`.github/workflows/pages.yml`).

Demos must not load third-party code that isn't version-pinned.
