---
doctype: Reference
module: 3dmap
function: M3dmapControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapControl"
---

# M3dmapControl

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

> Control a 3D alignment context, profiling 3D reconstruction context, draw 3D reconstruction context, or 3D reconstruction result buffer.

## Syntax

```c
void M3dmapControl(
    AIL_ID     ContextOrResult3dmapId,  //out
    AIL_INT    LabelOrIndex,            //in
    AIL_INT64  ControlType,             //in
    AIL_DOUBLE ControlValue             //in
)
```

## Description

This function allows you to control a specified setting of a 3D alignment context, profiling 3D reconstruction context, draw 3D reconstruction context, or 3D reconstruction result buffer.

## Parameters

### `ContextOrResult3dmapId` *(out, AIL_ID)*

Specifies the identifier of the 3D alignment context, profiling 3D reconstruction context, draw 3D reconstruction context, or 3D reconstruction result buffer whose setting to modify. The 3D alignment context, profiling 3D reconstruction context, or draw 3D reconstruction context must have been previously allocated using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_ALIGN_CONTEXT`](../../Reference/3dmap/M3dmapAlloc.md), [`M_LASER`](../../Reference/3dmap/M3dmapAlloc.md), or [`M_DRAW_3D_CONTEXT`](../../Reference/3dmap/M3dmapAlloc.md), respectively. The 3D reconstruction must have been previously allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), or [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md).

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the point cloud(s) in the specified 3D reconstruction result buffer, or the entire result buffer itself, to control. Only 3D reconstruction result buffers allocated using [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) have point clouds that can be specified using this parameter. For all other types of 3D reconstruction contexts and result buffers, set this parameter to `M_DEFAULT`.

*For specifying the point cloud(s) or point cloud container*

| Value | Description |
| --- | --- |
| `M_POINT_CLOUD_INDEX` | Specifies the index of a point cloud in the specified 3D reconstruction result buffer. |
| `M_POINT_CLOUD_LABEL` | Specifies the label of a point cloud in the specified 3D reconstruction result buffer. |
| `M_ALL` | Specifies to apply the control to all point clouds in the specified 3D reconstruction result buffer. |
| `M_GENERAL` | Specifies to control the specified 3D reconstruction result buffer allocated using [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md). |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the setting.

## Parameter Associations

### For a 3D alignment context

The following [`ControlType`](../../Reference/3dmap/M3dmapControl.md) and corresponding [`ControlValue`](../../Reference/3dmap/M3dmapControl.md) parameter settings can be specified for a 3D alignment context ([`M_ALIGN_CONTEXT`](../../Reference/3dmap/M3dmapAlloc.md)).  > **Note:** Measurements that rely on the presence of an alignment object will not be calculated if [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is set to [`M_FLAT_SURFACE`](../../Reference/3dmap/M3dmapControl.md). In this case, [`M_3D_SCALE_Y`](../../Reference/3dmap/M3dmapGetResult.md), [`M_3D_SHEAR_X`](../../Reference/3dmap/M3dmapGetResult.md), [`M_3D_SHEAR_Z`](../../Reference/3dmap/M3dmapGetResult.md), [`M_HOLES_FOUND`](../../Reference/3dmap/M3dmapGetResult.md), [`M_Y_MIRRORED`](../../Reference/3dmap/M3dmapGetResult.md), [`M_RMS_ERROR`](../../Reference/3dmap/M3dmapGetResult.md), [`M_PYRAMID_CORNERS_X`](../../Reference/3dmap/M3dmapGetResult.md), [`M_PYRAMID_CORNERS_Y`](../../Reference/3dmap/M3dmapGetResult.md), and [`M_PYRAMID_CORNERS_Z`](../../Reference/3dmap/M3dmapGetResult.md)will not be retrievable using [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md).

---

### `M_ALIGN_X_POSITION`

Sets whether to calculate the transformation required to align the origin of the working coordinate system with the origin of the alignment object along the X-axis. The alignment disk's origin is at its center; the alignment pyramid's origin is at the center of its truncated pyramid.  *[Image: M3dmap_m_align_x_position.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_OBJECT_ORIGIN` | Specifies to calculate the transformation to align the origin of the working coordinate system with the origin of the alignment object along the X-axis. |
| `M_SAME` *(default)* | Specifies not to calculate the transformation to align the origin of the working coordinate system along the X-axis. The origin of the working coordinate system will remain in its original position. |

