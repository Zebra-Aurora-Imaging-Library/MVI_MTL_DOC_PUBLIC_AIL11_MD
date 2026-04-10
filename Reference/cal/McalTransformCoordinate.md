---
doctype: Reference
module: cal
function: McalTransformCoordinate
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalTransformCoordinate"
---

# McalTransformCoordinate

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

> Convert coordinates between their world and pixel values.

## Syntax

```c
void McalTransformCoordinate(
    AIL_ID       CalibrationOrImageId,  //in
    AIL_INT64    TransformType,         //in
    AIL_DOUBLE   X,                     //in
    AIL_DOUBLE   Y,                     //in
    AIL_DOUBLE * ResXPtr,               //out
    AIL_DOUBLE * ResYPtr                //out
)
```

## Description

This function converts a pair of coordinates from their pixel value to their world value (or vice versa). The conversion can be performed according to a camera calibration context, calibrated image, or corrected image.

> **Note:** Note that, if you changed the origin and/or orientation of the relative coordinate system (using [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) or [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md)), world coordinates will be returned, or assumed to be given, with respect to this relative coordinate system.

This function converts the coordinates of a point by making a line connecting the center of the camera's lens with the point provided, and then finding the intersection of that line with the required plane. To convert an image pixel to a world point, this function defines a line connecting the center of the camera's lens with the image plane, and then returns the intersection of this line with the world plane.

*[Image: cal_transform_coordinate.png]*

However, if the image plane is not parallel to the relative coordinate system, due to the camera setup or a displacement of the relative coordinate system, not every point in the image plane will have a valid real-world equivalent. Three types of intersections can occur when transforming from the image plane to the world plane. If the specified point in the image plane corresponds to a point in front of the camera and in the relative coordinate system, that point in the relative coordinate system is returned. However, if the line traced through the specified point in the image plane does not intersect the XY (Z=0) plane of the relative coordinate system, **M_INVALID_POINT** is returned since there is no intersection. If the line's intersection with the XY (Z=0) plane of the relative coordinate system is behind the camera, then the mathematically computed value is returned, even though it is not the correct world location of the point specified in the image.

*[Image: cal_transform_coordinate_pixel_to_world.png]*

However, using [`M_NO_POINTS_BEHIND_CAMERA`](../../Reference/cal/McalTransformCoordinate.md) returns **M_INVALID_POINT** if the returned point is behind the camera.

## Parameters

### `CalibrationOrImageId` *(in, AIL_ID)*

Specifies the identifier of the camera calibration context, calibrated image, or corrected image. When an image is specified, the transformation uses the camera calibration information associated with this image. The image buffer must be a 1- or 3-band buffer.

### `TransformType` *(in, AIL_INT64)*

Specifies whether to perform a pixel-to-world or world-to-pixel conversion. This parameter must be set to one of the following values:

*For specifying pixel-to-world or world-to-pixel*

| Value | Description |
| --- | --- |
| `M_PIXEL_TO_WORLD` | Converts from pixel to world. |
| `M_WORLD_TO_PIXEL` | Converts from world to pixel. |

*For specifying to return invalid points*

| Value | Description |
| --- | --- |
| `M_NO_EXTRAPOLATED_POINTS` | Specifies that if a pixel involved in the transformation is not inside the calibrated region, **M_INVALID_POINT**will be returned, instead of a coordinate resulting from the extrapolation. The calibrated region is defined as the image region covered by the camera calibration grid. This region can be displayed by calling [`McalDraw`](../../Reference/cal/McalDraw.md) with [`M_DRAW_VALID_REGION`](../../Reference/cal/McalDraw.md).

> **Note:** This combination value only applies to piecewise linear camera calibrations ([`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md)); if this combination value is specified and the object passed to [`CalibrationOrImageId`](../../Reference/cal/McalTransformCoordinate.md) has any other type of camera calibration, this combination value is ignored. |
| `M_NO_POINTS_BEHIND_CAMERA` | Specifies that **M_INVALID_POINT** is returned when a computed point is mathematically valid but physically impossible (behind the camera).

> **Note:** If this value does not apply to the specified transformation type, then it is ignored. |

### `X` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the input.

### `Y` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the input.

### `ResXPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to place the returned value of the X-coordinate. One of the following can be written to the address.

*For specifying the potential return values of the X-coordinate*

| Value | Description |
| --- | --- |
| `M_INVALID_POINT` | Specifies a non-valid coordinate. |
| `Value` | Specifies the transformed X-coordinate as a value. |

### `ResYPtr` *(out, *AIL_DOUBLE)*

Specifies the addres of the variable in which to place the returned value of the Y-coordinate. One of the following can be written to the address.

*For specifying the potential return values of the Y-coordinate*

| Value | Description |
| --- | --- |
| `M_INVALID_POINT` | Specifies a non-valid coordinate. |
| `Value` | Specifies the transformed Y-coordinate as a value. |
