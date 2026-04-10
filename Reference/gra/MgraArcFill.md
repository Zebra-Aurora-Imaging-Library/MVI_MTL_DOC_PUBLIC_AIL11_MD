---
doctype: Reference
module: gra
function: MgraArcFill
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraArcFill"
---

# MgraArcFill

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

> Draw a filled arc (sector) in an image without rotation, or add it to a 2D graphics list.

## Syntax

```c
void MgraArcFill(
    AIL_ID     ContextGraId,            //in
    AIL_ID     DstImageBufOrListGraId,  //out
    AIL_DOUBLE XCenter,                 //in
    AIL_DOUBLE YCenter,                 //in
    AIL_DOUBLE XRad,                    //in
    AIL_DOUBLE YRad,                    //in
    AIL_DOUBLE StartAngle,              //in
    AIL_DOUBLE EndAngle                 //in
)
```

## Description

This function draws a filled arc (sector) destructively (raster-based) in the specified image. Alternatively, this function can add a vector-based version of a filled arc to the specified 2D graphics list, allowing you to, for example, non-destructively annotate a display without pixelation effects upon scaling. Note that the arc is filled with the specified foreground color ([`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md)).

The filled arc is based on an ellipse centered at [`XCenter`](../../Reference/gra/MgraArcFill.md) and [`YCenter`](../../Reference/gra/MgraArcFill.md), with radii [`XRad`](../../Reference/gra/MgraArcFill.md) and [`YRad`](../../Reference/gra/MgraArcFill.md). The arc is the part of the ellipse between the start angle ([`StartAngle`](../../Reference/gra/MgraArcFill.md)) and the end angle ([`EndAngle`](../../Reference/gra/MgraArcFill.md)), both measured with respect to the X-axis. The arc inherits all the relevant settings of the specified 2D graphics context, such as the foreground color ([`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md)). If part of the filled arc falls outside of the specified area (image or display), that part is clipped off. The following image shows how the parameters are used to define the arc:

*[Image: gra_arc_fill_definition.png]*

A filled arc's center position, radii, and angle values are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraArcFill`](../../Reference/gra/MgraArcFill.md) an uncalibrated image or the drawing target of the 2D graphics list is uncalibrated, the function will generate an error.

To modify or inquire 2D graphics context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraInquire`](../../Reference/gra/MgraInquire.md). To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) or [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

To create an arc that is not filled, use [`MgraArc`](../../Reference/gra/MgraArc.md). To create an arc that can be optionally filled and rotated, use [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md).

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DstImageBufOrListGraId`](../../Reference/gra/MgraArcFill.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

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

Specifies the identifier of a valid image buffer in which to draw the arc or the identifier of a valid 2D graphics list in which to add the arc. You must have allocated the image buffer or the 2D graphics list using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) or [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), respectively.

### `XCenter` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the center of the ellipse from which the arc is defined, in the input coordinate system.

### `YCenter` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the center of the ellipse from which the arc is defined, in the input coordinate system.

### `XRad` *(in, AIL_DOUBLE)*

Specifies the radius of the ellipse from which the arc is defined, along the X-axis of the input coordinate system.

*For specifying the radius along the X-axis*

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the radius along the X-axis. |

### `YRad` *(in, AIL_DOUBLE)*

Specifies the radius of the ellipse from which the arc is defined, along the Y-axis of the input coordinate system.

*For specifying the radius along the Y-axis*

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the radius along the Y-axis. |

### `StartAngle` *(in, AIL_DOUBLE)*

Specifies the angle at which to start drawing the arc, relative to the input coordinate system.

*For specifying the starting angle*

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the starting angle, in degrees. |

### `EndAngle` *(in, AIL_DOUBLE)*

Specifies the angle at which to end drawing the arc, relative to the input coordinate system.

*For specifying the ending angle*

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the ending angle, in degrees. |
