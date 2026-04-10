---
doctype: UserGuide
part: "3D related information"
chapter: Using_the_3D_Geometry_module
section: 3D_geometry_module_overview
module_tag: 3dgeo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Using_the_3D_Geometry_module / 3D geometry module overview"
---

# 3D Geometry module overview

The 3D Geometry module allows you to create 3D geometry objects or transformation matrix objects.

3D geometry objects can be used to limit or define the processing region when dealing with point clouds or depth maps. For example, you can use 3D geometries to crop a point cloud or perform an arithmetic operation on a depth map. You can also use a 3D geometry object to perform comparative measurements with a point cloud or depth map. For example, you can fit a 3D geometry to a point cloud or depth map and calculate distances between points and the fitted surface. The 3D Geometry module allows you to define a geometry object with one of the supported 3D shapes: box, cylinder, line, plane, or sphere. Using the 3D Image Processing module, you can translate, scale, or transform the resulting geometries. Functions that can use a 3D geometry object, take it as a parameter.

Transformation matrix objects store matrices that define a required 3D transformation. You can generate the required transformation coefficients using [`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), or [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) with a transformation to perform (for example, [`M_TRANSLATION`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)). Alternatively, you can copy a resulting matrix from other Aurora Imaging Library 3D modules (for example, the 3D Metrology and the 3D Registration modules) into the transformation matrix object. You can then apply the transformation matrix to a point cloud or a 3D geometry object, using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md). For information on using transformation matrix objects, see [Moving or scaling a point cloud or 3D geometry](../C35_3D_Image_processing/S03_Manipulating_a_point_cloud_or_3D_geometry_object.md).
