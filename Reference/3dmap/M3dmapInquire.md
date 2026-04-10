---
doctype: Reference
module: 3dmap
function: M3dmapInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapInquire"
---

# M3dmapInquire

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

> Inquire about a 3D alignment context, profiling 3D reconstruction context, draw 3D reconstruction context, or 3D reconstruction result buffer.

## Syntax

```c
AIL_INT M3dmapInquire(
    AIL_ID    ContextOrResult3dmapId,  //in
    AIL_INT   LabelOrIndex,            //in
    AIL_INT64 InquireType,             //in
    void *    UserVarPtr               //out
)
```

## Description

This function inquires about a specified setting of a 3D alignment context, profiling 3D reconstruction context, draw 3D reconstruction context, or 3D reconstruction result buffer.

If the inquired setting is set to [`M_DEFAULT`](../../Reference/3dmap/M3dmapControl.md), [`M3dmapInquire`](../../Reference/3dmap/M3dmapInquire.md) will return [`M_DEFAULT`](../../Reference/3dmap/M3dmapInquire.md). To inquire the actual default value, add [`M_DEFAULT`](../../Reference/3dmap/M3dmapInquire.md) to the [`InquireType`](../../Reference/3dmap/M3dmapInquire.md) parameter.

An [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) 3D reconstruction result buffer contains an array of distinct point clouds. You can retrieve results from an individual point cloud in the result buffer, from an aggregate of all point clouds in the result buffer, or from the result buffer itself using the [`LabelOrIndex`](../../Reference/3dmap/M3dmapInquire.md) parameter.

## Parameters

### `ContextOrResult3dmapId` *(in, AIL_ID)*

Specifies the identifier of the 3D alignment context, profiling 3D reconstruction context, draw 3D reconstruction context, or 3D reconstruction result buffer about which to inquire information.

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the point cloud(s) in the result buffer about which to inquire, or specifies the entire result buffer itself. Only 3D reconstruction result buffers allocated using [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) have point clouds that can be specified using this parameter. For other types of 3D reconstruction contexts and result buffers, as well as for 3D draw contexts, set this parameter to `M_DEFAULT`.

*For specifying the point cloud(s) or point cloud result buffer*

| Value | Description |
| --- | --- |
| `M_POINT_CLOUD_INDEX` | Specifies the index of a point cloud in the specified 3D reconstruction result buffer about which to inquire. |
| `M_POINT_CLOUD_LABEL` | Specifies the label of a point cloud in the specified 3D reconstruction result buffer about which to inquire. |
| `M_ALL` | Specifies to inquire all point clouds in the specified 3D reconstruction result buffer. |
| `M_GENERAL` | Specifies to inquire the specified 3D reconstruction result buffer allocated using [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md). |

### `InquireType` *(in, AIL_INT64)*

