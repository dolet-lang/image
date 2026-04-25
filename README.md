# Image Library

`image` is the pure image-processing layer for Dolet.

It owns:

- image buffers
- colors and pixel access
- file codecs such as PPM/BMP/QOI
- drawing primitives
- filters and transforms

It does not own:

- windows
- event loops
- GPU upload
- UI widgets
- image preview/viewer apps

Those belong in packages such as `image_viewer`, `eqoi`, `ui`, or `window`.

## Layout

```text
packages/image/
  mod.dlt
  module.meta
  README.md

  core/
    color.dlt       # ImageColor helpers, RGBA packing/unpacking
    image.dlt       # Image struct, memory ownership, pixel access

  codecs/
    ppm.dlt         # PPM load/save API; first simple codec target
    png.dlt         # PNG RGBA writer using zlib stored blocks
    jpeg.dlt        # JPEG RGB baseline writer

  ops/
    draw.dlt        # point, line, rect, fill_rect
    filters.dlt     # invert, grayscale, threshold
    transform.dlt   # flip, crop, resize
```

## Planned Usage

```dolet
import image

img: Image = Image.create(256, 256)
img.clear(image_rgb(20, 20, 30))

image_fill_rect(img, 32, 32, 96, 64, image_rgb(240, 80, 40))
image_draw_line(img, 0, 0, 255, 255, image_rgb(255, 255, 255))
image_filter_grayscale(img)

PPM.save(img, "out.ppm")
PNG.save(img, "out.png")
JPEG.save(img, "out.jpg", 85)
img.free()
```

## Direction

Version `0.1.0` focuses on a stable core layout and experimental PNG output.
The PNG writer emits valid RGBA8 PNG files with zlib stored blocks, which means
the files are larger than compressed PNGs but need no external zlib dependency.
The JPEG writer emits baseline RGB JPEG without chroma subsampling. PNG/JPEG
decoding and compressed Deflate can be added later.
