# Shaders Pro public archive

Public, self-hosted snapshot of the shader catalog at **https://shaders.com/pro**.

- Generated: see `manifest.json`
- 220 collections, 1,000 presets
- 10,000+ editable color/range controls with authored defaults
- Static preview covers, decoded component trees, referenced public assets
- Runtime: `shaders@4.0.7` JavaScript bundle (MIT)

This archive is published by the site owner for preservation and for the companion
component gallery. Only publicly reachable content was captured. The shader
definitions, preview images and runtime are MIT-licensed; see
`LICENSE-shaders.txt`.

## Layout

```
catalog.json                 lightweight card catalog (1,000 items)
data/<preset-id>.json        decoded component tree + control schema
covers/<preset-id>.jpg       720×406 public preview thumbnail
assets/<sha1-prefix>.<ext>   public SDF/image assets referenced by presets
runtime.js                   self-contained ESM runtime (createShader)
manifest.json                counts and format notes
archive.json                 scrape summary
```

## Render one preset

```js
import { createShader } from './runtime.js';

const data = await fetch('./data/<preset-id>.json').then((r) => r.json());
const shader = await createShader(canvas, {
  components: data.components,
  toneMapping: data.toneMapping,
});

// data.controls: [{ id, label, type, targets: [{ id, prop }], value, ... }]
for (const control of data.controls) {
  for (const target of control.targets) {
    shader.update(target.id, { [target.prop]: control.value });
  }
}
```

Use one live instance at a time. The list/card layer is intentionally static;
this archive does not contain videos.

## Notes

- The decoded tree uses the same abbreviated ids as the public preview runtime.
- Logo/SDF controls that need an uploaded, precomputed SDF are omitted; the
  authored logo binary is kept in the definition.
- Cursor/map/auto-animate driven props stay as authored driver objects and are
  not exposed as scalar controls, so the original motion is preserved.

Source site and content © the site owner. Runtime © Shader Effects, Inc.,
released under the MIT License.
