---
doctype: UserGuide
part: "3D related information"
chapter: Grabbing_from_3D_sensors
section: Alignment_object_requirements
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Grabbing_from_3D_sensors / Alignment object requirements"
---

# Alignment object constraints and requirements

There are three different types of alignment objects that can be used to calibrate your 3D profile sensor when using [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md). The flat surface object ([`M_FLAT_SURFACE`](../../Reference/3dmap/M3dmapControl.md)) is useful when you do not have the ability to create an alignment pyramid ([`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md)) or alignment disk ([`M_DISK`](../../Reference/3dmap/M3dmapControl.md)). The flat surface object is limited to aligning the origin of the Z-axis of the working coordinate system with its surface and to changing the positive direction of one or more axes of the working coordinate system, while maintaining the right hand rule. Calculations that rely on the presence of an alignment pyramid or disk will not be calculated, such as the values necessary to correct the scaling factor in the Y-direction and the shear in both the X- and Z-directions. Scanning the alignment pyramid or alignment disk and calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) will calculate these values and make the transformations necessary to calibrate your 3D profile sensor obtainable. For more information on 3D profile sensor placement and the types of distortions, see [Correcting for 3D profile sensor placement](S07_Steps_to_calibrate_3D_profile_sensor.md). Even if your 3D sensor is properly aligned, the default coordinate system of the 3D sensor (and of the resulting point cloud or depth map) might not be the most ideal for your application. For more information on aligning or flipping the default working coordinate systems, see [Aligning or flipping the coordinate system](S06_Aligning_or_flipping_the_coordinate_system.md).

In most cases, it is recommended to use the alignment pyramid ([`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md)) to calibrate your 3D profile sensor. Using [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) with the alignment pyramid provides more accurate calculations that are necessary for correcting larger shears and scale, and will also automatically deduce the camera tilt around the X-axis ([`M_CAMERA_TILT_X`](../../Reference/3dmap/M3dmapControl.md)). To apply these corrections, you can use [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md) for the transformation matrix, or with [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) when converting the 3D sensor data to a format that is 3D-processable. For more information, see [Steps to correct for 3D profile sensor placement](S07_Steps_to_calibrate_3D_profile_sensor.md). Additionally, when using the [`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md) alignment object, you can quantify and measure the RMS error of your alignment operation using [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_RMS_ERROR`](../../Reference/3dmap/M3dmapGetResult.md).

## Alignment pyramid constraints and requirements

The alignment pyramid is a machined calibration object that adheres to certain constraints and when scanned can be compared to the point cloud of your setup. This real-world object should be created based on the specifications and constraints listed below, or simply use the recommended dimensions (in millimeters) for the object depending on your setup.

The alignment pyramid is a truncated square pyramid on a wider rectangular base that has a chamfer in the bottom right corner. The truncated pyramid is off-center along the length of the wider rectangular base. The **alignment pyramid's wider rectangular base** refers to the flat rectangular surface below the truncated pyramid, with **(W)** in the X-direction and **(L)** in the Y-direction, while the **truncated pyramid's base** refers to the truncated pyramid's square base **(B)**.

*[Image: 3dmap_pyramid_dimensions_ug_sm.png]*

When using [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) with the alignment pyramid, the following constraints must be met:

- The top face and base of its truncated pyramid must be squares.
- The angle between the top and side faces of its truncated pyramid must be between 35 to 55 degrees (ideally 45 degrees).
- Its truncated pyramid's top face length **(A)** must be approximately 50% of the truncated pyramid's base length **(B)**.
- The alignment pyramid's wider rectangular base must be approximately 25% wider **(W)** in the X-direction and 50% longer **(L)** in the Y-direction than its truncated pyramid's base **(B)**.
- The alignment pyramid must cover at least 50% of the scanned width (X-direction).
- The alignment pyramid must cover at least 30% of the scanned length (Y-direction).
- Its truncated pyramid's faces (top and sides) and the top of the alignment pyramid's wider rectangular base must each be composed of at least 2000 points.
- The alignment pyramid's wider rectangular base must have a chamfer in the bottom right corner **(F)** with a length of at least 30% of the truncated pyramid's top face **(A)**. The chamfer must be at an approximate angle of 45 degrees.

### Recommended alignment pyramid dimensions

