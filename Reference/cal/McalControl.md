---
doctype: Reference
module: cal
function: McalControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalControl"
---

# McalControl

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

> Control the camera calibration information of a camera calibration context, calibrated image, or 3D draw calibration context.

## Syntax

```c
void McalControl(
    AIL_ID     ContextCalOrCalibratedAilObjectId,  //out
    AIL_INT64  ControlType,                        //in
    AIL_DOUBLE ControlValue                        //in
)
```

## Description

This function allows you to control a specified setting of a camera calibration context, calibrated image, or 3D draw calibration context.

If you control a camera calibration setting of a calibrated image with an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, the raster information will be discarded, causing the ROI to become an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI. See [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) for more information.

## Parameters

### `ContextCalOrCalibratedAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the camera calibration context, calibrated image, or 3D draw calibration context.

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For a camera calibration context

The following [`ControlType`](../../Reference/cal/McalControl.md) and [`ControlValue`](../../Reference/cal/McalControl.md) parameter settings can be specified for a camera calibration context:

---

### `M_CALIBRATION_PLANE`

Sets the plane in which the calibration points are defined.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.  For most calibration modes, the default value is [`M_ABSOLUTE_COORDINATE_SYSTEM`](../../Reference/cal/McalControl.md). For an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode, the default is [`M_RELATIVE_COORDINATE_SYSTEM`](../../Reference/cal/McalControl.md). |
| `M_ABSOLUTE_COORDINATE_SYSTEM` | Specifies that the calibration points are defined in the absolute coordinate system. |
| `M_RELATIVE_COORDINATE_SYSTEM` | Specifies that the calibration points are defined in the relative coordinate system. |

---

### `M_DRAW_CALIBRATION_ERROR_SCALE_FACTOR`

Sets the scale factor used when drawing arrows, representing the camera calibration error, using [`McalDraw`](../../Reference/cal/McalDraw.md) with [`M_DRAW_CALIBRATION_ERROR`](../../Reference/cal/McalDraw.md). This scale factor is either applied directly to the arrows, or applied in conjunction with Aurora Imaging Library's automatic scaling factor, depending on whether[`M_DRAW_CALIBRATION_ERROR_SCALE_MODE`](../../Reference/cal/McalControl.md) is [`M_ABSOLUTE`](../../Reference/cal/McalControl.md) or [`M_AUTO`](../../Reference/cal/McalControl.md).

| Value | Description |
| --- | --- |
| `Value > 0` *(default)* | Specifies the scale factor. |

---

### `M_DRAW_CALIBRATION_ERROR_SCALE_MODE`

Sets the scale mode used when drawing arrows, representing the camera calibration error, using [`McalDraw`](../../Reference/cal/McalDraw.md) with [`M_DRAW_CALIBRATION_ERROR`](../../Reference/cal/McalDraw.md).

| Value | Description |
| --- | --- |
| `M_ABSOLUTE` *(default)* | Specifies to use [`M_DRAW_CALIBRATION_ERROR_SCALE_FACTOR`](../../Reference/cal/McalControl.md) as a scaling factor that directly multiplies the arrows' horizontal and vertical displacements. |
| `M_AUTO` | Specifies that Aurora Imaging Library determines the scale mode. To do this, Aurora Imaging Library makes internal assumptions to establish an ideal scaling factor that optimizes the space taken by the arrows in the destination ([`DstImageBufOrListGraId`](../../Reference/cal/McalDraw.md)) while reducing the chance of overlapping arrows. When using [`M_AUTO`](../../Reference/gra/MgraVectors.md), Aurora Imaging Library multiplies the arrows' horizontal and vertical displacements by both [`M_DRAW_CALIBRATION_ERROR_SCALE_FACTOR`](../../Reference/cal/McalControl.md) and the internally established scale factor. |

---

### `M_FOREGROUND_VALUE`

Sets whether the circles in a circle grid, used with [`McalGrid`](../../Reference/cal/McalGrid.md), are lighter or darker than the background.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Determines the appropriate setting automatically. |
| `M_FOREGROUND_BLACK` | Specifies that the grid's circles are darker than the background. |
| `M_FOREGROUND_WHITE` | Specifies that the grid's circles are lighter than the background. |

