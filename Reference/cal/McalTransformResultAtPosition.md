---
doctype: Reference
module: cal
function: McalTransformResultAtPosition
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / cal / McalTransformResultAtPosition"
---

# McalTransformResultAtPosition

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

> Convert a result, , between its world and pixel value.

## Syntax

```c
void McalTransformResultAtPosition(
    AIL_ID       CalibrationOrImageId,  //in
    AIL_INT64    TransformType,         //in
    AIL_INT64    ResultType,            //in
    AIL_DOUBLE   PositionX,             //in
    AIL_DOUBLE   PositionY,             //in
    AIL_DOUBLE   Result,                //in
    AIL_DOUBLE * ConvertedResultPtr     //out
)
```

## Description

This function converts  from its pixel value to its world value or vice versa. This conversion is done locally at the specified position, and can be performed according to a camera calibration context, calibrated image, or corrected image. This function is useful when in the presence of distortion and the result is meaningless when converted from real-world to pixel units . For example, if a Model Finder model appears warped in a target image, but the camera calibration context of the image compensates for this during the model search, the resulting angle is meaningful in the real-world coordinate system, and meaningless in the pixel coordinate system unless converted at a specific position in the target image.

The position should be given in the units of the source coordinate system. For example, if converting from pixel units to world units, specify the position in pixel units.

If the position is from a corrected image ([`McalTransformImage`](../../Reference/cal/McalTransformImage.md)), you must specify the identifier of the image, rather than its camera calibration context, to get the .

> **Note:** Note that, if the relative plane used to return results is set behind the camera, the return is undefined unless [`M_NO_POINTS_BEHIND_CAMERA`](../../Reference/cal/McalTransformResultAtPosition.md) is used.

## Parameters

### `CalibrationOrImageId` *(in, AIL_ID)*

Specifies the identifier of the camera calibration context, calibrated image, or corrected image. When an image is specified, the transformation uses the camera calibration information associated with this image.

### `TransformType` *(in, AIL_INT64)*

Specifies whether to perform a pixel-to-world or world-to-pixel conversion. This parameter must be set to one of the following values:

*For specifying pixel-to-world or world-to-pixel*

| Value | Description |
| --- | --- |
| `M_PIXEL_TO_WORLD` | Converts from pixel to world units. |
| `M_WORLD_TO_PIXEL` | Converts from world to pixel units. |

*For specifying to return invalid points*

| Value | Description |
| --- | --- |
| `M_NO_EXTRAPOLATED_POINTS` | Specifies that if a pixel involved in the transformation is not inside the calibrated region, **M_INVALID_POINT**will be returned, instead of a coordinate resulting from the extrapolation. The calibrated region is defined as the image region covered by the camera calibration grid. To display this region, call [`McalDraw`](../../Reference/cal/McalDraw.md) with [`M_DRAW_VALID_REGION`](../../Reference/cal/McalDraw.md).

> **Note:** This value only applies to piecewise linear camera calibrations ([`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md)); if it is set and the image has any other type of camera calibration, it is ignored. |
| `M_NO_POINTS_BEHIND_CAMERA` | Returns **M_INVALID_POINT** instead of an undefined value. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to convert. This parameter must be set to the following value:

*For specifying the type of result*

| Value | Description |
| --- | --- |
| `M_ANGLE` | Specified that result to convert is an angle. Note that angles are always returned in the range of 0 to 360 degrees. |

### `PositionX` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the position at which to convert the result, in the units of the source coordinate system.

### `PositionY` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the position at which to convert the result, in the units of the source coordinate system.

### `Result` *(in, AIL_DOUBLE)*

Specifies the result to convert at the specified position. This result must be of the type specified using [`ResultType`](../../Reference/cal/McalTransformResultAtPosition.md).

### `ConvertedResultPtr` *(out, *AIL_DOUBLE)*

Specifies the address in which to place the converted result.