Specifies the setting to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`M3dmapInquire`](../../Reference/3dmap/M3dmapInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about the system

To inquire about the system on which the profiling 3D reconstruction context, draw 3D reconstruction context, or result buffer has been allocated, set the [`InquireType`](../../Reference/3dmap/M3dmapInquire.md) parameter to the value below.

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the profiling 3D reconstruction context, draw 3D reconstruction context, or result buffer has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### For a 3D alignment context

For a 3D alignment context, the [`InquireType`](../../Reference/3dmap/M3dmapInquire.md) parameter can be set to one of the following:

---

### `M_ALIGN_X_POSITION`

Inquires whether to calculate the transformation required to align the origin of the working coordinate system with the center of the alignment object along the X-axis.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_OBJECT_ORIGIN` | Specifies to calculate the transformation to align the origin of the working coordinate system with the origin of the alignment object along the X-axis. |
| `M_SAME` *(default)* | Specifies not to calculate the transformation to align the origin of the working coordinate system along the X-axis. |

---

### `M_ALIGN_XY_DIRECTION`

Inquires the direction of the X- and Y-axes of the working coordinate system relative to its original direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to calculate the transformation to align with default direction; this is specific to the type of alignment object. |
| `M_OBJECT_REVERSE_X` | Specifies to calculate the transformation to align the X-axis of the working coordinate system with the reverse direction of the X-axis of the alignment pyramid. |
| `M_OBJECT_REVERSE_Y` | Specifies to calculate the transformation to align the Y-axis of the working coordinate system with the reverse direction of the Y-axis of the alignment pyramid. |
| `M_OBJECT_SAME_X` | Specifies to calculate the transformation to align the X-axis of the working coordinate system with the direction of the X-axis of the alignment pyramid. |
| `M_OBJECT_SAME_Y` | Specifies to calculate the transformation to align the Y-axis of the working coordinate system with the direction of the Y-axis of the alignment pyramid. |
| `M_REVERSE_X` | Specifies to calculate the transformation required to reverse the direction of the X-axis of the working coordinate system. |
| `M_REVERSE_Y` | Specifies to calculate the transformation required to reverse the direction of the Y-axis of the working coordinate system. |
| `M_SAME_X` | Specifies to maintain the current direction of the X-axis of the working coordinate system. |
| `M_SAME_Y` | Specifies to maintain the current direction of the Y-axis of the working coordinate system. |

---

### `M_ALIGN_Y_POSITION`

Inquires whether to calculate the transformation required to align the origin of the working coordinate system with the center of the alignment object along the Y-axis.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_OBJECT_ORIGIN` | Specifies to calculate the transformation to align the origin of the working coordinate system with the origin of the alignment object along the Y-axis. |
| `M_SAME` *(default)* | Specifies not to calculate the transformation to align the origin of the working coordinate system along the Y-axis. |

---

### `M_ALIGN_Z_DIRECTION`

Inquires the direction of the Z-axis of the working coordinate system.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REVERSE` | Specifies to calculate the transformation required to reverse the direction of the Z-axis, relative to its original direction. |
| `M_SAME` *(default)* | Specifies not to calculate the transformation required to change the direction of the Z-axis. |
| `M_Z_DOWN` | Specifies to calculate the transformation required to point the Z-axis downwards. |
| `M_Z_UP` | Specifies to calculate the transformation required to point the Z-axis upwards. |

---

### `M_ALIGN_Z_POSITION`

Inquires whether to calculate the transformation required to align the origin of the working coordinate system with respect to the top or bottom of the alignment object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_OBJECT_BOTTOM` *(default)* | Specifies to calculate the transformation to align the origin of the working coordinate system with the bottom of the alignment object along the Z-axis. |
| `M_OBJECT_TOP` | Specifies to calculate the transformation to align the origin of the working coordinate system with the top of the alignment object along the Z-axis. |
| `M_SAME` | Specifies not to calculate the transformation to align the origin of the working coordinate system along the Z-axis. |

---

### `M_CAMERA_TILT_X`

Inquires whether to enable camera tilt correction around the X-axis, and in which direction to enable the correction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NEGATIVE` | Specifies that the camera tilt is in the negative direction around the X-axis. |
| `M_POSITIVE` | Specifies that the camera tilt is in the positive direction around the X-axis. |
| `M_ZERO` *(default)* | Specifies that there is no camera tilt around the X-axis. |

---

### `M_DIAMETER`

Inquires the diameter of the alignment disk.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_UNKNOWN` *(default)* | Specifies that the diameter of the alignment disk is unknown. |
| `Value > 0.0` | Specifies the diameter of the alignment disk. |

---

### `M_HEIGHT`

Inquires the height of the alignment object that was specified with [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the height of the alignment object. |

---

### `M_OBJECT_SHAPE`

Inquires the type of alignment object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISK` *(default)* | Specifies a disk-type object for alignment (alignment disk). |
| `M_FLAT_SURFACE` | Specifies a flat-surface-type object for alignment (alignment surface). |
| `M_PYRAMID` | Specifies a pyramid-type object for alignment (alignment pyramid). |

---

### `M_PYRAMID_BASE_LENGTH`

Inquires the length of the pyramid's bottom base for the alignment pyramid.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_UNKNOWN` *(default)* | Specifies that the length of the truncated pyramid's bottom base is unknown. |
| `Value > 0.0` | Specifies the length of the truncated pyramid's bottom base. |

---

### `M_PYRAMID_TOP_FACE_LENGTH`

Inquires the length of the pyramid's top base (top face) for the alignment pyramid.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_UNKNOWN` *(default)* | Specifies that the length of the truncated pyramid's top base is unknown. |
| `Value > 0.0` | Specifies the length of the truncated pyramid's top base. |

---

### `M_STEP_LENGTH`

Inquires the step length (scan step). The step length is distance between the scan lines.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the step length value. |

---

### `M_STEP_LENGTH_MODE`

Inquires the step length (scan step) mode. The step length is distance between the scan lines.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL_SIZE_Y` | Specifies that the step length is provided with the container that stores the point cloud of the alignment object. |
| `M_UNKNOWN` *(default)* | Specifies that the step length is unknown. |
| `M_USER_DEFINED` | Specifies that the step length is explicitly set with [`M_STEP_LENGTH`](../../Reference/3dmap/M3dmapInquire.md). |

### For 3D reconstruction contexts used for laser line profiling

For a profiling 3D reconstruction context of type [`M_LASER`](../../Reference/3dmap/M3dmapAlloc.md), the [`InquireType`](../../Reference/3dmap/M3dmapInquire.md) parameter can be set to one of the following:

---

### `M_CALIBRATION_DEPTHS`

Inquires the calibration depths used to calibrate the profiling 3D reconstruction context. These are the values given specified using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_CORRECTED_DEPTH`](../../Reference/3dmap/M3dmapInquire.md) before each call to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the calibration depths used to calibrate the profiling 3D reconstruction context. |

---

### `M_CALIBRATION_STATUS`

Inquires the status of the profiling 3D reconstruction calibration.

| Value | Description |
| --- | --- |
| `M_CALIBRATED` | Specifies that a successful call to [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) has been made and the context can now be used to produce calibrated data. |
| `M_GLOBAL_OPTIMIZATION_ERROR` | Specifies that the global optimization phase of [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md) failed because of a mathematical exception. |
| `M_INTERNAL_ERROR` | Specifies that an unexpected error occurred. |
| `M_LASER_LINE_NOT_DETECTED` | Specifies that the laser line could not be extracted from the image passed as input to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md). |
| `M_MATHEMATICAL_EXCEPTION` | Specifies that [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) could not calibrate properly. Verify that all function parameters, control type settings, and images are consistent. |
| `M_NOT_ENOUGH_MEMORY` | Specifies that there was not enough memory for [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) to complete its task. |
| `M_NOT_INITIALIZED` | Specifies that the context is not calibrated. [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) has not been called. |