---

### `M_GRID_FIDUCIAL`

Specifies that [`McalGrid`](../../Reference/cal/McalGrid.md) will look for a fiducial in the grid.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DATAMATRIX` | Specifies that a Data Matrix code is used as a fiducial in a chessboard grid.  This control type is only used for camera calibrations performed using [`McalGrid`](../../Reference/cal/McalGrid.md) set to [`M_CHESSBOARD_GRID`](../../Reference/cal/McalGrid.md).  You must set[`McalControl`](../../Reference/cal/McalControl.md) with [`M_GRID_PARTIAL`](../../Reference/cal/McalControl.md) to [`M_ENABLE`](../../Reference/cal/McalControl.md) and [`M_GRID_HINT_ANGLE_X`](../../Reference/cal/McalControl.md), [`M_GRID_HINT_PIXEL_X`](../../Reference/cal/McalControl.md), and [`M_GRID_HINT_PIXEL_Y`](../../Reference/cal/McalControl.md) to [`M_NONE`](../../Reference/cal/McalControl.md). Setting [`M_GRID_FIDUCIAL`](../../Reference/cal/McalControl.md) to [`M_DATAMATRIX`](../../Reference/cal/McalControl.md) in any other circumstance will generate an error when calling[`McalGrid`](../../Reference/cal/McalGrid.md). |
| `M_NONE` *(default)* | Specifies that there is no fiducial in the grid. |

---

### `M_GRID_HINT_ANGLE_X`

Specifies the hint angle used to establish the direction of the X-axis of the absolute (or relative) coordinate system, when calibrating with a partial chessboard grid.  To determine the X-axis of the absolute coordinate system when using a partial grid, [`McalGrid`](../../Reference/cal/McalGrid.md) first determines the two grid lines intersecting at the reference calibration point. The X- and Y-axes are always aligned with these grid lines. By default, the X-axis of the absolute coordinate system is the grid line intersecting the reference calibration point and closest to the image's X-axis, with the positive X-axis pointing right. To specify the other grid line and/or direction for the positive X-axis, you must specify a hint angle. When you specify a hint angle, the positive X-axis will be the grid line intersecting the reference calibration point and closest in angle to the hint angle.  This control type is only used for camera calibrations performed using [`McalGrid`](../../Reference/cal/McalGrid.md) set to [`M_CHESSBOARD_GRID`](../../Reference/cal/McalGrid.md), and [`McalControl`](../../Reference/cal/McalControl.md) with [`M_GRID_PARTIAL`](../../Reference/cal/McalControl.md) set to [`M_ENABLE`](../../Reference/cal/McalControl.md). Setting this control type in any other circumstance will generate an error when calling [`McalGrid`](../../Reference/cal/McalGrid.md).

| Value | Description |
| --- | --- |
| `M_NONE` *(default)* | Specifies that no hint angle is used. The positive X-axis is the right-pointing grid line closest to the horizontal. |
| `Value` | Specifies the hint angle, measured counter-clockwise. |

---

### `M_GRID_HINT_PIXEL_X`

Specifies the X-coordinate of the hint pixel. The hint pixel is used to help [`McalGrid`](../../Reference/cal/McalGrid.md) determine the location of the grid's reference calibration point. Specify an X- and Y-coordinate for the hint pixel that is close to the grid's reference calibration point in the image.  For complete grids, the corner calibration point closest to the origin of the pixel coordinate system (top-left corner of the image) is the default reference calibration point. If your grid's reference calibration point is not near the origin of the pixel coordinate system (for instance, if the grid is rotated in the image), you must specify a hint pixel. For complete grids, the corner calibration point closest to the hint pixel will be the grid's reference calibration point.  For complete grids, given that the number of rows and columns are specified, there can be either 2 valid corners (for a rectangle grid) or 4 corners (for a square grid), so the approximate X-coordinate does not have to be very precise.  For partial chessboard grids ([`M_GRID_PARTIAL`](../../Reference/cal/McalControl.md) set to [`M_ENABLE`](../../Reference/cal/McalControl.md)), the calibration point closest to the center of the image is the default reference calibration point. If you need your grid's reference calibration point in another place, you must specify a hint pixel; the calibration point closest to the hint pixel will be the grid's reference calibration point.  This control type is only used for camera calibrations performed using [`McalGrid`](../../Reference/cal/McalGrid.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies not to use a hint pixel.  When using a complete grid, the calibration point closest to the top-left corner of the image is used as the grid's reference calibration point.  When using a partial grid, the calibration point closest to the center of the image is used as the grid's reference calibration point.  > **Note:** Note that when you set [`M_GRID_HINT_PIXEL_X`](../../Reference/cal/McalControl.md) to [`M_NONE`](../../Reference/cal/McalControl.md), you must also set [`M_GRID_HINT_PIXEL_Y`](../../Reference/cal/McalControl.md) to [`M_NONE`](../../Reference/cal/McalControl.md). |
| `Value` | Specifies the X-coordinate of the hint pixel, in the pixel coordinate system. |

---

### `M_GRID_HINT_PIXEL_Y`

Specifies the Y-coordinate of the hint pixel. The hint pixel is used to help [`McalGrid`](../../Reference/cal/McalGrid.md) determine the location of the grid's reference calibration point. Specify an X- and Y-coordinate for the hint pixel that is close to the grid's reference calibration point in the image.  For complete grids, the corner calibration point closest to the origin of the pixel coordinate system (top-left corner of the image) is the default reference calibration point. If your grid's reference calibration point is not near the origin of the pixel coordinate system (for instance, if the grid is rotated in the image), you must specify a hint pixel. For complete grids, the corner calibration point closest to the hint pixel will be the grid's reference calibration point.  For complete grids, given that the number of rows and columns are specified, there can be either 2 valid corners (for a rectangle grid) or 4 corners (for a square grid), so the approximate Y-coordinate does not have to be very precise.  For partial chessboard grids ([`M_GRID_PARTIAL`](../../Reference/cal/McalControl.md) set to [`M_ENABLE`](../../Reference/cal/McalControl.md)), the calibration point closest to the center of the image is the default reference calibration point. If you need your grid's reference calibration point in another place, you must specify a hint pixel; the calibration point closest to the hint pixel will be the grid's reference calibration point.  This control type is only used for camera calibrations performed using [`McalGrid`](../../Reference/cal/McalGrid.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies not to use a hint pixel.  When using a complete grid, the calibration point closest to the top-left corner of the image is used as the grid's reference calibration point.  When using a partial grid, the calibration point closest to the center of the image is used as the grid's reference calibration point.  > **Note:** Note that when you set [`M_GRID_HINT_PIXEL_Y`](../../Reference/cal/McalControl.md) to [`M_NONE`](../../Reference/cal/McalControl.md), you must also set [`M_GRID_HINT_PIXEL_X`](../../Reference/cal/McalControl.md) to [`M_NONE`](../../Reference/cal/McalControl.md). |
| `Value` | Specifies the Y-coordinate of the hint pixel, in the pixel coordinate system. |

---

### `M_GRID_PARTIAL`

Specifies whether [`McalGrid`](../../Reference/cal/McalGrid.md) can assume that the chessboard grid is complete. A complete grid is one the has the exact number of rows and columns specified when calling [`McalGrid`](../../Reference/cal/McalGrid.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that [`McalGrid`](../../Reference/cal/McalGrid.md) will only calibrate the camera setup when a complete grid is found in the image.  For complete grids, the grid's default reference calibration point is the calibration point at the corner of the grid, closest to the top-left corner of the image. To specify the calibration point at another corner of the grid, for instance to indicate that the physical grid is rotated, you must specify a hint pixel using [`M_GRID_HINT_PIXEL_X`](../../Reference/cal/McalControl.md) and [`M_GRID_HINT_PIXEL_Y`](../../Reference/cal/McalControl.md). |
| `M_ENABLE` | Specifies that [`McalGrid`](../../Reference/cal/McalGrid.md) can calibrate the camera setup when a partial grid is found in the image.  When you enable partial grids, the calibration point closest to the center of the image is the grid's default reference calibration point. You can specify an alternative reference calibration point by specifying a hint pixel using [`M_GRID_HINT_PIXEL_X`](../../Reference/cal/McalControl.md) and [`M_GRID_HINT_PIXEL_Y`](../../Reference/cal/McalControl.md).  When you enable partial grids, you can specify how to handle potential outlying false calibration points using [`M_GRID_SHAPE`](../../Reference/cal/McalControl.md).  When enabling partial grids, you must specify that the grid is an [`M_CHESSBOARD_GRID`](../../Reference/cal/McalGrid.md) type grid when calling [`McalGrid`](../../Reference/cal/McalGrid.md), which includes both chessboard grids and fiducial grids. Enabling partial grids with an [`M_CIRCLE_GRID`](../../Reference/cal/McalGrid.md) type grid will result in an error. |

---

### `M_GRID_SHAPE`

Specifies how to handle potential outlying calibration points, using assumptions about the grid's shape in the image.  When [`M_GRID_PARTIAL`](../../Reference/cal/McalControl.md) is set to [`M_DISABLE`](../../Reference/cal/McalControl.md), you must set [`M_GRID_SHAPE`](../../Reference/cal/McalControl.md) to [`M_RECTANGLE`](../../Reference/cal/McalControl.md).  [`McalGrid`](../../Reference/cal/McalGrid.md) will try to find the outside edges of a partial chessboard grid if it is assumed to be rectangular. This affects whether potential calibration points are included in the camera calibration or are excluded as outliers.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` | Specifies to include all potential calibration points in the image; [`McalGrid`](../../Reference/cal/McalGrid.md) will not look for the boundary of the real-world grid. In this case, the shape of the partial grid is not bound.  Set this value when the partial grid in the image is not a rectangle. This can be either when the real-world grid is a rectangle, but is occluded by an object in the image, as in the image on the left, or when the real-world grid is not a rectangle, as in the image on the right.  *[Image: cal_grid_shape_any.png]*  With no fixed shape, no potential calibration point can be excluded as an outlier because it is not clear where the boundary of the partial grid is. This could lead to outlying false calibration points being included in the camera calibration, which would reduce the calibration's precision. In this case, it is highly recommended to check all calibration points extracted from the image using [`McalGrid`](../../Reference/cal/McalGrid.md)are valid, using [`McalDraw`](../../Reference/cal/McalDraw.md) with [`M_DRAW_IMAGE_POINTS`](../../Reference/cal/McalDraw.md). |
| `M_RECTANGLE` *(default)* | Specifies to exclude potential calibration points in the image that [`McalGrid`](../../Reference/cal/McalGrid.md) determines are outside the boundaries of the partial grid. In this case, the shape of the partial grid is a rectangle.  Set this value if the real-world grid is rectangular and the image of the partial grid is not occluded by any objects in the image. Note that a partial grid is still considered rectangular if the boundary of the partial grid is obscured by the end of the image.  *[Image: cal_grid_shape_rectangle.png]*  In this case, [`McalGrid`](../../Reference/cal/McalGrid.md) will look for the boundaries of the partial grid in the image. This allows the possibility of excluding false calibration points outside of the grid. |

