---
doctype: Reference
module: gra
function: MgraFill
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraFill"
---

# MgraFill

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

> Perform a boundary-type seed fill.

## Syntax

```c
void MgraFill(
    AIL_ID     ContextGraId,   //in
    AIL_ID     DstImageBufId,  //out
    AIL_DOUBLE XStart,         //in
    AIL_DOUBLE YStart          //in
)
```

## Description

This function performs a boundary-type seed fill. It fills in an area of the specified image with the foreground color of the specified 2D graphics context, starting from the seed position ([`XStart`](../../Reference/gra/MgraFill.md), [`YStart`](../../Reference/gra/MgraFill.md)). Filling occurs on adjacent pixels (vertically and horizontally to original seed pixel) that have the same value as the original seed pixel.

If [`MgraFill`](../../Reference/gra/MgraFill.md) is used on a multi-band buffer, each band will be processed separately. This means that each band of the adjacent pixels will be compared with the corresponding band of the seed pixel. This can produce strange results if, for example, you try to fill the inside of a red circle with blue. The blue will spread to the whole image since the red circle does not exist in the blue band.

A seed's coordinates are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraFill`](../../Reference/gra/MgraFill.md) an uncalibrated image, the function will generate an error.

Note that [`MgraFill`](../../Reference/gra/MgraFill.md) cannot be performed on graphics contained within a 2D graphics list.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of a valid image buffer in which to perform the filling operation. You must have allocated the image buffer using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md).

### `XStart` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the seed position in the input coordinate system. If the specified point is not within an enclosed area, filling occurs until the boundaries of the buffer are encountered.

### `YStart` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the seed position in the input coordinate system. If the specified point is not within an enclosed area, filling occurs until the boundaries of the buffer are encountered.
