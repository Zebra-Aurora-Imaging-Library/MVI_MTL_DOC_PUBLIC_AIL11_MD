---
doctype: UserGuide
part: "3D processing and analysis"
chapter: Common_tasks_with_3D_data
section: Implementation_considerations
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / Common_tasks_with_3D_data / Implementation considerations"
---

# Implementation considerations

Several considerations come into play when working with 3D data. For example, you will often want to remove the background from your scene before proceeding with most 3D operations. This section briefly describes some typical requirements or tasks when working in 3D.

*[Image: 3d_data_comparison.png]*

## Removing the background

If there is a large background plane (such as a table or conveyer) in the scene, you can remove it using one of the following methods (note that the first method is more accurate):

- Obtain a point cloud of the background without the objects, fit a plane to this point cloud using [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md), and then crop the background points in the original point cloud using [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) with the fitted plane. For more information, see [Cropping or masking points](../C35_3D_Image_processing/S05_Cropping_or_masking_points.md).
- Fit a plane to the background using [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md), copy the mask constructed from the points considered outliers during the fit using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_OUTLIER_MASK`](../../Reference/3dmet/M3dmetCopyResult.md) (foreground points will be considered outliers and will have a non-zero value), and then pass the resulting mask image to [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) to crop out the background plane. For more information, see [Using a fitted geometry to mask points from a point cloud](../C39_3D_Metrology/S04_3D_Fitting.md).

## Fixturing

Retrieving a fixturing matrix is often helpful (for example, you can use this matrix to fixture an object to the origin for comparison purposes with a reference model). You can typically obtain a fixturing matrix using an [`M...CopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) function (for instance, [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md) with [`M_FIXTURING_MATRIX`](../../Reference/3dmod/M3dmodCopyResult.md)).

Get the inverse of this matrix ([`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) with [`M_INVERSE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)) if your goal is to perform an action at the location at which an object was found or detected. For example, when drawing graphics, you can pass the inverse of the fixturing matrix to [`M3dgraText`](../../Reference/3dgra/M3dgraText.md) to annotate an object at its found location in the point cloud. Another use case is when you need to send a robot to pick up an object. See [Item picking robot](S04_Item_picking_robot.md).

## Sampling

The resolution between point clouds should be similar for some 3D operations (for example, when using the 3D Model Finder module with a surface model or when using the 3D Registration module to register multiple point clouds). You can use [`M3dimSample`](../../Reference/3dim/M3dimSample.md) to subsample or upsample a point cloud to ensure it has a suitable density. Note that upsampling is referred to as surface sampling, since it is typically applied to a meshed point cloud. Subsampling is useful, for example, to maintain an approximately similar distance between points after a merge operation. You can also use [`M3dimSample`](../../Reference/3dim/M3dimSample.md) to subsample an unorganized point cloud to obtain an organized point cloud, or to generate a point cloud from a 3D geometry. See [Sampling a point cloud or a 3D geometry](../C35_3D_Image_processing/S11_Sampling_a_point_cloud_or_a_3D_geometry.md) for more information.

Surface sampling is useful when using 3D registration to register a point cloud to a reference CAD model. In this scenario, since a meshed point cloud restored from a CAD file is often sparse, with planar surfaces consisting of much larger surface triangles compared to other areas, surface sampling of the CAD-sourced point cloud makes its point density compatible with that of the acquired point cloud. For more information, see [Surface sampling a meshed point cloud](../C35_3D_Image_processing/S11_Sampling_a_point_cloud_or_a_3D_geometry.md).

For details about sampling related to 3D model finder operations, see [Model and target point cloud resolution](../C37_3D_Model_finder/S08_Search_settings_specific_to_surface_models.md).

## Normals

