---
doctype: UserGuide
part: "3D related information"
chapter: Grabbing_from_3D_sensors
section: Steps_to_calibrate_3D_profile_sensor
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Grabbing_from_3D_sensors / Steps to calibrate 3D profile sensor"
---

# Correcting for 3D profile sensor placement

Correcting for the placement of your 3D profile sensor is an important step in your setup. Ideally, your 3D profile sensor should be setup so that the direction of motion is perpendicular to the laser plane and scale in the Y-direction reflects the distance between profiles. If this is not possible, you will need to align your 3D profile sensor to account for the pitch, scale, and yaw distortions that will result from such a setup.

*[Image: 3dmap_perpendicular.png]*

To correct for your 3D profile sensor's placement, you can use [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) to establish the adjustments that must be made. You can then use these results to modify 3D profile sensor settings or to transform the point cloud acquired from the 3D profile sensor. [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) uses the point cloud of an alignment disk or alignment pyramid (with expected proportions) in the target setup to establish the transformations required to align the setup.

*[Image: Alignment_disk_UG.png]*

## Steps to correct for 3D profile sensor placement

The following steps provide a basic methodology for correcting the placement of your 3D profile sensor setup:

1. Grab a point cloud of the alignment object (alignment disk or alignment pyramid) in the target setup, using [`MdigGrab`](../../Reference/dig/MdigGrab.md). For information on the requirements for the alignment object, see [Alignment object constraints and requirements](S08_Alignment_object_requirements.md).
2. If necessary, call [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) to convert the grabbed point cloud if it is not 3D processable.
3. Allocate both a 3D alignment context and 3D alignment result buffer using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_ALIGN_CONTEXT`](../../Reference/3dmap/M3dmapAlloc.md) and [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_ALIGN_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md), respectively.
4. Specify the type of alignment object:
   - For an alignment disk, set the object type using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) set to [`M_DISK`](../../Reference/3dmap/M3dmapControl.md).
   - For an alignment pyramid, set the object type using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) set to [`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md).
5. Set the dimensions of the alignment object:
   - For an alignment disk, set the diameter and height using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_DIAMETER`](../../Reference/3dmap/M3dmapControl.md) and [`M_HEIGHT`](../../Reference/3dmap/M3dmapControl.md), respectively.
   - For an alignment pyramid, set the pyramid base length, pyramid top face length, and height using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_PYRAMID_BASE_LENGTH`](../../Reference/3dmap/M3dmapControl.md), [`M_PYRAMID_TOP_FACE_LENGTH`](../../Reference/3dmap/M3dmapControl.md), and [`M_HEIGHT`](../../Reference/3dmap/M3dmapControl.md), respectively.
