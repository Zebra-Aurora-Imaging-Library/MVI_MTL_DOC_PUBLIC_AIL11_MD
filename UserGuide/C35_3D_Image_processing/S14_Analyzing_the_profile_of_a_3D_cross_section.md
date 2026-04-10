---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Analyzing_the_profile_of_a_3D_cross_section
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Analyzing the profile of a 3D cross section"
---

# Analyzing the profile of a 3D cross-section

Aurora Imaging Library allows you to take the profile of a point cloud, 3D geometry, mesh, or depth map, along a specified slicing plane. This can be useful, for example, to analyze and verify the contour of an object's surface along a slice.

## Taking a profile

To take the 3D profile of a point cloud, 3D geometry, or mesh along a slicing plane, use [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) with [`M_PROFILE_POINT_CLOUD`](../../Reference/3dim/M3dimProfile.md), [`M_PROFILE_GEOMETRY`](../../Reference/3dim/M3dimProfile.md), or [`M_PROFILE_MESH`](../../Reference/3dim/M3dimProfile.md), respectively. [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) allows you to limit the size of the slicing plane, so it requires an oriented plane (a plane with an origin and two in-plane axes). Therefore, instead of a geometry object, the function takes a transformation matrix that defines a coordinate system whose XY-plane (Z=0) is used as the slicing plane. You can establish this new coordinate system, using [`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md) with [`M_COORDINATE_SYSTEM_TRANSFORMATION`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md); for more information, refer to [Defining an oriented plane from a transformation matrix](../C44_Using_the_3D_Geometry_module/S02_Defining_3D_geometries.md). When taking the 3D profile of a mesh, the source container of the mesh must have an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component.

The image below shows a slicing plane being used to take the profile of a point cloud. Note that the slicing plane is oriented; it is defined by the XY-plane of a new coordinate system and thus has a set origin and its X and Y axes are explicitly defined. Using an oriented plane when taking the profile allows you to customize the slice. For example, if you want a partial profile without completely slicing through the point cloud, you can specify a limit on the X-dimension of the oriented plane.

*[Image: M3dim_profile_pointcloud.png]*

When taking the 3D profile of a 3D geometry or mesh, Aurora Imaging Library samples and extracts points along the intersection of the specified slicing plane and the geometry or mesh. For point clouds, however, the points in the profile do not necessarily fall exactly on the slicing plane. Instead, Aurora Imaging Library subdivides the slicing plane into a grid and establishes the dimension of each grid's cell according to specified values, known as the sampling distance. For each cell, the point closest to the slicing plane is selected for the profile, as long as it is within a certain distance, known as the cutoff distance, from the plane.

To take the 3D profile of a depth map, use [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) with [`M_PROFILE_DEPTH_MAP`](../../Reference/3dim/M3dimProfile.md) or [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md). You must pass the X- and Y-coordinates of the start and end points that define the line along which to extract points, represented in either world units ([`M_WORLD`](../../Reference/3dim/M3dimProfile.md)) or pixel units ([`M_PIXEL`](../../Reference/3dim/M3dimProfile.md)). The line is extended downwards, effectively defining a slicing plane, with the pixels' intensity values indicating depth (Z-values). When using [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md), if the specified line crosses a valid depth map pixel, the corresponding point is included in the profile. Whereas, [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md) can sample multiple source pixels for each profile point so that the thickness of the line is taken into consideration; you can use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_PROFILE_THICKNESS`](../../Reference/3dim/M3dimControl.md) to specify a thickness perpendicular to the profile line, and all points corresponding to pixels within the specified thickness of the line will be considered when determining the value of the profile point. Note that [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md) always uniformly extracts points along the profile line; pixels are sampled along the line using a specified sampling distance ([`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dim/M3dimControl.md)) so that each point in the profile is at an equal distance away from the next. You can use [`M_PROFILE_INTERPOLATION_MODE`](../../Reference/3dim/M3dimControl.md) to specify the interpolation mode used to calculate the value for each point in the sample. Use [`M_PROFILE_MIN_VALID_PERCENTAGE`](../../Reference/3dim/M3dimControl.md) to set the minimum percentage of points in a sample that must be valid to set the corresponding profile point to a valid value, and use [`M_PROFILE_ACCUMULATE_TYPE`](../../Reference/3dim/M3dimControl.md) to specify whether to take the minimum, maximum, or mean value of the points in the sample to create the profile point when the minimum valid percentage is met. These concepts are shown in the image below.

*[Image: 3dmeas_profile_samples.png]*

The image above illustrates the extracted profile when [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dim/M3dimControl.md) is set to 2, [`M_PROFILE_SAMPLE_SIZE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_RELATIVE_TO_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md), [`M_PROFILE_THICKNESS`](../../Reference/3dim/M3dimControl.md) is set to 5, [`M_PROFILE_THICKNESS_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_RELATIVE_TO_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md), [`M_PROFILE_INTERPOLATION_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimControl.md), [`M_PROFILE_MIN_VALID_PERCENTAGE`](../../Reference/3dim/M3dimControl.md) is set to 50%, and [`M_PROFILE_ACCUMULATE_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MEAN`](../../Reference/3dim/M3dimControl.md).

[`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) does not return points directly; instead, it stores the sampled points in a 3D image processing result buffer ([`M_PROFILE_RESULT`](../../Reference/3dim/M3dimAllocResult.md)). [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md) can store the points in a destination container or a 3D image processing result buffer. To retrieve these coordinates from the result buffer, call [`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md). You can retrieve the X-, Y-, and Z-coordinates relative to the working coordinate system with [`M_WORLD_X`](../../Reference/3dim/M3dimGetResult.md), [`M_WORLD_Y`](../../Reference/3dim/M3dimGetResult.md), and [`M_WORLD_Z`](../../Reference/3dim/M3dimGetResult.md). Alternatively, you can retrieve the X- and Y- coordinates along the slicing plane, with respect to the origin of the plane, with [`M_PROFILE_PLANE_X`](../../Reference/3dim/M3dimGetResult.md) and [`M_PROFILE_PLANE_Y`](../../Reference/3dim/M3dimGetResult.md). Note that for depth map profiles, since points are extracted along a specified line rather than an actual slicing plane, [`M_PROFILE_PLANE_X`](../../Reference/3dim/M3dimGetResult.md)will return the real world distance along the specified line for each extracted point and [`M_PROFILE_PLANE_Y`](../../Reference/3dim/M3dimGetResult.md) will return values that correspond to the depth at each pixel.

To draw the result of a profile operation, use [`M3dimDraw3d`](../../Reference/3dim/M3dimDraw3d.md). This function draws the profile points into a 3D graphics list.

## Setting the slicing plane according to a reference object

To take the profile of an object along a slicing plane that is at a known location on a reference object, you can:

1. Take a depth map of the object's point cloud using [`M3dimProject`](../../Reference/3dim/M3dimProject.md) or [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md). See [Generating fully corrected depth and intensity maps](S15_Generating_a_fully_corrected_depth_map.md) for more information.
2. Find a model of a distinguishing feature on the object using [`MmodFind`](../../Reference/mod/MmodFind.md). Note that since the Model Finder module works in 2D, the object is assumed to rotate only around its Z-axis, meaning it is always resting on the same side.
3. Retrieve the calibration fixture matrix that contains the transformation coefficients required to fixture to the found occurrence, using [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md), the Model Finder result buffer, and a previously allocated transformation matrix object.
4. Apply the calibration fixture matrix to the object's point cloud so that it is at the same known position from the working coordinate system as the reference object is from its working coordinate system. To do so, use [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md).
5. Take the profile along the slicing plane using [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md)with [`M_PROFILE_POINT_CLOUD`](../../Reference/3dim/M3dimProfile.md). Pass the transformation matrix that defines the slicing plane for the reference object.

Note that the profile could have been taken directly from the depth map. However, if vertical surfaces have been scanned, they will not appear in the depth map; they will only appear in the point cloud. In addition, if the point cloud includes both the top view and the bottom view of the object, the profile would include points along the bottom view. The following is an example of taking the profile of a serpentine belt tensioner along a slicing plane. The X- and Y-axes in the cross-sectional profile refer to the X- and Y-axes of the profile plane ([`M_PROFILE_PLANE_X`](../../Reference/3dim/M3dimGetResult.md) and [`M_PROFILE_PLANE_Y`](../../Reference/3dim/M3dimGetResult.md)), not the working coordinate system.

*[Image: Met-3d-wheel-01.png]*

For additional insight, you can run the example _3dProfileMetrology.cpp_ from Aurora Imaging Example Launcher.

## Analyzing a profile

To analyze the surface of the object along the profile, you can fit one or more geometric shapes to it, such as a line and an arc, using the Metrology module. To add the profile points to a Metrology context, use [`MmetPut`](../../Reference/met/MmetPut.md). Since the points are not necessarily an ordered and connected chain, do not have [`MmetPut`](../../Reference/met/MmetPut.md) automatically interpolate the gradient angle for the points (that is, set the [`ControlFlag`](../../Reference/met/MmetPut.md) parameter to [`M_DEFAULT`](../../Reference/met/MmetPut.md)). For more information, see [Using external edgels and points with constructed features](../C21_Metrology/S08_Using_external_edgels_and_points.md).

Note that the Metrology module can only analyze 2D points along the same plane (the X- and Y-coordinates). Therefore, to analyze the profile of an object along the plane, extract the coordinates of the points along the plane with respect to the plane's origin ([`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_PROFILE_PLANE_X`](../../Reference/3dim/M3dimGetResult.md) and [`M_PROFILE_PLANE_Y`](../../Reference/3dim/M3dimGetResult.md)), instead of with respect to the working coordinate system.

The following animation demonstrates how to define a 3D profile and then use the Metrology module to obtain certain measurements for analysis.

*[Image: Met3DCrossSection]*