Many 3D functions require a normals component ([`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md)), in which calculated point normal values are stored. Some 3D cameras and sensors provide normals; others do not. If yours does not, you can calculate a point cloud's unit normal vectors using [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md). Note that some functions will automatically generate a normals component if it is required for the operation (for example, when surface sampling using [`M3dimSample`](../../Reference/3dim/M3dimSample.md)).

Normals are required, for example, when using:

- [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) to find any model.
- [`M3dimFilter`](../../Reference/3dim/M3dimFilter.md) to filter using the source point cloud's normals information ([`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_USE_SOURCE_NORMALS`](../../Reference/3dim/M3dimControl.md) set to [`M_TRUE`](../../Reference/3dim/M3dimControl.md)).
- [`M3dimMesh`](../../Reference/3dim/M3dimMesh.md) to calculate a mesh using the local plane or smoothed surface reconstruction mode ([`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimControl.md) or [`M_MESH_SMOOTHED`](../../Reference/3dim/M3dimControl.md), respectively).
- [`M3dimSample`](../../Reference/3dim/M3dimSample.md) to subsample using certain subsample modes ([`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) set to [`M_SUBSAMPLE_NORMAL`](../../Reference/3dim/M3dimControl.md) or [`M_SUBSAMPLE_GEOMETRIC`](../../Reference/3dim/M3dimControl.md)).
- [`M3dimStat`](../../Reference/3dim/M3dimStat.md) to calculate statistics on the normals component ( [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_COMPONENT_OF_INTEREST`](../../Reference/3dim/M3dimControl.md) set to [`M_COMPONENT_NORMALS_AIL`](../../Reference/3dim/M3dimControl.md)).
- [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) to calculate a blob segmentation that uses normal vector angles ([`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_NORMAL_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md) or [`M_GLOBAL_NORMAL_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md) set to a non-infinite angle, or [`M_GLOBAL_PLANE_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md) set to a non-infinite distance).
- [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) to color a point cloud graphic using the source container's normals component ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) set to [`M_COMPONENT_NORMALS_AIL`](../../Reference/3dgra/M3dgraControl.md)).

Note, you must ensure that the normal vectors are normalized to 1 before calling a function that requires a normals component. These functions assume that non-zero normal vectors are unit normal vectors, and they do not normalize them. You can use [`M3dimFix`](../../Reference/3dim/M3dimFix.md) with [`M_NORMALS_NORMALIZED`](../../Reference/3dim/M3dimFix.md) to validate whether each point with a non-zero confidence has a normal with a norm of exactly 1 or 0, and normalize them to have a norm of 1 if not.

## Meshes

Generating a triangular mesh structure for a point cloud or depth map is known as surface reconstruction, and is useful for certain 3D operations, such as calculating volumes (see [Calculating a volume](../C39_3D_Metrology/S06_Calculate_volume.md)). A meshed point cloud can also help improve the performance of an [`M3dimProject`](../../Reference/3dim/M3dimProject.md) or [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) operation. Note also that a meshed point cloud typically shows surface texture better than non-meshed point clouds when displaying 3D data. For more information, see [Surface reconstruction](../C35_3D_Image_processing/S10_Generating_a_mesh_for_a_point_cloud.md).

## Merging

Merging multiple point clouds or depth maps together is useful to obtain a complete view of the scene when it is not possible to capture the entire scene with a single camera. You can use [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md) with a stitch context to combine multiple organized point clouds into a single organized point cloud, or multiple depth maps into a single depth map. See [Using 2D image processing techniques with 3D data](S02_Implementation_considerations.md) for information on using depth maps for faster processing of 3D data. [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md) supports stitching overlapping depth maps and depth maps with offsets that are not well aligned. You can use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) to control how the merge operation is performed.

When merging point clouds (unorganized or organized), you can pass [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md) a subsample context to reduce the number of points in the resulting point cloud. Note that if you pass a subsample context that specifies an organized destination point cloud, the resulting merged point cloud will be organized. Without such a context (or a stitch context), the resulting point cloud is always unorganized when using [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md). For information on point cloud organization, see [Organized and unorganized point clouds](../C35_3D_Image_processing/S12_Organized_and_unorganized_point_clouds.md).

To align the working coordinate systems of multiple point clouds and then merge, use [`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md) instead. See [Merging point clouds, retrieving specific results, and drawing results](../C40_3D_Registration/S08_Using_Result_Merge.md)for more information.

## Using 2D image processing techniques with 3D data

While 3D data and 2D image processing techniques are fundamentally different, there are situations where they can be used together in point cloud processing. Doing so is often advantageous in terms of time, since 3D operations are typically time-expensive.

One example of using 2D image processing techniques with 3D data is to use a 2D view of the 3D data to find the location of an object, and then to use this information to analyze the object in 3D. The [`M3dimProject`](../../Reference/3dim/M3dimProject.md) and [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) functions project point cloud data into a depth map, which you can then use with a 2D finder module, such as [Model Finder](../C08_Geometric_Model_Finder/ChapterInformation.md) or [Pattern Matching](../C07_Pattern_matching/ChapterInformation.md). Since the depth map is calibrated to give results in world units, you can map your 2D finder results back onto the 3D data to extract 3D properties of a found object of interest. For example, after using a 2D finder module, you can use [3D registration](../C40_3D_Registration/ChapterInformation.md) to obtain an object's exact pose in 3 dimensions.

Many 3D modules allow you to create various 2D images, which are useful for further processing or diagnostic analysis. For example, you can use [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) to create a distance image, an overlap mask, or a pair index image. These images allow you to examine the results of specific iterations of the registration process. See [3D Registration](../C40_3D_Registration/ChapterInformation.md) for more information.

Occasionally, you might want to use an individual component of a point cloud. Since a component is allocated as an Aurora Imaging Library buffer, you can use the component with any 2D function that takes a buffer. For example, you can threshold the Z-coordinates stored in a point cloud's range component using [`MimBinarize`](../../Reference/im/MimBinarize.md). Doing so allows you to isolate data from the background based on Z-values. This is a quicker operation than using [`M3dimProject`](../../Reference/3dim/M3dimProject.md) or [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) to project the point cloud into a depth map. When using [`MimBinarize`](../../Reference/im/MimBinarize.md), the resulting destination image buffer becomes a binarized mask that you can pass to [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md), which will crop out points that did not meet the binary threshold condition. You can then process the resulting data with a 3D module using, for example, statistical analysis (using [`M3dimStat`](../../Reference/3dim/M3dimStat.md)) to obtain point distribution data, or with the 3D blob analysis module to segment the points or calculate features for clusters of points.

Another component that is sometimes useful to manipulate directly is the confidence component. You could manipulate it to limit a processing operation to only certain points, rather than the whole point cloud. For more information, see [](../C41_3D_Containers/S07_Using_components_individually.md). You could also pass the confidence component to the 2D Blob Analysis module to generate a segmented label image. You can then use the label image to calculate 3D features for the blobs using 3D blob analysis ([`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) and then [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md)). For more information, see [](S05_Locating_defects_in_a_point_cloud.md).