---

### `M_LINK_TOOL_AND_HEAD`

Sets a rigid link between the head (camera or relative) coordinate system and the tool coordinate system.  When a rigid link exists between the tool and the head, moving one automatically moves the other accordingly. However, this link can be broken using [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md) set to [`M_DISABLE`](../../Reference/cal/McalControl.md). You can then set the tool coordinate system to a known location without affecting the head coordinate system. After moving the tool coordinate system, you can then re-establish the link so that you move the head coordinate system by moving the tool coordinate system.  > **Note:** Note that for [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md), the head represents a camera or a reference/calibration object depending on the type of robotic setup. When the robotic setup is [`M_MOVING_CAMERA`](../../Reference/cal/McalAlloc.md), the head refers to a camera attached to the last joint of the robotic arm. When the robotic setup is [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md), the head refers to a reference/calibration object attached to the last joint of the robotic arm.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_DISABLE` | Specifies to remove the link between the two coordinate systems, allowing both to be moved independently. |
| `M_ENABLE` *(default)* | Specifies to link the two coordinate systems, allowing both to be moved together. |

---

### `M_LOCALIZATION_NB_ITERATIONS_MAX`

Specifies the maximum number of iterations to attempt to fit the provided points when calculating the new position of the camera or relative coordinate system, when using [`McalList`](../../Reference/cal/McalList.md) and [`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md) or [`M_DISPLACE_RELATIVE_COORD`](../../Reference/cal/McalGrid.md) respectively. If the specified number of iterations is large, the computation could take longer to run, but the algorithm will be more robust. If [`M_LOCALIZATION_NB_OUTLIERS_MAX`](../../Reference/cal/McalControl.md) is set to 0, only 1 iteration is required and no more will be performed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the number of expected outliers. Only integer values are accepted. |

