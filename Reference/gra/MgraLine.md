---
doctype: Reference
module: gra
function: MgraLine
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraLine"
---

# MgraLine

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

> Draw a line in an image or add a line to a 2D graphics list.

## Syntax

```c
void MgraLine(
    AIL_ID     ContextGraId,            //in
    AIL_ID     DstImageBufOrListGraId,  //out
    AIL_DOUBLE XStart,                  //in
    AIL_DOUBLE YStart,                  //in
    AIL_DOUBLE XEnd,                    //in
    AIL_DOUBLE YEnd                     //in
)
```

## Description

This function draws a line destructively (raster-based) in the specified image. Alternatively, this function can add a vector-based version of the line to the specified 2D graphics list, allowing you to, for example, non-destructively annotate a display without pixelation effects upon scaling.

The line is based on a start point ([`XStart`](../../Reference/gra/MgraLine.md), [`YStart`](../../Reference/gra/MgraLine.md)) and an end point ([`XEnd`](../../Reference/gra/MgraLine.md), [`YEnd`](../../Reference/gra/MgraLine.md)). The line inherits all the relevant settings of the specified 2D graphics context, such as the foreground color (see [`MgraAlloc`](../../Reference/gra/MgraAlloc.md) for default context settings). If part of the line falls outside of the specified area (image or display), that part is clipped off.

To modify or inquire 2D graphics context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraInquire`](../../Reference/gra/MgraInquire.md). To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) or [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

A line's coordinates are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraLine`](../../Reference/gra/MgraLine.md) an uncalibrated image, the function will generate an error.

To create multiple lines, a polyline, or a polygon, use [`MgraLines`](../../Reference/gra/MgraLines.md).

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DstImageBufOrListGraId`](../../Reference/gra/MgraLine.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

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

Specifies the identifier of a valid image buffer in which to draw the line or the identifier of a valid 2D graphics list in which to add the line. You must have allocated the image buffer or the 2D graphics list using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) or [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), respectively.

### `XStart` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the start of the line in the input coordinate system.

### `YStart` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the start of the line in the input coordinate system.

### `XEnd` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the end of the line in the input coordinate system.

### `YEnd` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the end of the line in the input coordinate system.
