---
doctype: Reference
module: cal
function: McalUniform
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / cal / McalUniform"
---

# McalUniform

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

> Calibrate an image or a camera calibration context by specifying the translation, scale and offset of the absolute world coordinate system.

## Syntax

```c
void McalUniform(
    AIL_ID     CalibrationOrImageId,  //out
    AIL_DOUBLE WorldPosX,             //in
    AIL_DOUBLE WorldPosY,             //in
    AIL_DOUBLE PixelSizeX,            //in
    AIL_DOUBLE PixelSizeY,            //in
    AIL_DOUBLE PixelRotation,         //in
    AIL_INT64  ControlFlag            //in
)
```

## Description

This function allows you to calibrate an image or a camera calibration context with a uniform camera calibration, by specifying the translation, rotation, and scale of the transformation between pixel and world units. You do not need to allocate a camera calibration context or supply an image of a grid or a list as with the other camera calibration modes. If you choose to apply [`McalUniform`](../../Reference/cal/McalUniform.md) to a camera calibration context, however, you must allocate the camera calibration context using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_UNIFORM_TRANSFORMATION`](../../Reference/cal/McalAlloc.md). If you apply [`McalUniform`](../../Reference/cal/McalUniform.md) to an image, the associated camera calibration context will be [`M_DEFAULT_UNIFORM_CALIBRATION`](../../Reference/cal/McalInquire.md).

The origin of the relative coordinate system and of the tool coordinate system will be set at (0,0) in the absolute coordinate system. For more information on coordinate systems, see [Coordinate systems](../../UserGuide/C28_Calibration/S04_Coordinate_systems.md).

The following linear transformations are used to relate the pixel coordinate system, in pixels, to the absolute coordinate system, in world units:

*[Image: uniform_p_to_w.png]*

*[Image: uniform_w_to_p.png]*

where:

- _R_ = [`PixelRotation`](../../Reference/cal/McalUniform.md).
- _S<sub>x</sub> _ = [`PixelSizeX`](../../Reference/cal/McalUniform.md).
- _S<sub>y</sub> _ = [`PixelSizeY`](../../Reference/cal/McalUniform.md).
- _T<sub>x</sub> _ = [`WorldPosX`](../../Reference/cal/McalUniform.md).
- _T<sub>y</sub> _ = [`WorldPosY`](../../Reference/cal/McalUniform.md).

If the [`PixelRotation`](../../Reference/cal/McalUniform.md) parameter is set to 0 and the same pixel size is specified in X and Y, the image will be considered corrected.

For more information on uniform camera calibration, see [Camera calibration modes](../../UserGuide/C28_Calibration/S05_Calibration_modes_overview.md).

If you adjust the coordinate system of a calibrated image associated with an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, the raster information will be discarded, causing the ROI to become an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI. See [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) for more information.

## Parameters

### `CalibrationOrImageId` *(out, AIL_ID)*

Specifies the identifier of the camera calibration context or of the image to calibrate.

### `WorldPosX` *(in, AIL_DOUBLE)*

Specifies the X-position in the absolute coordinate system of the center of the top-left pixel of the image.

### `WorldPosY` *(in, AIL_DOUBLE)*

Specifies the Y-position in the absolute coordinate system of the center of the top-left pixel of the image.

### `PixelSizeX` *(in, AIL_DOUBLE)*

Specifies the scale between the world and pixel units in the X-direction. Specify the scale in world units per pixel. The scale must be a positive value.

### `PixelSizeY` *(in, AIL_DOUBLE)*

Specifies the scale between the world and pixel units in the Y-direction. Specify the scale in world units per pixel. The scale must be a positive value.

### `PixelRotation` *(in, AIL_DOUBLE)*

Specifies the angle of the X-axis of the pixel coordinate system measured in the absolute coordinate system, in degrees. A positive value indicates a counter-clockwise rotation (from the positive X-axis toward the negative Y-axis).

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
