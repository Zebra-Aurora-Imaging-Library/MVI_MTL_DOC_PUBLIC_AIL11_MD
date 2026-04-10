---
doctype: Reference
module: cal
function: McalInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / cal / McalInquire"
---

# McalInquire

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

> Inquire about a camera calibration context, a calibrated image, a calibrated result buffer, a fixturing offset object, a 3D draw calibration context, or about the camera calibration state of an image, digitizer, or result buffer.

## Syntax

```c
AIL_INT McalInquire(
    AIL_ID    ContextCalOrCalibratedAilObjectId,  //in
    AIL_INT64 InquireType,                        //in
    void *    UserVarPtr                          //out
)
```

## Description

This function inquires about a setting of a camera calibration context, a calibrated image, a calibrated result buffer, a fixturing offset object, or a 3D draw calibration context. It can also be used to determine if a camera calibration context is associated with an image, digitizer, or result buffer, and whether or not an image has been corrected.

When working in [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode, the function returns information about the last camera calibration performed on that object. To inquire about previous camera calibration poses within the [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context, you can use [`McalInquireSingle`](../../Reference/cal/McalInquireSingle.md) with the [`Index`](../../Reference/cal/McalInquireSingle.md) parameter set to the required camera calibration pose.

To inquire the default value of a setting, add [`M_DEFAULT`](../../Reference/cal/McalInquire.md) to the [`InquireType`](../../Reference/cal/McalInquire.md) parameter.

## Parameters

### `ContextCalOrCalibratedAilObjectId` *(in, AIL_ID)*

Specifies the identifier of the camera calibration context, image buffer, result buffer, digitizer, fixturing offset object, or 3D draw calibration context.

### `InquireType` *(in, AIL_INT64)*

Specifies the setting about which to inquire. The setting for [`InquireType`](../../Reference/cal/McalInquire.md) depends on whether you are inquiring about a camera calibration context, image, result buffer, or digitizer.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to return the value of the inquired setting. Since the [`McalInquire`](../../Reference/cal/McalInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For a 3D robotics camera calibration context

For a 3D robotics camera calibration context, [`InquireType`](../../Reference/cal/McalInquire.md) can be set to one of the following.

---

### `M_3D_ROBOTICS_CAMERA_MODEL`

Inquires the camera calibration model used by the 3D robotics camera calibration context. To use the Zhang-based camera model in 3D robotics mode use [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md). To use the Tsai-based camera model in 3D robotics mode, use [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md).

| Value | Description |
| --- | --- |
| `M_TSAI_BASED` | Specifies that Tsai-based camera model is used for the 3D robotics camera calibration context. |
| `M_ZHANG_BASED` | Specifies that Zhang-based camera model is used for the 3D robotics camera calibration context. |

---

### `M_3D_ROBOTICS_MODE`

Inquires the camera calibration mode used by the 3D robotics camera calibration context. To use a moving camera setup in 3D robotics mode, use [`M_MOVING_CAMERA`](../../Reference/cal/McalAlloc.md). To use a stationary camera setup in 3D robotics mode, use [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md).

| Value | Description |
| --- | --- |
| `M_MOVING_CAMERA` | Specifies that a moving camera setup is used for the 3D robotics camera calibration context. |
| `M_STATIONARY_CAMERA` | Specifies that a stationary camera setup is used for the 3D robotics camera calibration context. |

### For a camera calibration context

For a camera calibration context, [`InquireType`](../../Reference/cal/McalInquire.md) can be set to one of the following.

---

### `M_CALIBRATION_INPUT_DATA`

Specifies the type of data that was used to perform the camera calibration.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no camera calibration was performed on the camera calibration context. |
| `M_GRID` | Specifies that the camera calibration was performed using a camera calibration grid ([`McalGrid`](../../Reference/cal/McalGrid.md)). |
| `M_LIST` | Specifies that the camera calibration was performed by explicitly specifying the correspondence between some pixels and their real-world coordinates ([`McalList`](../../Reference/cal/McalList.md)). |
| `M_PARAMETRIC` | Specifies that the camera calibration was performed using an explicitly specified translation, scale, and offset from the absolute world coordinate system ([`McalUniform`](../../Reference/cal/McalUniform.md)). |

---

### `M_CALIBRATION_PLANE`

Inquires the plane in which the calibration points are defined.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_ABSOLUTE_COORDINATE_SYSTEM` | Specifies that the calibration points are defined in the absolute coordinate system. |
| `M_RELATIVE_COORDINATE_SYSTEM` | Specifies that the calibration points are defined in the relative coordinate system. |

---

### `M_CALIBRATION_STATUS`

Inquires the status of a camera calibration.

| Value | Description |
| --- | --- |
| `M_CALIBRATED` | Specifies that the camera calibration was successful. |
| `M_CALIBRATING` | Specifies that the last call to [`McalGrid`](../../Reference/cal/McalGrid.md)/[`McalList`](../../Reference/cal/McalList.md) was made with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) and the camera calibration was successful. |
| `M_GRID_NOT_FOUND` | Specifies that [`McalGrid`](../../Reference/cal/McalGrid.md) was unable to find an appropriate camera calibration grid in the provided image. |
| `M_INVALID_CALIBRATION_POINTS` | Specifies that the provided calibration points do not contain sufficient spatial information to perform a camera calibration. |
| `M_MATHEMATICAL_EXCEPTION` | Specifies that the calculation of the camera's parameters has failed. |
| `M_NOT_INITIALIZED` | Specifies that no camera calibration has been performed yet. |
| `M_PLANE_ANGLE_TOO_SMALL` | Specifies that the camera's optical axis is not sufficiently inclined. For full [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) camera calibration, the camera's optical axis should be placed at least at a 30-degrees angle away from the axis perpendicular to the camera calibration plane. |
| `M_TOO_MANY_OUTLIERS` | Specifies that the calculation performed by [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalList.md) or [`M_DISPLACE_RELATIVE_COORD`](../../Reference/cal/McalList.md) has failed. There were too many possible outliers to correctly compute the new location of the coordinate system. |

---

### `M_DRAW_CALIBRATION_ERROR_SCALE_FACTOR`

Inquires the scale factor used when drawing arrows, representing the camera calibration error, using [`McalDraw`](../../Reference/cal/McalDraw.md) with [`M_DRAW_CALIBRATION_ERROR`](../../Reference/cal/McalDraw.md).

| Value | Description |
| --- | --- |
| `Value > 0` *(default)* | Specifies the scale factor. |

---

### `M_DRAW_CALIBRATION_ERROR_SCALE_MODE`

Inquires the scale mode used when drawing arrows, representing the camera calibration error, using [`McalDraw`](../../Reference/cal/McalDraw.md) with [`M_DRAW_CALIBRATION_ERROR`](../../Reference/cal/McalDraw.md).

| Value | Description |
| --- | --- |
| `M_ABSOLUTE` *(default)* | Specifies to use [`M_DRAW_CALIBRATION_ERROR_SCALE_FACTOR`](../../Reference/cal/McalInquire.md) as a scaling factor that directly multiplies the arrows' horizontal and vertical displacements. |
| `M_AUTO` | Specifies that Aurora Imaging Library determines the scale mode. |

---

### `M_LINK_TOOL_AND_HEAD`

Inquires whether a rigid link exists between the head (camera or reference/calibration object) coordinate system and the tool coordinate system.  > **Note:** Note that for [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalInquire.md), the head represents a camera or a reference/calibration object depending on the type of robotic setup. When the robotic setup is [`M_MOVING_CAMERA`](../../Reference/cal/McalAlloc.md), the head refers to a camera attached to the last joint of the robotic arm. When the robotic setup is [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md), the head refers to a reference/calibration object attached to the last joint of the robotic arm.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_DISABLE` | Specifies to remove the link between the two coordinate systems, allowing both to be moved independently. |
| `M_ENABLE` *(default)* | Specifies to link the two coordinate systems, allowing both to be moved together. |

---

### `M_LOCALIZATION_NB_ITERATIONS_MAX`

Inquires the maximum number of iterations to attempt to fit the provided points when calculating the new position of the camera or relative coordinate system, when using [`McalList`](../../Reference/cal/McalList.md) and [`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md) or [`M_DISPLACE_RELATIVE_COORD`](../../Reference/cal/McalGrid.md) respectively.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the number of expected outliers. |

---

### `M_LOCALIZATION_NB_OUTLIERS_MAX`

Inquires the maximum number of possible outliers that can occur in the dataset used by [`McalList`](../../Reference/cal/McalList.md) or [`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md) or [`M_DISPLACE_RELATIVE_COORD`](../../Reference/cal/McalGrid.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of expected outliers. |

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the camera calibration context has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_RELATIVE_ORIGIN_ANGLE`

Inquires the angle of rotation of the relative world coordinate system.  For 2D-based camera calibration contexts, the angle of rotation is of the relative coordinate system about the Z-axis of the absolute coordinate system. A positive angle corresponds to the rotation of the X-axis in the direction of the negative Y-axis.  For 3D-based camera calibration contexts ([`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)), the axis of rotation is not necessarily the Z-axis. The returned value is equivalent to [`M_ROTATION_AXIS_ANGLE`](../../Reference/cal/McalGetCoordinateSystem.md) of [`M_ROTATION_AXIS_ANGLE`](../../Reference/cal/McalGetCoordinateSystem.md) in [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md).  The angle of rotation of the relative coordinate system is affected by transformations performed using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) or [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) with [`AngularOffset`](../../Reference/cal/McalRelativeOrigin.md). If you are using a 3D-based camera calibration context, it is recommended to use [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the angle of rotation, in degrees. |

---

### `M_RELATIVE_ORIGIN_X`

Inquires the X-coordinate of the origin of the relative world coordinate system.  The X-coordinate of the origin of the relative coordinate system is affected by transformations performed using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) or [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) with [`XOffset`](../../Reference/cal/McalRelativeOrigin.md). If you are using a 3D-based camera calibration context, it is recommended to use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate. |

---

### `M_RELATIVE_ORIGIN_Y`

Inquires the Y-coordinate of the origin of the relative world coordinate system.  The Y-coordinate of the origin of the relative coordinate system is affected by transformations performed using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) or [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) with [`YOffset`](../../Reference/cal/McalRelativeOrigin.md). If you are using a 3D-based camera calibration context, it is recommended to use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate. |

