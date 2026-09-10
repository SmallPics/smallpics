# Imgix Compatibility

Parameter mappings for moving Imgix image URLs to Small Pics. 

Enable the Imgix compatibility for any of your configured image sources which might require Imgix parameters.

## Parameter Compatibility

| Imgix        | Small Pics  | Supported | Notes                                                                                    |
|--------------|-------------|-----------|------------------------------------------------------------------------------------------|
| `ar`         | `ar`        | ✅         | Accepts `width:height` or decimal ratios, for example `16:9` or `3.1667`.                |
| `bg`         | `bg`        | ✅         |                                                                                          |
| `border`     | `border`    | ✅         |                                                                                          |
| `pad`        | `border`    | ✅         | Maps to `border=<pad>,<bg>,shrink`. If bg isn't set, then the background is transparent. |
| `bri`        | `bri`       | ✅         |                                                                                          |
| `con`        | `con`       | ✅         |                                                                                          |
| `crop`       | `fit`, `crop`       | 🟠        | See mapping table below.                                                                 |
| `dpr`        | `dpr`       | ✅         | Small Pics max is 8, Imgix max is 5.                                                     |
| `faceindex`  | `face`      | 🟠        | With `fit=facearea`. Accepts positive face numbers; ordering may differ from Imgix.      |
| `facepad`    | —          | 🔴        | Unsupported.                                                                             |
| `fit`        | `fit`       | ✅         | See mapping table below.                                                                 |
| `flip`       | `flip`      | ✅         | `hv` in Imgix is mapped to `both` in Small Pics                                          |
| `fm`         | `fm`        | 🟠        | Small Pics supports: `avif`, `webp`, `jpg`, `pjpg`, `png`, `gif`, `jxl`.                 |
| `gam`        | `gam`       | ✅         | See gamma value mapping below.                                                           |
| `h`          | `h`         | 🟢        | Pixels pass through. Fractions map to percentages: `h=0.5` → `h=50p`.                    |
| `mark`       | `mark`      | 🟠        | Small Pics only supports relative URLs. Images can be in different sources.              |
| `mark-alpha` | `markalpha` | ✅         |                                                                                          |
| `mark-pad`   | `markpad`   | ✅         | Imgix defaults to 5px.                                                                   |
| `mark-align` | `markpos`   | ✅         | Comma-separated positioning. Both default to bottom-right. See mapping below.            |
| `mark-h`     | `markh`     | ✅         | Relative positioning maps to `<value>h`, for example `0.1` becomes `10h`.                |
| `mark-w`     | `markw`     | ✅         | Relative positioning maps to `<value>w`, for example `0.1` becomes `10w`.                |
| `mark-x`     | `markpad`, `markpos` | ✅ | Sets x from the left edge. Relative values map to `w`, for example `0.1` becomes `10w`.  |
| `mark-y`     | `markpad`, `markpos` | ✅ | Sets y from the top edge. Relative values map to `h`, for example `0.1` becomes `10h`.   |
| `mark-fit`   | `markfit`   | ✅         | See mapping table below.                                                                 |
| `orient`     | `or`        | ✅         | Imgix uses EXIF values or degrees. See mapping below.                                    |
| `q`          | `q`         | ✅         |                                                                                          |
| `sharp`      | `sharp`     | ✅         |                                                                                          |
| `w`          | `w`         | 🟢        | Pixels pass through. Fractions map to percentages: `w=0.65` → `w=65p`.                   |

---

## Value Mappings

Small Pics uses `p` for axis-relative percentages, `w` for width, and `h` for height. Imgix fractional inputs still map to `w` or `h` as listed above.

### `fit` values

| Imgix            | Small Pics          |
|------------------|---------------------|
| `clip` (default) | `contain` (default) |
| `max`            | `max`               |
| `fillmax`        | `fill`              |
| `fill`           | `fill-max`          |
| `scale`          | `stretch`           |
| `clamp`          | `crop`    |
| `facearea`       | `zoom=facesarea` (without `faceindex`) |
| `min`            | ❌ Not supported     |

#### When `crop` is `"focalpoint"` and `fit` is `"crop"`