6. If you know the distance between scan lines (scan step) in the Y-direction and you don't want it to be calculated, you can manually set it using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md); first set [`M_STEP_LENGTH_MODE`](../../Reference/3dmap/M3dmapControl.md) to [`M_USER_DEFINED`](../../Reference/3dmap/M3dmapControl.md), and then set [`M_STEP_LENGTH`](../../Reference/3dmap/M3dmapControl.md) to the required value. If using a linear/rotary encoder in your 3D profile sensor setup, the scan step is typically the step of the encoder.
7. Call [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) to calculate the transformations necessary to correct the alignment of your 3D profile sensor.
8. Check the status of the alignment operation's calculation with [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) using [`M_STATUS`](../../Reference/3dmap/M3dmapGetResult.md). The status tells you if the calculation was successful or explains why if it did not complete successfully. If the retrieved [`M_STATUS`](../../Reference/3dmap/M3dmapGetResult.md) is not [`M_COMPLETE`](../../Reference/3dmap/M3dmapGetResult.md), make sure your setup is following the constraints outlined in [Alignment object constraints and requirements](S08_Alignment_object_requirements.md) before attempting other corrective measures.
   > **Note:** You can also verify that the holes on the alignment disk were properly found and taken into consideration during the calculation using [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_HOLES_FOUND`](../../Reference/3dmap/M3dmapGetResult.md).
9. Once the transformations to correct the 3D profile sensor data is calculated, you can correct your setup using the following different ways:
   - You can use [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md) to apply the calculated transformation matrix ([`M3dmapCopyResult`](../../Reference/3dmap/M3dmapCopyResult.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dmap/M3dmapCopyResult.md) or [`M_XY_TRANSFORMATION_MATRIX`](../../Reference/3dmap/M3dmapCopyResult.md)) to point clouds acquired by the 3D profile sensor.
   - You can use [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) when converting the 3D sensor data to a format that is 3D-processable. To do so, prior to calling [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), set [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_3D_SHEAR_X`](../../Reference/buf/MbufControl.md), [`M_3D_SCALE_Y`](../../Reference/buf/MbufControl.md), [`M_3D_SHEAR_Z`](../../Reference/buf/MbufControl.md) to the values retrieved using [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_3D_SHEAR_X`](../../Reference/3dmap/M3dmapGetResult.md), [`M_3D_SCALE_Y`](../../Reference/3dmap/M3dmapGetResult.md), [`M_3D_SHEAR_Z`](../../Reference/3dmap/M3dmapGetResult.md), respectively.
   - You can pass the calculated results (such as pitch, yaw, and shear values from [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md)) to a compatible 3D profile sensor (such as Zebra AltiZ) using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md). In this case, the 3D profile sensor will directly output corrected scan lines (Y-axis).
10. Free your allocated objects, using [`M3dmapFree`](../../Reference/3dmap/M3dmapFree.md), unless [`M_UNIQUE_ID`](../../Reference/3dmap/M3dmapAlloc.md) was specified during allocation.

## Types of distortions

Depending on your 3D profile sensor setup, you might need to correct for one or more of the following distortion types.

- **Pitch** (shear between the Z-axis and Y-axis).
- **Scale** (scale distortions in Y cause shrinking or stretching).
- **Yaw** (shear between the X-axis and the Y-axis).

### Pitch

Pitch around the X-axis will cause a shear between the Z-axis and the Y-axis in the scanned scene, which will produce an inaccurate thickness seen in the alignment disk (depth/Z-values). The image below provides a visualization of pitch and shear in the Z-direction.

*[Image: 3dmap_shear_in_Z.png]*

Typically, Z-values along the Y-axis are reported as if the 3D profile sensor is perfectly parallel to the plane of motion, not taking into account the pitch and that certain parts appear further. This can be seen with the lack of visible background gradient in the incorrect graphic (shown above), while a gradient is present in the corrected graphic, showing that the pitch has been corrected and taken into account. You can retrieve by how much to offset the Z-coordinates of the point cloud from the Z-coordinates of its previous row (to correct the shear in Z) using[`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with[`M_3D_SHEAR_Z`](../../Reference/3dmap/M3dmapGetResult.md). Using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md), you can set [`M_3D_SHEAR_Z`](../../Reference/buf/MbufControlContainer.md) of the range component to this value, prior to calling [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), to correct the shear in Z. Instead of retrieving the correction values, you can also retrieve the pitch of the 3D profile sensor (in degrees) with [`M_SENSOR_PITCH`](../../Reference/3dmap/M3dmapGetResult.md).

### Scale

Scale distortions in Y cause shrinking or stretching of the object scanned by the 3D profile sensor. The image below provides a visualization of what scale distortion in the Y-direction looks like.

*[Image: 3dmap_scale_in_Y.png]*

These scaling distortions occur when the speed of the conveyor or plane of motion is either faster or slower than the expected speed, causing the distance between scan lines (scan step) in the Y-direction to be closer or further apart. The Y-scaling factor necessary to correct the Y-coordinates of the point cloud can be retrieved using [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_3D_SCALE_Y`](../../Reference/3dmap/M3dmapGetResult.md). Using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md), you can set [`M_3D_SCALE_Y`](../../Reference/buf/MbufControlContainer.md) of the range component to this value, prior to calling [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), to correct the scale in Y. You can also retrieve the norm of the calculated step vector, which includes the scale in the Y-axis and, if present, the shear in the X and Z-axes; to do so, use [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_STEP_LENGTH`](../../Reference/3dmap/M3dmapGetResult.md).

### Yaw

Yaw around the Z-axis will cause a shear between the X-axis and the Y-axis in the scanned scene, which will produce a distorted result. The image below provides a visualization of yaw, and what shear in the X-direction looks like.

*[Image: 3dmap_shear_in_X.png]*

Typically, X-values are reported as if the laser plane was perfectly perpendicular to the direction of motion. You can retrieve by how much to offset the X-coordinates of the point cloud from the X-coordinates of its previous row (to correct the shear in X) using [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_3D_SHEAR_X`](../../Reference/3dmap/M3dmapGetResult.md). Using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md), you can set [`M_3D_SHEAR_X`](../../Reference/buf/MbufControlContainer.md) of the range component to this value, prior to calling [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), to correct the shear in X. Instead of retrieving the correction values, you can also retrieve the yaw of the 3D profile sensor (in degrees) with [`M_SENSOR_YAW`](../../Reference/3dmap/M3dmapGetResult.md).

## Example of correcting the placement of your 3D profile sensor setup

The example _alignlaserscans.cpp_ demonstrates how to correct the placement of your 3D profile sensor setup using [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md). It covers all the concepts explained in this section. To run this and other examples, use Aurora Imaging Example Launcher.