---

### `M_CAMERA_IMAGE_SIZE_X`

Inquires the X-size of the image(s) with which this profiling 3D reconstruction context was calibrated. This inquire type is valid only after a successful call to [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) on this profiling 3D reconstruction context.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the width of the image with which this profiling 3D reconstruction context was calibrated. The width is rounded to the nearest integer. |

---

### `M_CAMERA_IMAGE_SIZE_Y`

Inquires the Y-size of the image(s) with which this profiling 3D reconstruction context was calibrated. This inquire type is valid only after a successful call to [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) on this profiling 3D reconstruction context.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the height of the image with which this profiling 3D reconstruction context was calibrated. The height is rounded to the nearest integer. |

---

### `M_CORRECTED_DEPTH`

Inquires depth information about the next laser line image added.  For a profiling 3D reconstruction context allocated using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md), this inquire type inquires the gray level corresponding to the depth represented by the next laser line image added (using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md)) when calibrating your 3D reconstruction setup.  For a profiling 3D reconstruction context allocated using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md), this inquire type inquires the depth in world units represented by the next laser line image added (using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md)) when calibrating your 3D reconstruction setup.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= GrayValue <= 254` *(default)* | Specifies the gray level that will be used to represent the height of the next reference plane, when [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) is set to [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md). |
| `0 <= GrayValue <= 65534` | Specifies the gray level that will be used to represent the height of the next reference plane, when [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) is set to [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md). |
| `ZCoordinateValue` | Specifies the Z-coordinate (in world units) of the next reference plane. |

---

### `M_EXTRACTION_FIXED_POINT`

Inquires the number of binary digits used for the fractional part of the gray level in the uncorrected depth map, when using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) with [`M_LINE_ALREADY_EXTRACTED`](../../Reference/3dmap/M3dmapAddScan.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 7` *(default)* | Specifies the number of binary digits used for the fractional part of gray level values. |

