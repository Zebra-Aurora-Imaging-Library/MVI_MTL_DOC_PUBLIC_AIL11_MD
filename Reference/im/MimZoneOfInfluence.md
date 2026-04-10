---
doctype: Reference
module: im
function: MimZoneOfInfluence
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimZoneOfInfluence"
---

# MimZoneOfInfluence

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

> Perform a zone of influence detection.

## Syntax

```c
void MimZoneOfInfluence(
    AIL_ID    SrcImageBufId,  //in
    AIL_ID    DstImageBufId,  //out
    AIL_INT64 OperationFlag   //in
)
```

## Description

This function separates an image into zones, according to how much of a blob's surrounding background is within the blob's territorial boundaries, or "zone of influence". The image is considered to be binary, with background pixels equal to 0 (black), and all non-zero pixels treated as blobs. It gives every pixel in a blob's zone of influence the same value. Each zone of influence is numbered consecutively, beginning with 1. A blob's zone of influence consists of all pixels closer to that blob than to any other blob. There are as many zones as blobs.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source of the operation. This parameter must be given an image buffer identifier.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the resulting image. This parameter must be given an image buffer identifier.

### `OperationFlag` *(in, AIL_INT64)*

Specifies the type of 3x3 distance matrix used for the operation. The following table shows each value's respective 3x3 distance matrix for calculating the distance to a neighboring pixel, which is then used for the zone of influence computation.

*For controlling the type of 3x3 distance matrix*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_CHESSBOARD`](../../Reference/im/MimZoneOfInfluence.md). |
| `M_CHAMFER_3_4` | Uses a distance algorithm that makes a better approximation to actual Euclidean distance, and is therefore more accurate than [`M_CHESSBOARD`](../../Reference/im/MimZoneOfInfluence.md).

This is the 3x3 distance matrix:

*[Image: mimzone_champfmtx.png]* |
| `M_CHESSBOARD` | Specifies a chessboard matrix. This operation is faster than [`M_CHAMFER_3_4`](../../Reference/im/MimZoneOfInfluence.md).

This is the 3x3 distance matrix:

*[Image: mimzone_chessmtx.png]* |

## Remarks

> In-place processing is supported, but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).
