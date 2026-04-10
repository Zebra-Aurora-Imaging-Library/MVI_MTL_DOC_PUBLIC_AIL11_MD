---
doctype: Reference
module: 3dmap
function: M3dmapCalibrateMultiple
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmap / M3dmapCalibrateMultiple"
---

# M3dmapCalibrateMultiple

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

> Calibrate a 3D reconstruction setup that uses multiple camera-laser pairs.

## Syntax

```c
void M3dmapCalibrateMultiple(
    const AIL_ID * Context3dmapIdArrayPtr,  //in
    const AIL_ID * Result3dmapIdArrayPtr,   //in
    const AIL_ID * ContextCalIdArrayPtr,    //in
    AIL_INT        ArraySize,               //in
    AIL_INT64      ControlFlag              //in
)
```

## Description

This function calibrates your 3D reconstruction setup when using multiple camera-laser pairs.

By identifying the camera-laser pairs using [`M_CAMERA_LABEL()`](../../Reference/3dmap/M3dmapAlloc.md) and [`M_LASER_LABEL()`](../../Reference/3dmap/M3dmapAlloc.md) when allocating the 3D reconstruction context, you can determine which cameras and lasers should share a common 3D reconstruction calibration. When two contexts share the same laser label, they will be constrained to sharing the same laser plane equation. Similarly, when two contexts share the same camera label, they will be constrained to using the same camera calibration. By calling [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md), instead of using multiple calls to [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md), the 3D reconstruction calibrations will be optimized to satisfy the constraints imposed by all the various calibrations.

When constructing the arrays specified by [`Context3dmapIdArrayPtr`](../../Reference/3dmap/M3dmapCalibrateMultiple.md), [`Result3dmapIdArrayPtr`](../../Reference/3dmap/M3dmapCalibrateMultiple.md), and [`ContextCalIdArrayPtr`](../../Reference/3dmap/M3dmapCalibrateMultiple.md), you must establish a one to one relationship between each item; that is, the 3D reconstruction context is calibrated using the camera calibration and extracted laser line data found in the same array position. The three arrays passed to the function must have the same size specified by [`ArraySize`](../../Reference/3dmap/M3dmapCalibrateMultiple.md).

When using [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md), the following setup constraints must be met:

- Each camera calibration context must have been allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md)with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md).
- If the laser planes are not perpendicular to the object's motion (that is, if they are not perfectly vertical), the result buffers must contain laser line data extracted from multiple reference planes (using multiple calls to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) for each result buffer). The laser planes are assumed to be perfectly vertical if a single reference plane is provided for a given laser label.
- The laser lines projected on the XY-plane (Z=0) must be perpendicular to the object's motion.
- The camera must have a sufficient angle with respect to their respective laser planes to have accurate results; that is, the cameras must be able to acquire meaningful displacements of the laser lines for different depths. For more information, see [Camera angle requirement](../../UserGuide/C46_3D_reconstruction_using_laser_line_profiling/S04_Configuring_the_laser_line_profiling_setup.md).
- The camera calibration identifiers at two distinct indices in the [`ContextCalIdArrayPtr`](../../Reference/3dmap/M3dmapCalibrateMultiple.md) array must be the same if and only if the camera labels of the two 3D reconstruction contexts at the same indices in the [`Context3dmapIdArrayPtr`](../../Reference/3dmap/M3dmapCalibrateMultiple.md) array are also the same.

For the steps required to calibrate your 3D reconstruction setup in either mode, see [Calibrating your 3D reconstruction setup to create a point cloud](../../UserGuide/C46_3D_reconstruction_using_laser_line_profiling/S05_Calibrating_your_reconstruction_setup.md).

After calling [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md), you can use [`M3dmapInquire`](../../Reference/3dmap/M3dmapInquire.md) set to [`M_CALIBRATION_STATUS`](../../Reference/3dmap/M3dmapInquire.md) on each 3D reconstruction context to know the results of the 3D reconstruction calibration operation.

- If [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md) is successful, all statuses are set to [`M_CALIBRATED`](../../Reference/3dmap/M3dmapInquire.md).
- If [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md) fails because of a particular 3D reconstruction context, its calibration status is set according to the cause of failure. An Aurora Imaging Library error is also reported. The 3D reconstruction calibration status of the other contexts are set to [`M_NOT_INITIALIZED`](../../Reference/3dmap/M3dmapInquire.md). Apart from the calibration status, the 3D reconstruction contexts are unchanged, and can thus be used in a subsequent call to [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md).
- If [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md) fails during the global optimization phase, the calibration status of all the 3D reconstruction contexts is set to the cause of failure. A different Aurora Imaging Library error is also reported. Apart from the calibration status, the 3D reconstruction contexts are unchanged, and can thus be used in a subsequent call to [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md).
- If [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md) fails because of an invalid parameter passed to the function, the 3D reconstruction contexts remains unchanged (including the 3D reconstruction calibration status). An Aurora Imaging Library error is reported accordingly.

> **Note:** This function can only be called on 3D reconstruction contexts allocated with [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) reconstruction mode.

## Parameters

### `Context3dmapIdArrayPtr` *(in, *AIL_ID)*

Specifies the address of an array containing the identifiers of the 3D reconstruction context used to extract the laser line from grabbed images. The contexts must have been previously allocated on the required system using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md).

### `Result3dmapIdArrayPtr` *(in, *AIL_ID)*

Specifies the address of an array containing the identifiers of the 3D reconstruction result buffers that contains the laser line data required for calibration. The 3D reconstruction result buffers must have been previously allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) and contain data for at least one laser line.

### `ContextCalIdArrayPtr` *(in, *AIL_ID)*

Specifies the address of an array containing the identifiers of the camera calibration contexts of the cameras used to grab the laser lines. The contexts must have been previously allocated on the required system using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) and must be calibrated.

### `ArraySize` *(in, AIL_INT)*

Specifies the number of camera-laser pairs. This parameter must be greater or equal to 1.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter must be set to one of the following values:

*For specifying error reporting in the case of duplicate labels*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. If two or more 3D reconstruction contexts in [`Context3dmapIdArrayPtr`](../../Reference/3dmap/M3dmapCalibrateMultiple.md) have been allocated with the same values for [`M_CAMERA_LABEL()`](../../Reference/3dmap/M3dmapAlloc.md) and [`M_LASER_LABEL()`](../../Reference/3dmap/M3dmapAlloc.md), an error is reported. |
| `M_ALLOW_DUPLICATES` | Specifies to not report an error if two or more 3D reconstruction contexts in [`Context3dmapIdArrayPtr`](../../Reference/3dmap/M3dmapCalibrateMultiple.md) have been allocated with the same values for [`M_CAMERA_LABEL()`](../../Reference/3dmap/M3dmapAlloc.md) and [`M_LASER_LABEL()`](../../Reference/3dmap/M3dmapAlloc.md). |