---

### `M_LASER_CONTEXT_TYPE`

Inquires the profiling 3D reconstruction mode, which determines whether the context can associate with a camera calibration and create a fully corrected depth map.

| Value | Description |
| --- | --- |
| `M_CALIBRATED_CAMERA_LINEAR_MOTION` | Specifies that the 3D reconstruction context will include camera calibration information and depth correction information. |
| `M_DEPTH_CORRECTION` | Specifies that the 3D reconstruction context will include depth correction information, but will not include camera calibration information. |
| `M_CAMERA_LABEL` | Specifies the label for the camera used by this 3D reconstruction context. |
| `M_LASER_LABEL` | Specifies the label for the laser used by this 3D reconstruction context. |

---

### `M_LOCATE_PEAK_1D_CONTEXT_ID`

Inquires the identifier of the internal locate peak 1D context within the profiling 3D reconstruction context.

---

### `M_NUMBER_OF_CALIBRATION_DEPTHS`

Inquires the number of calibration depths used to calibrate the profiling 3D reconstruction context. This inquire type is valid only after a successful call to [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of calibration depths used to calibrate the profiling 3D reconstruction context. Only integer values are accepted. |

### For profiling 3D reconstruction contexts set to

For profiling 3D reconstruction contexts set to [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md), the [`InquireType`](../../Reference/3dmap/M3dmapInquire.md) parameter can be set to one of the following:

---

### `M_ASSUMED_PERPENDICULAR_TO_MOTION`

Inquires whether the laser plane was assumed to be perpendicular to the object's motion because a single reference plane was provided.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the laser plane was not assumed perpendicular to the object's motion. |
| `M_TRUE` | Specifies that the laser plane was assumed perpendicular to the object's motion. |

---

### `M_EXTRACTION_CHILD_OFFSET_X`

Inquires the X-offset that [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) assumes the laser line image buffer to have relative to the top-left pixel of the image buffer used during camera calibration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the X-offset, in pixels. |

---

### `M_EXTRACTION_CHILD_OFFSET_Y`

Inquires the Y-offset that [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) assumes the laser line image buffer to have relative to the top-left pixel of the image buffer used during camera calibration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the Y-offset, in pixels. |

---

### `M_EXTRACTION_RANGE_Z`

Inquires the mode that helps determine the range of valid Z-coordinates for extracted points when using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md).

