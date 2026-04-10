---
doctype: Reference
module: buf
function: MbufGet2d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufGet2d"
---

# MbufGet2d

> Get data from a two-dimensional area of a buffer and place it in a user-supplied array.

## Syntax

```c
void MbufGet2d(
    AIL_ID  SrcBufId,     //in
    AIL_INT OffX,         //in
    AIL_INT OffY,         //in
    AIL_INT SizeX,        //in
    AIL_INT SizeY,        //in
    void *  UserArrayPtr  //out
)
```

## Description

This function copies data from a specified two-dimensional area of an Aurora Imaging Library source buffer to a user-supplied array. If the source buffer has multiple bands, this function linearly copies the data from the specified two-dimensional area of each band. The internal data format of the source buffer need not match the data format of the user-supplied array (planar); an internal conversion will be performed if necessary. Note, however, if the formats do match, the operation will be much faster. For more information, refer to [`MbufGetColor2d`](../../Reference/buf/MbufGetColor2d.md) with [`M_PLANAR`](../../Reference/buf/MbufGetColor2d.md).

If the specified buffer is a binary image buffer, any extra bits to complete a byte in the array are undefined.

If the source buffer is compressed, Aurora Imaging Library decompresses the data before copying it to the user-supplied array.

## Parameters

### `SrcBufId` *(in, AIL_ID)*

Specifies the identifier of the source buffer.

### `OffX` *(in, AIL_INT)*

Specifies the horizontal offset of the required area, relative to the top-left coordinate of the source buffer.

### `OffY` *(in, AIL_INT)*

Specifies the vertical offset of the required area, relative to the top-left coordinate of the source buffer.

### `SizeX` *(in, AIL_INT)*

Specifies the width of the required area of the source buffer.

### `SizeY` *(in, AIL_INT)*

Specifies the height of the required area of the source buffer.

### `UserArrayPtr` *(out, *void)*

Specifies the address of the user array in which to copy the data from the source buffer. Ensure that the user array is large enough to receive the data to be copied from the source buffer.
