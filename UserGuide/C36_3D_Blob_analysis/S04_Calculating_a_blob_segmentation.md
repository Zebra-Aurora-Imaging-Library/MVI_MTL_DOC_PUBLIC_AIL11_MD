---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Blob_analysis
section: Calculating_a_blob_segmentation
module_tag: 3dblob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Blob_analysis / Calculating a blob segmentation"
---

# Calculating a blob segmentation

To analyze blobs, you must first segment the point cloud into separately identified blobs. You can calculate the blob segmentation from the point cloud itself (for example, using distances between points). Alternatively, you can define a blob segmentation using a label image, which has its pixels set to values that will identify corresponding points in a point cloud as belonging to a specific blob.

## Segmentation from the point cloud

To calculate the blob segmentation from a point cloud, use [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) with an appropriate segmentation 3D blob analysis context. You can use a predefined context that counts all valid points as belonging to a single blob ([`M_SEGMENTATION_CONTEXT_WHOLE_IMAGE`](../../Reference/3dblob/M3dblobSegment.md)), or you can use a previously allocated context ([`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_SEGMENTATION_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md)), for which you can modify blob segmentation settings using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md). Note that you can still choose to treat the point cloud as a single blob when using a previously allocated context ([`M_SEGMENTATION_MODE`](../../Reference/3dblob/M3dblobControl.md) set to [`M_WHOLE_IMAGE`](../../Reference/3dblob/M3dblobControl.md)).

## Segmentation using a label image

To define the blob segmentation using a label image, you must use [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) to copy the label image into a 3D blob analysis result buffer. When doing so, specify either [`M_SEGMENTATION_CONTEXT_LABEL_IMAGE`](../../Reference/3dblob/M3dblobSegment.md) (to use a predefined context) or specify a previously allocated context and set [`M_SEGMENTATION_MODE`](../../Reference/3dblob/M3dblobControl.md) to [`M_LABEL_IMAGE`](../../Reference/3dblob/M3dblobControl.md) using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md).

The value of each pixel in the label image will identify corresponding points in a point cloud as belonging to a specific blob, using a label that is unique for each blob. Note that the location of each pixel in the label image corresponds to the point at the same location in the point cloud container. Pixels with a value of zero represent points that are not part of any blob. Invalid points must have a label of zero; otherwise, 3D blob functions that use the label (for example, [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md)) might try to operate on the invalid points, causing an error.

To create a label image, you can use any binarized 2D version of the point cloud. For example, if your point cloud container is organized, you can use [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) on its confidence component. Alternatively, you can binarize the reflectance component, or perform any strategy that gives binarized results for the points, and store the binarization in a component or buffer, before passing it to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md). If the point cloud is unorganized, you can pass the point cloud to [`M3dimProject`](../../Reference/3dim/M3dimProject.md) or [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) to generate a depth map, which you can then pass to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md). Finally, you can pass the resulting (2D) blob analysis result buffer to [`MblobLabel`](../../Reference/blob/MblobLabel.md), which creates the label image. You can also use [`MimLabel`](../../Reference/im/MimLabel.md) to generate the label image, instead of using both [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) and [`MblobLabel`](../../Reference/blob/MblobLabel.md), provided that the label image is all that you require (that is, you don't need to perform calculations on 2D blobs).

Note that, when passing a depth map to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), the depth map should be associated with an ROI that acts as the blob identifier image. Use [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with [`M_RASTERIZE_DEPTH_MAP_VALID_PIXELS`](../../Reference/buf/MbufSetRegion.md) to associate an ROI with a depth map. For more information, see [Features related to depth maps](../C06_Blob_analysis/S12_Features_related_to_depth_maps.md).

Note that you can let Aurora Imaging Library recompute the labels in the label image so that the labels are consecutive. To do so, call [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) with a previously allocated context, for which you have enabled [`M_RELABEL_CONSECUTIVE`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

## Segmentation context control settings

When not using a default context for the blob segmentation, you can use [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) to modify segmentation criteria. Typically, you should leave [`M_SEGMENTATION_MODE`](../../Reference/3dblob/M3dblobControl.md) set to its default ([`M_EUCLIDEAN`](../../Reference/3dblob/M3dblobControl.md)), which allows you to specify settings for all other segmentation context control types. If segmenting in the whole image mode ([`M_WHOLE_IMAGE`](../../Reference/3dblob/M3dblobControl.md)), only [`M_NUMBER_OF_POINTS_MIN`](../../Reference/3dblob/M3dblobControl.md) and [`M_NUMBER_OF_POINTS_MAX`](../../Reference/3dblob/M3dblobControl.md) are available. Similarly, if segmenting using a label image, only minimum/maximum number of points, as well as [`M_RELABEL_CONSECUTIVE`](../../Reference/3dblob/M3dblobControl.md), are available.

To set the minimum or maximum number of points per blob, specify [`M_NUMBER_OF_POINTS_MIN`](../../Reference/3dblob/M3dblobControl.md) or [`M_NUMBER_OF_POINTS_MAX`](../../Reference/3dblob/M3dblobControl.md), respectively. You can use these control types to remove blobs that correspond to a few isolated points, or to remove the background, which is often the largest blob.

To distinguish between relatively flat regions, you can segment a point cloud into planar blobs. You can set a maximum angle between the normal of a point and a blob's average normal, for the point to be added to that blob with [`M_GLOBAL_NORMAL_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md). For example, consider a point cloud of the surfaces on a cube, with the normal vectors of each surface pointing outwards. A maximum angle of 45 degrees will segment the point cloud into six blobs (one for each surface). Similarly, for a hexagonal prism, a maximum angle of 30 degrees will segment the point cloud into one blob for each surface. In general, setting a smaller maximum angle forces the blobs into more planar (flatter) regions, allowing for less variation in each blob. Now consider that the normals of the top and bottom surfaces of the cube point in the same direction. To prevent these parallel surfaces from being merged into the same blob (due to having the same normal angle), you can set the maximum distance between planar blobs with [`M_GLOBAL_PLANE_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md).

*[Image: 3dblob_SegmentationGlobal.png]*

To separate the background from objects in your scene, you can also use [`M_COLOR_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md). This works well if your objects are similarly colored, and the background is distinct. [`M_COLOR_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md) specifies the maximum difference between the color of two points for them to be considered neighbors (provided that a reflectance or intensity component exists in the point cloud container). Set how to calculate the distance with [`M_COLOR_DISTANCE_MODE`](../../Reference/3dblob/M3dblobControl.md).

To distinguish between blobs that are touching, you can use the angle difference between point normal vectors ([`M_NORMAL_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md)), since the normals of neighboring points will oppose each other if the points belong to separate blobs. Set how to calculate the angle difference with [`M_NORMAL_DISTANCE_MODE`](../../Reference/3dblob/M3dblobControl.md). Use [`M_DIRECTION`](../../Reference/3dblob/M3dblobControl.md) (default) if you expect point normal vectors to be correctly oriented; use [`M_ORIENTATION`](../../Reference/3dblob/M3dblobControl.md) if not (that is, you expect some normals to be flipped).

