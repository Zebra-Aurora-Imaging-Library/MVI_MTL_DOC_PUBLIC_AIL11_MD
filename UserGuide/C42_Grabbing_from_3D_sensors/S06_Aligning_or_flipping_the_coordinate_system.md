---
doctype: UserGuide
part: "3D related information"
chapter: Grabbing_from_3D_sensors
section: Aligning_or_flipping_the_coordinate_system
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Grabbing_from_3D_sensors / Aligning or flipping the coordinate system"
---

# Aligning or flipping the coordinate system

Even if your 3D sensor is properly aligned, the default coordinate system of the 3D sensor (and of the resulting point cloud or depth map) might not be the most ideal for your application. This section presents strategies to align, move, or flip a point cloud's coordinate system and its axes. If you are trying to adjust for distortion or to correct for the placement of your 3D profile sensor, refer to [Correcting for 3D profile sensor placement](S07_Steps_to_calibrate_3D_profile_sensor.md).

## Changing the Anchor coordinate system of a GenIcam-compliant 3D sensor

The **Anchor coordinate system** is a Cartesian coordinate system relative to which, by default, 3D sensors return results. Some GenICam-compliant 3D sensors allow you to change the location of the Anchor coordinate system's origin and the orientation of its axes. For example, Zebra AltiZ allows you to place the Anchor coordinate system in one of three locations, or modes, as shown in the following image: ReferencePoint mode, Distance mode, and Height mode.

*[Image: AltiZ_anchor_coordinate.png]*

Note that all three of the Anchor coordinate system modes supported by Zebra AltiZ respect the right-hand rule.

## Inverting or flipping the Z-axis

If you have already acquired 3D data in the form of a point cloud from a 3D sensor, you might want to change the coordinates of the points in the point cloud. For example, if the 3D sensor's Anchor coordinate system had its Z-axis pointing from the sensor towards the scanned objects, as is the case in ReferencePoint mode and Distance mode for Zebra AltiZ, then the parts of the scanned objects that are closest to the sensor will correspond to points in the point cloud with the lowest Z-coordinates. Assuming that the 3D profile sensor is above the object to be scanned, then the tallest parts of the object will correspond to the points with the lowest Z-coordinates, which can be unintuitive or problematic for further analysis. You can invert, or flip, the Z-axis of your point cloud's working coordinate system, so that the tallest parts of the object correspond to points with the greatest Z-coordinates.

To invert, or flip, the Z-axis of your point cloud's working coordinate system, you can rotate the points in the point cloud about the X-axis or Y-axis. To rotate the point cloud, use [`M3dimRotate`](../../Reference/3dim/M3dimRotate.md) with either [`M_ROTATION_X`](../../Reference/3dim/M3dimRotate.md) or [`M_ROTATION_Y`](../../Reference/3dim/M3dimRotate.md) set to 180 degrees. Rotating the points by 180 degrees about one axis negates their coordinates along the other two axes. The working coordinate system will continue to respect the right-hand rule. Equivalently, you can calculate the transformation necessary to rotate the points in the point cloud such that the direction of the Z-axis is flipped, using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md) set to [`M_REVERSE`](../../Reference/3dmap/M3dmapControl.md), and [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md).

The following images depict a point cloud undergoing a rotation about the X-axis. In the leftmost image, the point cloud is originally oriented such that the top of the object corresponds to points with lower Z-coordinates than the bottom of the object. The second image shows a rotation of 180 degrees about the X-axis; note that this rotation inverts, or flips, the Y- and Z-coordinates of the points in the point cloud. In the rightmost image, the display is reoriented so that the point cloud appears right side up. The top of the point cloud now corresponds to points with higher Z-coordinates than the bottom of the point cloud; the Z-axis has effectively been flipped.

*[Image: 3dim_flip_rotate_around_x.png]*

You might be tempted to negate the Z-coordinates of each point in the point cloud by simply scaling the points using [`M3dimScale`](../../Reference/3dim/M3dimScale.md), with [`ScaleZ`](../../Reference/3dim/M3dimScale.md) set to -1. However, doing so mirrors the object, instead of resulting in the same object, which can lead to incorrect analysis. For more information, see [Scaling](../C35_3D_Image_processing/S03_Manipulating_a_point_cloud_or_3D_geometry_object.md).

Note that even if you correct the direction of the Z-axis in the point cloud's working coordinate system, the location of the point cloud's origin might not be suitable for your application. For example, you might want points corresponding to the bottom of your scanned object to have no height (Z=0) and lie in the XY-plane. You can use [`M3dimTranslate`](../../Reference/3dim/M3dimTranslate.md) with [`TranslationZ`](../../Reference/3dim/M3dimTranslate.md) to translate all points in your point cloud up or down along the Z-axis, allowing you to control where the XY-plane (Z=0) lies in your point cloud. You can also refer to the following subsection for more strategies on manipulating the position of your point cloud's working coordinate system.

The following image shows the rotated point cloud from above translated up 5 units along the Z-axis. The points corresponding to the bottom of the gear now lie in the XY-plane.

*[Image: 3dim_translate_up_z.png]*

## Aligning the working coordinate system with a known location or orientation

You can align the working coordinate system of your point cloud with a known location or orientation, using [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md). This function uses the point cloud of a flat surface, an alignment pyramid (with expected proportions and a chamfer), or an alignment disk (with expected proportions and two directional holes) in the target setup to establish the transformations. For information on how to construct an appropriate alignment object, refer to [Alignment object constraints and requirements](S08_Alignment_object_requirements.md).

