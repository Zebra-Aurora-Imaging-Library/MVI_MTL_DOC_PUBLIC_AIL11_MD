---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Working_with_3D
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Working with 3D"
---

# Working with 3D

The Aurora Imaging Library 3D modules (M3d...) support display, analysis, and processing of 3D data of a scene. The 3D data can either be a point cloud or a depth map. A point cloud is a collection of data points, each with X-, Y-, and Z-coordinate values, and represents an object or scene in 3D space. A depth map is a representation of the depth information for every pixel within a given scene; each pixel can be seen as a 3D coordinate, with the intensity of the pixel corresponding to the Z-coordinate, and the X- and Y-coordinates of the pixel corresponding to the X- and Y-coordinates of a 3D point (where pixel and world units are suitably scaled).

You can perform arithmetic and statistical operations, as well as comparative measurements between point clouds, depth maps, and 3D geometries. For example, you can fit a 3D geometry to a point cloud or depth map and calculate distances between points and the fitted surface.

You can scale, rotate, and translate 3D data, compute 3D profiles to examine a cross-section of data, and merge multiple point clouds or perform pairwise registration between point clouds. You can also sample a subset of points or reconstruct surfaces (meshes) for point clouds.

You can grab point clouds or depth maps from a 3D sensor or generate point clouds from a laser line profiling setup. You can also load point clouds from PLY and STL files, or create them manually in your application. PLY and STL are 3D data formats that can be used to store point clouds acquired from 3D sensors or generated from CAD drawings.

## 3D data containers and grabbing

Typically, Aurora Imaging Library 3D functions work with 3D data stored in an Aurora Imaging Library container. A container is an object that contains buffers, called its components. You allocate a container using[`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md).

To grab 3D data from a 3D sensor, you should pass a container to [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigProcess`](../../Reference/dig/MdigProcess.md), or [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md). These functions will automatically allocate components in the container appropriate for the data being transmitted from the 3D sensor.

Each component of a 3D container stores a different type of information about the 3D scene. The type of data stored in a component is typically set automatically; if not, you must specify it using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md)with [`M_COMPONENT_TYPE`](../../Reference/buf/MbufControlContainer.md). Typically, respective positions in each component correspond to information about the same position in the scene.

*[Image: buf_point_cloud_container.png]*

*[Image: buf_depth_map_container.png]*

A 3D container has either a range component or a disparity component. A range component stores coordinates of points, or a depth map. A disparity component stores depth information grabbed from a stereoscopic camera. Typically, a 3D container also has a confidence component that stores whether each point should be used or ignored (for example, a point might be invalid because it was occluded during acquisition). The container can also have other components, such as an intensity component that stores the intensity or color of the points.

3D data in a container can be organized or unorganized. Organized 3D data is typically stored in components that are buffers with several rows; it is assumed that points close to each other in 3D space are also close to each other in the components (although not necessarily adjacent). Unorganized 3D data is stored in components that are buffers with a single row; the position of the points in the components has no meaning. For more information on point cloud organization, see [Organized and unorganized point clouds](../C35_3D_Image_processing/S12_Organized_and_unorganized_point_clouds.md).

For more information on grabbing from devices that transmit 3D data in a format that is compliant with an industry standard (such as GigE Vision or GenICam), see [Working with compliant cameras](../C42_Grabbing_from_3D_sensors/S04_Working_with_compliant_cameras.md). Note that some 3D sensors transmit data in a non-compliant format. In this case, see [Working with non-compliant cameras](../C42_Grabbing_from_3D_sensors/S05_Working_with_noncompliant_cameras.md).

## Converting 3D data for processing and 3D display

Although you can grab 3D data in many formats, you can only process and/or display 3D data if it is in the right format. You can inquire if it is 3D-processable or 3D-displayable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), or [`M_3D_DISPLAYABLE`](../../Reference/buf/MbufInquireContainer.md), respectively. If it isn't, you can convert it using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) (if the 3D data is in a format that is compliant with an industry standard). You can also use this function to convert a fully-corrected depth map image buffer to a 3D-processable point cloud, 3D-processable depth map, or a natively 3D-displayable container.

Before converting a 3D container using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the 3D settings of the range or disparity component must be set correctly. These settings identify how 3D data is stored in the container (such as what scaling must be applied in each axis direction for the data to be natively calibrated). If your 3D sensor is configured to transmit this information, these settings will be set automatically during acquisition. Otherwise, you must set the 3D settings of the range or disparity component manually using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md)with settings from the table[`Component3DSettings`](../../Reference/buf/MbufControlContainer.md).

