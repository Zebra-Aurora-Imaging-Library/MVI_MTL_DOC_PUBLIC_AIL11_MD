---
doctype: Reference
module: gra
function: MgraRect
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraRect"
---

# MgraRect

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

> Draw a rectangle in an image without rotation or fill color, or add it to a 2D graphics list.

## Syntax

```c
void MgraRect(
    AIL_ID     ContextGraId,            //in
    AIL_ID     DstImageBufOrListGraId,  //out
    AIL_DOUBLE XStart,                  //in
    AIL_DOUBLE YStart,                  //in
    AIL_DOUBLE XEnd,                    //in
    AIL_DOUBLE YEnd                     //in
)
```

## Description

This function draws a rectangle destructively (raster-based) in the specified image. Alternatively, this function can add a vector-based version of a rectangle to the specified 2D graphics list, allowing you to, for example, non-destructively annotate a display without pixelation effects upon scaling.

The rectangle is created from the top-left corner ([`XStart`](../../Reference/gra/MgraRect.md), [`YStart`](../../Reference/gra/MgraRect.md)) to the bottom-right corner ([`XEnd`](../../Reference/gra/MgraRect.md), [`YEnd`](../../Reference/gra/MgraRect.md)). The rectangle inherits all the relevant settings of the specified 2D graphics context, such as the foreground color (see [`MgraAlloc`](../../Reference/gra/MgraAlloc.md) for default context settings). If part of the rectangle falls outside of the specified area (image or display), that part is clipped off.

To modify or inquire 2D graphics context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraInquire`](../../Reference/gra/MgraInquire.md). To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) or [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

A rectangle's coordinates are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md)with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraRect`](../../Reference/gra/MgraRect.md) an uncalibrated image, the function will generate an error.

To create a filled rectangle, use [`MgraRectFill`](../../Reference/gra/MgraRectFill.md). To create a rectangle that can be optionally filled and rotated, use [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md).

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DstImageBufOrListGraId`](../../Reference/gra/MgraRect.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

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

Specifies the identifier of a valid image buffer in which to draw the rectangle or the identifier of a valid 2D graphics list in which to add the rectangle. You must have allocated the image buffer or the 2D graphics list using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) or [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), respectively.

### `XStart` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the top-left corner of the rectangle in the input coordinate system.

### `YStart` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the top-left corner of the rectangle in the input coordinate system.

### `XEnd` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the bottom-right corner of the rectangle in the input coordinate system.

### `YEnd` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the bottom-right corner of the rectangle in the input coordinate system.
