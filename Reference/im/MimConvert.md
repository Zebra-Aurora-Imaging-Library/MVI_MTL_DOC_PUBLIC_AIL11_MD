---
doctype: Reference
module: im
function: MimConvert
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimConvert"
---

# MimConvert

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Perform a color conversion.

## Syntax

```c
void MimConvert(
    AIL_ID SrcImageBufId,              //in
    AIL_ID DstImageBufId,              //out
    AIL_ID ArrayBufIdOrConversionType  //in
)
```

## Description

This function performs a color conversion on the source image and places the result in the destination buffer.

If necessary, you can perform a mathematically optimal color-to-grayscale conversion using [`McolProject`](../../Reference/col/McolProject.md) with [`M_PRINCIPAL_COMPONENT_PROJECTION`](../../Reference/col/McolProject.md).

When converting from sRGB ([`M_SRGB_...`](../../Reference/im/MimConvert.md)), your color data must adhere to standard RGB specifications (sRGB), as defined by the International Electrotechnical Commission (IEC) Project Team 61966-2-1. Conversions to sRGB ([`M_...SRGB`](../../Reference/im/MimConvert.md)) will also adhere to these standards. If converting from linear sRGB ([`M_SRGB_LINEAR_...`](../../Reference/im/MimConvert.md)), your color data must not be corrected (gamma corrected). Conversions to linear sRGB ([`M_...SRGB_LINEAR`](../../Reference/im/MimConvert.md)) will also not be corrected.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer.

### `ArrayBufIdOrConversionType` *(in, AIL_ID)*

Specifies the type of conversion to perform. The conversion can be specified with either a predefined or custom transformation matrix. This parameter can be set to one of the following.

*For specifying the type of conversion to perform*

| Value | Description |
| --- | --- |
| `M_HSL_TO_RGB` | Specifies an HSL to RGB conversion. Both the source and destination image buffers must be 3-band image buffers. |
| `M_HSV_TO_RGB` | Specifies an HSV to RGB conversion. Both the source and destination image buffers must be 3-band image buffers. |
| `M_LAB_TO_SRGB` | Specifies a LAB (CIELAB) to sRGB conversion. The LAB color space is based on the color's luminance (L), its position between red and green (A), and its position between yellow and blue (B). Both the source and destination image buffers must be 3-band image buffers. |
| `M_LAB_TO_SRGB_LINEAR` | Specifies a LAB (CIELAB) to linear sRGB conversion. The LAB color space is based on the color's luminance (L), its position between red and green (A), and its position between yellow and blue (B). Both the source and destination image buffers must be 3-band image buffers. |
| `M_L_TO_RGB` | Specifies an L (luminance) to RGB conversion. The luminance (intensity) is repeated in each color band of the destination buffer, creating a monochromatic (gray) RGB buffer. The source buffer must be a 1-band image buffer and the destination buffer must be a 3-band image buffer. |
| `M_LCH_TO_SRGB` | Specifies an LCH to sRGB conversion. The LCH color space is similar to the LAB color space, with an emphasis on polar coordinates (absolute black and absolute white). LCH is based on luminance (L), chroma (C), and hue (H). Both the source and destination image buffers must be 3-band image buffers. |
| `M_LCH_TO_SRGB_LINEAR` | Specifies an LCH to linear sRGB conversion. The LCH color space is similar to the LAB color space, with an emphasis on polar coordinates (absolute black and absolute white). LCH is based on luminance (L), chroma (C), and hue (H). Both the source and destination image buffers must be 3-band image buffers. |
| `M_RGB_NORMALIZE` | Specifies an RGB-to-normalized-RGB conversion. Each band of every pixel is scaled to a value between 0 and 1(inclusive), for example, the normalized R band is calculated according to _R_Normalized_ = _R_ / (_R_ + _G_ + _B_). To do so, the sum of the RGB bands of each pixel is computed, and then, each band for that pixel is divided by that sum. If the destination buffer is an integer buffer, this value is then rescaled between 0 and the depth of the buffer. Both the source and destination image buffers must be 3-band image buffers.

This normalizing of the RGB color space can help remove unwanted variations in shades from an image. |
| `M_RGB_TO_H` | Specifies an RGB to H conversion, where H represents the hue of the HSL color space. The source image must be a 3-band image buffer and the destination buffer must be a 1-band image buffer. |
| `M_RGB_TO_HSL` | Specifies an RGB to HSL (hue, luminance (intensity), and saturation) conversion. Both the source and destination image buffers must be 3-band image buffers. The hue (H) is stored in band 0, the luminance (L) is stored in band 2, and the saturation (S) is stored in band 1. |
| `M_RGB_TO_HSV` | Specifies an RGB to HSV (hue, saturation, and value) conversion. Both the source and destination image buffers must be 3-band image buffers. |
| `M_RGB_TO_L` | Specifies an RGB to L conversion, where L represents the luminance (intensity) of the HSL color space. The source image must be a 3-band image buffer and the destination buffer must be a 1-band image buffer. |
| `M_RGB_TO_Y` | Specifies an RGB to Y conversion, where Y represents the luminance of the YUV color space. The source buffer must have 3 bands. The destination buffer can have 1 or 3 bands. If it has 3 bands, only the first band is overwritten. |
| `M_SRGB_LINEAR_TO_LAB` | Specifies a linear sRGB to LAB (CIELAB) conversion. The LAB color space is based on the color's luminance (L), its position between red and green (A), and its position between yellow and blue (B). Both the source and destination image buffers must be 3-band image buffers. |
| `M_SRGB_LINEAR_TO_LCH` | Specifies a linear sRGB to LCH conversion. The LCH color space is similar to the LAB color space, with an emphasis on polar coordinates (absolute black and absolute white). LCH is based on luminance (L), chroma (C), and hue (H). Both the source and destination image buffers must be 3-band image buffers. |
| `M_SRGB_TO_LAB` | Specifies an sRGB to LAB (CIELAB) conversion. The LAB color space is based on the color's luminance (L), its position between red and green (A), and its position between yellow and blue (B). Both the source and destination image buffers must be 3-band image buffers. |
| `M_SRGB_TO_LCH` | Specifies an sRGB to LCH conversion. The LCH color space is similar to the LAB color space, with an emphasis on polar coordinates (absolute black and absolute white). LCH is based on luminance (L), chroma (C), and hue (H). Both the source and destination image buffers must be 3-band image buffers. |
| `Array buffer identifier` | Specifies a general matrix multiplication transform will be used to modify the source image's color information. The matrix multiplication is performed using the provided 1-band, 32-bit floating-point buffer allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md). You should use [`MbufPut`](../../Reference/buf/MbufPut.md) to define the values of the matrix. The source image buffer must be a 3-band buffer. The computation is shown below.

*[Image: mimconvertmatrices.png]*

When the provided array buffer is a 3x3 matrix or a 4x3 matrix (with optional offset values), the destination should be a 3-band buffer. When the provided array buffer is a 3x1 matrix or a 4x1 matrix (with an optional offset value), the destination should be a 1-band buffer.

The final values are clipped (saturated) according to the destination buffer's bit depth. |

Note that [`M_RGB_TO_L`](../../Reference/im/MimConvert.md) and [`M_RGB_TO_Y`](../../Reference/im/MimConvert.md) produce similar results but not identical results because they are calculated to different color spaces.