---

### `M_LOCALIZATION_NB_OUTLIERS_MAX`

Specifies the maximum number of possible outliers that can occur in the dataset used by [`McalList`](../../Reference/cal/McalList.md) or [`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md) or [`M_DISPLACE_RELATIVE_COORD`](../../Reference/cal/McalGrid.md). If the specified number of outliers is large, more iterations are required to determine which points provide the best solution; therefore, computation can take longer, but the function will be more robust to outliers. Also, the function could fail if the value of [`M_LOCALIZATION_NB_ITERATIONS_MAX`](../../Reference/cal/McalControl.md) is too low.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of expected outliers. Only integer values are accepted. |

---

### `M_TOOL_POSITION_X`

Sets the X-position of the tool coordinate system in the absolute coordinate system. The X-position is initialized to 0.0.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate, in world units. |

---

### `M_TOOL_POSITION_Y`

Sets the Y-position of the tool coordinate system in the absolute coordinate system. The Y-position is initialized to 0.0.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate, in world units. |

---

### `M_TOOL_POSITION_Z`

Sets the Z-position of the tool coordinate system in the absolute coordinate system. The Z-position is initialized to 0.0.  > **Note:** When using the linear interpolation or perspective transformation camera calibration mode, [`M_TOOL_POSITION_Z`](../../Reference/cal/McalControl.md) must be set to [`M_DEFAULT`](../../Reference/cal/McalControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Z-coordinate, in world units. |

---

### `M_TOOL_ROTATION_Z`

Sets the angle at which to rotate the tool coordinate system about its Z-axis.  A positive angle rotates the tool coordinate system in the counter-clockwise direction (from the positive X-axis to the negative Y-axis of the tool coordinate system). The rotation angle is initialized to 0.0.  *[Image: tool_rotation.png]*  > **Note:** Note that this control is only available for 2D-based camera calibration modes ([`M_UNIFORM_TRANSFORMATION`](../../Reference/cal/McalAlloc.md),[`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md), or [`M_PERSPECTIVE_TRANSFORMATION`](../../Reference/cal/McalAlloc.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the angle, in degrees. |

---

### `M_TRANSFORM_CACHE`

Sets whether to use a cache to accelerate the [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) function.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to use a cache. Not using a cache saves memory but slows down subsequent calls to [`McalTransformImage`](../../Reference/cal/McalTransformImage.md). |
| `M_ENABLE` *(default)* | Specifies to use a cache. |

### For a 3D-based camera calibration context

The following [`ControlType`](../../Reference/cal/McalControl.md) and [`ControlValue`](../../Reference/cal/McalControl.md) parameter settings can be specified only for a 3D-based camera calibration context ( [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)):

---

### `M_CCD_ASPECT_RATIO`

Sets the width-to-height ratio of the individual elements of the CCD. If this value is not set and is required by the camera calibration mode, the ratio will default to 1.0.

| Value | Description |
| --- | --- |
| `1.0` *(default)* | Specifies that the width and height of the CCD element are equal. |
| `Value > 0.0` | Specifies the value of the width of a CCD element divided by its height. |

---

### `M_DISTORTION_RADIAL_1`

Sets the value of the second order radial distortion coefficient used in the camera calibration algorithm. Radial distortion refers to image distortions caused by the camera's lens.  > **Note:** Use this to override the value determined by the previous call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) if you want to use calibration values from another source.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the value of the second order radial distortion coefficient. |

---

### `M_DISTORTION_RADIAL_2`

Sets the value of the fourth order radial distortion coefficient used in the camera calibration algorithm. Radial distortion refers to image distortions caused by the camera's lens. This radial distortion coefficient is only supported for [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration modes.  > **Note:** Use this to override the value determined by the previous call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) if you want to use calibration values from another source.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the value of the fourth order radial distortion coefficient. |

---

### `M_FOCAL_LENGTH`

Sets the effective focal length of the pinhole camera model used in the camera calibration.  > **Note:** Use this to override the value determined by the previous call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) if you want to use calibration values from another source.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the effective focal length of the pinhole camera model, expressed in horizontal pixels. |

