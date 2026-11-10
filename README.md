# Shaders Pro public archive

Public, self-hosted snapshot of the shader catalog at **https://shaders.com/pro**.

- Generated: see `manifest.json`
- 220 collections, 1,000 presets
- 10,629 editable controls with authored defaults
  (7,244 range · 3,181 color · 168 logo · 36 image)
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
// type: "range" | "color" | "logo" | "file"
for (const control of data.controls) {
  for (const target of control.targets) {
    shader.update(target.id, { [target.prop]: control.value });
  }
}
```

Use one live instance at a time. The list/card layer is intentionally static;
this archive does not contain videos.

## Control types

| type | count | target prop | notes |
| ---- | ----- | ----------- | ----- |
| `range` | 7,244 | scalar props | `min`, `max`, `step`, `value` come from the authored control |
| `color` | 3,181 | color props | hex/rgb strings, applied as authored |
| `logo` | 168 | `shapeSdfUrl` | an uploaded SVG is rasterised and converted to a 512×512 SDF by the companion UI; the authored `.bin` lives in `assets/` |
| `file` | 36 | `url` | image upload, any `image/*` |

## Notes

- The decoded tree uses the same abbreviated ids as the public preview runtime.
- Cursor/map/auto-animate driven props stay as authored driver objects and are
  not exposed as scalar controls, so the original motion is preserved.
- Ten presets ship with no controls; their look is fully authored in the tree.

Source site and content © the site owner. Runtime © Shader Effects, Inc.,
released under the MIT License.
