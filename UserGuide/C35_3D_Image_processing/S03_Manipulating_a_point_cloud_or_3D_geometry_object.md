---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Manipulating_a_point_cloud_or_3D_geometry_object
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Manipulating a point cloud or 3D geometry object"
---

# Moving or scaling a point cloud or 3D geometry

With the 3D Image Processing module, you can translate, rotate, and scale a point cloud or 3D geometry, using [`M3dimTranslate`](../../Reference/3dim/M3dimTranslate.md), [`M3dimRotate`](../../Reference/3dim/M3dimRotate.md), and [`M3dimScale`](../../Reference/3dim/M3dimScale.md), respectively. You can also apply these transformations or a combination of these, using a transformation matrix; in this case, you can use [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) or [`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md) to generate the required transformation matrix, and then use [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md) to apply it.

## Translation

To displace a point cloud or 3D geometry, use [`M3dimTranslate`](../../Reference/3dim/M3dimTranslate.md). Specify the required displacements along each axis direction, in world units. The translation applies to all points in the source object.

The following image shows the translation of a point cloud.

*[Image: 3dim_translate.png]*

If you want to apply additional transformations, or if you need to specify the required translation to a function that takes a transformation matrix object, you should instead generate a transformation matrix. To define a transformation matrix to perform a translation, use [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) with the [`M_TRANSLATION`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) transformation type and the required coordinate displacements along each of the axes of the working coordinate system. Refer to [Transforming points using a transformation matrix](S03_Manipulating_a_point_cloud_or_3D_geometry_object.md) for more information.

## Rotation

To rotate a point cloud or 3D geometry, use [`M3dimRotate`](../../Reference/3dim/M3dimRotate.md). Specify the center of rotation (the point around which to rotate) and the type of rotation to perform. Note that the default center of rotation is the origin (0,0,0). You can rotate around one axis (for example, [`M_ROTATION_X`](../../Reference/3dim/M3dimRotate.md)) or around all three axes (for example, [`M_ROTATION_XYZ`](../../Reference/3dim/M3dimRotate.md)). The three rotations about the axes of the working coordinate system are also known as roll, pitch, and yaw. You can also specify a vector around which to rotate ([`M_ROTATION_AXIS_ANGLE`](../../Reference/3dim/M3dimRotate.md)). For each of these rotation types, you must specify the amount of rotation in degrees. You can also specify a quaternion rotation ([`M_ROTATION_QUATERNION`](../../Reference/3dim/M3dimRotate.md)).

The following images show the rotation of a point cloud, around the X-axis (left) and around a specified vector (right), using the default center of rotation (0,0,0).

*[Image: 3dim_rotate_Xaxis.png]*

*[Image: 3dim_rotate_uniqueaxis.png]*

You can specify a custom center of rotation or, for 3D geometry objects, you can choose to center the rotation around the volumetric center of the object ([`M_GEOMETRY_CENTER`](../../Reference/3dim/M3dimRotate.md)). To rotate around the volumetric center of a point cloud, you can establish its centroid using [`M3dimStat`](../../Reference/3dim/M3dimStat.md) with [`M_STAT_CONTEXT_CENTROID`](../../Reference/3dim/M3dimStat.md). If you displace the center of rotation, the rotation occurs around a line that is parallel to the axis of rotation and runs through the center point.

The following image shows the rotation of a point cloud, around a line that is parallel to the X-axis and runs through the point (0,3,4).

*[Image: 3dim_rotate_Xaxis_new_center.png]*

If you want to apply additional transformations, or if you need to specify the required rotation to a function that takes a transformation matrix object, you should instead generate a transformation matrix. To define a transformation matrix to perform a rotation, use [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) and specify one of several rotation operations ([`M_ROTATION_...`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)). Refer to [Transforming points using a transformation matrix](S03_Manipulating_a_point_cloud_or_3D_geometry_object.md) for more information.

## Scaling

To scale the distance between points in a point cloud or 3D geometry, use [`M3dimScale`](../../Reference/3dim/M3dimScale.md). Specify the required factor by which to scale in each axis direction. The scaling is applied to each point's distance from the origin or specified center point, along X, Y, or Z. For example, if you specify a 0.5 scale factor along each axis, the resulting point cloud is half the size. That is, each point's distance from the center point (along X, Y, and Z) is half the distance as that of points in the source point cloud. As with rotation, you can specify to scale with respect to the volumetric center of a 3D geometry so that the geometry's center does not move; the geometry just changes in size.

Note that applying a non-uniform scale (that is, a scale with a different factor along each axis) changes the shape of the point cloud or 3D geometry. You cannot apply a non-uniform scale to a box, sphere, or cylinder 3D geometry.

To obtain a mirror-image of your source object, you can set negative scale factors. However, a mirrored object can be problematic if you intend to perform additional analysis. For example, any text or symbols on the object will be mirrored.

The following image contains the original point cloud that will be scaled in the table below.

*[Image: 3dim_scale.png]*

The following table shows the effect of different scale factors on the given point cloud.

| Scale factor | Effect | Scaled point cloud (positive scale factor) | Scaled point cloud (negative scale factor) |
| --- | --- | --- | --- |
| `_ScaleFactor_ > 1` or `_ScaleFactor_ &lt; -1` | - Each coordinate's distance from the origin increases.<br/>- The point cloud or 3D geometry becomes larger.<br/>- If the scale factor is negative, the point cloud or 3D geometry is reflected on the opposite side of the origin. | *[Image: 3dim_scale_larger.png]* | *[Image: 3dim_scale_neg_larger.png]* |
| `-1 &lt; _ScaleFactor_ &lt; 1` | - Each coordinate's distance from the origin decreases.<br/>- The point cloud or 3D geometry becomes smaller.<br/>- If the scale factor is negative, the point cloud or 3D geometry is reflected on the opposite side of the origin. | *[Image: 3dim_scale_smaller.png]* | *[Image: 3dim_scale_neg_smaller.png]* |

If you want to apply additional transformations, or if you need to specify the required scale to a function that takes a transformation matrix object, you should instead generate a transformation matrix. To define a transformation matrix to perform a scaling operation, use [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) with the [`M_SCALE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) transformation type and a scale factor for which coordinate distances will be multiplied. Refer to [Transforming points using a transformation matrix](S03_Manipulating_a_point_cloud_or_3D_geometry_object.md) for more information.

