---
doctype: Reference
module: seq
function: MseqFeed
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / seq / MseqFeed"
---

# MseqFeed

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

> Queue an image on which to operate.

## Syntax

```c
void MseqFeed(
    AIL_ID    ContextSeqId,  //in
    AIL_ID    ImageBufId,    //in
    AIL_INT64 InitFlag       //in
)
```

## Description

This function queues an image which the sequence processing session must operate on. Once queued, the image is copied to an internal buffer and the original image buffer can be used for other purposes. If you call [`MseqFeed`](../../Reference/seq/MseqFeed.md) before you start the sequence processing session, an error will be generated. The sequence processing session has a limited queue size; if the queue is full and you call [`MseqFeed`](../../Reference/seq/MseqFeed.md) faster than processing occurs, [`MseqFeed`](../../Reference/seq/MseqFeed.md) will wait for a place in the queue to become available before returning.

> **Note:** The specified image is copied to the internal buffer using [`MbufCopy`](../../Reference/buf/MbufCopy.md). The internal image buffer is an unsigned 8-bit YUV12 buffer. [`MseqFeed`](../../Reference/seq/MseqFeed.md) does not compensate for lost data if the depth or type of the specified image buffer does not match the internal buffer (see [`MbufCopy`](../../Reference/buf/MbufCopy.md) for more information regarding what happens to data when the source and destination image buffers do not match).

## Parameters

### `ContextSeqId` *(in, AIL_ID)*

Specifies the identifier of the sequence context. The sequence context must have been previously allocated using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md).

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the buffer containing the image to be queued for the sequence processing operation.

### `InitFlag` *(in, AIL_INT64)*

Reserved for future use. Set this parameter to `M_DEFAULT`.