`fp-x` and `fp-y` select the focal point, and `fp-z` sets the [zoom](../api/README.md#zoom---zoom). For example, `fp-x=0.5&fp-y=0.5&fp-z=1.5` is equivalent to `fit=crop&fp=50w:50h&zoom=1.5`.

Without `fp-z`, this example is equivalent to `fit=crop&fp=50w:50h`.

#### When `crop` is set

Maps to `fit=crop&crop=<position>`

| Imgix          | Small Pics          |
|----------------|---------------------|
| `top`          | `fit=crop&crop=top`         |
| `bottom`       | `fit=crop&crop=bottom`      |
| `left`         | `fit=crop&crop=left`        |
| `right`        | `fit=crop&crop=right`       |
| `left,top`     | `fit=crop&crop=top-left`    |
| `bottom,left`  | `fit=crop&crop=bottom-left` |
| `right,top`    | `fit=crop&crop=top-right`   |
| `bottom,right` | `fit=crop&crop=bottom-right`|

### Face cropping

| Imgix | Small Pics |
|-------|------------|
| `fit=crop&crop=faces` | `fit=crop&crop=face` |
| `fit=crop&crop=faces,top` | `fit=crop&crop=face,top` |
| `fit=crop&crop=faces,top,left` | `fit=crop&crop=face,top-left` |
| `fit=facearea` | `zoom=facesarea` |
| `fit=facearea&faceindex=2` | `face=2&zoom=face` |
| `fit=facearea,fill` | `fit=fill-max&zoom=facesarea` |
| `fit=facearea,crop&crop=top` | `fit=crop&crop=top&zoom=facesarea` |

`crop=faces` centers the crop on one face without zooming in. With no face, it uses a centered crop. Put `faces` first when adding a directional fallback; corner pairs work in either order. Unsupported fallbacks, including `entropy`, `edges`, and `focalpoint`, use a centered crop.

`fit=facearea` frames all detected faces. Add `faceindex` to select one face. Its fallback can be `clip`, `max`, `fillmax`, `fill`, `scale`, `clamp`, or `crop`. With no supported fallback, it uses `contain` when no matching face is found.

If the group cannot fit, the crop centers on the box around all faces. Some faces may fall outside the crop. See [frame all faces](../api/README.md#frame-all-faces).

Face detection and ordering can differ from Imgix. `faceindex` only applies to `facearea` and uses the native [face index](../api/README.md#face---face-index) rules. Empty or invalid values select the first face; leaving it out selects the group.

`facepad` is unsupported and ignored. Its multiplier does not map directly to [zoompad](../api/README.md#zoompad---zoom-padding), which uses pixels or percentages of the output dimensions.

### `markfit` values

| Imgix            | Small Pics          |
|------------------|---------------------|
| `clip` (default) | `contain` (default) |
| `max`            | `max`               |
| `crop`           | `crop`             |
| `scale`          | `stretch`           |

### `gam` (gamma) mapping

| Imgix      | Small Pics   |
|------------|--------------|
| 1 to 100   | 0.1 to < 1.0 |
| -1 to -100 | 1.0 to 9.9   |

### `or` (orientation) mapping

Small Pics doesn't support EXIF values, instead the values are mapped to Small Pics `or` and `flip` parameters.

| Imgix         | Orientation | Flip |
|---------------|-------------|------|
| `2`           | -           | `h`  |
| `3` or `180`  | `180`       | -    |
| `4`           | -           | `v`  |
| `5`           | `90`        | `h`  |
| `6` or `270`  | `270`       | -    |
| `7`           | `90`        | `v`  |
| `8` or `90`   | `90`        | -    |
| default / `0` | `0`         | -    |

### Watermark offsets

`mark-x` and `mark-y` map to the x and y values in `markpad=x:y`. Each supplied offset overrides alignment and padding on that axis. The other axis keeps its alignment and uses `mark-pad`, which defaults to `5`.

For example, `mark-x=12&mark-y=0.2` maps to `markpos=top-left&markpad=12:20h`.

`mark-align=right,bottom&mark-x=12` maps to `markpos=bottom-left&markpad=12:5`.

### `mark-align` to `markpos` mapping

| Imgix            | Small Pics     |
|------------------|----------------|
| `top`            | `top-right`    |
| `middle`         | `right`        |
| `bottom`         | `bottom-right` |
| `left`           | `bottom-left`  |
| `center`         | `bottom`       |
| `right`          | `bottom-right` |
| `left,top`       | `top-left`     |
| `left,middle`    | `left`         |
| `bottom,left`    | `bottom-left`  |
| `center,top`     | `top`          |
| `center,middle`  | `center`       |
| `bottom,center`  | `bottom`       |
| `right,top`      | `top-right`    |
| `middle,right`   | `right`        |
| `bottom,right`   | `bottom-right` |
