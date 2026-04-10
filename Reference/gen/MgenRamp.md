---
doctype: Reference
module: gen
function: MgenRamp
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / gen / MgenRamp"
---

# MgenRamp

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

> Generate 2D ramp data into a buffer.

## Syntax

```c
void MgenRamp(
    AIL_ID     BufId,       //out
    AIL_DOUBLE ScaleX,      //in
    AIL_DOUBLE ScaleY,      //in
    AIL_DOUBLE Offset,      //in
    AIL_INT64  ControlFlag  //in
)
```

## Description

This function uses the specified scale factors and offset to generate a 2D ramp (linearly increasing/decreasing data in both X and Y) in the specified buffer. You can specify any Aurora Imaging Library buffer, such as an array, image, kernel, LUT, or structuring element buffer.

This function generates the ramp data using the following formula: `[`BufId`](../../Reference/gen/MgenRamp.md)[_x_,_y_] = [`ScaleX`](../../Reference/gen/MgenRamp.md) * _x_ + [`ScaleY`](../../Reference/gen/MgenRamp.md) * _y_ + [`Offset`](../../Reference/gen/MgenRamp.md).`

This function can be useful, for example, to simulate non-uniform lighting in an image (non-uniform lighting is generally approximated by an intensity ramp that is added to the image), or to generate the pixel coordinates of all the pixels of an image.

## Parameters

### `BufId` *(out, AIL_ID)*

Specifies the identifier of the buffer in which to generate values. The buffer must have been previously allocated on the system using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md), [`M_IMAGE`](../../Reference/buf/MbufAlloc1d.md), [`M_KERNEL`](../../Reference/buf/MbufAlloc1d.md), [`M_LUT`](../../Reference/buf/MbufAlloc1d.md), or [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc1d.md).

### `ScaleX` *(in, AIL_DOUBLE)*

Specifies the factor by which to scale in X.

### `ScaleY` *(in, AIL_DOUBLE)*

Specifies the factor by which to scale in Y.

### `Offset` *(in, AIL_DOUBLE)*

Specifies the offset. This corresponds to the value placed in the first location of the buffer.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