3D displays, and some 3D processing functions, support fully-corrected depth maps stored in image buffers. You cannot use the range component of a container as though it were a fully-corrected depth map image buffer; you must project the 3D data in the container to a fully-corrected depth map image buffer using [`M3dimProject`](../../Reference/3dim/M3dimProject.md) or [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md). If the range component stores a depth map in a specific format, you can convert the container to a fully-corrected depth map image buffer using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

For more information, see[Preparing a container for display or processing](../C41_3D_Containers/S04_Preparing_a_container_for_display_or_processing.md).

## Displaying 3D data

To display 3D data in 3D, you must first allocate a 3D display using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md). You can then select a 3D-displayable container or a fully-corrected depth map image buffer to the 3D display, using [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md). If you pass a container that is not in a 3D-displayable format, Aurora Imaging Library will try to compensate by converting it internally. If this is not possible, an error will be generated.

Once a 3D display is shown in a window, you can look at the 3D data in the 3D display from different angles, either interactively using the mouse and keyboard or by manually specifying the required view using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md). For more information, see [Manipulating the view](../C43_3D_Display_and_graphics/S05_Manipulating_the_view.md).

You can also annotate the display with 3D graphics. To do so, inquire the 3D display's internal 3D graphics list using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md)with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md), and then use the functions of the [`M3dgra...`](../../Reference/3dgra/M3dgraBox.md) module to draw into this 3D graphics list. For more information, see [Annotating the 3D display](../C43_3D_Display_and_graphics/S06_Annotating_the_3D_display.md).

### PLY files and 3D Viewer

By default on Windows, PLY files are associated to Microsoft 3D Viewer. To successfully open a point cloud from a PLY file using 3D Viewer, the point cloud must contain a mesh; otherwise, you will get the error: "Couldn't load 3D model".

## Processing 3D data

3D processing and analysis functions can work on Aurora Imaging Library 3D-processable point cloud containers. Some can work on 3D-processable depth map containers and fully corrected depth map image buffers. Note that if a function has an optional functionality that is available for point cloud containers but not depth map image buffers, it will not be available for depth map containers.

### Calibration

When working with point cloud containers and/or 3D geometry objects, you do not associate them with a calibration context; all source data is treated as if it is natively calibrated. That is, a point cloud or geometry's coordinates are expected to be real world values.

Depth map containers are also not associated with a calibration context. 3D properties (for example, [`M_3D_OFFSET_X`](../../Reference/buf/MbufControlContainer.md) and [`M_3D_OFFSET_Y`](../../Reference/buf/MbufControlContainer.md)) are applied to the X and Y-coordinates stored in the range component to achieve world coordinates, which can be used in 3D processing and analysis functions. To work with a depth map image buffer, with the 3D modules, it must be in a calibrated image buffer and be fully corrected; when fully corrected, a depth map's pixels represent a constant size in X and Y in the world and the difference of one gray level in the depth map corresponds to a specified real-world distance ([`McalControl`](../../Reference/cal/McalControl.md) with [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md)). You can copy calibration information between depth map containers, from a depth map container to an image buffer, or from a depth map image buffer to a container, using [`M3dimConvertDepthMap`](../../Reference/3dim/M3dimConvertDepthMap.md) with [`M_CALIBRATE`](../../Reference/3dim/M3dimConvertDepthMap.md).

Coordinates of a point cloud reflect the calibration set at acquisition time using the 3D sensor; coordinates of point clouds loaded from PLY or STL files are always in world units. Each point cloud container has its own working coordinate system within which 3D point coordinates are expressed. You might need to transform the points in a point cloud so that they are expressed in the same working coordinate system as another point cloud using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md).

When a 3D geometry object is defined (for example, using [`M3dgeoBox`](../../Reference/3dgeo/M3dgeoBox.md)), its coordinates are assumed to reflect real-world distances.

### Storing the output of 3D functions

Many functions of the 3D Image Processing module require you to specify source and destination containers. Most of these functions compute results that affect 1 component. If the destination container doesn't have the component, the function adds it to the destination container. If it already has the component, the function tries to use it.

If the type, size, or number of bands of the components in the destination container are not appropriate to store resulting components, components are freed and reallocated as required at an appropriate size, with new Aurora Imaging Library identifiers.

Components are added, removed, or adjusted in the destination container so that it is 3D-processable.

In some cases, when affecting the range component, only the coordinates of the points are affected instead of where that data is saved in the component (for example, [`M3dimRotate`](../../Reference/3dim/M3dimRotate.md)). Similarly, the confidence component is typically used to mask out data in the range component instead of physically changing the size of the component (for example, [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md)).

