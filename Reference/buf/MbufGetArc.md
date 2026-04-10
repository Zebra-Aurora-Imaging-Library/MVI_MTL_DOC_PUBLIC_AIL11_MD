---
doctype: Reference
module: buf
function: MbufGetArc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufGetArc"
---

# MbufGetArc

> Read the pixels along a specified arc, count the pixels, and store their values, and coordinates, in user-defined arrays.

## Syntax

```c
AIL_INT MbufGetArc(
    AIL_ID     ImageBufId,     //in
    AIL_INT    XCenter,        //in
    AIL_INT    YCenter,        //in
    AIL_INT    XRad,           //in
    AIL_INT    YRad,           //in
    AIL_DOUBLE StartAngle,     //in
    AIL_DOUBLE EndAngle,       //in
    AIL_INT64  Mode,           //in
    AIL_INT *  NbPixelsPtr,    //out
    void *     ValueArrayPtr,  //out
    void *     PosXArrayPtr,   //out
    void *     PosYArrayPtr    //out
)
```

## Description

This function reads the series of pixels along an elliptic arc from an image and stores their values, and coordinates, in user-defined arrays. The arc is based on an ellipse centered at [`XCenter`](../../Reference/buf/MbufGetArc.md) and [`YCenter`](../../Reference/buf/MbufGetArc.md), with radii [`XRad`](../../Reference/buf/MbufGetArc.md) and [`YRad`](../../Reference/buf/MbufGetArc.md). The arc is the part of the ellipse between the start angle ([`StartAngle`](../../Reference/buf/MbufGetArc.md)) and the end angle ([`EndAngle`](../../Reference/buf/MbufGetArc.md)), both measured with respect to the X-axis. The following image shows how the parameters are used to define an arc:*[Image: gra_arc_definition.png]*

To determine the exact size of the array needed for a given set of parameter values, call this function twice: the first time to determine the required array size and the second time to fill the arrays. When calling for the first time, specify the same values for [`ImageBufId`](../../Reference/buf/MbufGetArc.md), [`XCenter`](../../Reference/buf/MbufGetArc.md), [`YCenter`](../../Reference/buf/MbufGetArc.md), [`XRad`](../../Reference/buf/MbufGetArc.md),[`YRad`](../../Reference/buf/MbufGetArc.md), [`StartAngle`](../../Reference/buf/MbufGetArc.md), [`EndAngle`](../../Reference/buf/MbufGetArc.md), and [`Mode`](../../Reference/buf/MbufGetArc.md) as you would for the second call to this function. In addition, set [`NbPixelsPtr`](../../Reference/buf/MbufGetArc.md) to an appropriate address, and set the array parameters to [`M_NULL`](../../Reference/buf/MbufGetArc.md).

If the source buffer is compressed, Aurora Imaging Library decompresses the data before copying it to the user-supplied array.

## Parameters

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of a single-band (monochrome) source image buffer.

### `XCenter` *(in, AIL_INT)*

Specifies the X-coordinate of the center of the ellipse from which the arc is defined, in the pixel coordinate system.

### `YCenter` *(in, AIL_INT)*

Specifies the Y-coordinate of the center of the ellipse from which the arc is defined, in the pixel coordinate system.

### `XRad` *(in, AIL_INT)*

Specifies the radius of the ellipse from which the arc is defined, along the X-axis of the pixel coordinate system.

*For specifying the radius along the X-axis*

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the radius along the X-axis. |

### `YRad` *(in, AIL_INT)*

Specifies the radius of the ellipse from which the arc is defined, along the Y-axis of the pixel coordinate system.

*For specifying the radius along the Y-axis*

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the radius along the Y-axis. |

### `StartAngle` *(in, AIL_DOUBLE)*

Specifies the angle at which to start reading the arc, relative to the positive X-axis of the pixel coordinate system.

*For specifying the starting angle*

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the angle at which to start reading the arc, in degrees. |

### `EndAngle` *(in, AIL_DOUBLE)*

Specifies the angle at which to end reading the arc, relative to the positive X-axis of the pixel coordinate system.

*For specifying the ending angle*

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the angle at which to end reading the arc, in degrees. |

### `Mode` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `NbPixelsPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the number of pixels found along the arc. Set this parameter to `M_NULL` if it is not to be evaluated.

### `ValueArrayPtr` *(out, *void)*

Specifies the address of the user array in which to store the pixel values from the image buffer. Ensure that the user array is large enough to receive the data to be stored. When not using this parameter or when determining the required size of this array for subsequent calls to [`MbufGetArc`](../../Reference/buf/MbufGetArc.md), set this parameter to `M_NULL`. In this case, nothing is read from the image buffer.

### `PosXArrayPtr` *(out, *void)*

Specifies the address of the user array in which to store the X-coordinates of the points along the arc, from the image buffer. Ensure that the user array is large enough to receive the data to be stored. When not using this parameter or when determining the required size of this array for subsequent calls to [`MbufGetArc`](../../Reference/buf/MbufGetArc.md), set this value to `M_NULL`. In this case, nothing is read from the image buffer.

### `PosYArrayPtr` *(out, *void)*

Specifies the address of the user array in which to store the Y-coordinates of the points along the arc, from the image buffer. Ensure that the user array is large enough to receive the data to be stored. When not using this parameter or when determining the required size of this array for subsequent calls to [`MbufGetArc`](../../Reference/buf/MbufGetArc.md), set this value to `M_NULL`. In this case, nothing is read from the image buffer.

## Return Value

**Type:** `AIL_INT`

The returned value is the number of pixels found along the arc.
