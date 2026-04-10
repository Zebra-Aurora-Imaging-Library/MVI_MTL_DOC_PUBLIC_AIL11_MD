---
doctype: Reference
module: 3dmap
function: M3dmapCalibrate
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapCalibrate"
---

# M3dmapCalibrate

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

> Calibrate a 3D reconstruction setup.

## Syntax

```c
void M3dmapCalibrate(
    AIL_ID    Context3dmapId,  //in
    AIL_ID    Result3dmapId,   //in
    AIL_ID    ContextCalId,    //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function calibrates your 3D reconstruction setup.

If the specified 3D reconstruction context is set to generate fully corrected depth maps ([`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) 3D reconstruction mode), the 3D reconstruction calibration is done using the specified camera calibration context and the extracted laser line data stored in the result buffer. In this mode, the following setup constraints must be met:

- You must specify a 3D-based camera calibration context that was allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md)with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md).
- If the laser plane is not perpendicular to the object's motion (that is, if it is not perfectly vertical), the result buffer must contain laser line data extracted from multiple reference planes (multiple calls to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md)). The laser plane is assumed to be perfectly vertical if a single reference plane is used.
- The laser line on the XY-plane (Z=0) must be perfectly perpendicular to the object's motion.
- The camera must have a sufficient angle with respect to the Z-axis to have accurate results; that is, the camera must be able to grab meaningful displacements of the laser line for different depths. For more information, see [Camera angle requirement](../../UserGuide/C46_3D_reconstruction_using_laser_line_profiling/S04_Configuring_the_laser_line_profiling_setup.md).

If the specified 3D reconstruction context is set to generate partially corrected depth maps ([`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md) 3D reconstruction mode), the 3D reconstruction calibration is done with the extracted laser line data stored in the result buffer. With this type of context, there is no constraint on the orientation of the laser plane; that is, it does not need to be perfectly vertical or perpendicular to the object's motion. You do not need to provide a camera calibration context and the [`ContextCalId`](../../Reference/3dmap/M3dmapCalibrate.md) parameter must be set to [`M_NULL`](../../Reference/3dmap/M3dmapCalibrate.md).

For the steps required to calibrate your 3D reconstruction setup in either mode, see [Calibrating your 3D reconstruction setup to create a point cloud](../../UserGuide/C46_3D_reconstruction_using_laser_line_profiling/S05_Calibrating_your_reconstruction_setup.md).

## Parameters

### `Context3dmapId` *(in, AIL_ID)*

Specifies the identifier of the 3D reconstruction context used to extract the laser line from grabbed images. The context must have been previously allocated on the required system using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md).

### `Result3dmapId` *(in, AIL_ID)*

Specifies the identifier of the 3D reconstruction result buffer that contains the laser line data required for calibration. The 3D reconstruction result buffer must have been previously allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) and contain data for at least one laser line.

### `ContextCalId` *(in, AIL_ID)*

Specifies the identifier of the camera calibration context that contains the 3D camera calibration data or `M_NULL`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