Typically, when working with point clouds, processing is most efficient if the source data is organized.

### Regions of interest

Some Aurora Imaging Library 3D functions support depth maps with regions of interest (ROIs). For example, in the 3D Metrology module, the [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) function permits a source depth map image buffer or depth map container to have a raster type ROI or region component, respectively. A region component stores an ROI for the range component of the container. 3D processing functions only operate on points inside the ROI.

When converting 3D data to a depth map image buffer or depth map container, using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) or [`M3dimConvertDepthMap`](../../Reference/3dim/M3dimConvertDepthMap.md), you can copy an ROI from the source to an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI in the destination depth map image buffer or [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufInquireContainer.md) component in the destination depth map container, using [`M_PROPAGATE_REGION`](../../Reference/buf/MbufConvert3d.md). If the source has an ROI and you don't propagate it, each pixel outside the ROI is set to the invalid value in the destination image buffer or given a confidence value of 0 in the destination container.

3D-processable point cloud containers cannot have an [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufInquireContainer.md) component. There are 4 ways to specify a region of interest for a point cloud container:

- Pass a 3D geometry object to the functions that support them to limit the processing region. To do so, allocate a 3D geometry object (using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md)), define the geometry's shape (using the appropriate function in the 3D Geometry module, or obtained as a result from another 3D image processing function), and then pass the 3D geometry object to the 3D function.
- Copy the points of interest into a separate container and then use this container. To copy the points of interest, allocate a 3D geometry object, define the geometry's shape, and then use [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) to crop points that are inside/outside the specified geometry object. [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) can also perform cropping in-place (that is, the source container is the same as the destination container) without any reallocation if the resulting cropped point cloud is forced to have the same organizational structure as the source ([`M_SAME`](../../Reference/3dim/M3dimCrop.md)).
- Mask unwanted points using a calibrated mask image buffer. To do so, use [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md); 3D points projected onto 0-valued pixels in the specified calibrated mask image buffer are not kept in the resulting point cloud.
- Change the data in the confidence component of a container to mask out unwanted regions. To do so, inquire the confidence component's Aurora Imaging Library identifier using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_COMPONENT_ID`](../../Reference/buf/MbufInquireContainer.md), and then pass the identifier of the component to Aurora Imaging Library functions that work on image buffers to create the mask. For example, you can use the component with [`MbufCopyCond`](../../Reference/buf/MbufCopyCond.md), Aurora Imaging Library image processing functions (such as [`MimArith`](../../Reference/im/MimArith.md)), or draw into the component using the 2D graphics functions ([`Mgra...`](../../Reference/gra/MgraLines.md)). Values set to 0 in the confidence component ([`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufInquireContainer.md)) cause the corresponding coordinates in the range component to be ignored.

### Fixturing

Since you do not associate point cloud containers, depth map containers, and 3D geometry objects with a calibration context, you use a different fixturing method with the Aurora Imaging Library 3D modules, instead of the relative coordinate system method implemented for the 2D modules.

Before using a 3D processing or analysis function, you can transform coordinates to fixture point clouds or 3D geometries to a required position and orientation. To do so:

1. Allocate a transformation matrix object using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md).
2. Generate the required transformation coefficients using [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) or[`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). Alternatively, you can copy a resulting matrix from another Aurora Imaging Library 3D module (for example, the 3D Metrology or 3D Registration module) into the transformation matrix object.
3. Apply the transformation matrix to the 3D points using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md).

Typically, you store the resulting transformed points to a different container if your source point cloud has multiple regions that need to be analyzed.

For example, if the surface on which an object will be scanned is at a slant, obtain a point cloud of only the surface either by scanning the surface without the object, or masking out the object from an existing scan. You can then fit a plane to the surface using [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md), and copy the fixturing matrix of the fitted plane to a transformation matrix object using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_FIXTURING_MATRIX`](../../Reference/3dmet/M3dmetCopyResult.md). You can then transform the point clouds of subsequently scanned objects with this matrix using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md).

To depth map containers, you can add a matrix component ([`M_COMPONENT_MATRIX_AIL`](../../Reference/buf/MbufControlContainer.md)) to transform the depth map coordinates and fixture the depth map to a required position and orientation. For example, the 4x4 transformation matrix can be used to translate or rotate points in the XY-plane.

For more information, see [Fixturing in 3D](../C45_Fixturing_in_3D/S01_Fixturing_in_3D_overview.md).
