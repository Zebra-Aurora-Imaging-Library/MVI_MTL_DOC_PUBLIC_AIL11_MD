---
doctype: UserGuide
part: "3D processing and analysis"
chapter: Common_tasks_with_3D_data
section: Locating_defects_in_a_point_cloud
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / Common_tasks_with_3D_data / Locating defects in a point cloud"
---

# Locating defects in a point cloud

When working in 3D, you will often want to identify differences between a target point cloud and a reference model, and to locate those differences. For example, if you have a scanned object and a CAD model, any scratches, bumps, or other irregularities should be identified and located.

The following is a general outline for this process:

1. Use the [3D Registration](../C40_3D_Registration/ChapterInformation.md) or [3D Model Finder](../C37_3D_Model_finder/ChapterInformation.md) module to align the target and reference point clouds. These modules allow you find the transformation required to align the two point clouds ([`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) with [`M_REGISTRATION_MATRIX`](../../Reference/3dreg/M3dregCopyResult.md) or [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md) with [`M_FIXTURING_MATRIX`](../../Reference/3dmod/M3dmodCopyResult.md), respectively). You can then apply the transformation to the target point cloud using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md) to fixture it to the same working coordinate system.
   Whether to use 3D registration or 3D model finder depends on your application. Both modules can register a target to a reference point cloud. With 3D registration, the reference object must be a point cloud. With 3D model finder, you should use a surface model ([`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) with [`M_SURFACE`](../../Reference/3dmod/M3dmodDefine.md)), unless your object can be approximated by a 3D geometry (such as a cylinder). Note that for geometric models, you can use [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) to define the model parametrically. You cannot parametrically define an object with the 3D Registration module.
   For objects for which you have a model and scene available, with little to no background and minimal rotation of the object within the scene, use 3D registration.
   *[Image: LocatingDefectsReferenceAndTarget.png]*
2. Use [`M3dmetDistance`](../../Reference/3dmet/M3dmetDistance.md) to compute the distance between target and reference points. This results in a distance image. Alternatively, use [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) to write the distance result to a container, image buffer, or a calculate 3D metrology result buffer.
   *[Image: LocatingDefectsDistanceImage.png]*
3. Threshold the distance image using [`MimBinarize`](../../Reference/im/MimBinarize.md) to identify the target points whose distances are too far from the reference (and therefore indicate a potential flaw or defect).
4. Pass the binarized image to [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) to crop the target point cloud. Once cropped, the points that are kept in the target point cloud are those whose distances from the reference point cloud are considered unacceptable. Note that you can specify to store results in an unorganized point cloud ([`M_UNORGANIZED`](../../Reference/3dim/M3dimCrop.md)); this typically uses less memory. However, if the target point cloud was initially organized, keeping the organization could prove useful. See step 5 below.
   *[Image: LocatingDefectsDefect.png]*
5. Segment your point cloud.
   1. If your data is organized, use the binarized image with the 2D Blob Analysis module to define a segmentation. This approach is typically faster than calculating a segmentation from the point cloud. See [](S05_Locating_defects_in_a_point_cloud.md).
   2. If your data is unorganized or the above method didn't meet your needs, use the [3D Blob Analysis](../C36_3D_Blob_analysis/ChapterInformation.md) module to segment the cropped point cloud into separately identified blobs. Use [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) to perform the segmentation. The resulting blobs should be your flaws. Keep in mind the following:
      - Carefully set the [`M_MAX_DISTANCE`](../../Reference/3dblob/M3dblobControl.md) control type. The specified distance should ensure that points belonging to a defect are considered neighbors and therefore grouped together. To get a good initial estimate for [`M_MAX_DISTANCE`](../../Reference/3dblob/M3dblobControl.md), you can perform a distance-to-nearest-neighbor statistics calculation (using [`M3dimStat`](../../Reference/3dim/M3dimStat.md) with [`M_STAT_CONTEXT_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimStat.md)) on the original point cloud. You might want to use a multiplication factor that is greater than 1 on the retrieved statistic ([`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_DISTANCE_TO_NEAREST_NEIGHBOR_MAX`](../../Reference/3dim/M3dimGetResult.md)).
      - Set the [`M_NUMBER_OF_POINTS_MIN`](../../Reference/3dblob/M3dblobControl.md) control type to an appropriate value to create blobs with enough points to be considered a defect; groups of points with less than this minimum will not be segmented into blobs and are ignored.
      - The KD tree search mode ([`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dblob/M3dblobControl.md) set to [`M_TREE`](../../Reference/3dblob/M3dblobControl.md)) is typically more precise than the other modes when determining which points are neighbors for the [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) operation. This is because the KD tree search mode chooses points that are closest in 3D space, using the Euclidean distance.
6. Once segmented into blobs, you can retrieve the orientation and center point of each blob using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md). See [Retrieving the orientation and center point of a 3D object](S03_Retrieving_the_orientation_and_center_point.md).

Once you have performed the segmentation, you can do the following with the resulting blobs:

- Use [`M3dblobExtract`](../../Reference/3dblob/M3dblobExtract.md) to copy the blobs (flaws) to their own container.
- Compute and retrieve blob features such as the centroid and bounding box, using [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) and [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md).
- Perform additional operations on the blobs using [`M3dblobSelect`](../../Reference/3dblob/M3dblobSelect.md) [`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md), and/or [`M3dblobSort`](../../Reference/3dblob/M3dblobSort.md).
- Copy blob results into an index or label image, using [`M3dblobCopyResult`](../../Reference/3dblob/M3dblobCopyResult.md) with [`M_INDEX_IMAGE`](../../Reference/3dblob/M3dblobCopyResult.md) or [`M_LABEL_IMAGE`](../../Reference/3dblob/M3dblobCopyResult.md), respectively. You can use these images to identify specific blobs, since each pixel in an index or label image is set to the index or label of its corresponding blob. You can then pass a specific index or label to [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) to target an individual blob for feature calculation.

For a discussion of other considerations when working with 3D data, see [Implementation considerations](S02_Implementation_considerations.md).

## Segmenting using 2D blob analysis

To define a segmentation from a binarized image, perform the following:

1. Use [`MblobControl`](../../Reference/blob/MblobControl.md) to configure a 2D blob analysis context with appropriate settings, including [`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md) set to [`M_INDIVIDUAL`](../../Reference/blob/MblobControl.md) and [`M_IDENTIFIER_TYPE`](../../Reference/blob/MblobControl.md) set to [`M_BINARY`](../../Reference/blob/MblobControl.md).
2. Pass the binarized image to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) to compute labels for the binarized pixel groupings.
3. Call [`MblobLabel`](../../Reference/blob/MblobLabel.md) to generate a label image.
4. Pass the label image to [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) while also specifying the [`M_SEGMENTATION_CONTEXT_LABEL_IMAGE`](../../Reference/3dblob/M3dblobSegment.md) predefined context. The resulting 3D blob analysis result buffer will hold the segmentation in which flaws are separately identified into blobs.

With this approach, points are grouped into blobs based on the 2D pixel lattice of the range component, rather than the world distance between points.
