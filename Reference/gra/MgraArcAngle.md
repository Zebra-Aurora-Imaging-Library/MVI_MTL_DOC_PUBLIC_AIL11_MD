---
doctype: Reference
module: gra
function: MgraArcAngle
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraArcAngle"
---

# MgraArcAngle

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

> Draw an arc or sector in an image, with optional rotation and fill color, or add it to a 2D graphics list.

## Syntax

```c
void MgraArcAngle(
    AIL_ID     ContextGraId,            //in
    AIL_ID     DstImageBufOrListGraId,  //out
    AIL_DOUBLE XCenter,                 //in
    AIL_DOUBLE YCenter,                 //in
    AIL_DOUBLE XRad,                    //in
    AIL_DOUBLE YRad,                    //in
    AIL_DOUBLE StartAngle,              //in
    AIL_DOUBLE EndAngle,                //in
    AIL_DOUBLE XAxisAngle,              //in
    AIL_INT64  ControlFlag              //in
)
```

## Description

This function draws an arc or sector destructively (raster-based) in the specified image. Alternatively, this function can add a vector-based version of an arc or sector to the specified 2D graphics list, allowing you to, for example, non-destructively annotate a display without pixelation effects upon scaling.

The arc or sector graphic is based on an ellipse centered at [`XCenter`](../../Reference/gra/MgraArcAngle.md) and [`YCenter`](../../Reference/gra/MgraArcAngle.md), with radii [`XRad`](../../Reference/gra/MgraArcAngle.md) and [`YRad`](../../Reference/gra/MgraArcAngle.md). The arc is the curve of the ellipse between the start angle ([`StartAngle`](../../Reference/gra/MgraArcAngle.md)) and the end angle ([`EndAngle`](../../Reference/gra/MgraArcAngle.md)), with an orientation along the X-axis ([`XAxisAngle`](../../Reference/gra/MgraArcAngle.md)). The arc or sector inherits all the relevant settings of the specified 2D graphics context, such as the foreground color ([`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md)). If part of the arc or sector falls outside of the specified area (image or display), that part is clipped off. The following image shows how the parameters are used to define the arc:

*[Image: gra_arc_angle_definition.png]*

The center position, radii, and angle values are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md) an uncalibrated image or the drawing target of the 2D graphics list is uncalibrated, the function will generate an error.

By default, only the arc is drawn in the image ([`M_CONTOUR`](../../Reference/gra/MgraArcAngle.md)). With the [`ControlFlag`](../../Reference/gra/MgraArcAngle.md) parameter, you can change this behavior to draw a sector of the ellipse ([`M_SECTOR`](../../Reference/gra/MgraArcAngle.md)) instead, with lines extending from the center of the ellipse to the start and end points of the arc. However, you can choose to draw it filled with the specified foreground color by specifying the [`M_FILLED`](../../Reference/gra/MgraArcAngle.md) combination constant.

To modify or inquire 2D graphics context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraInquire`](../../Reference/gra/MgraInquire.md). To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) or [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

To create a filled arc (sector) that is not rotated, you can also use [`MgraArcFill`](../../Reference/gra/MgraArcFill.md). To create an arc that is not rotated or filled, you can also use [`MgraArc`](../../Reference/gra/MgraArc.md).

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DstImageBufOrListGraId`](../../Reference/gra/MgraArcAngle.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

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

Specifies the identifier of a valid image buffer in which to draw the arc or sector, or the identifier of a valid 2D graphics list in which to add the arc or sector. You must have allocated the image buffer or the 2D graphics list using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) or [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), respectively.

### `XCenter` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the center of the ellipse from which the arc or sector is defined, in the input coordinate system.

### `YCenter` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the center of the ellipse from which the arc or sector is defined, in the input coordinate system.

### `XRad` *(in, AIL_DOUBLE)*

Specifies the radius of the ellipse from which the arc or sector is defined, along the X-axis in the input coordinate system.

*For specifying the radius along the X-axis*

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the radius along the X-axis. |

### `YRad` *(in, AIL_DOUBLE)*

Specifies the radius of the ellipse from which the arc or sector is defined, along the Y-axis in the input coordinate system.

*For specifying the radius along the Y-axis*

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the radius along the Y-axis. |

### `StartAngle` *(in, AIL_DOUBLE)*

Specifies the angle at which to start drawing the arc or sector, relative to the input coordinate system.

*For specifying the starting angle*

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the starting angle, in degrees. |

### `EndAngle` *(in, AIL_DOUBLE)*

Specifies the angle at which to end drawing the arc or sector, relative to the input coordinate system.

*For specifying the ending angle*

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the ending angle, in degrees. |

### `XAxisAngle` *(in, AIL_DOUBLE)*

Specifies the amount to rotate the ellipse used to define the arc or sector, in degrees, relative to the input coordinate system.

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to draw an arc or sector. This parameter must be set to one of the following values:

*To specify whether to draw an arc or sector*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CONTOUR` *(default)* | Specifies that the arc (the curve between the specified start and end angles) is drawn without lines extending from the center of the ellipse to the start and end points of the arc.

*[Image: graphicArcAngle.png]* |
| `M_SECTOR` | Specifies that a sector is drawn with lines extending from the center of the ellipse to the start and end points of the arc, unless the specified start and end angles form a closed curve.

*[Image: graphicArcAngleSector.png]* |

*For specifying whether the shape is filled*

| Value | Description |
| --- | --- |
| `M_FILLED` | Specifies that the shape is filled with specified foreground color ([`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md)), resulting in a filled sector. |
