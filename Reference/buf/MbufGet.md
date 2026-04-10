---
doctype: Reference
module: buf
function: MbufGet
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufGet"
---

# MbufGet

> Get data from a buffer and place it in a user-supplied array.

## Syntax

```c
void MbufGet(
    AIL_ID SrcBufId,     //in
    void * UserArrayPtr  //out
)
```

## Description

This function copies data from a specified Aurora Imaging Library source buffer to a user-supplied array. If the source buffer has multiple bands, Aurora Imaging Library copies all bands, one after the other. The internal data format of the source buffer need not match the data format of the user-supplied array (planar); an internal conversion will be performed if necessary. Note, however, if the formats do match, the operation will be much faster. For more information, refer to [`MbufGetColor`](../../Reference/buf/MbufGetColor.md) with [`M_PLANAR`](../../Reference/buf/MbufGetColor.md).

If the specified buffer is a binary image buffer, any extra bits to complete a byte in the array are undefined.

If the source buffer is compressed, Aurora Imaging Library decompresses the data before copying it to the user-supplied array.

## Parameters

### `SrcBufId` *(in, AIL_ID)*

Specifies the identifier of the source buffer.

### `UserArrayPtr` *(out, *void)*

Specifies the address of the user array in which to copy the data from the source buffer. Ensure that the user array is large enough to receive the data to be copied from the source buffer.