---

### `M_PRINCIPAL_POINT_X`

Sets the X-coordinate of the intersection of the camera's optical axis and the image plane.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies half of the image's width, in pixels. |
| `Value` | Specifies the X-coordinate, in pixels. This only has an effect if [`M_PRINCIPAL_POINT_Y`](../../Reference/cal/McalControl.md) is not set to [`M_DEFAULT`](../../Reference/cal/McalControl.md). |

---

### `M_PRINCIPAL_POINT_Y`

Sets the Y-coordinate of the intersection of the camera's optical axis and the image plane.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies half of the image's height, in pixels. |
| `Value` | Specifies the Y-coordinate, in pixels. This only has an effect if [`M_PRINCIPAL_POINT_X`](../../Reference/cal/McalControl.md) is not set to [`M_DEFAULT`](../../Reference/cal/McalControl.md). |

### For a calibrated image

The following [`ControlType`](../../Reference/cal/McalControl.md) and [`ControlValue`](../../Reference/cal/McalControl.md) parameter settings can be specified for a calibrated image:

---

### `M_CALIBRATION_CHILD_OFFSET_X`

Sets the X-offset of a child buffer, relative to its highest order calibrated parent buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset, relative to the child buffer's highest order calibrated parent buffer. |

---

### `M_CALIBRATION_CHILD_OFFSET_Y`