The following are the recommended dimensions for the alignment pyramid that should be used with [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) to calculate the transformations and information necessary to correct the alignment of a 3D profile sensor. You must specify the dimensions of the alignment pyramid using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_PYRAMID_BASE_LENGTH`](../../Reference/3dmap/M3dmapControl.md),[`M_PYRAMID_TOP_FACE_LENGTH`](../../Reference/3dmap/M3dmapControl.md), and[`M_HEIGHT`](../../Reference/3dmap/M3dmapControl.md) before calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md). You should use matte gray sandblasted aluminum or 96% alumina ceramic for the material of the alignment pyramid. The top face of the truncated pyramid and the bottom of the alignment pyramid's wider rectangular base must have a flatness tolerance that falls between two parallel planes spaced 0.05 mm apart. Typically, medium-sized alignment pyramids are best for most alignment setups.

*[Image: 3dmap_pyramid_dimensions.png]*

| *[Image: z_TableSpacer20x20.png]* | Recommended values (millimeters)[^base] |
| --- | --- |
| **L** (Alignment pyramid's wider rectangular base length) | **W** (Alignment pyramid's wider rectangular base width) | **B** (Truncated pyramid's base length) | **A** (Truncated pyramid's top face length) | **E** (Side channel width) | **F** (Chamfer length) | **H** (Truncated pyramid's height) | **T** (Alignment pyramid's wider rectangular base thickness)[^h] |
| Small | 66.0 | 55.0 | 44.0 | 22.0 | 5.5 | 5.0 | 11.0 | 5.0 |
| Medium | 108.0 | 90.0 | 72.0 | 36.0 | 9.0 | 10.0 | 18.0 | 5.0 |
| Large | 210.0 | 175.0 | 140.0 | 70.0 | 17.5 | 15.0 | 35.0 | 5.0 |

[^base]: Note that the **alignment pyramid's wider rectangular base** refers to the flat rectangular surface below the truncated pyramid, while the **truncated pyramid's base** refers to the truncated pyramid's square base.

[^h]: The thickness is not used for the calibration, and is therefore free to be modified as needed for manufacturing purposes.

## Alignment disk constraints and requirements

The alignment disk is a machined calibration object that adheres to certain constraints and when scanned can be compared to the point cloud of your setup. This real-world object should be created based on the specifications and constraints listed below, or simply use the recommended dimensions (in millimeters) for the object depending on your setup.

*[Image: 3dmap_disk_dimensions_ug_sm.png]*

When using [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) with the alignment disk, the following constraints must be met:

- The alignment disk must cover at least 50% of the scanned width (X-direction).
- The alignment disk must cover at least 30% of the scanned length (Y-direction).
- The alignment disk's edge must be fully visible in the scan.
- The alignment disk's holes must be at least 30 scan lines (Y-direction) and 30 points (X-direction). The radii of the holes must be within 5 to 10% of the disk's radius, and the depth of the holes **(P)** must be at least 20% of the total disk's height **(H)**.
- The floor (background) must be present in the scan. If you are using a stage, ensure its surface is parallel to the motion.

### Recommended alignment disk dimensions

The following are the recommended dimensions for the alignment disk that should be used with [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) to calculate the transformations and information necessary to correct the alignment of a 3D profile sensor. You must specify the diameter and height of the alignment disk using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_DIAMETER`](../../Reference/3dmap/M3dmapControl.md) and [`M_HEIGHT`](../../Reference/3dmap/M3dmapControl.md) before calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md). You should use matte gray sandblasted aluminum or 96% alumina ceramic for the material of the alignment disk, and the surface for both sides of the alignment disk must have a flatness tolerance that falls between two parallel planes spaced 0.005 mm apart. Typically, medium-sized alignment disks are best for most alignment setups.

*[Image: 3dmap_billet_dimensions.png]*

| *[Image: z_TableSpacer20x20.png]* | Recommended values (millimeters) |
| --- | --- |
| **D** (Alignment disk diameter) | **d** (Alignment disk hole diameter) | **P** (Alignment disk hole depth) | **A** (First hole distance from center) | **B** (Second hole distance from center) | **H** (Alignment disk height) |
| Small | 45.0 | 6.0 | 10.0 | 15.0 | 10.0 | 30.0 |
| Medium | 70.0 | 6.0 | 10.0 | 25.0 | 15.0 | 50.0 |
| Large | 150.0 | 10.0 | 10.0 | 50.0 | 20.0 | 50.0 |