---

### `M_RELATIVE_ORIGIN_Z`

Inquires the Z-coordinate of the origin of the relative world coordinate system.  The Z-coordinate of the origin of the relative coordinate system is affected by transformations performed using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) or [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) with [`ZOffset`](../../Reference/cal/McalRelativeOrigin.md). If you are using a 3D-based camera calibration context, it is recommended to use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate. |

---

### `M_TOOL_POSITION_X`

Inquires the X-position of the origin of the tool coordinate system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate, in world units. |

---

### `M_TOOL_POSITION_Y`

Inquires the Y-position of the origin of the tool coordinate system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate, in world units. |

---

### `M_TOOL_POSITION_Z`

Inquires the Z-position of the origin of the tool coordinate system.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Z-coordinate, in world units. |

---

### `M_TOOL_ROTATION_Z`

Inquires the rotation angle of the tool coordinate system about its Z-axis.  A positive angle corresponds to the rotation of the tool coordinate system in the counter-clockwise direction (from the positive X-axis to the negative Y-axis of the tool coordinate system).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the angle, in degrees. |

---

### `M_TRANSFORM_CACHE`

Inquires whether a cache is used to accelerate [`McalTransformImage`](../../Reference/cal/McalTransformImage.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to use a cache. |
| `M_ENABLE` *(default)* | Specifies to use a cache. |

### For a camera calibration context calibrated using McalGrid()

For a camera calibration context that has been calibrated using [`McalGrid`](../../Reference/cal/McalGrid.md), the [`InquireType`](../../Reference/cal/McalInquire.md) parameter can be set to one of the following values.

---

### `M_COLUMN_NUMBER`

Inquires the number of columns in the camera calibration grid.

| Value | Description |
| --- | --- |
| `Value >= 2` | Specifies the number of columns. |

---

### `M_COLUMN_SPACING`

Inquires the number of world units between columns.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the spacing between columns. |

---

### `M_FOREGROUND_VALUE`

Inquires whether the grid's circles are lighter or darker than the background.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Determines the appropriate setting automatically. |
| `M_FOREGROUND_BLACK` | Specifies that the grid's circles are darker than the background. |
| `M_FOREGROUND_WHITE` | Specifies that the grid's circles are lighter than the background. |

---

### `M_GRID_FIDUCIAL`

Inquires whether the partial chessboard grid has a fiducial.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DATAMATRIX` | Specifies that a Data Matrix code is used as a fiducial in a chessboard grid. |
| `M_NONE` *(default)* | Specifies that there is no fiducial in the grid. |

---

### `M_GRID_HINT_ANGLE_X`

Inquires the hint angle used to help determine the orientation of the X-axis when calibrating your camera with a partial chessboard grid.

| Value | Description |
| --- | --- |
| `M_NONE` *(default)* | Specifies that no hint angle is used. |
| `Value` | Specifies the hint angle, measured counter-clockwise. |

---

### `M_GRID_HINT_PIXEL_X`

Inquires the X-coordinate of the hint pixel used to help determine the grid's reference calibration point when calibrating your camera with a grid.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies not to use a hint pixel. |
| `Value` | Specifies the X-coordinate of the hint pixel, in the pixel coordinate system. |

---

### `M_GRID_HINT_PIXEL_Y`

Inquires the Y-coordinate of the hint pixel used to help determine the grid's reference calibration point when calibrating your camera with a grid.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies not to use a hint pixel. |
| `Value` | Specifies the Y-coordinate of the hint pixel, in the pixel coordinate system. |

---

### `M_GRID_ORIGIN_X`

Inquires the X-coordinate of the grid's reference calibration point, in the world coordinate system. The world coordinate system could be either the absolute coordinate system or the relative coordinate system, depending on which coordinate system was specified when you called [`McalGrid`](../../Reference/cal/McalGrid.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate. |

---

### `M_GRID_ORIGIN_Y`

Inquires the Y-coordinate of the grid's reference calibration point, in the world coordinate system. The world coordinate system could be either the absolute coordinate system or the relative coordinate system, depending on which coordinate system was specified when you called [`McalGrid`](../../Reference/cal/McalGrid.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate. |

---

### `M_GRID_ORIGIN_Z`

Inquires the Z-coordinate of the grid's reference calibration point, in the world coordinate system. The world coordinate system could be either the absolute coordinate system or the relative coordinate system, depending on which coordinate system was specified when you called [`McalGrid`](../../Reference/cal/McalGrid.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate. |

---

### `M_GRID_PARTIAL`

Inquires whether the chessboard grid in the camera calibration image is allowed to be a partial grid.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that [`McalGrid`](../../Reference/cal/McalGrid.md) will only calibrate the camera setup when a complete grid is found in the image. |
| `M_ENABLE` | Specifies that [`McalGrid`](../../Reference/cal/McalGrid.md) can calibrate the camera setup when a partial grid is found in the image. |

---

### `M_GRID_SHAPE`

Inquires whether the partial chessboard grid in the camera calibration image is assumed to be a rectangle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` | Specifies to include all potential calibration points in the image; [`McalGrid`](../../Reference/cal/McalGrid.md) will not look for the boundary of the real-world grid. |
| `M_RECTANGLE` *(default)* | Specifies to exclude potential calibration points in the image that [`McalGrid`](../../Reference/cal/McalGrid.md) determines are outside the boundaries of the partial grid. |

---

### `M_GRID_TYPE`

Inquires the type of grid used to perform the camera calibration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CHESSBOARD_GRID` | Specifies a chessboard grid. |
| `M_CIRCLE_GRID` *(default)* | Specifies a grid of circles. |
| `M_Y_AXIS_CLOCKWISE` *(default)* | Orients the positive Y-axis 90° clockwise from the X-axis. |
| `M_Y_AXIS_COUNTER_CLOCKWISE` | Orients the positive Y-axis 90° counter-clockwise from the X-axis. |
| `M_FAST` | Speeds up the time it takes to perform the camera calibration by using a faster camera calibration algorithm. |

---

### `M_GRID_UNIT_SHORT_NAME`

Inquires the abbreviated name of the world units encoded in the Data Matrix code of the fiducial grid used to calibrate the camera calibration context.  > **Note:** Note that this information is not used by the Calibration module. No unit consistency is enforced and no unit conversion is performed.

| Value | Description |
| --- | --- |
| `"cm"` | Specifies the grid units are centimeters. |
| `"ft"` | Specifies the grid units are feet. |
| `"in"` | Specifies the grid units are inches. |
| `"km"` | Specifies the grid units are kilometers. |
| `"m"` | Specifies the grid units are meters. |
| `"miles"` | Specifies the grid units are miles. |
| `"mils"` | Specifies the grid units are mils. |
| `"mm"` | Specifies the grid units are millimeters. |
| `"um"` | Specifies the grid units are micrometers.  > **Note:** Note that micrometers are abbreviated as "um" to ensure easy printing in a non-unicode environment. |
| `"units"` | Specifies the grid units are unknown. |

---

### `M_GRID_UNITS`

Inquires the world units that were encoded in the Data Matrix code of the fiducial grid used to calibrate the camera calibration context.  > **Note:** Note that this information is not used by the Calibration module. No unit consistency is enforced and no unit conversion is performed.

| Value | Description |
| --- | --- |
| `M_CENTIMETERS` | Specifies that the grid units are measured in centimeters. |
| `M_FEET` | Specifies that the grid units are measured in feet. |
| `M_INCHES` | Specifies that the grid units are measured in inches. |
| `M_KILOMETERS` | Specifies that the grid units are measured in kilometers. |
| `M_METERS` | Specifies that the grid units are measured in meters. |
| `M_MICROMETERS` | Specifies that the grid units are measured in micrometers. |
| `M_MILES` | Specifies that the grid units are measured in miles. |
| `M_MILLIMETERS` | Specifies that the grid units are measured in millimeters. |
| `M_MILS` | Specifies that the grid units are measured in mils. |
| `M_UNKNOWN` | Specifies that grid units are measured in an unknown unit.  If your calibration grid does not have a fiducial, the grid units are unknown. |

---

### `M_ROW_NUMBER`

Inquires the number of rows in the camera calibration grid.

| Value | Description |
| --- | --- |
| `Value >= 2` | Specifies the number of rows. |

---

### `M_ROW_SPACING`

Inquires the spacing between rows in the camera calibration grid.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the spacing between rows, in world units. |

### For a camera calibration context calibrated using McalGrid() or McalList()

For a camera calibration context that has been calibrated using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md), the [`InquireType`](../../Reference/cal/McalInquire.md) parameter can be set to one of the following values.

---

### `M_AVERAGE_PIXEL_ERROR`

Inquires the average camera calibration error in the pixel coordinate system. You can only inquire this value for a successfully calibrated camera calibration context.  This is the average distance in the pixel coordinate system between the initial calibration points and their projected points in an image.  > **Note:** When working in [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode, this only returns the average camera calibration error of the last camera calibration call. To return an average camera calibration error for all camera calibration grids used, it is recommended to use [`M_GLOBAL_AVERAGE_PIXEL_ERROR`](../../Reference/cal/McalInquire.md) instead.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the average camera calibration error, in pixels. |

---

### `M_AVERAGE_WORLD_ERROR`

Inquires the average camera calibration error in the absolute coordinate system. You can only inquire this value for a successfully calibrated camera calibration context.  This is the average distance in the absolute coordinate system between the initial calibration points and their projected points in an image.  > **Note:** When working in [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode, this only returns the average camera calibration error of the last camera calibration call. To return an average camera calibration error for all camera calibration grids used, it is recommended to use [`M_GLOBAL_AVERAGE_WORLD_ERROR`](../../Reference/cal/McalInquire.md) instead.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the average camera calibration error, in world units. |

---

### `M_CALIBRATION_IMAGE_POINTS_X`

Inquires the X-pixel coordinate of the calibration points.  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, the calibration points' pixel coordinates are determined by the pixel positions of the centers of the circles in a circle grid or the intersections of four squares/rectangles in a chessboard grid.  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, you explicitly set the calibration points' pixel coordinates with the [`PixCoordXArrayPtr`](../../Reference/cal/McalList.md) parameter.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate, in pixels. |

---

### `M_CALIBRATION_IMAGE_POINTS_Y`

Inquires the Y-pixel coordinate of the calibration points.  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, the calibration points' pixel coordinates are determined by the pixel positions of the centers of the circles in a circle grid or the intersections of four squares/rectangles in a chessboard grid.  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, you explicitly set the calibration points' pixel coordinates with the [`PixCoordYArrayPtr`](../../Reference/cal/McalList.md) parameter.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate, in pixels. |

---

### `M_CALIBRATION_WORLD_POINTS_X`

Inquires the X-world coordinate of the calibration points. The coordinate is expressed in world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)).  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, the calibration points are computed from the parameters [`GridOffsetX`](../../Reference/cal/McalGrid.md), [`ColumnNumber`](../../Reference/cal/McalGrid.md), and [`ColumnSpacing`](../../Reference/cal/McalGrid.md).  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, the world coordinates of the calibration points are those you set with the [`WorldCoordXArrayPtr`](../../Reference/cal/McalList.md) parameter.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate, in real-world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)). |

---

### `M_CALIBRATION_WORLD_POINTS_Y`

Inquires the Y-world coordinate of the calibration points. The coordinate is expressed in world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)).  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, the calibration points are computed from the parameters [`GridOffsetY`](../../Reference/cal/McalGrid.md), [`RowNumber`](../../Reference/cal/McalGrid.md), and [`RowSpacing`](../../Reference/cal/McalGrid.md).  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, the world coordinates of the calibration points are those you set with the [`WorldCoordYArrayPtr`](../../Reference/cal/McalList.md) parameter.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate, in real-world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)). |

---

### `M_CALIBRATION_WORLD_POINTS_Z`

Inquires the Z-world coordinate of the calibration points that are based on explicitly specified values. The computed calibration points are expressed in world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)). If this value is inquired for 2D-based camera calibration contexts, the specified array will be filled with 0.0 values.  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, you explicitly set the calibration points' pixel coordinates with the [`WorldCoordZArrayPtr`](../../Reference/cal/McalList.md) parameter.  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, you explicitly set the Z-coordinate of all the calibration points with the [`GridOffsetZ`](../../Reference/cal/McalGrid.md) parameter.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate, in real-world units of the camera calibration plane ([`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md)). |