Scaling a point cloud is useful for numeric stability. For example, if you scan a scene with a 3D sensor that outputs a point cloud scaled in microns, the dimensions of your point cloud can be millions of microns long. If you need to perform calculations using the dimensions of a point cloud, or the objects therein, it is useful to scale down the point cloud so that all measurements are of order unity.

If you have performed moments statistics calculations on your point cloud ([`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_MOMENTS`](../../Reference/3dim/M3dimControl.md) and then [`M3dimStat`](../../Reference/3dim/M3dimStat.md)), you can use a transformation matrix to scale the point cloud to dimensions at or near one unit. This means that the centroid is positioned at (0,0,0) and the point cloud is transformed so that it has a unit variance along each axis. To do so, you must first allocate a transformation matrix object, using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). Copy the results of the moments statistics calculations to the transformation matrix, using[`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md) with [`M_STANDARDIZATION_MATRIX`](../../Reference/3dim/M3dimCopyResult.md). You can then apply the transformation matrix to the 3D points using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md). Note that the moments results must be of order 2 or greater.

## Transforming points using a transformation matrix

To apply an arbitrary linear transformation to a point cloud or 3D geometry, apply the following:

1. Allocate a transformation matrix object using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md).
2. Generate the required transformation coefficients using [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md).
3. Apply the transformation matrix to the 3D points using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md).
   > **Note:** Note that you can also apply the transformation matrix to an array of 3D coordinates using [`M3dimMatrixTransformList`](../../Reference/3dim/M3dimMatrixTransformList.md).

If you apply multiple transformations consecutively (using [`M3dimTranslate`](../../Reference/3dim/M3dimTranslate.md), [`M3dimRotate`](../../Reference/3dim/M3dimRotate.md), and/or [`M3dimScale`](../../Reference/3dim/M3dimScale.md)), Aurora Imaging Library computes a new point cloud for each transformation. For better performance, you can generate a transformation matrix that combines all the transformations into one, and then apply it as a single transformation. To do so, use [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) with[`M_COMPOSE_WITH_CURRENT`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), which will compose the coefficients of a specified transformation with the existing coefficients of the specified transformation matrix object. Alternatively, you can use [`M_COMPOSE_TWO_MATRICES`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) to combine the transformation defined by two matrices into one matrix. Note that the order of the matrices matters. The matrix composition follows the equation AB = C, where A is the first source matrix, B is the second source matrix, and C is the resulting transformation matrix. Note that transformation matrix B is applied before transformation matrix A. Therefore, transformation matrix C describes the transformation obtained by applying transformation matrix B followed by transformation matrix A.

The following images show the result of applying a translation and then a rotation, versus applying a rotation and then a translation to the same point cloud. The image on the left depicts a point cloud undergoing a translation through X, Y, and Z followed by a rotation about the Z-axis. You can use [`M_COMPOSE_TWO_MATRICES`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) and specify the rotation matrix as matrix A and the translation matrix as matrix B to compose a new transformation matrix that can obtain the same effect in one single action. The image on the right depicts a point cloud undergoing the same two transformations, but in the opposite order (rotation followed by translation). To compose the transformation matrix that has the effect of applying a rotation followed by a translation, use [`M_COMPOSE_TWO_MATRICES`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) and specify the translation matrix as matrix A and the rotation matrix as matrix B.

*[Image: 3dim_translate_then_rotate.png]*

*[Image: 3dim_rotate_then_translate.png]*

The following code snippet is an example of the steps required to transform a point cloud. The snippet shows how you can fixture to the origin a point cloud that has been translated and rotated by known amounts. To do so, it takes the inverse of the composition of the point cloud's translation and rotation, and applies it to the point cloud. Note that the snippet combines the translation and rotation transformations into one matrix, using [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) with[`M_COMPOSE_WITH_CURRENT`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md).

> **Code example:** [userguide.3d_processing.manipulating_a_point_cloud](userguide.3d_processing.manipulating_a_point_cloud)

You can also define a transformation matrix to transform points using [`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md) with [`M_POSITION_TRANSFORMATION`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). This automatically generates the transformation matrix required to offset your 3D points by a specified position and orientation. When applied using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md), point coordinates are actively transformed and expressed relative to the original origin and axes. In the example below, the point P (3, 0, 2) is moved to P′ (4, 0, 3). Note that P's offset from the original coordinate system is equivalent to P′'s offset from the new coordinate system.

*[Image: 3dgeo_pos_transform_shifted_origin.png]*

## Changing the working coordinate system

You can change coordinates such that your 3D points are expressed relative to a new coordinate system (fixtured), using either [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) or [`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). You can use these functions to automatically generate the transformation matrices required to fixture point clouds or 3D geometries to the specified object or position and orientation.

Using [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), you can specify a transformation matrix that can move the working coordinate system to a 3D geometry ([`M_FIXTURE_TO_GEOMETRY`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)), or you can specify a transformation that can move the XY (Z=0) plane of the working coordinate system to a specified plane ([`M_FIXTURE_TO_PLANE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)).

Alternatively, you can define a transformation matrix using [`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md) with [`M_COORDINATE_SYSTEM_TRANSFORMATION`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). This allows you to set a new origin and axes. When applied using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md), point coordinates are passively transformed and expressed relative to the new origin and axes. In the example below, the point P (3, 0, 2) is expressed as P′ (2, 0, 1) in the new coordinate system.

*[Image: 3dgeo_coord_sys_transform_shifted_origin.png]*

For more information on fixturing in 3D, see [How to fixture in 3D](../C45_Fixturing_in_3D/S01_Fixturing_in_3D_overview.md).
