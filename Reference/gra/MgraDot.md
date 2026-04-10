---
doctype: Reference
module: gra
function: MgraDot
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraDot"
---

# MgraDot

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

> Draw a dot in an image or add a dot to a 2D graphics list.

## Syntax

```c
void MgraDot(
    AIL_ID     ContextGraId,            //in
    AIL_ID     DstImageBufOrListGraId,  //out
    AIL_DOUBLE XPos,                    //in
    AIL_DOUBLE YPos                     //in
)
```

## Description

This function draws a dot destructively (raster-based) in the specified image. Alternatively, this function can add a vector-based version of a dot to the specified 2D graphics list, allowing you to, for example, non-destructively annotate a display without pixelation effects upon scaling.

The dot is based on a point positioned at [`XPos`](../../Reference/gra/MgraDot.md) and [`YPos`](../../Reference/gra/MgraDot.md). The dot inherits all the relevant settings of the specified 2D graphics context, such as the foreground color (see [`MgraAlloc`](../../Reference/gra/MgraAlloc.md) for default context settings). If the dot falls outside of the specified area, it is not created.

To modify or inquire 2D graphics context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraInquire`](../../Reference/gra/MgraInquire.md). To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) or [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

The dot's coordinates are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraDot`](../../Reference/gra/MgraDot.md) an uncalibrated image, the function will generate an error.

To create one or more dots, use [`MgraDots`](../../Reference/gra/MgraDots.md).

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DstImageBufOrListGraId`](../../Reference/gra/MgraDot.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of a valid image buffer in which to draw the dot or the identifier of a valid 2D graphics list in which to add the dot. You must have allocated the image buffer or the 2D graphics list using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) or [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), respectively.

### `XPos` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the dot in the input coordinate system.

### `YPos` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the dot in the input coordinate system.