| Value | Description |
| --- | --- |
| `M_GREATER` | Specifies that the Z-axis range is defined by a lower limit corresponding to [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapInquire.md). |
| `M_IN_RANGE` | Specifies that the Z-axis range is defined by the inside range of a lower limit and a upper limit corresponding to [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapInquire.md) and [`M_EXTRACTION_RANGE_Z_LIMIT2`](../../Reference/3dmap/M3dmapInquire.md). |
| `M_INFINITE` *(default)* | Specifies that the range covers the entire Z-axis, so all points are kept. |
| `M_LESS` | Specifies that the Z-axis range is defined by a upper limit corresponding to [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapInquire.md). |
| `M_OUT_RANGE` | Specifies that the Z-axis range is defined by the outside range of a lower limit and a upper limit corresponding to [`M_EXTRACTION_RANGE_Z_LIMIT1`](../../Reference/3dmap/M3dmapInquire.md) and [`M_EXTRACTION_RANGE_Z_LIMIT2`](../../Reference/3dmap/M3dmapInquire.md). |

---

### `M_EXTRACTION_RANGE_Z_LIMIT1`

Inquires the first limit value that determines the range of valid Z-coordinates for extracted points.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the first limit value, which can be either the lower limit or upper limit. |

---

### `M_EXTRACTION_RANGE_Z_LIMIT2`

Inquires the first limit value that determines the range of valid Z-coordinates for extracted points.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the second limit value, which can be either the lower limit or upper limit. |

---

### `M_FIT_RMS_ERROR`

