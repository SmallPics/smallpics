# Imgix Compatibility

Enable the Imgix compatibility for any of your configured image origins which might require Imgix parameters.

## Parameter Compatibility

| Imgix        | Small Pics  | Supported | Notes                                                                                    |
|--------------|-------------|-----------|------------------------------------------------------------------------------------------|
| `ar`         | `ar`        | ✅         | Accepts `width:height` or decimal ratios, for example `16:9` or `3.1667`.                 |
| `bg`         | `bg`        | ✅         |                                                                                          |
| `border`     | `border`    | ✅         |                                                                                          |
| `pad`        | `border`    | ✅         | Maps to `border=<pad>,<bg>,shrink`. If bg isn't set, then the background is transparent. |
| `bri`        | `bri`       | ✅         |                                                                                          |
| `con`        | `con`       | ✅         |                                                                                          |
| `crop`       | `fit`       | 🟠        | See mapping table below.                                                                 |
| `dpr`        | `dpr`       | ✅         | Small Pics max is 8, Imgix max is 5.                                                     |
| `fit`        | `fit`       | ✅         | See mapping table below.                                                                 |
| `flip`       | `flip`      | ✅         | `hv` in Imgix is mapped to `both` in Small Pics                                          |
| `fm`         | `fm`        | 🟠        | Small Pics supports: `avif`, `webp`, `jpg`, `pjpg`, `png`, `gif`, `jxl`.                  |
| `gam`        | `gam`       | ✅         | See gamma value mapping below.                                                           |
| `h`          | `h`         | 🟠        | Small Pics doesn't yet have support for relative sizes.                                  |
| `mark`       | `mark`      | 🟠        | Small Pics only supports relative URLs. Images can be in different origins.              |
| `mark-alpha` | `markalpha` | ✅         |                                                                                          |
| `mark-pad`   | `markpad`   | ✅         | Imgix defaults to 5px.                                                                   |
| `mark-align` | `markpos`   | ✅         | Comma-separated positioning. Both default to bottom-right. See mapping below.            |
| `mark-h`     | `markh`     | ✅         | Relative positioning maps to `<value>h`, for example `0.1` becomes `10h`.                |
| `mark-w`     | `markw`     | ✅         | Relative positioning maps to `<value>w`, for example `0.1` becomes `10w`.                |
| `mark-x`     | `markx`     | ✅         | Relative positioning maps to `<value>w`, for example `0.1` becomes `10w`.                |
| `mark-y`     | `marky`     | ✅         | Relative positioning maps to `<value>h`, for example `0.1` becomes `10h`.                |
| `mark-fit`   | `markfit`   | ✅         | See mapping table below.                                                                 |
| `orient`     | `or`        | ✅         | Imgix uses EXIF values or degrees. See mapping below.                                    |
| `q`          | `q`         | ✅         |                                                                                          |
| `sharp`      | `sharp`     | ✅         |                                                                                          |
| `w`          | `w`         | 🟠        | Small Pics doesn't yet have support for relative sizes.                                  |

---

## Value Mappings

### `fit` values

| Imgix            | Small Pics          |
|------------------|---------------------|
| `clip` (default) | `contain` (default) |
| `max`            | `max`               |
| `fillmax`        | `fill`              |
| `fill`           | `fill-max`          |
| `scale`          | `stretch`           |
| `clamp`          | `cover` / `crop`    |
| `facearea`       | ❌ Not supported     |
| `min`            | ❌ Not supported     |

#### When `crop` is `"focalpoint"` and `fit` is `"crop"`

Maps `fp-x`, `fp-y` and `fp-z` to `fit=crop-<x%>-<y%>-<zoom>`. For example `fit=crop-50-50-1.5`.

If `zoom` isn't provided, then it maps to `fit=crop-<x%>-<y%>`.

#### When `crop` is set

Maps to `fit=cover-<value>`

| Imgix          | Small Pics          |
|----------------|---------------------|
| `top`          | `cover-top`         |
| `bottom`       | `cover-bottom`      |
| `left`         | `cover-left`        |
| `right`        | `cover-right`       |
| `left,top`     | `cover-top-left`    |
| `bottom,left`  | `cover-bottom-left` |
| `right,top`    | `cover-top-right`   |
| `bottom,right` | `cover-bottom-right`|

### `markfit` values

| Imgix            | Small Pics          |
|------------------|---------------------|
| `clip` (default) | `contain` (default) |
| `max`            | `max`               |
| `crop`           | `cover`             |
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
