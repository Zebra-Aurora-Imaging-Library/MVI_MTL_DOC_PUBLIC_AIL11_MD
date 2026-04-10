---
doctype: Reference
module: cal
function: McalGrid
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalGrid"
---

# McalGrid

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

> Calibrate your camera setup using a grid.

## Syntax

```c
void McalGrid(
    AIL_ID     CalibrationId,  //out
    AIL_ID     SrcImageBufId,  //in
    AIL_DOUBLE GridOffsetX,    //in
    AIL_DOUBLE GridOffsetY,    //in
    AIL_DOUBLE GridOffsetZ,    //in
    AIL_INT    RowNumber,      //in
    AIL_INT    ColumnNumber,   //in
    AIL_DOUBLE RowSpacing,     //in
    AIL_DOUBLE ColumnSpacing,  //in
    AIL_INT64  Operation,      //in
    AIL_INT64  GridType        //in
)
```

## Description

This function uses an image of a grid of circles or squares and the world description of this grid to establish calibration points, calibrate your camera setup, and create an absolute world coordinate system. If using a circle grid, the center of each circle in the real-world grid is used as a calibration point; if using a chessboard grid, the intersection of four squares/rectangles is used as a calibration point. [`McalGrid`](../../Reference/cal/McalGrid.md) automatically determines the pixel coordinates of the calibration points in the image using feature extraction algorithms, and calculates the world coordinates of the calibration points using the world description of the grid (number of rows/columns and the space between rows/columns). From these calibration points and the specified calibration mode, [`McalGrid`](../../Reference/cal/McalGrid.md) establishes a pixel-to-world and world-to-pixel mapping function. The mapping is stored in the specified calibration context.

[`McalGrid`](../../Reference/cal/McalGrid.md) can also extract calibration points from a special type of chessboard grid which contains a fiducial; this is referred to as a fiducial grid. The fiducial supported in a fiducial grid is a Data Matrix code that encodes the world description of the grid itself. This information includes the grid spacing, the units in which the spacing is measured, the position of the grid's reference calibration point, and the grid's orientation.

Aurora Imaging Library is distributed with five different camera calibration grids that you can print and use: _ChessboardCalibrationGrid_15x16_Letter.pdf_, _ChessboardCalibrationGrid_18x22_Letter.pdf_, _CircleCalibrationGrid_15x16_Letter.pdf_, _CircleCalibrationGrid_18x22_Letter.pdf_, and _FiducialCalibrationGrid_Letter.pdf_. These files are located in the installed Images directory (for example, _C:\Program Files\Aurora Imaging Library\11\Images_). Each file includes the world description of the grid. For guidelines when printing the grid, see [Printing the grid](../../UserGuide/C28_Calibration/S19_Generating_a_grid_for_calibration.md).

After calling [`McalGrid`](../../Reference/cal/McalGrid.md), you can inquire about the success of the camera calibration using[`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_CALIBRATION_STATUS`](../../Reference/cal/McalInquire.md). You can inquire about the accuracy of the mapping function (camera calibration error) using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_AVERAGE_PIXEL_ERROR`](../../Reference/cal/McalInquire.md), [`M_MAXIMUM_PIXEL_ERROR`](../../Reference/cal/McalInquire.md), [`M_AVERAGE_WORLD_ERROR`](../../Reference/cal/McalInquire.md), or [`M_MAXIMUM_WORLD_ERROR`](../../Reference/cal/McalInquire.md). In addition, you can visualize the calibration points that were used, using[`McalDraw`](../../Reference/cal/McalDraw.md) with [`M_DRAW_IMAGE_POINTS`](../../Reference/cal/McalDraw.md); this visualization can be especially effective when overlaid on the image of the original grid. It is important that the extracted calibration points are directly in the middle of each circle, for a circle grid, and at the intersection of the grid squares/rectangles, for a chessboard grid. Any extra points, missing points, or imprecisely positioned points will result in calibration inaccuracy. For more information, see [Inquiring about your camera calibration](../../UserGuide/C28_Calibration/S13_Inquiring_about_your_calibration.md).

In the default calibration mode, [`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md) mode, a mapping function is created so that comparing transformed coordinates (either pixel-to-world or world-to-pixel) of the calibration points results in virtually no measured calibration error. This produces increased calibration accuracy for areas of the image inside the grid and reduced accuracy for areas outside the grid. For all other calibration modes, the mapping function applies the calibration error equally to all areas in the image, regardless of where the grid was in the image. If there are any problems with the mapping function, the calibration error is averaged out over the entire image.

