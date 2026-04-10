---
doctype: Reference
module: buf
function: MbufGetColor2d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufGetColor2d"
---

# MbufGetColor2d

> Get data from an area of one or all bands of a buffer and place it in a user-supplied array.

## Syntax

```c
void MbufGetColor2d(
    AIL_ID    SrcBufId,     //in
    AIL_INT64 DataFormat,   //in
    AIL_INT   Band,         //in
    AIL_INT   OffX,         //in
    AIL_INT   OffY,         //in
    AIL_INT   SizeX,        //in
    AIL_INT   SizeY,        //in
    void *    UserArrayPtr  //out
)
```

## Description

This function copies data from a specific area of one or all bands of a specified Aurora Imaging Library source buffer to a user-supplied array.

If the source buffer is compressed, Aurora Imaging Library decompresses the data before copying it to the user-supplied array.

## Parameters

### `SrcBufId` *(in, AIL_ID)*

Specifies the identifier of the source buffer.

### `DataFormat` *(in, AIL_INT64)*

Specifies the data format to use to save the data in the user array. The internal data format of the source buffer need not match the specified data format of the user-supplied array; an internal conversion will be performed if necessary. Note, however, if the formats do match, the operation will be much faster.

### `Band` *(in, AIL_INT)*

Specifies the index of the band to copy. This parameter can be set to one of the values below.

*For specifying the index of the band to copy*

| Value | Description |
| --- | --- |
| `M_ALL_BANDS` | Copies from all bands. |
| `M_BLUE` | Copies from the blue band (for RGB buffers). |
| `M_GREEN` | Copies from the green band (for RGB buffers). |
| `M_HUE` | Copies from the hue band (for HSL and HSV buffers). |
| `M_LUMINANCE` | Copies from the luminance band (for HSL buffers). |
| `M_RED` | Copies from the red band (for RGB buffers). |
| `M_SATURATION` | Copies from the saturation band (for HSL and HSV buffers). |
| `M_V` | Copies from the V band (for HSV buffers). |
| `0 <= Value < #bands` | Specifies the index of the band to copy.

The relationship between index value and band for RGB, HSL, and HSV buffers is the following:

|   |   |
| --- | --- |
| 0 | Corresponds to the red band for RGB buffers, the hue band for HSL and HSV buffers. |
| 1 | Corresponds to the green band for RGB buffers or the saturation band for HSL and HSV buffers. |
| 2 | Corresponds to the blue band for RGB buffers, or the luminance band for HSL buffers, or the V band for HSV buffers. | |

### `OffX` *(in, AIL_INT)*

Specifies the horizontal offset (relative to the top-left source buffer coordinate) of the source buffer area from which to get the data.

### `OffY` *(in, AIL_INT)*

Specifies the vertical offset (relative to the top-left source buffer coordinate) of the source buffer area from which to get the data.

### `SizeX` *(in, AIL_INT)*

Specifies the width of the source buffer area from which to get the data.

### `SizeY` *(in, AIL_INT)*

Specifies the height of the source buffer area from which to get the data.

### `UserArrayPtr` *(out, *void)*

Specifies the address of the user array in which to copy the data from the source buffer. Ensure that the user array is large enough to receive the data from the specified area and one or all bands of the source buffer.

## Parameter Associations

### For specifying the data format

Note that _SizeX_ and _SizeY_ denote the source buffer's width and height, respectively.

---

### `M_PACKED`

Copies the bands in an interleaved manner (for example, RGB RGB RGB...). This format is only valid when the [`Band`](../../Reference/buf/MbufGetColor2d.md) parameter is set to [`M_ALL_BANDS`](../../Reference/buf/MbufGetColor2d.md).

---

### `M_PLANAR`

Copies the bands one after the other. This format is to be used when copying from all bands of the source buffer.

---

### `M_SINGLE_BAND`

Copies a single band. This format is not valid when the [`Band`](../../Reference/buf/MbufGetColor2d.md) parameter is set to [`M_ALL_BANDS`](../../Reference/buf/MbufGetColor2d.md).

### Combination Constants — For setting the packed data format

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set the packed data format.

For more information on these formats, see [RGB buffers](../../UserGuide/C23_Data_buffers/S10_RGB_buffers.md).

#### `M_BGR24`

Stores the data in a BGR24 packed format. In this format, each pixel is internally stored as three consecutive bytes.

#### `M_BGR32`

Stores the data in a BGR32 packed format. In this format, each pixel is internally stored as four consecutive bytes. Every fourth byte is a "don't care" byte.

#### `M_RGB15`

Stores the data in an RGB15 packed format. In this format, each pixel is internally stored as a 16-bit word with a 5-bit blue value (least significant), a 5-bit green value, a 5-bit red value, and a "don't care" bit (most significant), in little-endian order (RGB 5:5:5).

#### `M_RGB16`

Stores the data in an RGB16 packed format. In this format, each pixel is internally stored as a 16-bit word with a 5-bit blue value (least significant), a 6-bit green value, and a 5-bit red value (most significant), in little-endian order (RGB 5:6:5).

#### `M_RGB24`

Stores the data in an RGB24 packed format. In this format, each pixel is internally stored as three consecutive bytes.
