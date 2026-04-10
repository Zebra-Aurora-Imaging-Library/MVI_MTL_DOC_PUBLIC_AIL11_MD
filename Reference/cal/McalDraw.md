---
doctype: Reference
module: cal
function: McalDraw
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / cal / McalDraw"
---

# McalDraw

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

> Draw specific features of results obtained from a camera calibration operation.

## Syntax

```c
void McalDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ContextCalOrImageBufId,  //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT   Index,                   //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific features of results, obtained from a camera calibration operation, in the destination image buffer or 2D graphics list.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ContextCalOrImageBufId` *(in, AIL_ID)*

Specifies the camera calibration context, calibrated image, corrected image, or fixturing offset object from which to extract the information to draw.

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. If a buffer is specified, it must be a 1- or 3-band, unsigned, 8-bit image buffer. By drawing into the display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform.

*For a camera calibration context, calibrated image, or corrected image*

| Value | Description |
| --- | --- |
| `M_DRAW_ABSOLUTE_COORDINATE_SYSTEM` | Draws the absolute coordinate system of the specified source camera calibration context or calibrated image ([`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md)). If [`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md) is set to [`M_NULL`](../../Reference/cal/McalDraw.md), the function draws the absolute coordinate system of the destination image, [`DstImageBufOrListGraId`](../../Reference/cal/McalDraw.md). In the case of a 2D graphics list being passed as the destination, the function will use the camera calibration information associated with the image in which the graphics in the 2D graphics list are rendered; when the 2D graphics list is associated with a display, it uses the camera calibration information of the image currently selected to the display.

An error is reported if [`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md) is [`M_NULL`](../../Reference/cal/McalDraw.md) and the image passed to [`DstImageBufOrListGraId`](../../Reference/cal/McalDraw.md) is not a calibrated image. In the case of a 2D graphics list being passed as the destination, the error will occur when the graphics in the 2D graphics list are rendered in an uncalibrated image or display.

If both [`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md) and [`DstImageBufOrListGraId`](../../Reference/cal/McalDraw.md) have camera calibration information associated with them, the camera calibration information associated with [`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md) will be used to define the position of the coordinate system, while [`DstImageBufOrListGraId`](../../Reference/cal/McalDraw.md) will be used to define the mapping of the coordinates between world and pixel units. |
| `M_DRAW_PIXEL_COORDINATE_SYSTEM` | Draws the pixel coordinate system of the destination image, [`DstImageBufOrListGraId`](../../Reference/cal/McalDraw.md) with the origin placed at the center of the top-left pixel. [`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md) must be set to [`M_NULL`](../../Reference/cal/McalDraw.md).

In the case of a 2D graphics list being passed as the destination, the function will draw the pixel coordinate system of the image in which the graphics in the 2D graphics list are rendered; when the 2D graphics list is associated with a display, it draws the pixel coordinate system of the image currently selected to the display. |
| `M_DRAW_RELATIVE_COORDINATE_SYSTEM` | Draws the relative coordinate system of the specified source camera calibration context or calibrated image ([`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md)). If [`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md) is set to [`M_NULL`](../../Reference/cal/McalDraw.md), the function draws the relative coordinate system of the destination image, [`DstImageBufOrListGraId`](../../Reference/cal/McalDraw.md). In the case of a 2D graphics list being passed as the destination, the function will use the camera calibration information associated with the image in which the graphics in the 2D graphics list are rendered; when the 2D graphics list is associated with a display, it uses the camera calibration information of the image currently selected to the display.

An error is reported if [`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md) is [`M_NULL`](../../Reference/cal/McalDraw.md) and the image passed to [`DstImageBufOrListGraId`](../../Reference/cal/McalDraw.md) is not a calibrated image. In the case of a 2D graphics list being passed as the destination, the error will occur when the graphics in the 2D graphics list are rendered in an uncalibrated image or display.

If both [`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md) and [`DstImageBufOrListGraId`](../../Reference/cal/McalDraw.md) have camera calibration information associated with them, the camera calibration information associated with [`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md) will be used to define the position of the coordinate system, while [`DstImageBufOrListGraId`](../../Reference/cal/McalDraw.md) will be used to define the mapping of the coordinates between world and pixel units. |

*For a camera calibration context*

| Value | Description |
| --- | --- |
| `M_DRAW_FIDUCIAL_BOX` | Draws a box around the fiducial located during calibration, for fiducial grids with a Data Matrix code ([`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_GRID_FIDUCIAL`](../../Reference/cal/McalInquire.md) set to [`M_DATAMATRIX`](../../Reference/cal/McalInquire.md)). |
| `M_DRAW_IMAGE_POINTS` | Draws the calibration points, used with [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md), to calibrate the source camera calibration context ([`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md)). It draws the points at the original pixel positions specified (or established) for the calibration points during camera calibration; it does not use the mapping established during camera calibration. |
| `M_DRAW_VALID_REGION` | Draws a contour of the rectangle surrounding the calibrated area when using piecewise linear camera calibration mode ([`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md)). The points transformed in this calibrated area are never extrapolated, only interpolated. |
| `M_DRAW_VALID_REGION_FILLED` | Draws a filled rectangle on the calibrated area when using piecewise linear camera calibration mode ([`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md)). The points transformed in this calibrated area are never extrapolated, only interpolated. |
| `M_DRAW_WORLD_POINTS` | Draws the real-world positions of the calibration points, used with [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md), to calibrate the source camera calibration context ([`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md)).

To draw the real-world positions, it transforms these positions to pixel units using the camera calibration context associated with destination image, or using [`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md) if the destination image is not calibrated. It does not use the original pixel positions specified (or established) for the calibration points during camera calibration; it uses the mapping established during camera calibration. |

*For a fixturing offset object*

| Value | Description |
| --- | --- |
| `M_DRAW_FIXTURING_OFFSET` | Specifies to draw the reference location that would have been used to place the current relative coordinate system of the destination image, given the specified fixturing offset object. [`McalDraw`](../../Reference/cal/McalDraw.md) draws a line from the current origin of the relative coordinate system to the inferred reference position. It also draws a small arrow at this position, which illustrates the inferred reference angle.

If the destination image is not calibrated, [`McalDraw`](../../Reference/cal/McalDraw.md) behaves as if it were calibrated with a uniform scale of 1 and an absolute and relative coordinate system equal to the pixel coordinate system. Note, however, that the destination image remains uncalibrated.

In the case of a 2D graphics list being passed as the destination, the function will use the camera calibration information associated to the image at the time of annotation. |

*For specifying how to draw the coordinate system*

| Value | Description |
| --- | --- |
| `M_DRAW_ALL` *(default)* | Draws the coordinate system using all the features listed in this table. The behavior of [`M_DRAW_AXES`](../../Reference/cal/McalDraw.md) is chosen over [`M_DRAW_FRAME`](../../Reference/cal/McalDraw.md). The following is an example of the annotations.

*[Image: cal_draw_all_example.png]* |
| `M_DRAW_AXES` | Draws the X-axis and Y-axis lines with arrow heads and axis labels. If the Z-axis can be seen, it is drawn with an arrow and label. The following is an example of the annotations.

*[Image: cal_draw_axes_example.png]* |
| `M_DRAW_FRAME` | Draws three arrows (instead of full axes) and axis labels. The following is an example of the annotations.

*[Image: cal_draw_frame_example.png]* |
| `M_DRAW_LEGEND` | Draws the legend in the image at the bottom-right. The major unit describes the distance in real-world units between two major marks, and the minor unit describes the distance in real-world units between two minor marks. The following is an example of the annotations.

*[Image: cal_draw_legend_example.png]* |
| `M_DRAW_MAJOR_MARKS` | Draws crosses at each major tick mark. The following is an example of the annotations.

*[Image: cal_draw_major_marks_example.png]* |
| `M_DRAW_MINOR_MARKS` | Draws dots at each minor tick mark. The following is an example of the annotations.

*[Image: cal_draw_minor_marks_example.png]* |

*For specifying where the axes will be displayed*

| Value | Description |
| --- | --- |
| `M_ALWAYS_SHOW_AXES` | Specifies to choose a new location for the axes to cross if, . |

*For specifying to draw the calibration error*

| Value | Description |
| --- | --- |
| `M_DRAW_CALIBRATION_ERROR` | Specifies that instead of drawing crosshairs at calibration points, this draws an arrow between the original pixel calibration points and the real-world positions of the calibration points transformed into pixel units using the camera calibration context (associated with destination image, or specified using [`ContextCalOrImageBufId`](../../Reference/cal/McalDraw.md) if the destination image is not calibrated). For example, if you specify [`M_DRAW_IMAGE_POINTS`](../../Reference/cal/McalDraw.md), the arrow is drawn from the calibration points of [`M_DRAW_IMAGE_POINTS`](../../Reference/cal/McalDraw.md) to the corresponding calibration points of [`M_DRAW_WORLD_POINTS`](../../Reference/cal/McalDraw.md). In practice, the error is too small to see, so it can be scaled up using [`McalControl`](../../Reference/cal/McalControl.md) with[`M_DRAW_CALIBRATION_ERROR_SCALE_MODE`](../../Reference/cal/McalControl.md) or [`M_DRAW_CALIBRATION_ERROR_SCALE_FACTOR`](../../Reference/cal/McalControl.md). |

### `Index` *(in, AIL_INT)*

Specifies the index of the pose of an [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context from which the calibration points or fiducial box should be drawn. If this information is not required or supported, set this parameter to [`M_DEFAULT`](../../Reference/cal/McalDraw.md).

*For specifying the index of the pose*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to draw the calibration points of the last pose, for [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration contexts. For all other cases, implements the default behavior. |
| `0 <= Value < Number of poses` | Specifies the index of a camera calibration pose. Use [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_NUMBER_OF_CALIBRATION_POSES`](../../Reference/cal/McalInquire.md) to determine the number of poses.

This setting must be used only when drawing calibration points of a camera calibration context in [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) mode. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
