---
doctype: Reference
module: buf
function: MbufPut
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufPut"
---

# MbufPut

> Put data from a user-supplied array into a data buffer.

## Syntax

```c
void MbufPut(
    AIL_ID       DestBufId,    //out
    const void * UserArrayPtr  //in
)
```

## Description

This function copies data from a user-supplied array to a specified Aurora Imaging Library destination buffer. If the destination buffer has multiple bands, Aurora Imaging Library copies the data from the user-supplied array to the destination buffer sequentially. The internal data format of the user-supplied array need not match the data format of the Aurora Imaging Library destination buffer (planar); an internal conversion will be performed if necessary. Note, however, if the formats do match, the operation will be much faster. For more information, refer to [`MbufPutColor`](../../Reference/buf/MbufPutColor.md) with [`M_PLANAR`](../../Reference/buf/MbufPutColor.md).

## Parameters

### `DestBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer.

### `UserArrayPtr` *(in, *void)*

Specifies the address of the user array from which to put data into the destination buffer. Ensure that user array is large enough to contain the data to be copied to the destination buffer.
