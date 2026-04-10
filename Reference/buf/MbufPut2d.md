---
doctype: Reference
module: buf
function: MbufPut2d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufPut2d"
---

# MbufPut2d

> Put data from a user-supplied array into a two-dimensional area of a buffer.

## Syntax

```c
void MbufPut2d(
    AIL_ID       DestBufId,    //out
    AIL_INT      OffX,         //in
    AIL_INT      OffY,         //in
    AIL_INT      SizeX,        //in
    AIL_INT      SizeY,        //in
    const void * UserArrayPtr  //in
)
```

## Description

This function copies data from a user-supplied array to a two-dimensional area of the specified Aurora Imaging Library destination buffer. If the user-supplied array has multiple bands, Aurora Imaging Library copies the data from the user-supplied array to the destination buffer sequentially. The internal data format of the user-supplied array need not match the data format of the Aurora Imaging Library destination buffer (planar); an internal conversion will be performed if necessary. Note, however, if the formats do match, the operation will be much faster. For more information, refer to [`MbufPutColor2d`](../../Reference/buf/MbufPutColor2d.md) with [`M_PLANAR`](../../Reference/buf/MbufPutColor.md).

## Parameters

### `DestBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer.

### `OffX` *(in, AIL_INT)*

Specifies the horizontal offset of the destination buffer area in which to put the data, relative to the destination buffer's top-left pixel.

### `OffY` *(in, AIL_INT)*

Specifies the vertical offset of the destination buffer area in which to put the data, relative to the destination buffer's top-left pixel.

### `SizeX` *(in, AIL_INT)*

Specifies the width of the destination buffer area in which to put the data.

### `SizeY` *(in, AIL_INT)*

Specifies the height of the destination buffer area in which to put the data.

### `UserArrayPtr` *(in, *void)*

Specifies the address of the user array from which to copy data into the destination buffer. Ensure that user array is large enough to contain the data to be copied to the destination buffer.