For 3D-based camera calibration modes ([`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)), it is possible to use a grid that is not in the XY-plane (Z=0) of the absolute coordinate system. This allows you to account for two XY-planes in the working area of the camera or even for the thickness of the calibration grid itself. For both cases, you can use the [`GridOffsetZ`](../../Reference/cal/McalGrid.md) parameter to specify the height or thickness of the grid being used.

When working in [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) camera calibration mode, you must first call this function with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) at least three times to accumulate data with different images and orientations. Calling this function with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) more than 3 times greatly improves the accuracy of the camera calibration. Before each data accumulation call, you must change the position and orientation of the tool holding the camera with respect to the robot base. More specifically, you must rotate the tool along at least two non-parallel axes, retrieve the tool's new position with respect to the robot base from the robot encoder, and using this information, set the position of the tool coordinate system with respect to the robot base coordinate system using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md). When you are done accumulating data, you must call this function with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md) and no image to perform the full camera calibration. Once you perform a full camera calibration, you can no longer accumulate camera calibration poses.

When working in [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode, you must first call this function with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) at least three times to accumulate data with different images and orientations. Calling this function with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) more than three times greatly improves the accuracy of the camera calibration. Before each call, move the grid or camera such that the grid is not viewed at the same plane by the camera as a previous call of this function with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md). The pose provided in the last call will be used to automatically set the position of the absolute and camera coordinate systems. It is recommended to accumulate calibration data at various perceived depths, and at the edges of the working area of your application. When you are done accumulating data, you must call this function with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md) and no image to perform the full camera calibration. Once you perform a full camera calibration, you can no longer accumulate camera calibration poses.

When working in [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode, you must first call this function with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) at least three times to accumulate data with different images and orientations. Calling this function with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) more than three times greatly improves the accuracy of the camera calibration. Before each call, you must follow the steps described in the previous two paragraphs. When you are done accumulating data, you must call this function with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md) and no image to perform the full camera calibration. Once you perform a full camera calibration, you can no longer accumulate camera calibration poses.

For 3D-based camera calibration modes, [`McalGrid`](../../Reference/cal/McalGrid.md) performs two types of calculations. It calculates the camera's internal attributes, and it calculates the orientation and distance between the camera and camera calibration plane. Initially, the latter is used to set the camera coordinate system. If you move the camera or the grid (not both), you can have [`McalGrid`](../../Reference/cal/McalGrid.md) recalculate only the orientation and distance between them. In this case, depending on what you moved, you can specify that [`McalGrid`](../../Reference/cal/McalGrid.md) displace either the camera coordinate system or the relative coordinate system, using [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md) or [`M_DISPLACE_RELATIVE_COORD`](../../Reference/cal/McalGrid.md), respectively. Note that [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md) is not supported for an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode. When using [`M_DISPLACE_RELATIVE_COORD`](../../Reference/cal/McalGrid.md), the camera calibration plane is considered to be the XY-plane of the relative coordinate system, regardless of how [`McalControl`](../../Reference/cal/McalControl.md) with [`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md) is set.

> **Note:** Note that the internal attributes calculated for the camera are not those of the physical camera, but those of the ideal pinhole-camera used to model the physical camera.

Also, for most 3D-based camera calibration modes, a successful call to [`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md) creates a rigid link between the tool coordinate system and the head (camera or reference/calibration object) coordinate system. This link ensures that moving either the tool or the head coordinate system will affect both. This link can be broken using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md).

Note that for an [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) calibration context, which requires multiple calls to [`McalGrid`](../../Reference/cal/McalGrid.md), you can inquire about any particular call using [`McalInquireSingle`](../../Reference/cal/McalInquireSingle.md).

Depending on the type of calibration grid and the orientation of the camera and grid in your setup, you might have to specify a hint pixel to indicate the grid's reference calibration point, used to determine the origin of the absolute coordinate system. You can do this using [`McalControl`](../../Reference/cal/McalControl.md) set to [`M_GRID_HINT_PIXEL_X`](../../Reference/cal/McalControl.md) and [`M_GRID_HINT_PIXEL_Y`](../../Reference/cal/McalControl.md). Note that the default position of the reference calibration point for complete grids (grids that are unobstructed and show all specified rows and columns) is the calibration point in the corner of the grid closest to the top-left corner of the image. For chessboard grids that are partially obscured and for all fiducial grids, the default reference calibration point is the calibration point closest to the center of the image.

You can limit results to a region of interest (ROI) of an image buffer using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)).

For more information on performing camera calibration using a chessboard or circle grid, see [Calibrating using calibration points from a grid](../../UserGuide/C28_Calibration/S10_Calibrating_using_calibration_points_from_a_grid.md). For information on camera calibration using a fiducial grid, see [Calibrating using a fiducial grid](../../UserGuide/C28_Calibration/S11_Calibrating_using_a_fiducial_grid.md).

## Parameters

### `CalibrationId` *(out, AIL_ID)*

Specifies the identifier of the camera calibration context.

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer containing the grid. It must be an 8- or 16-bit unsigned buffer, with 1 or 3 bands.

### `GridOffsetX` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the grid's reference calibration point, in the real world. Specify its X-coordinate in the world coordinate system specified using[`McalControl`](../../Reference/cal/McalControl.md) with [`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md). Typically, set this value to 0.

### `GridOffsetY` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the grid's reference calibration point, in the real world. Specify its Y-coordinate in the world coordinate system specified using[`McalControl`](../../Reference/cal/McalControl.md) with[`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md). Typically, set this value to 0.

### `GridOffsetZ` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the grid's reference calibration point, in the real world. Specify its Z-coordinate in the world coordinate system specified using[`McalControl`](../../Reference/cal/McalControl.md) with [`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md). Typically, set this value to 0.

### `RowNumber` *(in, AIL_INT)*

Specifies the number of rows in the camera calibration grid.

*For specifying the row number*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you are performing an [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md) operation in [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode. |
| `M_UNKNOWN` | Specifies that you are calibrating using a partial chessboard grid.

This can be set if and only if [`McalControl`](../../Reference/cal/McalControl.md) with [`M_GRID_PARTIAL`](../../Reference/cal/McalControl.md) is set to [`M_ENABLE`](../../Reference/cal/McalControl.md). |
| `Value >= 2` | Specifies the total number of rows in the real-world grid. Generally, camera calibration accuracy increases with the number of rows.

The minimum number of rows is 2 when [`GridType`](../../Reference/cal/McalGrid.md) is set to [`M_CIRCLE_GRID`](../../Reference/cal/McalGrid.md) and 3 when set to [`M_CHESSBOARD_GRID`](../../Reference/cal/McalGrid.md).

For [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) camera calibration modes, you must provide a grid that contains at least 6 grid points, even though the minimum number of rows and the minimum number of columns are both 2.

For [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration modes, you must provide a grid that contains at least 8 grid points, even though the minimum number of rows and the minimum number of columns are both 2.

> **Note:** Note that in the case of a rotated image of a rectangular camera calibration grid, you must specify the number of rows of the real-world grid and not the number of rows as there appears to be in the rotated image of the grid. |

### `ColumnNumber` *(in, AIL_INT)*

Specifies the number of columns in the camera calibration grid.

*For specifying the column number*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you are performing an [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md) operation in [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode. |
| `M_UNKNOWN` | Specifies that you are calibrating using a partial chessboard grid.

This can be set if and only if [`McalControl`](../../Reference/cal/McalControl.md) with [`M_GRID_PARTIAL`](../../Reference/cal/McalControl.md) is set to [`M_ENABLE`](../../Reference/cal/McalControl.md). |
| `Value >= 2` | Specifies the total number of columns in the real-world grid. Generally, camera calibration accuracy increases with the number of columns.

The minimum number of columns is 2 when [`GridType`](../../Reference/cal/McalGrid.md) is set to [`M_CIRCLE_GRID`](../../Reference/cal/McalGrid.md) and 3 when set to [`M_CHESSBOARD_GRID`](../../Reference/cal/McalGrid.md).

For [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) camera calibration modes, you must provide a grid that contains at least 6 grid points, even though the minimum number of rows and the minimum number of columns are both 2.

For [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration modes, you must provide a grid that contains at least 8 grid points, even though the minimum number of rows and the minimum number of columns are both 2.

> **Note:** Note that in the case of a rotated image of a rectangular camera calibration grid, you must specify the number of columns of the real-world grid and not the number of rows as there appears to be in the rotated image of the grid. |

### `RowSpacing` *(in, AIL_DOUBLE)*

Specifies the number of world units between rows.

*For specifying the row spacing*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to not use row spacing.

You must set this value when performing an [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md) operation in [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode. |
| `M_FROM_FIDUCIAL` | Specifies to set the row spacing according to the data in the chessboard grid fiducial. |
| `Value > 0.0` | Specifies the row spacing, in world units. |

### `ColumnSpacing` *(in, AIL_DOUBLE)*

Specifies the number of world units between columns.

*For specifying the column spacing*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to not use column spacing.

You must set this value when performing an[`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md) operation in [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode. |
| `M_FROM_FIDUCIAL` | Specifies to set the column spacing according to the data in the chessboard grid fiducial. |
| `Value > 0.0` | Specifies the column spacing, in world units. |

### `Operation` *(in, AIL_INT64)*

Specifies the camera calibration operation to perform. This parameter must be set to one of the following values:

*For specifying the camera calibration operation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCUMULATE` | Calculates only the position of the calibration points and stores them in the camera calibration context.

For [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), at least three calls to [`McalGrid`](../../Reference/cal/McalGrid.md) should be made with this operation before performing the full camera calibration with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md). Providing more than three calls can greatly improve the accuracy of the camera calibration.

> **Note:** This operation is only supported for [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration. |
| `M_DISPLACE_CAMERA_COORD` | Calculates only the position and orientation between the camera and the camera calibration plane, and displaces the camera coordinate system accordingly. Note that besides the camera coordinate system, this also displaces the tool coordinate system (if still linked with [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md)); no other coordinate system is affected. This camera calibration operation is only supported for 3D-based camera calibration contexts that are fully calibrated with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md).

If the camera is moved to a new position but its internal attributes are already known by a previous full camera calibration, you can use this operation to allow for a faster camera calibration.

> **Note:** Note that [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md) is not supported for an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration. |
| `M_DISPLACE_RELATIVE_COORD` | Calculates only the position and orientation between the camera and the camera calibration plane, and displaces the relative coordinate system accordingly. This camera calibration operation is only supported for 3D-based camera calibration contexts that are fully calibrated with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md).

You can use this operation to move the relative coordinate system only and remain calibrated. This operation keeps the camera fixed with respect to the absolute coordinate system, and its intrinsic attributes unmodified. You can then obtain the position and orientation of the relative coordinate system with respect to any other coordinate system using [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md), and calculate the unknown position of an object in space.

When using this operation, the camera calibration plane is considered to be the relative coordinate system regardless of the [`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md) setting. |
| `M_FULL_CALIBRATION` *(default)* | Performs a full camera calibration. For 3D-based camera calibration contexts, this operation calculates the camera's internal attributes and the difference in position and orientation between the camera and the camera calibration plane. It then sets the camera coordinate system accordingly.

> **Note:** When working in [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode, the camera's optical axis should be at least 30 degrees away from the axis perpendicular to the camera calibration plane (also known as, angle of incidence). Otherwise, the camera calibration might fail. |

### `GridType` *(in, AIL_INT64)*

Specifies the type of grid used. This parameter must be set to one of the following values:

*For specifying the type of grid*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CHESSBOARD_GRID` | Specifies a chessboard grid. The number of rows and columns represents the number of corners horizontally and vertically. A corner is the intersection of 4 cells, meaning that the corners on the perimeter do not count. Note that, for a chessboard grid type, the number of rows and the number of columns must be at least 3. The following is an example of a valid 4x4 chessboard grid. *[Image: calChessBoardGrid.png]*

> **Note:** Note that when calibrating using a chessboard grid, the entire grid does not have to be present in the camera calibration image. In this case, you must call [`McalControl`](../../Reference/cal/McalControl.md) with [`M_GRID_PARTIAL`](../../Reference/cal/McalControl.md) set to [`M_ENABLE`](../../Reference/cal/McalControl.md). |
| `M_CIRCLE_GRID` *(default)* | Specifies a grid of circles. Note that, for a grid of circles, the number of rows and the numbers of columns must be at least 2. To create an accurate (subpixel) mapping, the image of the grid should meet the following guidelines (at the working resolution):

- The radius of the grid's circles should range between 6 and 10 pixels.
- The center-to-center distance between the grid's circles should range from 18 to 32 pixels (22 pixels recommended).
- The minimum distance between the edges of the circles should be 6 pixels.
- The grid should be large enough to cover the area of the image from which you want real-world results.
- The grid must be on a single plane although it can be at a slant.
- The image of the grid should have high contrast.

> **Note:** Note that circle grids must be complete grids in which all circles are present in a rectangular grid. |

*For specifying the orientation of the Y-axis*

| Value | Description |
| --- | --- |
| `M_Y_AXIS_CLOCKWISE` *(default)* | Orients the positive Y-axis 90° clockwise from the X-axis.

Typically, for a complete grid that has its reference calibration point at the top-left of the grid, the orientation of the Y-axis is along the first column of circles in a grid of circles or along the leftmost vertical line of intersection points in a chessboard grid. The axis is oriented from the top to the bottom. |
| `M_Y_AXIS_COUNTER_CLOCKWISE` | Orients the positive Y-axis 90° counter-clockwise from the X-axis.

Typically, for a complete grid that has its reference calibration point at the top-left of the grid, the orientation of the Y-axis is along the first column of circles in a grid of circles, or along the leftmost vertical line of intersection points in a chessboard grid. The axis is oriented from the bottom to the top. |

*For increasing the speed of camera calibration*

| Value | Description |
| --- | --- |
| `M_FAST` | Speeds up the time it takes to perform the camera calibration by using a faster camera calibration algorithm. Note that using a faster camera calibration algorithm can decrease the robustness and accuracy of the camera calibration.

When using [`M_FAST`](../../Reference/cal/McalGrid.md), memory consumption is reduced. |
