---
doctype: Reference
module: im
function: MimShift
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimShift"
---

# MimShift

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

> Perform a point-to-point bit shift.

## Syntax

```c
void MimShift(
    AIL_ID  SrcImageBufId,  //in
    AIL_ID  DstImageBufId,  //out
    AIL_INT NbBitsToShift   //in
)
```

## Description

This function performs left or right bit-shifting on each pixel in the specified image. The shift operation is signed or unsigned depending on the source image buffer's data type.

Note that floating-point values will be cast to _AIL_UINT32_ before performing the shift operation (except when shifting by 0). Therefore, unexpected results can occur if a floating-point value is larger than the _AIL_UINT32_ range.

Note that, if you shift by 0, only a copy operation will be performed.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)).

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the data source of the operation. This parameter must be given an image buffer identifier.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the results. This parameter must be given an image buffer identifier.

### `NbBitsToShift` *(in, AIL_INT)*

Specifies the number of bits to shift. If the given value is negative, each pixel in the specified image is right bit-shifted by the specified number of bits; otherwise, it is left bit-shifted.

## Remarks

> Optimized in-place processing is supported (source equals destination), but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).