Set the maximum distance within which two points are considered neighbors with [`M_MAX_DISTANCE`](../../Reference/3dblob/M3dblobControl.md).

The control types [`M_NORMAL_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md) and [`M_MAX_DISTANCE`](../../Reference/3dblob/M3dblobControl.md) have no set maximum, by default. Typically, you should set one or both of these to an appropriate value for your source point cloud.

Set the maximum number of neighboring points to consider at each point with [`M_MAXIMUM_NUMBER_NEIGHBORS`](../../Reference/3dblob/M3dblobControl.md); the default is 49 points. Typically, the speed of a segmentation operation is proportional to the number of neighbors at each point. If the maximum distance and the maximum angle difference between point normal vectors are well chosen, increasing the maximum number of neighbors will only slow down the [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) operation. Lowering it too much, however, will lead to oversegmentation (too many blobs). See the illustration below.

|   |   |
| --- | --- |
| *[Image: 3dblob_MaxNumberNeighbors.png]* | All points (black and gray) belong to the same object. To belong to the same blob, all points must be neighbors or have neighbors in common. A lower maximum number of neighbors (inner ring) fails to include points to the right, so the black and gray points will be segmented to different blobs. A larger maximum number of neighbors (outer ring) includes more points than necessary; the extra included points are already considered part of the same blob because they have neighbors in common with points in the inner rings. The ideal maximum number (middle ring) includes both black and gray points, without taking extra time processing surplus points. If the objects that you need to segment are close together, adjust the maximum and normal distance settings. |
