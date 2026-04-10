---
doctype: Reference
module: cal
function: McalRelativeOrigin
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / cal / McalRelativeOrigin"
---

# McalRelativeOrigin

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

> Change the origin and/or orientation of the relative coordinate system.

## Syntax

```c
void McalRelativeOrigin(
    AIL_ID     CalibrationOrImageId,  //out
    AIL_DOUBLE XOffset,               //in
    AIL_DOUBLE YOffset,               //in
    AIL_DOUBLE ZOffset,               //in
    AIL_DOUBLE AngularOffset,         //in
    AIL_INT64  ControlFlag            //in
)
```

## Description

This function changes the origin and/or orientation of the relative coordinate system of a camera calibration context or a calibrated image. If you specify a camera calibration context, the new location of the relative coordinate system will be taken into account in images associated with the camera calibration context after the function call, but images previously associated with the camera calibration context will not be affected. After the function call, you can  with the new relative coordinate system in world units with respect to the new origin.

Moving the relative coordinate system of an image is useful for analyzing an object with respect to a temporary local coordinate system. The relative coordinate system is moved with respect to the specified reference coordinate system. The reference coordinate system can be the absolute coordinate system ([`M_ASSIGN`](../../Reference/cal/McalRelativeOrigin.md)) or the current location of relative coordinate system ([`M_COMPOSE_WITH_CURRENT`](../../Reference/cal/McalRelativeOrigin.md)); when it is the absolute coordinate system, the relative coordinate system is re-initialized to the location and orientation of the absolute coordinate system before applying the specified transformation. You can also use [`McalFixture`](../../Reference/cal/McalFixture.md) to move the relative coordinate system to the location of a specified occurrence of an Aurora Imaging Library Pattern Matching or Model Finder model.

If you specify both a rotation and a translation of the relative coordinate system, the rotation is done first.

> **Note:** Note that when the reference coordinate system is the absolute coordinate system, setting the [`XOffset`](../../Reference/cal/McalRelativeOrigin.md), [`YOffset`](../../Reference/cal/McalRelativeOrigin.md),[`ZOffset`](../../Reference/cal/McalRelativeOrigin.md), and [`AngularOffset`](../../Reference/cal/McalRelativeOrigin.md) parameters to 0 has the effect of resetting the relative coordinate system to that of the absolute coordinate system.

To manipulate the relative coordinate system of a 3D-based  ([`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)), it is recommended to use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) instead of [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) since the former supports more transformation types.

If you adjust the relative coordinate system of a calibrated image associated with an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, the raster information will be discarded, causing the ROI to become an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI. See [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) for more information.

## Parameters

### `CalibrationOrImageId` *(out, AIL_ID)*

Specifies the identifier of the camera calibration context or calibrated image.

### `XOffset` *(in, AIL_DOUBLE)*

Specifies the X-offset of the relative coordinate system origin from the reference coordinate system, in world-units.

### `YOffset` *(in, AIL_DOUBLE)*

Specifies the Y-offset of the relative coordinate system origin from the reference coordinate system, in world-units.

### `ZOffset` *(in, AIL_DOUBLE)*

Specifies the Z-offset of the relative coordinate system origin from the reference coordinate system, in world-units.

### `AngularOffset` *(in, AIL_DOUBLE)*

Specifies the angle, in degrees, at which to rotate the relative coordinate system around the Z-axis of the reference coordinate system. A positive angle will rotate the relative coordinate system in the direction from the positive X-axis to the negative Y-axis of the reference coordinate system.

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to apply the transformation. This establishes the reference coordinate system. This parameter can be set to one of the following values:

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ASSIGN` *(default)* | Specifies to assign the specified transformation to the relative coordinate system from the origin and orientation of the absolute coordinate system. |
| `M_COMPOSE_WITH_CURRENT` | Specifies to compose the specified transformation with the current position and orientation of the relative coordinate system. This effectively applies the transformation to the current position and orientation of the relative coordinate system. |