Using the point cloud of a flat surface is useful when you do not have the ability to create an alignment object (pyramid or disk). With the point cloud of a flat surface perpendicular to your 3D sensor, you can align the origin of the Z-axis (Z=0) of your point cloud's working coordinate system to a known location and/or you can change the positive direction of its axes. Prior to calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md), use [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) set to [`M_FLAT_SURFACE`](../../Reference/3dmap/M3dmapControl.md).

The following image shows an example of how you can use a flat surface object to align your point cloud's working coordinate system. Using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_ALIGN_Z_POSITION`](../../Reference/3dmap/M3dmapControl.md) and[`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md), you can calculate the transformation necessary to align the coordinate system with the flat surface object such that the flat surface defines the XY-plane (Z=0).

*[Image: 3dmap_align_z_pos.png]*

You can change the positive direction of one or more axes of the working coordinate system using [`M_ALIGN_XY_DIRECTION`](../../Reference/3dmap/M3dmapControl.md) and [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md). This allows you to specify the positive direction of any axis that is best suited for your setup, depending on the location and orientation of your 3D sensor with respect to the conveyor or plane of motion. Changing the direction of one axis automatically updates the other axes to maintain the right-hand rule.

The flat surface object can be any non-elastic flat surface, such as the flat conveyor belt, a flat square-like piece of sandblasted aluminum, 96% alumina ceramic, or an equivalently flat piece of medium-density fiberboard (MDF). In any case, your flat surface object must fill the 3D sensor's whole field of view in your target setup. Calculations that rely on the presence of an alignment pyramid or alignment disk will not be calculated if[`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is set to [`M_FLAT_SURFACE`](../../Reference/3dmap/M3dmapControl.md). In this case, [`M_3D_SCALE_Y`](../../Reference/3dmap/M3dmapGetResult.md), [`M_3D_SHEAR_X`](../../Reference/3dmap/M3dmapGetResult.md), and [`M_3D_SHEAR_Z`](../../Reference/3dmap/M3dmapGetResult.md) will not be calculated or obtainable in [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md). The matrix that corrects the shear and scale for the laser scan lines ([`M3dmapCopyResult`](../../Reference/3dmap/M3dmapCopyResult.md) with [`M_SHEAR_MATRIX`](../../Reference/3dmap/M3dmapCopyResult.md)) will be the identity matrix, and the matrix that corrects the shear, scale, and rotations of the laser scan lines ([`M3dmapCopyResult`](../../Reference/3dmap/M3dmapCopyResult.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dmap/M3dmapCopyResult.md)) will be the same as [`M_RIGID_MATRIX`](../../Reference/3dmap/M3dmapCopyResult.md).

If you do have an appropriate alignment pyramid or disk, you can control how [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) calculates the transformations necessary to align your point cloud with the point cloud of the alignment object at a known location or orientation. For example, you can specify to calculate the transformation to align the origin of the working coordinate system with the center of the alignment object in the X-direction by calling [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_ALIGN_X_POSITION`](../../Reference/3dmap/M3dmapControl.md) set to [`M_OBJECT_ORIGIN`](../../Reference/3dmap/M3dmapControl.md). After calling [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md), you can then get the transformation needed to align the working coordinate system and apply the calculated transformation matrix to your point cloud.

*[Image: M3dmap_m_align_x_position_UserGuideVersion.png]*

You can specify to calculate the transformation to align the origin of the working coordinate system along the Z-axis with either the top or bottom of the alignment object using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md) set to [`M_OBJECT_TOP`](../../Reference/3dmap/M3dmapControl.md) or [`M_OBJECT_BOTTOM`](../../Reference/3dmap/M3dmapControl.md), respectively.

*[Image: M3dmap_m_align_z_position_UserGuideVersion.png]*

[`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) assumes that the alignment object or flat surface object is parallel to the floor of your setup, and, in the case of a flat surface object, that the 3D sensor is perpendicular to the target setup. You cannot, for example, align your coordinate system with the point cloud of an alignment object or flat surface object that is elevated on one end and therefore is not parallel to the floor. Attempting to do so will cause your application to fail.

Even if you have correctly positioned your target setup, your 3D sensor might not be perfectly perpendicular to the objects you need to scan, which results in small distortions in the resulting point clouds. To correct these distortions using an alignment pyramid or disk, refer to [Correcting for 3D profile sensor placement](S07_Steps_to_calibrate_3D_profile_sensor.md).

You can also align the working coordinate system without an alignment object or flat surface object, by fitting the background of your point cloud to a plane. For example, you can use [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) with [`M_PLANE`](../../Reference/3dmet/M3dmetFit.md) to fit the bottom of the point cloud, corresponding to the surface of the conveyor belt, to a plane. You can then copy the results of the fitting operation into a transformation matrix, using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_FIXTURING_MATRIX`](../../Reference/3dmet/M3dmetCopyResult.md). This transformation matrix represents the transformation that could move the bottom of the point cloud to the XY-plane (Z=0), with its normal as the Z-axis. The centroid of all inlier points used for the fitting becomes the origin.

For more information on fixturing your coordinate system in 3D, refer to [How to fixture in 3D](../C45_Fixturing_in_3D/S01_Fixturing_in_3D_overview.md).

## Applying the transformations to your point cloud

Once the transformations to align the coordinate system are calculated, you can apply them to your 3D sensor data. To do so, use [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md) and apply the calculated transformation matrix ([`M3dmapCopyResult`](../../Reference/3dmap/M3dmapCopyResult.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dmap/M3dmapCopyResult.md)) to a point cloud acquired by your 3D sensor.