Sets the Y-offset of a child buffer, relative to its highest order calibrated parent buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset, relative to the child buffer's highest order calibrated parent buffer. |

### For a calibrated and corrected image

The following [`ControlType`](../../Reference/cal/McalControl.md) and [`ControlValue`](../../Reference/cal/McalControl.md) parameter settings can be specified for a calibrated image:

---

### `M_GRAY_LEVEL_SIZE_Z`

Sets the difference in height corresponding to a difference of one gray level in a fully-corrected depth map.

| Value | Description |
| --- | --- |
| `M_INVALID_SCALE` *(default)* | Specifies that the image is not a depth map. |
| `Value != 0.0` | Specifies the height, in world units, corresponding to a difference of one gray level. |

---

### `M_WORLD_POS_Z`

Sets the real-world Z-offset of a gray level of 0 in the corrected image, useful in depth maps.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the base height of a gray level of 0. |

### For a 3D draw calibration context

The following [`ControlType`](../../Reference/cal/McalControl.md) and [`ControlValue`](../../Reference/cal/McalControl.md) parameter settings can be specified for a 3D draw calibration context:

---

### `M_DRAW_ABSOLUTE_COORDINATE_SYSTEM`

Sets whether to draw the absolute coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the absolute coordinate system's axes. |
| `M_ENABLE` *(default)* | Specifies to draw the absolute coordinate system's axes. |

---

### `M_DRAW_CAMERA_COORDINATE_SYSTEM`

Sets whether to draw the camera coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the camera coordinate system's axes. |
| `M_ENABLE` *(default)* | Specifies to draw the camera coordinate system's axes. |

---

### `M_DRAW_CAMERA_COORDINATE_SYSTEM_NAME`

Sets the name to draw for the camera coordinate system; the initial value is "Camera".

| Value | Description |
| --- | --- |
| `"String"` | Specifies the name of the camera coordinate system. |

---

### `M_DRAW_COORDINATE_SYSTEM_LENGTH`

Sets the length at which to draw the specified coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the length (in world units) at which to draw the axes of the specified coordinate system. |

