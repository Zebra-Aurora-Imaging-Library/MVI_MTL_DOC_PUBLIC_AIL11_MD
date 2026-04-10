---
doctype: Reference
module: buf
function: MbufPutList
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufPutList"
---

# MbufPutList

> Overwrite the values in the destination buffer at the specified positions, with values from a user-defined array.

## Syntax

```c
void MbufPutList(
    AIL_ID             DestBufId,     //out
    AIL_INT            NumPixels,     //in
    const AIL_DOUBLE * PixXArrayPtr,  //in
    const AIL_DOUBLE * PixYArrayPtr,  //in
    AIL_INT64          OverscanMode,  //in
    const void *       UserArrayPtr   //in
)
```

## Description

This function overwrites the values in the destination buffer at the specified positions, with values taken from a user-defined array. If non-integer positions are given, the values are written to the closest integer positions.

When executing the operation on a buffer that has multiple bands, the bands are processed sequentially. For example, with a planar RGB buffer, the red band is processed first; the red values at the specified positions are replaced by those in the first third of the user-defined array. Then, the green band is processed; the green values at the specified positions are replaced by those in the second third of the user-defined array (immediately following the red values). Finally, the blue band is processed; the blue values at the specified positions are replaced by those in the last third of the user-defined array (immediately following the green values). If you want to overwrite, for example, 3 pixels, the function will process the red band of each pixel, then green band, and finally the blue band (RRR, GGG, BBB not RGB, RGB, RGB).

> **Note:** Note that if the destination buffer has multiple bands, its format should match the implicit format of the user array; otherwise, the operation is executed using a temporary buffer and then an internal conversion has to be done (from the temporary buffer to the destination). Likewise, if the destination buffer is compressed, the operation is executed using a temporary buffer and then an internal compression has to be done (from the temporary buffer to the destination).

## Parameters

### `DestBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer. This buffer can be a single-band (monochrome) or multi-band buffer and is not limited to image buffers.

### `NumPixels` *(in, AIL_INT)*

Specifies the number of values to put into the destination buffer.

### `PixXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of an array containing the X-coordinate of the positions in the buffer to overwrite.

### `PixYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of an array containing the Y-coordinate of the positions in the buffer to overwrite.

### `OverscanMode` *(in, AIL_INT64)*

Specifies the overscan mode. This specified mode controls how the function behaves with positions that fall outside the destination buffer.

*For specifying the overscan mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_OVERSCAN_DISABLE` | Specifies that if a position falls outside the destination buffer, the corresponding entry in the user-defined array is not used. |
| `M_OVERSCAN_ENABLE` *(default)* | Specifies that if a position falls outside the destination buffer, the function will try to modify the destination's ancestor (if applicable), if the given position is within its limits. Otherwise, the corresponding entry in the user-defined array is not used. |

### `UserArrayPtr` *(in, *void)*

Specifies the address of a user-defined array which contains values to write into the destination buffer. The data type of this array must match the data type of the destination buffer. Its number of entries is determined by the number of values to put and also by the number of bands of the destination buffer.
