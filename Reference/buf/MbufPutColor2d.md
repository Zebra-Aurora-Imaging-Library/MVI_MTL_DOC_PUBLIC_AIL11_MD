---
doctype: Reference
module: buf
function: MbufPutColor2d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufPutColor2d"
---

# MbufPutColor2d

> Put data from a user-supplied array into an area of one or all bands of a data buffer.

## Syntax

```c
void MbufPutColor2d(
    AIL_ID       DestBufId,    //out
    AIL_INT64    DataFormat,   //in
    AIL_INT      Band,         //in
    AIL_INT      OffX,         //in
    AIL_INT      OffY,         //in
    AIL_INT      SizeX,        //in
    AIL_INT      SizeY,        //in
    const void * UserArrayPtr  //in
)
```

## Description

This function copies data from a user-supplied array to a specified area in one or all bands of a specified Aurora Imaging Library destination buffer.

## Parameters

### `DestBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer.

### `DataFormat` *(in, AIL_INT64)*

Specifies the data format of the user-supplied array. The internal data format of the destination buffer need not match the specified data format of the user-supplied array; an internal conversion will be performed if necessary. Note, however, if the formats do match, the operation will be much faster.

### `Band` *(in, AIL_INT)*

Specifies the index of the band in which to copy. This parameter can be set to one of the values below.

*For specifying the index of the band*

| Value | Description |
| --- | --- |
| `M_ALL_BANDS` | Copies to all bands. |
| `M_BLUE` | Copies to the blue band (for RGB buffers). |
| `M_GREEN` | Copies to the green band (for RGB buffers). |
| `M_HUE` | Copies to the hue band (for HSL, and HSV buffers). |
| `M_LUMINANCE` | Copies to the luminance band (for HSL buffers). |
| `M_RED` | Copies to the red band (for RGB buffers). |
| `M_SATURATION` | Copies to the saturation band (for HSL and HSV buffers). |
| `M_V` | Specifies the V band (for HSV buffers). |
| `0 <= Value < #bands` | Specifies the index of the band to copy.

The relationship between index value and band for RGB, HSL, and HSV buffers is the following:

|   |   |
| --- | --- |
| 0 | Corresponds to the red band for RGB buffers, or the hue band for HSL and HSV buffers. |
| 1 | Corresponds to the green band for RGB buffers, or the saturation band for HSL and HSV buffers. |
| 2 | Corresponds to the blue band for RGB buffers, or the luminance band for HSL buffers, or the V band for HSV buffers. | |

### `OffX` *(in, AIL_INT)*

Specifies the horizontal offset of the destination buffer area in which to put the data, relative to the destination buffer's top-left coordinate.

### `OffY` *(in, AIL_INT)*

Specifies the vertical offset of the destination buffer area in which to put the data, relative to the destination buffer's top-left coordinate.

### `SizeX` *(in, AIL_INT)*

Specifies the width of the destination buffer area in which to put the data.

### `SizeY` *(in, AIL_INT)*

Specifies the height of the destination buffer area in which to put the data.

### `UserArrayPtr` *(in, *void)*

Specifies the address of the user array from which to copy data into the destination buffer. Ensure that user array is large enough to contain the data to be copied to the destination buffer.

## Parameter Associations

### For specifying the data format

Note that _SizeX_ and _SizeY_ denote the destination buffer's width and height, respectively.

---

### `M_PACKED`

Copies the bands in an interleaved manner (for example, RGB RGB RGB...). This format is only valid when the [`Band`](../../Reference/buf/MbufPutColor2d.md) parameter is set to [`M_ALL_BANDS`](../../Reference/buf/MbufPutColor2d.md).

---

### `M_PLANAR`

Copies the bands one after the other. This format is to be used when copying to all bands ([`M_ALL_BANDS`](../../Reference/buf/MbufPutColor2d.md)) of the destination buffer.

---

### `M_SINGLE_BAND`

Copies to a single band. This format is not valid when the [`Band`](../../Reference/buf/MbufPutColor2d.md) parameter is set to [`M_ALL_BANDS`](../../Reference/buf/MbufPutColor2d.md).

### Combination Constants — For setting the packed data format

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set the packed data format.

For more information on these formats, see [RGB buffers](../../UserGuide/C23_Data_buffers/S10_RGB_buffers.md).

#### `M_BGR24`

Stores the data in a BGR24 packed format. In this format, each pixel is internally stored as three consecutive bytes.

#### `M_BGR32`

Stores the data in a BGR32 packed format. In this format, each pixel is internally stored as four consecutive bytes. Every fourth byte is a "don't care" byte.

#### `M_RGB15`

Stores the data in a RGB15 packed format. In this format, each pixel is internally stored as a 16-bit word with a 5-bit blue value (least significant), a 5-bit green value, a 5-bit red value, and a "don't care" bit (most significant), in little-endian order (RGB 5:5:5).

#### `M_RGB16`

Stores the data in a RGB16 packed format. In this format, each pixel is internally stored as a 16-bit word with a 5-bit blue value (least significant), a 6-bit green value, and a 5-bit red value (most significant), in little-endian order (RGB 5:6:5).

#### `M_RGB24`

Stores the data in a RGB24 packed format. In this format, each pixel is internally stored as three consecutive bytes.