---

### `M_DRAW_FRUSTUM`

Sets whether to draw the frustum of the camera's view. The frustum is the truncated pyramid of vision that originates at the camera's position and, when drawn, ends at the relative XY plane. When enabled, [`M_DRAW_FRUSTUM`](../../Reference/cal/McalControl.md) draws a wireframe frustum, using the color specified with [`M_DRAW_FRUSTUM_COLOR`](../../Reference/cal/McalControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the frustum. |
| `M_ENABLE` *(default)* | Specifies to draw the frustum. |

---

### `M_DRAW_FRUSTUM_COLOR`

Sets the frustum's color.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies an RGB value to draw in an 8-bit, 3-band buffer. The red, green, and blue values must be values between 0 and 255, inclusive. |
| `M_COLOR_YELLOW` *(default)* | Specifies the color yellow. |
| `M_NO_COLOR` | Specifies no color. |

---

### `M_DRAW_RELATIVE_COORDINATE_SYSTEM`

Sets whether to draw the relative coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the relative coordinate system's axes. |
| `M_ENABLE` *(default)* | Specifies to draw the relative coordinate system's axes. |

---

### `M_DRAW_RELATIVE_COORDINATE_SYSTEM_NAME`

Sets the name to draw for the relative coordinate system; the initial value is "Relative".

| Value | Description |
| --- | --- |
| `"String"` | Specifies the name of the relative coordinate system. |

---

### `M_DRAW_RELATIVE_XY_PLANE`

Sets whether to draw the relative XY plane.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the relative XY plane. |
| `M_ENABLE` *(default)* | Specifies to draw the relative XY plane. |

---

### `M_DRAW_RELATIVE_XY_PLANE_COLOR_FILL`

Sets the relative XY plane's fill color.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies an RGB value to draw in an 8-bit, 3-band buffer. The red, green, and blue values must be values between 0 and 255, inclusive. |
| `M_AUTO_COLOR` *(default)* | Specifies either the color white or the texture image.  If a texture image is specified (using [`McalDraw3d`](../../Reference/cal/McalDraw3d.md) with [`RelXYPlaneTextureImageBufId`](../../Reference/cal/McalDraw3d.md)), the texture image is drawn on the laser plane. Otherwise, the plane is drawn with [`M_COLOR_WHITE`](../../Reference/cal/McalControl.md). |
| `M_NO_COLOR` | Specifies no color. |
| `M_TEXTURE_IMAGE` | Specifies to use the image passed to [`McalDraw3d`](../../Reference/cal/McalDraw3d.md) with [`RelXYPlaneTextureImageBufId`](../../Reference/cal/McalDraw3d.md), when drawing the relative XY plane. The texture image is typically a 2D image of the 3D scene. For example, you can specify the image used for calibration. If no texture image is specified using [`McalDraw3d`](../../Reference/cal/McalDraw3d.md), setting [`M_DRAW_RELATIVE_XY_PLANE_COLOR_FILL`](../../Reference/cal/McalControl.md) to [`M_TEXTURE_IMAGE`](../../Reference/cal/McalControl.md) will cause an error. |

---

### `M_DRAW_RELATIVE_XY_PLANE_COLOR_OUTLINE`

Sets the relative XY plane's outline color.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies an RGB value to draw in an 8-bit, 3-band buffer. The red, green, and blue values must be values between 0 and 255, inclusive. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |
| `M_NO_COLOR` | Specifies no color. |

---

### `M_DRAW_RELATIVE_XY_PLANE_OPACITY`

Sets the relative XY plane's opacity.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the relative XY plane's opacity. |

---

### `M_DRAW_ROBOT_BASE_COORDINATE_SYSTEM`

Sets whether to draw the robot base coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the robot base coordinate system's axes. |
| `M_ENABLE` *(default)* | Specifies to draw the robot base coordinate system's axes. |

---

### `M_DRAW_TOOL_COORDINATE_SYSTEM`

Sets whether to draw the tool coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to draw the tool coordinate system's axes. |
| `M_ENABLE` | Specifies to draw the tool coordinate system's axes. |