---

### `M_ALIGN_XY_DIRECTION`

Sets whether to calculate the transformation required to change the direction of the X- and Y-axes of the working coordinate system.  *[Image: M3dmap_XY_dir.png]*  > **Note:** Note that changing the direction of one axis automatically updates the other axes to maintain the right-hand rule. Also note that Z=0 depends on [`M_ALIGN_Z_POSITION`](../../Reference/3dmap/M3dmapControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to calculate the transformation to align with default direction; this is specific to the type of alignment object.  For the alignment disk ([`M_DISK`](../../Reference/3dmap/M3dmapControl.md)) and alignment surface ([`M_FLAT_SURFACE`](../../Reference/3dmap/M3dmapControl.md)), specifies to use [`M_SAME_X`](../../Reference/3dmap/M3dmapControl.md).  For the alignment pyramid ([`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md)), specifies to use [`M_OBJECT_SAME_X`](../../Reference/3dmap/M3dmapControl.md). |
| `M_OBJECT_REVERSE_X` | Specifies to calculate the transformation to align the X-axis of the working coordinate system with the reverse direction of the X-axis of the alignment pyramid. Depending on the setting of [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), the Y-axis of the working coordinate system will either align with the same or reverse direction of the Y-axis of the alignment pyramid.  > **Note:** This control value is only supported for the alignment pyramid ([`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md)). |
| `M_OBJECT_REVERSE_Y` | Specifies to calculate the transformation to align the Y-axis of the working coordinate system with the reverse direction of the Y-axis of the alignment pyramid. Depending on the setting of [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), the X-axis of the working coordinate system will either align with the same or reverse direction of the X-axis of the alignment pyramid.  > **Note:** This control value is only supported for the alignment pyramid ([`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md)). |
| `M_OBJECT_SAME_X` | Specifies to calculate the transformation to align the X-axis of the working coordinate system with the direction of the X-axis of the alignment pyramid. Depending on the setting of [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), the Y-axis of the working coordinate system will either align with the same or reverse direction of the Y-axis of the alignment pyramid.  > **Note:** This control value is only supported for the alignment pyramid ([`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md)). |
| `M_OBJECT_SAME_Y` | Specifies to calculate the transformation to align the Y-axis of the working coordinate system with the direction of the Y-axis of the alignment pyramid. Depending on the setting of [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), the X-axis of the working coordinate system will either align with the same or reverse direction of the X-axis of the alignment pyramid.  > **Note:** This control value is only supported for the alignment pyramid ([`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md)). |
| `M_REVERSE_X` | Specifies to calculate the transformation required to reverse the direction of the X-axis of the working coordinate system. Depending on the setting of [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), the Y-axis of the working coordinate system will either point in the current or reverse direction.  > **Note:** This control value is only supported for the alignment disk ([`M_DISK`](../../Reference/3dmap/M3dmapControl.md)) and alignment surface ([`M_FLAT_SURFACE`](../../Reference/3dmap/M3dmapControl.md)). |
| `M_REVERSE_Y` | Specifies to calculate the transformation required to reverse the direction of the Y-axis of the working coordinate system. Depending on the setting of [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), the X-axis of the working coordinate system will either point in the current or reverse direction.  > **Note:** This control value is only supported for the alignment disk ([`M_DISK`](../../Reference/3dmap/M3dmapControl.md)) and alignment surface ([`M_FLAT_SURFACE`](../../Reference/3dmap/M3dmapControl.md)). |
| `M_SAME_X` | Specifies to maintain the current direction of the X-axis of the working coordinate system. Depending on the setting of [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), the Y-axis of the working coordinate system will either point in the current or reverse direction.  > **Note:** This control value is only supported for the alignment disk ([`M_DISK`](../../Reference/3dmap/M3dmapControl.md)) and alignment surface ([`M_FLAT_SURFACE`](../../Reference/3dmap/M3dmapControl.md)). |
| `M_SAME_Y` | Specifies to maintain the current direction of the Y-axis of the working coordinate system. Depending on the setting of [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), the X-axis of the working coordinate system will either point in the current or reverse direction.  > **Note:** This control value is only supported for the alignment disk ([`M_DISK`](../../Reference/3dmap/M3dmapControl.md)) and alignment surface ([`M_FLAT_SURFACE`](../../Reference/3dmap/M3dmapControl.md)). |

---

### `M_ALIGN_Y_POSITION`

Sets whether to calculate the transformation required to align the origin of the working coordinate system with the origin of the alignment object along the Y-axis. The alignment disk's origin is at its center; the alignment pyramid's origin is at the center of its truncated pyramid.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_OBJECT_ORIGIN` | Specifies to calculate the transformation to align the origin of the working coordinate system with the origin of the alignment object along the Y-axis. |
| `M_SAME` *(default)* | Specifies not to calculate the transformation to align the origin of the working coordinate system along the Y-axis. The origin of the working coordinate system will remain in its original position. |

---

### `M_ALIGN_Z_DIRECTION`

Sets whether to calculate the transformation required to align the direction of the Z-axis of the working coordinate system.  > **Note:** Note that changing the direction of one axis automatically updates the other axes to maintain the right-hand rule.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REVERSE` | Specifies to calculate the transformation required to reverse the direction of the Z-axis, relative to its original direction. |
| `M_SAME` *(default)* | Specifies not to calculate the transformation required to change the direction of the Z-axis. The current direction of the Z-axis will be maintained. |
| `M_Z_DOWN` | Specifies to calculate the transformation required to point the Z-axis downwards. This means that the Z-coordinates decrease in the direction in which the height increases.  > **Note:** Note that this leads to negative Z-coordinates for the alignment object's surface. |
| `M_Z_UP` | Specifies to calculate the transformation required to point the Z-axis upwards. This means that the Z-coordinates increase in the direction in which the height increase.  > **Note:** Note that this leads to positive Z-coordinates for the alignment object's surface. |

---

### `M_ALIGN_Z_POSITION`

Sets whether to calculate the transformation required to align the origin of the working coordinate system to the top or bottom of the alignment object.  *[Image: M3dmap_m_align_z_position.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_OBJECT_BOTTOM` *(default)* | Specifies to calculate the transformation to align the origin of the working coordinate system with the bottom of the alignment object along the Z-axis. |
| `M_OBJECT_TOP` | Specifies to calculate the transformation to align the origin of the working coordinate system with the top of the alignment object along the Z-axis. |
| `M_SAME` | Specifies not to calculate the transformation to align the origin of the working coordinate system along the Z-axis. The origin of the working coordinate system will remain in its original position. |

---

### `M_CAMERA_TILT_X`

Sets whether to enable camera tilt correction around the X-axis, and in which direction to enable the correction.  > **Note:** Note that this does not apply to the alignment pyramid ([`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md)) for which the tilt direction is automatically deduced.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NEGATIVE` | Specifies that the camera tilt is in the negative direction around the X-axis. |
| `M_POSITIVE` | Specifies that the camera tilt is in the positive direction around the X-axis. |
| `M_ZERO` *(default)* | Specifies that there is no camera tilt around the X-axis. |

---

### `M_DIAMETER`

Sets the diameter of the alignment disk.  > **Note:** Note that if [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is set to[`M_DISK`](../../Reference/3dmap/M3dmapControl.md), you must set[`M_DIAMETER`](../../Reference/3dmap/M3dmapControl.md) to a positive value that is not [`M_UNKNOWN`](../../Reference/3dmap/M3dmapControl.md) before calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_UNKNOWN` *(default)* | Specifies that the diameter of the alignment disk is unknown. |
| `Value > 0.0` | Specifies the diameter of the alignment disk. |

---

### `M_HEIGHT`

Sets the height of the alignment object that was specified with [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the height of the alignment object. |

---

### `M_OBJECT_SHAPE`

Sets the type of alignment object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISK` *(default)* | Specifies a disk-type object for alignment (alignment disk). The alignment disk is a short cylinder (which includes two holes at specific offsets with respect to the origin of the alignment disk).  *[Image: 3dmap_billet_dimensions_ref.png]*  The alignment disk must meet the following constraints:  - The alignment disk must cover at least 50% of the scanned width (X-direction). - The alignment disk must cover at least 30% of the scanned length (Y-direction). - The alignment disk's edge must be fully visible in the scan. - The alignment disk's holes must be at least 30 scan lines in the Y-direction and 30 points in the X-direction. The radii of the holes must be within 5 to 10% of the disk's radius, and the depth of the holes must be at least 20% of the total disk's height. - The floor (background) must be present in the scan. If you are using a stage, ensure its surface is parallel to the motion.  For more information on the constraints and recommended dimension values, see [Alignment cylinder constraints and requirements](../../UserGuide/C42_Grabbing_from_3D_sensors/S08_Alignment_object_requirements.md).  > **Note:** You must define the dimensions of the alignment disk ([`M_DIAMETER`](../../Reference/3dmap/M3dmapControl.md) and [`M_HEIGHT`](../../Reference/3dmap/M3dmapControl.md)) before calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md). |
| `M_FLAT_SURFACE` | Specifies a flat-surface-type object for alignment (alignment surface).  > **Note:** For example, a flat square-like piece of sandblasted aluminum, or an equivalently flat piece of medium-density fiberboard (MDF). |
| `M_PYRAMID` | Specifies a pyramid-type object for alignment (alignment pyramid). The alignment pyramid is a truncated square pyramid on a wider rectangular base that has a chamfer in the bottom right corner. The truncated pyramid is off-center along the length on the wider rectangular base.  *[Image: 3dmap_pyramid_dimensions_ref.png]*  The alignment pyramid must meet the following constraints:  - The top face and base of its truncated pyramid must be squares. - The angle between the top and side faces of its truncated pyramid must be between 35 to 55 degrees (ideally 45 degrees). - Its truncated pyramid's top face length **(A)** must be approximately 50% of the truncated pyramid's bottom base length **(B)**. - The alignment pyramid's wider rectangular base must be approximately 25% wider **(W)** in the X-direction and 50% longer **(L)** in the Y-direction than its truncated pyramid's base **(B)**. - The alignment pyramid must cover at least 50% of the scanned width (X-direction). - The alignment pyramid must cover at least 30% of the scanned length (Y-direction). - Its truncated pyramid's faces (top and sides) and the top of the alignment pyramid's wider rectangular base must each be composed of at least 2000 points. - The alignment pyramid's wider rectangular base must have a chamfer in the bottom right corner **(F)** with a length of at least 30% of the truncated pyramid's top face **(A)** at an approximate angle of 45 degrees.  For more information on the constraints and recommended dimension values, see [Alignment pyramid constraints and requirements](../../UserGuide/C42_Grabbing_from_3D_sensors/S08_Alignment_object_requirements.md).  > **Note:** You must define the dimensions of the alignment pyramid ([`M_PYRAMID_BASE_LENGTH`](../../Reference/3dmap/M3dmapControl.md),[`M_PYRAMID_TOP_FACE_LENGTH`](../../Reference/3dmap/M3dmapControl.md), and[`M_HEIGHT`](../../Reference/3dmap/M3dmapControl.md)) before calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md). |

---

### `M_PYRAMID_BASE_LENGTH`

Sets the length of the truncated pyramid's bottom base when using an alignment pyramid.  *[Image: 3dmap_pyramid_dimensions_bottom_control.png]*  > **Note:** Note that if [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is set to[`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md), you must set[`M_PYRAMID_BASE_LENGTH`](../../Reference/3dmap/M3dmapControl.md) to a positive value that is not [`M_UNKNOWN`](../../Reference/3dmap/M3dmapControl.md) before calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_UNKNOWN` *(default)* | Specifies that the length of the truncated pyramid's bottom base is unknown. |
| `Value > 0.0` | Specifies the length of the truncated pyramid's bottom base. |

---

### `M_PYRAMID_TOP_FACE_LENGTH`

Sets the length of the truncated pyramid's top base (top face) when using an alignment pyramid.  *[Image: 3dmap_pyramid_dimensions_top_control.png]*  > **Note:** Note that if [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is set to[`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md), you must set[`M_PYRAMID_TOP_FACE_LENGTH`](../../Reference/3dmap/M3dmapControl.md) to a positive value that is not [`M_UNKNOWN`](../../Reference/3dmap/M3dmapControl.md) before calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_UNKNOWN` *(default)* | Specifies that the length of the truncated pyramid's top base is unknown. |
| `Value > 0.0` | Specifies the length of the truncated pyramid's top base. |

---

### `M_STEP_LENGTH`

Sets the step length (scan step). The step length is the distance between the scan lines.  > **Note:** Note that [`M_STEP_LENGTH`](../../Reference/3dmap/M3dmapControl.md) has no affect unless [`M_STEP_LENGTH_MODE`](../../Reference/3dmap/M3dmapControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dmap/M3dmapControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the step length value. |

---

### `M_STEP_LENGTH_MODE`

Sets the step length (scan step) mode. The step length is the distance between the scan lines.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL_SIZE_Y` | Specifies that the step length is provided with the container that stores the point cloud of the alignment object.  The [`M_3D_SCALE_Y`](../../Reference/3dmap/M3dmapGetResult.md) result is constrained by the value set using [`M_STEP_LENGTH`](../../Reference/3dmap/M3dmapControl.md). |
| `M_UNKNOWN` *(default)* | Specifies that the step length is unknown.  The [`M_STEP_LENGTH`](../../Reference/3dmap/M3dmapControl.md) and [`M_3D_SCALE_Y`](../../Reference/3dmap/M3dmapGetResult.md) are found by analyzing the point cloud without any constraints. |
| `M_USER_DEFINED` | Specifies that the step length is explicitly set with [`M_STEP_LENGTH`](../../Reference/3dmap/M3dmapControl.md).  The [`M_3D_SCALE_Y`](../../Reference/3dmap/M3dmapGetResult.md) result is constrained by the value set using [`M_STEP_LENGTH`](../../Reference/3dmap/M3dmapControl.md). |

### For a profiling 3D reconstruction context

The following [`ControlType`](../../Reference/3dmap/M3dmapControl.md) and corresponding [`ControlValue`](../../Reference/3dmap/M3dmapControl.md) settings are available for any kind of profiling ([`M_LASER`](../../Reference/3dmap/M3dmapAlloc.md)) 3D reconstruction context:

---

### `M_CORRECTED_DEPTH`

Specifies the Z-coordinate in world units (for point clouds) or gray level (for partially corrected depth maps) used to represent the height of the next reference plane (the next call to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md)), during 3D reconstruction calibration for depth.  If [`ContextOrResult3dmapId`](../../Reference/3dmap/M3dmapControl.md) is a profiling 3D reconstruction context allocated with [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md), this value must be specified in world units. The specified Z-coordinate is the actual height of the reference plane in the next calibration image.  If [`ContextOrResult3dmapId`](../../Reference/3dmap/M3dmapControl.md) is a profiling 3D reconstruction context allocated with [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md), this value must be specified in gray levels. The specified gray level is paired with the position of the laser line over a reference plane in the calibration image. Note that a partially corrected depth map has no coordinate system, and so there is no actual height involved.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= GrayValue <= 254` *(default)* | Specifies the gray level that will be used to represent the height of the next reference plane, when [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) is set to [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md).  For 8-bit depth maps, the range is 0 to 254. Note that 255 is used to indicate an invalid value (no data). |
| `0 <= GrayValue <= 65534` | Specifies the gray level that will be used to represent the height of the next reference plane, when [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) is set to [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md).  For 16-bit depth maps, the range is 0 to 65534. Note that 65535 is used to indicate an invalid value (no data). |
| `ZCoordinateValue` | Specifies the Z-coordinate (in world units) of the next reference plane. Generally, this value will be negative, since the Z-axis typically points downwards and has its origin on the conveyor. |

---

### `M_EXTRACTION_FIXED_POINT`

Sets the number of binary digits used for the fractional part of the gray level values in uncorrected depth maps, when using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) with [`M_LINE_ALREADY_EXTRACTED`](../../Reference/3dmap/M3dmapAddScan.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 7` *(default)* | Specifies the number of binary digits used for the fractional part of gray level values. |

### For a profiling 3D reconstruction context in

The following [`ControlType`](../../Reference/3dmap/M3dmapControl.md) and [`ControlValue`](../../Reference/3dmap/M3dmapControl.md) settings are available only for a profiling 3D reconstruction context in [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) 3D reconstruction mode:

---

### `M_EXTRACTION_CHILD_OFFSET_X`

Sets the X-offset that [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) assumes the laser line image buffer to have relative to the top-left pixel of the image buffer used during camera calibration.  If you supply [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) with a child buffer so that it processes only a subset of the grabbed laser line image, and you are generating a fully corrected depth map, you must specify the X- and Y-offsets of that child buffer, relative to the top-left pixel of the image buffer used during camera calibration. If you know, for example, that the laser line will appear only at certain heights in the grabbed laser line image, using a child buffer can be useful to narrow down the search region and reduce processing time.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the X-offset, in pixels. |

---

### `M_EXTRACTION_CHILD_OFFSET_Y`

Sets the Y-offset that [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) assumes the laser line image buffer to have relative to the top-left pixel of the image buffer used during camera calibration.  If you supply [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) with a child buffer so that it processes only a subset of the grabbed laser line image, and you are generating a fully corrected depth map, you must specify the X- and Y-offsets of that child buffer, relative to the top-left pixel of the image buffer used during camera calibration. If you know, for example, that the laser line will appear only at certain heights in the grabbed laser line image, using a child buffer can be useful to narrow down the search region and reduce processing time.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the Y-offset, in pixels. |

---

### `M_EXTRACTION_RANGE_Z`

Sets the mode that helps determine the range of valid Z-coordinates for extracted points when using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md).  An extracted point that has a Z-coordinate outside of this range is set to **M_INVALID_POINT**.

| Value | Description |
| --- | --- |
| `M_GREATER` | Specifies that the Z-axis range is defined by a lower limit corresponding to [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapControl.md). All points having a Z coordinate greater than [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapControl.md) are kept. |
| `M_IN_RANGE` | Specifies that the Z-axis range is defined by the inside range of a lower limit and a upper limit corresponding to [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapControl.md) and [`M_EXTRACTION_RANGE_Z_LIMIT2`](../../Reference/3dmap/M3dmapControl.md). All points having a Z coordinate greater than or equal to the lower limit and less than or equal to the upper limit are kept.  [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapControl.md) is the lower limit of the Z-coordinate extraction range when it is lower than [`M_EXTRACTION_RANGE_Z_LIMIT2`](../../Reference/3dmap/M3dmapControl.md).  [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapControl.md) is the upper limit of the Z-coordinate extraction range when it is higher than [`M_EXTRACTION_RANGE_Z_LIMIT2`](../../Reference/3dmap/M3dmapControl.md). |
| `M_INFINITE` *(default)* | Specifies that the range covers the entire Z-axis, so all points are kept. |
| `M_LESS` | Specifies that the Z-axis range is defined by a upper limit corresponding to [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapControl.md). All points having a Z coordinate less than [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapControl.md) are kept. |
| `M_OUT_RANGE` | Specifies that the Z-axis range is defined by the outside range of a lower limit and a upper limit corresponding to [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapControl.md) and [`M_EXTRACTION_RANGE_Z_LIMIT2`](../../Reference/3dmap/M3dmapControl.md).  [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapControl.md) is the lower limit of the Z-coordinate extraction range when it is lower than [`M_EXTRACTION_RANGE_Z_LIMIT2`](../../Reference/3dmap/M3dmapControl.md).  [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapControl.md) is the upper limit of the Z-coordinate extraction range when it is higher than [`M_EXTRACTION_RANGE_Z_LIMIT2`](../../Reference/3dmap/M3dmapControl.md). |

---

### `M_EXTRACTION_RANGE_Z_LIMIT1`

Sets the first limit value that determines the range of valid Z-coordinates for extracted points.  This value is ignored when [`M_EXTRACTION_RANGE_Z`](../../Reference/3dmap/M3dmapControl.md) is set to [`M_INFINITE`](../../Reference/3dmap/M3dmapControl.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the first limit value, which can be either the lower limit or upper limit. |

---

### `M_EXTRACTION_RANGE_Z_LIMIT2`

Sets the second limit value that determines the range of valid Z-coordinates for extracted points.  > **Note:** Note that when [`M_EXTRACTION_RANGE_Z`](../../Reference/3dmap/M3dmapControl.md) is set to [`M_INFINITE`](../../Reference/3dmap/M3dmapControl.md), [`M_GREATER`](../../Reference/3dmap/M3dmapControl.md), or [`M_LESS`](../../Reference/3dmap/M3dmapControl.md), this value is ignored.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the second limit value, which can be either the lower limit or upper limit. |

---

### `M_SCAN_SPEED`

Sets the speed and direction of the object being scanned, along the object's plane of motion.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the speed, in world units per frame. Note that if the object is moving in the negative Y-axis, specify a negative value. |

### For a 3D reconstruction result buffer set to

The following [`ControlType`](../../Reference/3dmap/M3dmapControl.md) and [`ControlValue`](../../Reference/3dmap/M3dmapControl.md) settings are available only for a 3D reconstruction result buffer set to [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md), [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), or [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md):

---

### `M_MAX_FRAMES`

Sets the maximum number of scanned laser lines that the 3D reconstruction result buffer should keep internally. Note that, if the value of this control type is changed after it has already been set, laser lines can be lost.  When you specify a 3D reconstruction result buffer of type [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) with [`LabelOrIndex`](../../Reference/3dmap/M3dmapControl.md) set to [`M_GENERAL`](../../Reference/3dmap/M3dmapControl.md)), this control will set the maximum number of scanned laser lines kept by each subsequent point cloud generated in this result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the maximum number of scanned laser lines to keep. |

### For a 3D reconstruction result buffer set to

The following controls apply to [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) 3D reconstruction result buffers, when [`LabelOrIndex`](../../Reference/3dmap/M3dmapControl.md) is set to [`M_GENERAL`](../../Reference/3dmap/M3dmapControl.md).

---

### `M_RESULTS_DISPLACEMENT_MODE`

Sets the displacement mode, which determines whether the coordinates of scanned objects include the movement (displacement) of the conveyor.  By default, the coordinates of scanned objects are stored as though the objects are fixed at their initial position when the first call to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) was made, despite the ongoing movement of the conveyor.  When the coordinates are used (for instance, when returning the coordinates or generating a depth map), the displacement mode specifies whether to include the distance that the conveyor moved since the first call to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md). When you include this distance, you are getting the real-time coordinates of the objects at the moment the coordinates are returned or the depth map is generated.  You can retrieve this conveyor displacement (Y-axis displacement) using [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_TOTAL_DISPLACEMENT_Y`](../../Reference/3dmap/M3dmapGetResult.md).  > **Note:** In some circumstances, such as when manually defining the extraction box when generating a depth map, you might have to keep in mind whether an object's coordinates will include displacement when used. For more information, see [Results displacement mode](../../UserGuide/C46_3D_reconstruction_using_laser_line_profiling/S07_3D_coordinate_systems.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CURRENT` | Specifies that the results include the ongoing Y-axis displacement. The results return the current position of the object on the conveyor.  Specifically, the results will reflect the 3D coordinates at the last call of [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md). |
| `M_FIXED` *(default)* | Specifies that the results do not include the ongoing Y-axis displacement. The resulting coordinates are those of the object as if it were still at the original position on the conveyor, just prior to being scanned.  In this mode, you can use [`M_RESULTS_DISPLACEMENT_Y`](../../Reference/3dmap/M3dmapControl.md) to add a displacement offset to the 3D coordinates. |

---

### `M_RESULTS_DISPLACEMENT_Y`

Sets the Y-axis displacement to add to the resulting 3D coordinates of an object when [`M_RESULTS_DISPLACEMENT_MODE`](../../Reference/3dmap/M3dmapControl.md) is set to [`M_FIXED`](../../Reference/3dmap/M3dmapControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0.0. |
| `Value` | Specifies the Y-axis displacement.  > **Note:** Note that total displacement can be negative depending on the direction of the conveyor. |

### For a draw 3D reconstruction context

The following [`ControlType`](../../Reference/3dmap/M3dmapControl.md) and corresponding [`ControlValue`](../../Reference/3dmap/M3dmapControl.md) settings are available for a draw 3D reconstruction context:

---

### `M_DRAW_LASER_LINE_COORDINATE_SYSTEM`

Sets whether to draw the laser line coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the laser line coordinate system's axes. |
| `M_ENABLE` *(default)* | Specifies to draw the laser line coordinate system's axes. |

---

### `M_DRAW_LASER_PLANE`

Sets whether to draw the laser plane.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the laser plane. |
| `M_ENABLE` *(default)* | Specifies to draw the laser plane. |

---

### `M_DRAW_LASER_PLANE_COLOR_FILL`

Sets the laser plane's fill color.

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
| `M_AUTO_COLOR` *(default)* | Specifies either the color red or the texture image.  If a texture image is specified (using [`M3dmapDraw3d`](../../Reference/3dmap/M3dmapDraw3d.md) with [`LaserPlaneTextureImageBufId`](../../Reference/3dmap/M3dmapDraw3d.md)), the texture image is drawn on the laser plane. Otherwise, the plane is drawn with [`M_COLOR_RED`](../../Reference/3dmap/M3dmapControl.md). The texture image is typically the grabbed image of the laser line. |
| `M_NO_COLOR` | Specifies no color. |
| `M_TEXTURE_IMAGE` | Specifies to use the image passed to [`M3dmapDraw3d`](../../Reference/3dmap/M3dmapDraw3d.md) with [`LaserPlaneTextureImageBufId`](../../Reference/3dmap/M3dmapDraw3d.md), when drawing the laser plane. The texture image is typically the grabbed image of the laser line. Setting [`M_DRAW_LASER_PLANE_COLOR_FILL`](../../Reference/3dmap/M3dmapControl.md) to [`M_TEXTURE_IMAGE`](../../Reference/3dmap/M3dmapControl.md) and then calling [`M3dmapDraw3d`](../../Reference/3dmap/M3dmapDraw3d.md) without passing an image to the [`LaserPlaneTextureImageBufId`](../../Reference/3dmap/M3dmapDraw3d.md) parameter causes an error. |

---

### `M_DRAW_LASER_PLANE_COLOR_OUTLINE`

Sets the laser plane's outline color.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. The red, green, and blue values must be values between 0 and 255, inclusive. When drawing in a 16-bit or 32-bit color buffer, the bands of the RGB value are cast to the type of the destination buffer's bands. To specify a value for a specific band of a 16-bit or 32-bit color buffer, use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/3dmap/M3dmapControl.md) combined with the appropriate constant ([`M_RED`](../../Reference/3dmap/M3dmapControl.md), [`M_GREEN`](../../Reference/3dmap/M3dmapControl.md), or [`M_BLUE`](../../Reference/3dmap/M3dmapControl.md)). |
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
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |
| `M_NO_COLOR` | Specifies no color. |

---

### `M_DRAW_LASER_PLANE_OPACITY`

Sets the laser plane's opacity.  > **Note:** This control type is only available when the laser plane's fill color is set to a uniform color.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the laser plane's opacity. |