Inquires the root mean squared error.  This inquire type returns the root mean squared error of all inlier 3D points used to fit the laser plane to the calibration laser lines upon calling [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md). For more information, consult [Inspecting laser line calibration](../../UserGuide/C46_3D_reconstruction_using_laser_line_profiling/S05_Calibrating_your_reconstruction_setup.md). In this case, this inquire type is only supported after a successful call to [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the root mean square error. |

---

### `M_LASER_PLANE_A`

Inquires the coefficient _a_ of the laser plane equation, `ax + by + cz + d = 0`.

| Value | Description |
| --- | --- |
| `Value` | Specifies the coefficient _a_ of the laser plane equation. |

---

### `M_LASER_PLANE_B`

Inquires the coefficient _b_ of the laser plane equation, `ax + by + cz + d = 0`.

| Value | Description |
| --- | --- |
| `Value` | Specifies the coefficient _b_ of the laser plane equation. |

---

### `M_LASER_PLANE_C`

Inquires the coefficient _c_ of the laser plane equation, `ax + by + cz + d = 0`.

| Value | Description |
| --- | --- |
| `Value` | Specifies the coefficient _c_ of the laser plane equation. |

---

### `M_LASER_PLANE_D`

Inquires the coefficient _d_ of the laser plane equation, `ax + by + cz + d = 0`.

| Value | Description |
| --- | --- |
| `Value` | Specifies the coefficient _d_ of the laser plane equation. |

---

### `M_SCAN_SPEED`

Inquires the speed of the object being scanned. Note that this value is negative if the object is moving away from the camera.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the speed, in world units per frame. |

### For profiling 3D reconstruction contexts set to

For profiling 3D reconstruction contexts set to [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md), the [`InquireType`](../../Reference/3dmap/M3dmapInquire.md) parameter can be set to one of the following:

---

### `M_NUMBER_OF_COLUMNS`

Inquires the number of columns in the uncorrected depth map used for calibration. This corresponds to either the X-size or Y-size of the laser line images passed to the 3D reconstruction result buffer, depending on the [`M_SCAN_LANE_DIRECTION`](../../Reference/3dmap/M3dmapGetResult.md) setting.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of columns. |

---

### `M_NUMBER_OF_COLUMNS_WITH_INVERSIONS`

Inquires the number of columns in the uncorrected depth map used for calibration, for which at least one inversion occurred.  To determine the fraction of columns or rows of the camera image that were not ideally calibrated because of inversions, divide the value returned using [`M_NUMBER_OF_COLUMNS_WITH_INVERSIONS`](../../Reference/3dmap/M3dmapInquire.md) by [`M_NUMBER_OF_COLUMNS`](../../Reference/3dmap/M3dmapInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of columns. |

---

### `M_NUMBER_OF_COLUMNS_WITH_MISSING_DATA`

Inquires the number of columns in the uncorrected depth map used for calibration, for which at least one calibration laser line was not detected.  To determine the fraction of columns or rows of the camera image that were not ideally calibrated because of missing data, divide the value returned using [`M_NUMBER_OF_COLUMNS_WITH_MISSING_DATA`](../../Reference/3dmap/M3dmapInquire.md) by [`M_NUMBER_OF_COLUMNS`](../../Reference/3dmap/M3dmapInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of columns. |

---

### `M_NUMBER_OF_INVERSIONS_PER_COLUMN`

Inquires the number of inversions, per column, in the uncorrected depth map used for calibration.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of inversions per column. |

---

### `M_NUMBER_OF_MISSING_DATA_PER_COLUMN`

Inquires the number of missing data points per column in the uncorrected depth map used for calibration.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of missing data points per column. |

### For a 3D reconstruction result buffer set to

For a point cloud container (3D reconstruction result buffer of type [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) and [`LabelOrIndex`](../../Reference/3dmap/M3dmapInquire.md) set to [`M_GENERAL`](../../Reference/3dmap/M3dmapInquire.md)), the [`InquireType`](../../Reference/3dmap/M3dmapInquire.md) parameter can be set to one of the following:

---

### `M_NUMBER_OF_POINT_CLOUDS`

Inquires the number of point clouds in the specified 3D reconstruction result buffer.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of point clouds. |

---

### `M_RESULTS_DISPLACEMENT_MODE`

Inquires the displacement mode, which determines how the 3D coordinates of a scanned object are returned with respect to the ongoing movement (displacement) of the conveyor.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CURRENT` | Specifies that the results include the ongoing Y-axis displacement. |
| `M_FIXED` *(default)* | Specifies that the results do not include the ongoing Y-axis displacement. |

---

### `M_RESULTS_DISPLACEMENT_Y`

Inquires the specified Y-axis displacement added to resulting 3D coordinates when [`M_RESULTS_DISPLACEMENT_MODE`](../../Reference/3dmap/M3dmapInquire.md) is set to [`M_FIXED`](../../Reference/3dmap/M3dmapInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0.0. |
| `Value` | Specifies the Y-axis displacement. |

### For a point cloud

For a point cloud (3D reconstruction result buffer of type [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) and [`LabelOrIndex`](../../Reference/3dmap/M3dmapInquire.md) set to **M_POINT_CLOUD_INDEX()**, **M_POINT_CLOUD_LABEL()**, or [`M_ALL`](../../Reference/3dmap/M3dmapInquire.md)), the [`InquireType`](../../Reference/3dmap/M3dmapInquire.md) parameter can be set to one of the following:

---

### `M_CAMERA_LABEL_VALUE`

Inquires the camera label of the 3D reconstruction context associated with the specified point cloud.

| Value | Description |
| --- | --- |
| `1 <= Value <= 1023` | Specifies the camera label value. The default value is 1. |

---

### `M_LASER_LABEL_VALUE`

Inquires the laser label of the 3D reconstruction context associated with the specified point cloud.

| Value | Description |
| --- | --- |
| `1 <= Value <= 2047` | Specifies the laser label value. The default value is 1. |

---

### `M_POINT_CLOUD_INDEX_VALUE`

Inquires the index of the point cloud with the specified label.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that there is no point cloud in the 3D reconstruction result buffer with the specified label. |
| `0 <= Point cloud array index <= 134217726` | Specifies the index of the point cloud. |

---

### `M_POINT_CLOUD_LABEL_VALUE`

Inquires the label of the point cloud with the specified index.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that there is no point cloud in the 3D reconstruction result buffer with the specified index. |
| `1 <= Point cloud label <= 134217727` | Specifies the label of the point cloud. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### For a draw 3D reconstruction context

For a draw 3D reconstruction context.

---

### `M_DRAW_ABSOLUTE_COORDINATE_SYSTEM`

Inquires whether to draw the absolute coordinate system's axes.

| Value | Description |
| --- | --- |
| *(see [`M_DRAW_ABSOLUTE_COORDINATE_SYSTEM`](Reference/3dmap/M3dmapControl.md))* |  |

---

### `M_DRAW_CAMERA_COORDINATE_SYSTEM`

Inquires whether to draw the camera coordinate system's axes.

| Value | Description |
| --- | --- |
| *(see [`M_DRAW_CAMERA_COORDINATE_SYSTEM`](Reference/3dmap/M3dmapControl.md))* |  |

---

### `M_DRAW_CAMERA_COORDINATE_SYSTEM_NAME`

Inquires the name to draw for the camera coordinate system; returns "Camera" if no name was specified.

---

### `M_DRAW_COORDINATE_SYSTEM_LENGTH`

Inquires the drawn length of the specified coordinate system's axes.

| Value | Description |
| --- | --- |
| *(see [`M_DRAW_COORDINATE_SYSTEM_LENGTH`](Reference/3dmap/M3dmapControl.md))* |  |

---

### `M_DRAW_FRUSTUM`

Inquires whether to draw the frustum.

| Value | Description |
| --- | --- |
| *(see [`M_DRAW_FRUSTUM`](Reference/3dmap/M3dmapControl.md))* |  |

---

### `M_DRAW_FRUSTUM_COLOR`

Inquires the frustum's color.

| Value | Description |
| --- | --- |
| *(see [`M_DRAW_FRUSTUM_COLOR`](Reference/3dmap/M3dmapControl.md))* |  |

---

### `M_DRAW_LASER_LINE_COORDINATE_SYSTEM`

Inquires whether to draw the laser line coordinate system's axes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the laser line coordinate system's axes. |
| `M_ENABLE` *(default)* | Specifies to draw the laser line coordinate system's axes. |

---

### `M_DRAW_LASER_PLANE`

Inquires how to draw the laser plane.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the laser plane. |
| `M_ENABLE` *(default)* | Specifies to draw the laser plane. |

---

### `M_DRAW_LASER_PLANE_COLOR_FILL`

Inquires the laser plane's fill color.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_COLOR` *(default)* | Specifies either the color red or the texture image. |
| `M_NO_COLOR` | Specifies no color. |
| `M_TEXTURE_IMAGE` | Specifies to use the image passed to [`M3dmapDraw3d`](../../Reference/3dmap/M3dmapDraw3d.md) with [`LaserPlaneTextureImageBufId`](../../Reference/3dmap/M3dmapDraw3d.md), when drawing the laser plane. |

---

### `M_DRAW_LASER_PLANE_COLOR_OUTLINE`

Inquires the laser plane's outline color.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |
| `M_NO_COLOR` | Specifies no color. |

---

### `M_DRAW_LASER_PLANE_OPACITY`

Inquires the laser plane's opacity.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the laser plane's opacity. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### For inquiring the maximum number of frames

For a 3D reconstruction result buffer of type [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), or [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md), the [`InquireType`](../../Reference/3dmap/M3dmapInquire.md) parameter can be set to one of the following:

---

### `M_MAX_FRAMES`

Inquires the maximum number of scanned laser lines that the result buffer keeps internally.  When you specify a result buffer of type [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) with [`LabelOrIndex`](../../Reference/3dmap/M3dmapInquire.md) set to [`M_GENERAL`](../../Reference/3dmap/M3dmapInquire.md), this inquire type returns the maximum number of scanned laser lines that will be used for each subsequent point cloud created in this 3D reconstruction result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the maximum number of scanned laser lines to keep. |

### Combination Constants — For inquiring the default value of an inquire type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

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

Note that the variables _x_, _y_, _z_, and _d_ are expressed in world units in the absolute coordinate system, while the coefficients _a_, _b_, and _c_are dimensionless.

This inquire type is only supported after a successful call to [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) or [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md).
