---
doctype: UserGuide
part: "3D related information"
chapter: Fixturing_in_3D
section: Fixturing_in_3D_overview
module_tag: 3dgeo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Fixturing_in_3D / Fixturing in 3D overview"
---

# How to fixture in 3D

You might want to apply the same measurement or inspection process to every instance of an object that could appear any number of times at any location or orientation in your 3D scene. A technique known as fixturing can be used to transform your 3D points so that the object is at a fixed position and orientation.

## Basic steps for fixturing

Since you do not associate point cloud containers and 3D geometry objects with a calibration context, you use a different method to fixture when using the Aurora Imaging Library 3D modules, instead of the relative coordinate system method implemented for the 2D modules. Before using a 3D processing or analysis function, you can transform coordinates to fixture point clouds or 3D geometries to a required position and orientation. To do so:

1. Allocate a transformation matrix object using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md).
2. Generate the required transformation coefficients by calling [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)with the transformation that you want to perform (for example, [`M_TRANSLATION`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)), or by calling [`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). Alternatively, you can copy a resulting matrix from another Aurora Imaging Library 3D module (for example, the 3D Metrology or 3D Registration module) into the transformation matrix object.
3. Apply the transformation matrix to the 3D points using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md). Typically, you store the resulting transformed points in a different container if your source point cloud has multiple regions that need to be analyzed.

There are three typical cases in which you might want to fixture:

- Fixturing to a known position.
- Fixturing to a reference object.
- Fixturing to a plane.

## Fixturing to a known position

To generate the transformation coefficients required to fixture a point cloud to a predetermined position and orientation, specified by a known translation, rotation, or scale, call [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)with the transformation that you want to perform.

Alternatively, call [`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md)with [`M_COORDINATE_SYSTEM_TRANSFORMATION`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md) and the origin and axes that define your new working coordinate system. When applied using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md), point coordinates are passively transformed and expressed relative to the new origin and axes. In the example below, the point P (3, 0, 2) is expressed as P′ (2, 0, 1) in the new coordinate system.

*[Image: 3dgeo_coord_sys_transform_shifted_origin.png]*

To establish the location, you can, for example, generate a depth map of the point cloud, search for a distinguishing feature in the depth map, retrieve the calibration fixture matrix, and apply the matrix using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md). For more information, see [Setting the slicing plane according to a reference object](../C35_3D_Image_processing/S14_Analyzing_the_profile_of_a_3D_cross_section.md).

If you need to apply multiple transformations (for example, a rotation and a translation), you can use the transformation that results from the composition of two matrices using [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) with[`M_COMPOSE_TWO_MATRICES`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md). Alternatively, you can use [`M_COMPOSE_WITH_CURRENT`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) to compose the coefficients of a specified transformation with the existing coefficients of the specified transformation matrix object.

## Fixturing to a reference object

To generate the transformation coefficients required to fixture a scanned 3D object at an unknown orientation or position such that its points align with those of a reference object, you can use either the 3D Registration module or the 3D Model Finder module.

If you know your object is close to the reference (not significantly rotated or flipped), you should use the 3D Registration module. First, perform 3D registration on the point cloud by calling [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md), which will calculate the transformation needed to achieve alignment between the source point cloud and its reference point cloud. Then, copy the registration matrix to a transformation matrix object using [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md)with [`M_REGISTRATION_MATRIX`](../../Reference/3dreg/M3dregCopyResult.md).

The following animation shows how to use 3D registration to fixture an object to a reference model:

*[Image: 3dim_FixturingIn3D]*

The following code snippet is an example of the steps required to fixture to a reference object using the 3D Registration module:

> **Code example:** [userguide.fixturing_in_3d.how_to_fixture_in_3d01](userguide.fixturing_in_3d.how_to_fixture_in_3d01)

If the object is far from the reference or there is a lot of occlusion, you should use the 3D Model Finder module. First search for the object using [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). Then, copy the fixturing matrix of the found occurrence to a transformation matrix object using [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md) with [`M_FIXTURING_MATRIX`](../../Reference/3dmod/M3dmodCopyResult.md).

## Fixturing to a plane

To generate the transformation coefficients required to fixture a point cloud to a plane, you can use the 3D Metrology module. For example, if the surface on which an object is scanned is at a slant, you can fit a plane to the surface, and then fixture subsequent scanned objects to that plane, such that they are no longer at a slant.

To obtain a point cloud of only the slanted surface, either scan the surface without the object, or mask out the object from an existing scan. You can then fit a plane to the surface using [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) with [`M_PLANE`](../../Reference/3dmet/M3dmetFit.md), which will generate the transformation coefficients required to fixture a point cloud to the plane. Then, copy the fixturing matrix of the fitted plane to a transformation matrix object using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_FIXTURING_MATRIX`](../../Reference/3dmet/M3dmetCopyResult.md).

The following code snippet is an example of the steps required to fixture to a plane using the 3D Metrology module:

> **Code example:** [userguide.fixturing_in_3d.how_to_fixture_in_3d02](userguide.fixturing_in_3d.how_to_fixture_in_3d02)

Another possible application for fixturing to a plane is when you have a scanned object that has a slanted region that you want to inspect. In this case, it might be useful to isolate and flatten the slant. To do so:

1. Isolate the region in the object's point cloud using [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md).
2. Fit a plane to this region, using [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md).
3. Apply the resulting transformation matrix to the points, using[`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md).

The following animation shows how to use the 3D Metrology module to fixture an object to a plane:

*[Image: 3dim_FixturingIn3D_Plane]*

## Applying a transformation matrix

Once you have a transformation matrix, you are ready to apply the transformation. If necessary, use [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)with [`M_INVERSE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)to reverse the direction of the transformation, as transformation matrices are not bidirectional. Then, call [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md)to apply the transformation matrix to your point cloud. This will transform the points to the specified position and orientation and write them to a 3D Aurora Imaging Library object of the same type as the specified point cloud container.
