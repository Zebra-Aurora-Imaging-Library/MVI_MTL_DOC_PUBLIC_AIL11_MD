---
doctype: Reference
module: cal
function: McalTransformResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalTransformResult"
---

# McalTransformResult

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

> Convert a result between its world and pixel value.

## Syntax

```c
void McalTransformResult(
    AIL_ID       CalibrationOrImageId,  //in
    AIL_INT64    TransformType,         //in
    AIL_INT64    ResultType,            //in
    AIL_DOUBLE   Result,                //in
    AIL_DOUBLE * TransformedResultPtr   //out
)
```

## Description

This function converts a specific result (a length, area, or angle) from its pixel value to its world value or vice versa. The conversion can be performed according to a camera calibration context, calibrated image, or corrected image. However, since this function uses the average pixel size to perform the conversion, results will be more accurate if you use a corrected image.

## Parameters

### `CalibrationOrImageId` *(in, AIL_ID)*

Specifies the identifier of the camera calibration context, calibrated image, or corrected image. When an image is specified, the transformation uses the camera calibration information associated with this image.

### `TransformType` *(in, AIL_INT64)*

Specifies whether to perform a pixel-to-world or world-to-pixel conversion. This parameter must be set to one of the following values:

*For specifying pixel-to-world or world-to-pixel*

| Value | Description |
| --- | --- |
| `M_PIXEL_TO_WORLD` | Converts from pixel to world. |
| `M_WORLD_TO_PIXEL` | Converts from world to pixel. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result the given input value represents. This parameter must be set to one of the following values:

*For specifying the type of result*

| Value | Description |
| --- | --- |
| `M_ANGLE` | Represents an angle.

An angle interpreted with respect to the pixel coordinate system ([`M_PIXEL_TO_WORLD`](../../Reference/cal/McalTransformResult.md)) is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system ([`M_WORLD_TO_PIXEL`](../../Reference/cal/McalTransformResult.md)), see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md). |
| `M_AREA` | Represents an area. |
| `M_LENGTH` | Represents a length (for example, the perimeter of an object). |
| `M_LENGTH_X` | Represents a length in the X-direction only. |
| `M_LENGTH_Y` | Represents a length in the Y-direction only. |

### `Result` *(in, AIL_DOUBLE)*

Specifies the input value.

### `TransformedResultPtr` *(out, *AIL_DOUBLE)*

Specifies the address in which to place the output value.
