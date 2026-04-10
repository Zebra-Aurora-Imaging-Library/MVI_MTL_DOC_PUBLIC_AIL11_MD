---
doctype: Reference
module: buf
function: MbufGetList
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufGetList"
---

# MbufGetList

> Read the values in the source buffer, at the specified positions, and place their values in a user-defined array.

## Syntax

```c
void MbufGetList(
    AIL_ID             SrcBufId,           //in
    AIL_INT            NumPixels,          //in
    const AIL_DOUBLE * PixXArrayPtr,       //in
    const AIL_DOUBLE * PixYArrayPtr,       //in
    AIL_INT64          InterpolationMode,  //in
    void *             UserArrayPtr        //out
)
```

## Description

This function reads the values in the source buffer, at the specified positions, and places their values in a user-defined array.

If the source buffer is compressed, Aurora Imaging Library decompresses the data before copying it to the user-supplied array.

When executing the operation on a buffer that has multiple bands, the bands are read sequentially. For example, with a planar RGB buffer, the red band is read first; the red values at the specified positions are placed in the first third of the user-defined array. Then, the green band is read; the green values at the specified positions are placed in the second third of the user-defined array (immediately following the red values). Finally, the blue band is read; the blue values at the specified positions are placed in the last third of the user-defined array (immediately following the green values). If you want to read, for example, 3 pixels, the function will store the red band of each pixel, then the green band, and finally the blue band (RRR, GGG, BBB not RGB, RGB, RGB).

> **Note:** Note that if the source buffer has multiple bands, its format should match the implicit format of the user array; otherwise an internal conversion has to be done (from the source to a temporary buffer) before executing the operation using the temporary buffer. Likewise, if the source buffer is compressed, an internal decompression has to be done (from the source to a temporary buffer) before executing the operation using the temporary buffer.

## Parameters

### `SrcBufId` *(in, AIL_ID)*

Specifies the identifier of the source buffer. This buffer can be a single-band (monochrome) or multi-band buffer and is not limited to image buffers.

### `NumPixels` *(in, AIL_INT)*

Specifies the number of values to get.

### `PixXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of an array containing the X-position corresponding to the pixels to get.

### `PixYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of an array containing the Y-position corresponding to the pixels to get.

### `InterpolationMode` *(in, AIL_INT64)*

Specifies the interpolation mode. The specified mode controls how the function behaves with non-integer positions.

*For specifying the interpolation mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_NEAREST_NEIGHBOR`](../../Reference/buf/MbufGetList.md) with [`M_OVERSCAN_ENABLE`](../../Reference/buf/MbufGetList.md). |
| `M_BICUBIC` | Specifies bicubic interpolation. The value placed in the user-defined array is determined by taking a weighted average of the 16 values (4x4) that surround the specified position. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated according to the depth and data type of the array. |
| `M_BILINEAR` | Specifies bilinear interpolation. The value placed in the user-defined array is determined by taking a weighted average of the 4 values (2x2) that surround the specified position. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The value placed in the user-defined array is that of the value of the pixel closest to the specified position. |

*For overscan*

| Value | Description |
| --- | --- |
| `M_OVERSCAN_CLEAR` | Sets the destination position to 0, if the associated point falls outside the source buffer. |
| `M_OVERSCAN_DISABLE` | Leaves the destination position as is, if the associated point falls outside the source buffer. |
| `M_OVERSCAN_ENABLE` | Uses the values of the source buffer's ancestor buffer, if the associated point falls outside the source buffer. If the source buffer is not a child buffer or if the point falls outside the ancestor buffer, it leaves the destination point as is. |

### `UserArrayPtr` *(out, *void)*

Specifies the address of a user-defined array in which the values are placed. The data type of this array must match the data type of the source buffer. Its number of entries is determined by the number of values to get and also by the number of bands of the source buffer.
