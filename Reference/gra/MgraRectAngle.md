---
doctype: Reference
module: gra
function: MgraRectAngle
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraRectAngle"
---

# MgraRectAngle

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

> Draw a rectangle in an image with optional rotation and fill color, or add it to a 2D graphics list.

## Syntax

```c
void MgraRectAngle(
    AIL_ID     ContextGraId,               //in
    AIL_ID     DestImageBufIdOrGraListId,  //out
    AIL_DOUBLE XPos,                       //in
    AIL_DOUBLE YPos,                       //in
    AIL_DOUBLE Width,                      //in
    AIL_DOUBLE Height,                     //in
    AIL_DOUBLE Angle,                      //in
    AIL_INT64  ControlFlag                 //in
)
```

## Description

This function draws a rectangle destructively (raster-based) in the specified image with an optional rotation and fill color. Alternatively, this function can add a vector-based version of a rectangle to the specified 2D graphics list, allowing you to, for example, non-destructively annotate a display without pixelation effects upon scaling.

The rectangle is created using a point ([`XPos`](../../Reference/gra/MgraRectAngle.md), [`YPos`](../../Reference/gra/MgraRectAngle.md)), a width ([`Width`](../../Reference/gra/MgraRectAngle.md)), a height ([`Height`](../../Reference/gra/MgraRectAngle.md)), and an angle of rotation ([`Angle`](../../Reference/gra/MgraRectAngle.md)). The rectangle inherits all the relevant settings of the specified 2D graphics context, such as the foreground color (see [`MgraAlloc`](../../Reference/gra/MgraAlloc.md) for default context settings). If part of the rectangle falls outside of the specified area (image or display), that part is clipped off.

To modify or inquire 2D graphics context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraInquire`](../../Reference/gra/MgraInquire.md). To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) or [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

A rectangle's coordinates, dimensions, and angle are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md) an uncalibrated image or the drawing target of the 2D graphics list is uncalibrated, the function will generate an error.

To create a filled rectangle that is not rotated, use [`MgraRectFill`](../../Reference/gra/MgraRectFill.md). To create a rectangle that is not rotated or filled, use [`MgraRect`](../../Reference/gra/MgraRect.md).

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DestImageBufIdOrGraListId`](../../Reference/gra/MgraRectAngle.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `DestImageBufIdOrGraListId` *(out, AIL_ID)*

Specifies the identifier of a valid image buffer in which to draw the rectangle or the identifier of a valid 2D graphics list in which to add the rectangle. You must have allocated the image buffer or the 2D graphics list using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) or [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), respectively.

### `XPos` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the rectangle's position in the input coordinate system.

### `YPos` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the rectangle's position in the input coordinate system.

### `Width` *(in, AIL_DOUBLE)*

Specifies the width of the rectangle, relative to the input coordinate system.

### `Height` *(in, AIL_DOUBLE)*

Specifies the height of the rectangle, relative to the input coordinate system.

### `Angle` *(in, AIL_DOUBLE)*

Specifies the angle with which to rotate the rectangle, in degrees, relative to the input coordinate system. The rectangle will either be rotated around its center (when using [`M_CENTER_AND_DIMENSION`](../../Reference/gra/MgraRectAngle.md)) or its top-left corner (when using [`M_CORNER_AND_DIMENSION`](../../Reference/gra/MgraRectAngle.md)).

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to draw the rectangle in the image or add the rectangle to the 2D graphics list.

*For specifying how to draw or add the rectangle*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_CORNER_AND_DIMENSION`](../../Reference/gra/MgraRectAngle.md). |
| `M_CENTER_AND_DIMENSION` | Specifies that the rectangle is drawn according to its center position, as defined by the specified coordinates. Coordinates are set with the [`XPos`](../../Reference/gra/MgraRectAngle.md) and [`YPos`](../../Reference/gra/MgraRectAngle.md) parameters. |
| `M_CORNER_AND_DIMENSION` | Specifies that the rectangle is drawn according to the position of its top-left corner, as defined by the specified coordinates. Coordinates are set with the [`XPos`](../../Reference/gra/MgraRectAngle.md) and [`YPos`](../../Reference/gra/MgraRectAngle.md) parameters. |

*For specifying whether the rectangle is filled*

| Value | Description |
| --- | --- |
| `M_FILLED` | Specifies that the rectangle is filled with specified foreground color ([`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md)). |
