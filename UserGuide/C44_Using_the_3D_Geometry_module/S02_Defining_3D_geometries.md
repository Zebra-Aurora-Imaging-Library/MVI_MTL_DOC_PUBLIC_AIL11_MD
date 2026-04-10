---
doctype: UserGuide
part: "3D related information"
chapter: Using_the_3D_Geometry_module
section: Defining_3D_geometries
module_tag: 3dgeo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Using_the_3D_Geometry_module / Defining 3D geometries"
---

# Defining 3D geometries

3D geometry objects must be allocated and defined before using them as a source in other 3D modules.

## Steps to defining and using a 3D geometry object

The following steps provide a basic methodology for defining 3D geometry objects.

1. Allocate a 3D geometry object on the specified system, using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).
2. Define the geometry of the object by calling one of the 3D geometry definition functions (for example, [`M3dgeoBox`](../../Reference/3dgeo/M3dgeoBox.md) or [`M3dgeoCylinder`](../../Reference/3dgeo/M3dgeoCylinder.md)); see [3D geometry objects](S02_Defining_3D_geometries.md). You can also copy a geometry result into the 3D geometry object to define it (for example, [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md) with [`M_BOUNDING_BOX`](../../Reference/3dim/M3dimCopyResult.md)).
3. If necessary, manipulate the defined geometry object using the 3D Image Processing module. You can translate, rotate, scale, or otherwise transform the geometry using [`M3dimTranslate`](../../Reference/3dim/M3dimTranslate.md), [`M3dimRotate`](../../Reference/3dim/M3dimRotate.md), [`M3dimScale`](../../Reference/3dim/M3dimScale.md), or [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md), respectively.
4. Pass the 3D geometry object to a function that can take one (for example, [`M3dmetDistance`](../../Reference/3dmet/M3dmetDistance.md) or [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md)).
5. Free the 3D geometry object, using [`M3dgeoFree`](../../Reference/3dgeo/M3dgeoFree.md), unless [`M_UNIQUE_ID`](../../Reference/3dgeo/M3dgeoAlloc.md) was specified during allocation.

## 3D geometry objects

The following table lists all 3D geometries that you can define; it also indicates the [`M3dgeo...`](../../Reference/3dgeo/M3dgeoBox.md) functions that you can use to define them.

| Geometry name | Example | Geometry definition function |
| --- | --- | --- |
| Box | *[Image: 3dgeo_box.png]* | [`M3dgeoBox`](../../Reference/3dgeo/M3dgeoBox.md) |
| Cylinder | *[Image: 3dgeo_cylinder.png]* | [`M3dgeoCylinder`](../../Reference/3dgeo/M3dgeoCylinder.md) |
| Line | *[Image: 3dgeo_line.png]* | [`M3dgeoLine`](../../Reference/3dgeo/M3dgeoLine.md) |
| Plane | *[Image: 3dgeo_plane.png]* | [`M3dgeoPlane`](../../Reference/3dgeo/M3dgeoPlane.md) |
| Point | *[Image: 3dgeo_point.png]* | [`M3dgeoPoint`](../../Reference/3dgeo/M3dgeoPoint.md) |
| Sphere | *[Image: 3dgeo_sphere.png]* | [`M3dgeoSphere`](../../Reference/3dgeo/M3dgeoSphere.md) |

## Finding missing coordinates for a 3D line, plane, or sphere

You can use [`M3dgeoEvalCurve`](../../Reference/3dgeo/M3dgeoEvalCurve.md) or [`M3dgeoEvalSurface`](../../Reference/3dgeo/M3dgeoEvalSurface.md) to find missing coordinates for a 3D line, plane, or sphere, given an incomplete set of coordinates. For example, if you pass a 3D line geometry and a corresponding array of X-coordinates to [`M3dgeoEvalCurve`](../../Reference/3dgeo/M3dgeoEvalCurve.md), the function can find the missing Y- and Z-coordinates that correspond to points on the curve (3D line). Similarly, you can pass a 3D plane or sphere to [`M3dgeoEvalSurface`](../../Reference/3dgeo/M3dgeoEvalSurface.md) with corresponding arrays of X- and Y- coordinates, and the function can find the missing Z-coordinates for points that lie on the surface of the 3D plane or sphere. These functions output the missing coordinates in a destination array or arrays, in either a packed or unpacked format.

The following image shows an [`M3dgeoEvalSurface`](../../Reference/3dgeo/M3dgeoEvalSurface.md) operation for a sphere, with source coordinate pairs on the XZ-plane, and resulting point locations on the sphere's surface, for which the function has found the missing Y-coordinates. In this case, only the points corresponding to the largest calculated Y-coordinates are shown.

*[Image: 3dgeo_FindPointsOnSphere.png]*

You can use [`M3dgeoEvalSurface`](../../Reference/3dgeo/M3dgeoEvalSurface.md) to increase the density of a plane or sphere in a point cloud. For example, if you have a sphere that is too sparse, you can estimate its position and radius, and then use [`M3dgeoEvalSurface`](../../Reference/3dgeo/M3dgeoEvalSurface.md) to generate more points on the surface, resulting in a denser point cloud. Another use-case for these functions is when you need an incomplete point cloud version of a 3D line, plane, or sphere (for example, one that is occluded or contains holes). You can use [`M3dgeoEvalCurve`](../../Reference/3dgeo/M3dgeoEvalCurve.md) or [`M3dgeoEvalSurface`](../../Reference/3dgeo/M3dgeoEvalSurface.md) to construct the point cloud point by point. To create a point cloud of a 3D geometry that does not exist in your current point cloud, use [`M3dimSample`](../../Reference/3dim/M3dimSample.md).

## Defining an oriented plane using a transformation matrix

If you want to define a plane with finite dimensions or with two in-plane axes whose orientations are explicitly specified, the 3D plane geometry object is often not suitable. For example, when taking the profile of a point cloud, 3D geometry, or mesh using [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md), you need to define a slicing plane with an explicitly specified origin and axes orientation along the plane. You might want to begin the profile at a specific location, or use a finite slicing plane, so your plane must be defined with an origin and a direction for the X-axis.

Functions that require an oriented plane (for example, [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md)) take the definition of a new coordinate system instead. They use its XY-plane (Z=0) as the oriented plane. As input, they take a transformation matrix that defines the new coordinate system. To set up the matrix, use [`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md) with [`M_COORDINATE_SYSTEM_TRANSFORMATION`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md); specify the coordinates of the new coordinate system's origin and the components of two of its axes. The third axis will automatically be calculated such that it is perpendicular to the first two and respects the right hand rule. [`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md) with [`M_COORDINATE_SYSTEM_TRANSFORMATION`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md)returns a transformation matrix that moves your new coordinate system to your current working coordinate system. If you want to take a profile, for example, [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) will use the XY-plane of your new coordinate system as the slicing plane along which to extract points, and you can set a limit on the X-dimension, so that the slicing plane is finite.

The image below shows how a function like [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) uses the XY-plane (Z=0) of the new specified coordinate system; the function takes the XY-plane as the oriented plane.

*[Image: 3dim_New-coordinate-system.png]*
