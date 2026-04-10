---
doctype: UserGuide
part: "3D related information"
chapter: Using_the_3D_Geometry_module
section: Defining_transformation_matrix_objects
module_tag: 3dgeo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Using_the_3D_Geometry_module / Defining transformation matrix objects"
---

# Defining transformation matrix objects

As a consistent way of retrieving or specifying a 3D transformation, Aurora Imaging Library uses a transformation matrix object to store the matrix that defines the transformation. Although you can, you typically don't need to explicitly specify the coefficients of the matrix. Aurora Imaging Library 3D functions represent calculated 3D transformations as matrices and store them into specified transformation matrix objects (for example, [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md) with [`M_FIXTURING_MATRIX`](../../Reference/3dmod/M3dmodCopyResult.md)). In addition, there are Aurora Imaging Library functions that allow you to specify the required 3D transformation, and they calculate and return the corresponding matrix into the specified transformation matrix object. You can then use it to specify the required 3D transformation to functions that take a transformation matrix object (for example, [`M3dgraText`](../../Reference/3dgra/M3dgraText.md)). You can also use it to apply the transformation to a point cloud or 3D geometry, using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md); this allows you to displace, rotate, or scale coordinates, or allows you to fixture the point cloud or 3D geometry to a required position or orientation.

Note that you can apply simple 3D transformations directly to a 3D geometry or point cloud, without a transformation matrix object, using [`M3dimTranslate`](../../Reference/3dim/M3dimTranslate.md), [`M3dimRotate`](../../Reference/3dim/M3dimRotate.md), and [`M3dimScale`](../../Reference/3dim/M3dimScale.md).

For more information about the different types of 3D transformations and a more detailed explanation about using transformation matrices to fixture or transform points, see [Moving or scaling a point cloud or 3D geometry](../C35_3D_Image_processing/S03_Manipulating_a_point_cloud_or_3D_geometry_object.md).
