---
doctype: Reference
module: 3dmap
function: M3dmapAlignScan
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapAlignScan"
---

# M3dmapAlignScan

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

> Calculates the transformations and information necessary to calibrate a 3D profile sensor.

## Syntax

```c
void M3dmapAlignScan(
    AIL_ID    AlignContext3dmapId,  //in
    AIL_ID    ContainerBufId,       //in
    AIL_ID    AlignResult3dmapId,   //out
    AIL_INT64 ControlFlag           //in
)
```

## Description

This function calculates the transformations necessary to calibrate your 3D profile sensor using an alignment object that was specified using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md).

This function uses a point cloud of an alignment object, with expected markings and constraints, to establish the transformations and information necessary to calibrate the 3D sensor that transmitted the point cloud. The calibration, when applied to the data transmitted by the 3D sensor, corrects the data so that the alignment object is represented correctly.

To retrieve the calculated sensor data, use [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) and [`M3dmapCopyResult`](../../Reference/3dmap/M3dmapCopyResult.md). The sensor data includes values such as [`M_SENSOR_PITCH`](../../Reference/3dmap/M3dmapGetResult.md), [`M_SENSOR_PITCH_BEFORE_YAW`](../../Reference/3dmap/M3dmapGetResult.md), [`M_SENSOR_YAW`](../../Reference/3dmap/M3dmapGetResult.md), [`M_SENSOR_YAW_BEFORE_PITCH`](../../Reference/3dmap/M3dmapGetResult.md), [`M_3D_SHEAR_X`](../../Reference/3dmap/M3dmapGetResult.md), [`M_3D_SHEAR_Z`](../../Reference/3dmap/M3dmapGetResult.md), and [`M_3D_SCALE_Y`](../../Reference/3dmap/M3dmapGetResult.md).

You must use [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) set to [`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md), [`M_DISK`](../../Reference/3dmap/M3dmapControl.md), or [`M_FLAT_SURFACE`](../../Reference/3dmap/M3dmapControl.md) prior to calling this function. When using an alignment pyramid or disk, certain constraints must be met. You must also specify the dimensions of your alignment object before calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md).

- For an alignment disk ([`M_DISK`](../../Reference/3dmap/M3dmapControl.md)):
  - You must specify the dimensions of[`M_HEIGHT`](../../Reference/3dmap/M3dmapControl.md) and [`M_DIAMETER`](../../Reference/3dmap/M3dmapControl.md) using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) before calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md).
  - For information on the alignment disk constraints, see [Alignment disk constraints and requirements](../../UserGuide/C42_Grabbing_from_3D_sensors/S08_Alignment_object_requirements.md).
- For an alignment pyramid ([`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md)):
  - You must specify the dimensions of[`M_HEIGHT`](../../Reference/3dmap/M3dmapControl.md), [`M_PYRAMID_BASE_LENGTH`](../../Reference/3dmap/M3dmapControl.md), and [`M_PYRAMID_TOP_FACE_LENGTH`](../../Reference/3dmap/M3dmapControl.md) using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) before calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md).
  - For information on the alignment pyramid constraints, see [Alignment pyramid constraints and requirements](../../UserGuide/C42_Grabbing_from_3D_sensors/S08_Alignment_object_requirements.md).

The following are the different ways to calibrate a 3D profile sensor with the calculated sensor data.

- You can use [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md) to apply the calculated transformation matrix ([`M3dmapCopyResult`](../../Reference/3dmap/M3dmapCopyResult.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dmap/M3dmapCopyResult.md)) to point clouds acquired by the 3D profile sensor.
- You can use [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) when converting the 3D sensor data to a format that is 3D-processable. To do so, prior to calling [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), set [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_3D_SHEAR_X`](../../Reference/buf/MbufControl.md), [`M_3D_SCALE_Y`](../../Reference/buf/MbufControl.md), [`M_3D_SHEAR_Z`](../../Reference/buf/MbufControl.md) to the values retrieved using [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_3D_SHEAR_X`](../../Reference/3dmap/M3dmapGetResult.md), [`M_3D_SCALE_Y`](../../Reference/3dmap/M3dmapGetResult.md), [`M_3D_SHEAR_Z`](../../Reference/3dmap/M3dmapGetResult.md), respectively.
- You can pass the calculated results (such as pitch, yaw, and shear values) to a compatible 3D profile sensor (such as Zebra AltiZ) using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md). In this case, the 3D profile sensor will directly output calibrated scan lines.

## Parameters

### `AlignContext3dmapId` *(in, AIL_ID)*

Specifies the identifier of the 3D alignment context to use for the alignment operation. The context must have been previously allocated using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_ALIGN_CONTEXT`](../../Reference/3dmap/M3dmapAlloc.md).

### `ContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the 3D-processable point cloud container that stores the point cloud of the alignment object.

### `AlignResult3dmapId` *(out, AIL_ID)*

Specifies the identifier of the 3D alignment result buffer in which to store results. The result buffer must have been previously allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_ALIGN_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