---

### `M_GLOBAL_AVERAGE_PIXEL_ERROR`

Inquires the average camera calibration error, in pixels, for all the points used in all successive calls to [`McalGrid`](../../Reference/cal/McalGrid.md) and [`McalList`](../../Reference/cal/McalList.md). You can only inquire this value for a successfully calibrated [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the average camera calibration error, in pixels. |

---

### `M_GLOBAL_AVERAGE_WORLD_ERROR`

Inquires the average camera calibration error, in world units, for all the points used in all successive calls to [`McalGrid`](../../Reference/cal/McalGrid.md) and [`McalList`](../../Reference/cal/McalList.md). You can only inquire this value for a successfully calibrated [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the average camera calibration error, in world units. |

---

### `M_GLOBAL_MAXIMUM_PIXEL_ERROR`

Inquires the maximum camera calibration error, in pixels, for all the points used in all successive calls to [`McalGrid`](../../Reference/cal/McalGrid.md) and [`McalList`](../../Reference/cal/McalList.md). You can only inquire this value for a successfully calibrated [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the maximum camera calibration error, in pixels. |

---

### `M_GLOBAL_MAXIMUM_WORLD_ERROR`

Inquires the maximum camera calibration error, in world units, for all the points used in all successive calls to [`McalGrid`](../../Reference/cal/McalGrid.md) and [`McalList`](../../Reference/cal/McalList.md). You can only inquire this value for a successfully calibrated [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the maximum camera calibration error, in world units. |

---

### `M_MAXIMUM_PIXEL_ERROR`

Inquires the maximum camera calibration error, in pixels. You can only inquire this value for a successfully calibrated camera calibration context.  > **Note:** For an [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context, this only returns the average camera calibration error of the last camera calibration call. To return an average camera calibration error for all camera calibration grids used, it is recommended to use [`M_GLOBAL_MAXIMUM_PIXEL_ERROR`](../../Reference/cal/McalInquire.md) instead.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the maximum camera calibration error, in pixels. |

---

### `M_MAXIMUM_WORLD_ERROR`

Inquires the maximum camera calibration error, in world units. You can only inquire this value for a successfully calibrated camera calibration context.  > **Note:** For an [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context, this only returns the average camera calibration error of the last camera calibration call. To return an average camera calibration error for all camera calibration grids used, it is recommended to use [`M_GLOBAL_MAXIMUM_WORLD_ERROR`](../../Reference/cal/McalInquire.md) instead.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the maximum camera calibration error, in world units. |

---

### `M_NUMBER_OF_CALIBRATION_POINTS`

Inquires the number of calibration points found by [`McalGrid`](../../Reference/cal/McalGrid.md) or passed to [`McalList`](../../Reference/cal/McalList.md).  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, you can explicitly set the number of calibration points with the [`NumPoint`](../../Reference/cal/McalList.md) parameter.  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, the number of calibration points is determined by the number of columns and rows in your grid.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of calibration points. |

---

### `M_NUMBER_OF_CALIBRATION_POSES`

Inquires the number of calls made to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with the same [`ContextCalOrCalibratedAilObjectId`](../../Reference/cal/McalInquire.md) parameter passed. You can only inquire this value for a successfully calibrated [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of camera calibration poses. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### For a camera calibration context calibrated using McalGrid(), McalList(), or McalUniform()

For a camera calibration context that has been calibrated using [`McalGrid`](../../Reference/cal/McalGrid.md), [`McalList`](../../Reference/cal/McalList.md), or [`McalUniform`](../../Reference/cal/McalUniform.md), the [`InquireType`](../../Reference/cal/McalInquire.md) parameter can be set to one of the following values.

---

### `M_ASPECT_RATIO`

Inquires the average aspect ratio. The ratio is the average pixel width, divided by average pixel height, calculated with world units.

| Value | Description |
| --- | --- |
| `M_INVALID_SCALE` | Specifies that camera calibration was not successful. This value can also be returned if the camera is positioned and oriented in such a way that all points in the image plane are invalid. |
| `Value > 0.0` | Specifies the average aspect ratio. |

### For a 3D-based camera calibration context

For a 3D-based camera calibration context ( [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)), the [`InquireType`](../../Reference/cal/McalInquire.md) parameter can be set to one of the following values.

---

### `M_CCD_ASPECT_RATIO`

Inquires the width to height ratio of the individual elements of the CCD.

| Value | Description |
| --- | --- |
| `1.0` *(default)* | Specifies that the width and height of the CCD element are equal. |
| `Value > 0.0` | Specifies the value of the width of a CCD element divided by its height. |

---

### `M_DISTORTION_RADIAL_1`

Inquires the value of the second order radial distortion coefficient used in the camera calibration algorithm. Radial distortion refers to image distortions caused by the camera's lens.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the value of the second order radial distortion coefficient. |

---

### `M_DISTORTION_RADIAL_2`

Inquires the value of the fourth order radial distortion coefficient used in the camera calibration algorithm. Radial distortion refers to image distortions caused by the camera's lens. This radial distortion coefficient is only supported for [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration modes.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the value of the fourth order radial distortion coefficient. |

---

### `M_FOCAL_LENGTH`

Inquires the effective focal length of the pinhole camera model used in the camera calibration.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the effective focal length of the pinhole camera model, expressed in horizontal pixels. |

---

### `M_PRINCIPAL_POINT_X`

Inquires the X-coordinate of the intersection of the camera's optical axis and the image plane.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies half of the image's width, in pixels. |
| `Value` | Specifies the X-coordinate, in pixels. |

---

### `M_PRINCIPAL_POINT_Y`

Inquires the Y-coordinate of the intersection of the camera's optical axis and the image plane.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies half of the image's height, in pixels. |
| `Value` | Specifies the Y-coordinate, in pixels. |

### For any image, digitizer, or result buffer

For any image, result buffer, or digitizer, the [`InquireType`](../../Reference/cal/McalInquire.md) parameter can be set to the following value.

---

### `M_ASSOCIATED_CALIBRATION`

Inquires the identifier of the associated camera calibration context.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that there is no camera calibration context associated with the image or digitizer. |
| `M_DEFAULT_UNIFORM_CALIBRATION` | Specifies that the image was calibrated using [`McalUniform`](../../Reference/cal/McalUniform.md). |
| `Calibration object identifier` | Specifies the camera calibration context that is associated with the image or digitizer. |

### For a camera calibration context, image, or result buffer

For a camera calibration context, image, or result buffer, the [`InquireType`](../../Reference/cal/McalInquire.md) parameter can be set to one of the following values. Note that a result buffer has exactly the same calibration information as the calibration information of the image used to obtain the results, if the result buffer's Aurora Imaging Library module supports returning results in real-world units; so when you inquire about a result buffer, the information returned is about the image on which the results were obtained.

---

### `M_CALIBRATION_CATEGORY`

Inquires whether the specified Aurora Imaging Library object is or is associated with a camera calibration context, and whether the associated context was allocated with a 2D-based or 3D-based calibration mode.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies the Aurora Imaging Library object is not associated with a camera calibration context.  Inquiring the camera calibration category of a camera calibration context never returns [`M_NULL`](../../Reference/cal/McalInquire.md). |
| `M_2D_CALIBRATION` | Specifies that the camera calibration context was allocated with a 2D-based camera calibration mode, such as [`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md).  When the specified Aurora Imaging Library object is an image with a constant pixel size, such as corrected images and depth maps, [`M_2D_CALIBRATION`](../../Reference/cal/McalInquire.md) is returned, regardless of the calibration mode of the camera calibration context with which the object is associated. |
| `M_3D_CALIBRATION` | Specifies that the camera calibration context was allocated with a 3D-based camera calibration mode, such as [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md). |

---

### `M_CALIBRATION_MODE`

Inquires the camera calibration mode of the associated camera calibration of the specified Aurora Imaging Library object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_3D_ROBOTICS` | Specifies a camera calibration context for a 3D-based camera calibration that uses a Tsai-based camera model in robotics mode. |
| `M_3D_ROBOTICS_ZHANG_BASED` | Specifies a camera calibration context for a 3D-based camera calibration that uses a Zhang-based camera model in robotics mode. |
| `M_LINEAR_INTERPOLATION` *(default)* | Specifies a camera calibration context for a 2D-based camera calibration that uses piecewise linear interpolation mode. |
| `M_PERSPECTIVE_TRANSFORMATION` | Specifies a camera calibration context for a 2D-based camera calibration that uses perspective transformation mode. |
| `M_TSAI_BASED` | Specifies a camera calibration context for a 3D-based camera calibration that uses Tsai-based mode. |
| `M_ZHANG_BASED` | Specifies a camera calibration context for a 3D-based camera calibration that uses Zhang-based mode. |
| `M_NULL` | Specifies the Aurora Imaging Library object is not associated with a camera calibration.  Inquiring the camera calibration mode of a camera calibration context never returns [`M_NULL`](../../Reference/cal/McalInquire.md). |
| `M_UNIFORM_TRANSFORMATION` | Specifies uniform transformation mode.  Images with a constant pixel size (including corrected images and depth maps) are always uniform, even if the camera calibration mode of their associated camera calibration is not. |

---

### `M_CONSTANT_PIXEL_SIZE`

Inquires whether the image has a constant pixel size.  When inquiring a camera calibration context, [`M_CONSTANT_PIXEL_SIZE`](../../Reference/cal/McalInquire.md) will always return [`M_FALSE`](../../Reference/cal/McalInquire.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the image does not have a constant pixel size, or that you are inquiring a camera calibration context. The size of each pixel depends on the camera calibration context associated with the image. |
| `M_TRUE` | Specifies that the image has a constant pixel size. Note, however, that pixels are not necessarily square; they can be rectangular and might be rotated. |

---

### `M_CORRECTION_STATE`

Inquires whether the image has been physically corrected. Physically corrected images have square pixels with a constant size and no rotation between the pixel and absolute coordinate systems. Images can be corrected using [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) with [`M_FULL_CORRECTION`](../../Reference/cal/McalTransformImage.md).  When inquiring a camera calibration context, [`M_CORRECTION_STATE`](../../Reference/cal/McalInquire.md) will always return [`M_FALSE`](../../Reference/cal/McalInquire.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the image has been neither corrected nor calibrated, or that you are inquiring a camera calibration context. |
| `M_TRUE` | Specifies that the image has been corrected. |

---

### `M_DEPTH_MAP`

Inquires whether the Aurora Imaging Library object is a fully corrected depth map. A fully corrected buffer is a calibrated buffer with a constant pixel size and with a valid Z-scale. Note that the size of the pixels is not necessarily square.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the Aurora Imaging Library object is not a fully corrected depth map.  Inquiring [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md) on a camera calibration context returns [`M_FALSE`](../../Reference/cal/McalInquire.md). |
| `M_TRUE` | Specifies that the image is a fully corrected depth map.  > **Note:** Note that when [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md) returns [`M_TRUE`](../../Reference/cal/McalInquire.md), [`M_CONSTANT_PIXEL_SIZE`](../../Reference/cal/McalInquire.md) also returns [`M_TRUE`](../../Reference/cal/McalInquire.md) and [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md) will not return [`M_INVALID_SCALE`](../../Reference/cal/McalControl.md). |

---

### `M_Y_AXIS_DIRECTION`

Inquires the direction the Y-axis of the absolute coordinate system is oriented with respect to its positive X-axis.  If you used [`McalList`](../../Reference/cal/McalList.md) to calibrate your camera setup, the Y-axis orientation was determined by the calibration points that you specified. For more information, see [Calibrating using calibration points from a list](../../UserGuide/C28_Calibration/S12_Calibrating_using_calibration_points_from_a_list.md).  If you used [`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate your camera setup, the explicitly specified Y-axis orientation is used.  > **Note:** Note that this constant does not support images without an associated camera calibration context. An uncalibrated image will return an error.

| Value | Description |
| --- | --- |
| `M_Y_AXIS_CLOCKWISE` | Specifies that the positive Y-axis is oriented 90° clockwise with respect to the positive X-axis. |
| `M_Y_AXIS_COUNTER_CLOCKWISE` | Specifies that the positive Y-axis is oriented 90° counter-clockwise with respect to the positive X-axis. |

### For a calibrated image or result buffer

For an image or result buffer that is calibrated, the [`InquireType`](../../Reference/cal/McalInquire.md) parameter can be set to one of the following values. Note that a result buffer has exactly the same calibration information as the calibration information of the image used to obtain the results, if the result buffer's Aurora Imaging Library module supports returning results in real-world units; so when you inquire about a result buffer, the information returned is about the image on which the results were obtained.

---

### `M_CALIBRATION_CHILD_OFFSET_X`

Inquires the X-offset of a child buffer relative to the highest calibrated parent image that was originally associated with the camera calibration context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset, relative to the child buffer's highest order calibrated parent buffer. |

---

### `M_CALIBRATION_CHILD_OFFSET_Y`

Inquires the Y-offset of a child buffer relative to the highest calibrated parent image that was originally associated with the camera calibration context.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset, relative to the child buffer's highest order calibrated parent buffer. |

### For a calibrated image with distortions

For an image that is calibrated and is distorted, the [`InquireType`](../../Reference/cal/McalInquire.md) parameter can be set to one of the following values, before calling [`McalTransformImage`](../../Reference/cal/McalTransformImage.md).

---

### `M_TRANSFORM_CLIP_SIZE_X_PRESERVE_PIXEL_SIZE`

Inquires the buffer width ([`SizeX`](../../Reference/buf/MbufAlloc2d.md)) that the destination image should have to preserve the average pixel size of the source image, when calling [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) with [`M_CLIP`](../../Reference/cal/McalTransformImage.md).

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the width cannot be determined. This occurs when [`M_ASPECT_RATIO`](../../Reference/cal/McalInquire.md) returns [`M_INVALID_SCALE`](../../Reference/cal/McalInquire.md), or when most of the image border pixels are outside of the calibration region. |
| `Value` | Specifies the ideal width of the destination image buffer to preserve the average pixel size of the source image, in pixels. |

---

### `M_TRANSFORM_CLIP_SIZE_Y_PRESERVE_PIXEL_SIZE`

Inquires the buffer height ([`SizeY`](../../Reference/buf/MbufAlloc2d.md)) that the destination image should have to preserve the average pixel size of the source image, when calling [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) with [`M_CLIP`](../../Reference/cal/McalTransformImage.md).

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the height cannot be determined. This occurs when [`M_ASPECT_RATIO`](../../Reference/cal/McalInquire.md) returns [`M_INVALID_SCALE`](../../Reference/cal/McalInquire.md), or when most of the image border pixels are outside of the calibration region. |
| `Value` | Specifies the ideal height of the destination image buffer to preserve the average pixel size of the source image, in pixels. |

---

### `M_TRANSFORM_FIT_SIZE_X_PRESERVE_PIXEL_SIZE`

Inquires the buffer width ([`SizeX`](../../Reference/buf/MbufAlloc2d.md)) that the destination image should have to preserve the average pixel size of the source image, when calling [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) with [`M_FIT`](../../Reference/cal/McalTransformImage.md).

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the width cannot be determined. This occurs when [`M_ASPECT_RATIO`](../../Reference/cal/McalInquire.md) returns [`M_INVALID_SCALE`](../../Reference/cal/McalInquire.md), or when most of the image border pixels are outside of the calibration region. |
| `Value` | Specifies the ideal width of the destination image buffer to preserve the average pixel size of the source image, in pixels. |

---

### `M_TRANSFORM_FIT_SIZE_Y_PRESERVE_PIXEL_SIZE`

Inquires the buffer height ([`SizeY`](../../Reference/buf/MbufAlloc2d.md)) that the destination image should have to preserve the average pixel size of the source image, when calling [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) with [`M_FIT`](../../Reference/cal/McalTransformImage.md).

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the height cannot be determined. This occurs when [`M_ASPECT_RATIO`](../../Reference/cal/McalInquire.md) returns [`M_INVALID_SCALE`](../../Reference/cal/McalInquire.md), or when most of the image border pixels are outside of the calibration region. |
| `Value` | Specifies the ideal height of the destination image buffer to preserve the average pixel size of the source image, in pixels. |

### For a calibrated image or result buffer, with a constant pixel size

The following formulas give the relationship between coordinates in pixel units and world units when an image or result buffer is calibrated and has a constant pixel size:  *[Image: uniform_p_to_w.png]*  *[Image: uniform_w_to_p.png]*  If an image or result buffer is calibrated and has a constant pixel-size, the [`InquireType`](../../Reference/cal/McalInquire.md) parameter can be set to one of the following values.

---

### `M_GRAY_LEVEL_SIZE_Z`

Inquires the step, in world units, along the Z-axis of the relative coordinate system, represented by one gray level. Since the Z-axis is pointing downwards, positive [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalInquire.md) values mean that lower (darker) pixel values represent higher world points (top-black); negative [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalInquire.md) values mean that higher (brighter) pixel values represent higher world points (top-white).  This inquire type is only useful when inquiring about an image buffer that contains a depth map.

| Value | Description |
| --- | --- |
| `M_INVALID_SCALE` *(default)* | Specifies that the image is not a depth map. |
| `Value != 0.0` | Specifies the height, in world units, corresponding to a difference of one gray level. |

---

### `M_PIXEL_ROTATION`

Inquires the angle of the X-axis of the pixel coordinate system measured in the relative world coordinate system. A positive value indicates a counter-clockwise rotation (from the positive X-axis of the relative world coordinate system toward its negative Y-axis).  This value corresponds to _R_ in the above formulas.

| Value | Description |
| --- | --- |
| `Value` | Specifies the angle, in degrees. |

---

### `M_PIXEL_SIZE_X`

Inquires the width of the pixels in the corrected image.  You can multiply a measure in pixels, along the X-axis, by [`M_PIXEL_SIZE_X`](../../Reference/cal/McalInquire.md) to get the measure in world units. If there is rotation between the relative world coordinate system and the pixel coordinate system, this distance won't be along the X-axis of the relative world coordinate system.  This value corresponds to _S<sub>x</sub> _ in the above formulas.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the width, in world units/pixel. |

---

### `M_PIXEL_SIZE_Y`

Inquires the height of the pixels in the corrected image.  You can multiply a measure in pixels, along the Y-axis, by [`M_PIXEL_SIZE_Y`](../../Reference/cal/McalInquire.md) to get the measure in world units. If there is rotation between the relative world coordinate system and the pixel coordinate system, this distance won't be along the Y-axis of the relative world coordinate system.  This value corresponds to _S<sub>y</sub> _ in the above formulas.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the height, in world units/pixel. |

---

### `M_WORLD_POS_X`

Inquires the X-coordinate of the center of the top-left pixel in the highest calibrated parent image that was originally associated with the camera calibration context. This value corresponds to _T<sub>x</sub> _ in the above formulas.  > **Note:** Note that this also applies to a clone of a child buffer (which is not a child itself).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate, expressed in the relative world coordinate system. |

---

### `M_WORLD_POS_Y`

Inquires the Y-coordinate of the center of the top-left pixel in the highest calibrated parent image that was originally associated with the camera calibration context. This value corresponds to _T<sub>y</sub> _ in the above formulas.  > **Note:** Note that this also applies to a clone of a child buffer (which is not a child itself).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate, expressed in the relative world coordinate system. |

---

### `M_WORLD_POS_Z`

Inquires the Z-coordinate of gray level 0 in the corrected image.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the base height of a gray level of 0, expressed in the relative world coordinate
                                 system. |

### Combination Constants — For inquiring the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### For a fixturing offset object

For a fixturing offset object, the [`InquireType`](../../Reference/cal/McalInquire.md) parameter can be set to one of the following values.

---

### `M_ANGLE`

Inquires the angular offset to apply to the reference location when setting the relative world coordinate system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the angular offset. |

---

### `M_POSITION_X`

Inquires the X-offset to apply to the reference location when setting the relative world coordinate system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset. |

---

### `M_POSITION_Y`

Inquires the Y-offset to apply to the reference location when setting the relative world coordinate system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset. |

### For a 3D draw calibration context

For a 3D draw calibration context, [`InquireType`](../../Reference/cal/McalInquire.md) can be set to one of the following.

---

### `M_DRAW_ABSOLUTE_COORDINATE_SYSTEM`

Inquires whether to draw the absolute coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the absolute coordinate system's axes. |
| `M_ENABLE` *(default)* | Specifies to draw the absolute coordinate system's axes. |

---

### `M_DRAW_CAMERA_COORDINATE_SYSTEM`

Inquires whether to draw the camera coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the camera coordinate system's axes. |
| `M_ENABLE` *(default)* | Specifies to draw the camera coordinate system's axes. |

---

### `M_DRAW_CAMERA_COORDINATE_SYSTEM_NAME`

Inquires the name to draw for the camera coordinate system.

---

### `M_DRAW_COORDINATE_SYSTEM_LENGTH`

Inquires the length at which to draw the specified coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the length (in world units) at which to draw the axes of the specified coordinate system. |

---

### `M_DRAW_FRUSTUM`

Inquires whether to draw the frustum.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the frustum. |
| `M_ENABLE` *(default)* | Specifies to draw the frustum. |

---

### `M_DRAW_FRUSTUM_COLOR`

Inquires the frustum's color.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies an RGB value to draw in an 8-bit, 3-band buffer. The red, green, and blue values must be values between 0 and 255, inclusive. |
| `M_COLOR_YELLOW` *(default)* | Specifies the color yellow. |
| `M_NO_COLOR` | Specifies no color. |

---

### `M_DRAW_RELATIVE_COORDINATE_SYSTEM`

Inquires whether to draw the relative coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the relative coordinate system's axes. |
| `M_ENABLE` *(default)* | Specifies to draw the relative coordinate system's axes. |

---

### `M_DRAW_RELATIVE_COORDINATE_SYSTEM_NAME`

Inquires the name to draw for the relative coordinate system.

---

### `M_DRAW_RELATIVE_XY_PLANE`

Inquires whether to draw the relative XY plane.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the relative XY plane. |
| `M_ENABLE` *(default)* | Specifies to draw the relative XY plane. |

---

### `M_DRAW_RELATIVE_XY_PLANE_COLOR_FILL`

Inquires the relative XY plane's fill color.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies an RGB value to draw in an 8-bit, 3-band buffer. The red, green, and blue values must be values between 0 and 255, inclusive. |
| `M_AUTO_COLOR` *(default)* | Specifies either the color white or the texture image.If a texture image is specified (using [`McalDraw3d`](../../Reference/cal/McalDraw3d.md) with [`RelXYPlaneTextureImageBufId`](../../Reference/cal/McalDraw3d.md)), the texture image is drawn on the laser plane. Otherwise, the plane is drawn with [`M_COLOR_WHITE`](../../Reference/cal/McalInquire.md). |
| `M_NO_COLOR` | Specifies no color. |
| `M_TEXTURE_IMAGE` | Specifies to use the image passed to [`McalDraw3d`](../../Reference/cal/McalDraw3d.md) with [`RelXYPlaneTextureImageBufId`](../../Reference/cal/McalDraw3d.md), when drawing the relative XY plane. The texture image is typically a 2D image of the 3D scene. For example, you can specify the image used for calibration. If no texture image is specified using [`McalDraw3d`](../../Reference/cal/McalDraw3d.md), setting [`M_DRAW_RELATIVE_XY_PLANE_COLOR_FILL`](../../Reference/cal/McalInquire.md) to [`M_TEXTURE_IMAGE`](../../Reference/cal/McalInquire.md) will cause an error. |

---

### `M_DRAW_RELATIVE_XY_PLANE_COLOR_OUTLINE`

Inquires the relative XY plane's outline color.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies an RGB value to draw in an 8-bit, 3-band buffer. The red, green, and blue values must be values between 0 and 255, inclusive. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |
| `M_NO_COLOR` | Specifies no color. |

---

### `M_DRAW_RELATIVE_XY_PLANE_OPACITY`

Inquires the relative XY plane's opacity.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the relative XY plane's opacity. |

---

### `M_DRAW_ROBOT_BASE_COORDINATE_SYSTEM`

Inquires whether to draw the robot base coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the robot base coordinate system's axes. |
| `M_ENABLE` *(default)* | Specifies to draw the robot base coordinate system's axes. |

---

### `M_DRAW_TOOL_COORDINATE_SYSTEM`

Inquires whether to draw the tool coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to draw the tool coordinate system's axes. |
| `M_ENABLE` | Specifies to draw the tool coordinate system's axes. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested information to an _AIL_FLOAT_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT16`

Casts the requested results to an _AIL_INT16_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
