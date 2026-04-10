---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Finding_the_transformation_between_2_sets_of_points
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Finding the transformation between 2 sets of points"
---

# Finding the transformation between 2 sets of points with a known pairing

You can use [`M3dimFindTransformation`](../../Reference/3dim/M3dimFindTransformation.md) to find the best rigid or affine transformation between two sets of points, if a minimum pairing between the points is known. A rigid transformation can be a combination of translation and/or rotation transformations. An affine transformation can be a combination of translation and/or rotation and/or scale transformations; the scale can be uniform or non-uniform.

> **Note:** Note that if the pairing is not known, or if no real pairing exists, you should use [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) to calculate the optimal pairwise registration. You can then obtain the transformation matrix that aligns the points, using [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) with [`M_REGISTRATION_MATRIX`](../../Reference/3dreg/M3dregCopyResult.md). For more information, refer to the [3D Registration](../C40_3D_Registration/ChapterInformation.md) module.

If you know there is a minimum pairing between the points, you can find point pairs in several ways. For example, you can compute an object's ideal corner points to create a target set of points. Then, find the same corners in the point cloud of the object; to do so, use [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) to fit planes to the object's faces, and then use [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) to find the planes' intersecting corner points. Once you have your source and target sets of points, you can pass them to [`M3dimFindTransformation`](../../Reference/3dim/M3dimFindTransformation.md), which calculates the transformation required to fixture source points to the target positions. Note that in this example, the target points are computed independently, based on known dimensions of an ideal object, and are not calculated using Aurora Imaging Library.

Another way to find point pairs is to locate common points between two views of the same scene (for example, from two 3D sensors set up at different angles). You can extract points from the same object located in both views (for example, a calibration grid or a code). To do so, pass the acquired point cloud's reflectance component to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McodeRead`](../../Reference/code/McodeRead.md) and use the respective Calibration or Code modules to identify and extract common 2D points. Then, pass the world coordinates of the extracted points to [`M3dimGetList`](../../Reference/3dim/M3dimGetList.md), which returns corresponding 3D coordinates. Repeat as required to establish sets of points that you can pass to [`M3dimFindTransformation`](../../Reference/3dim/M3dimFindTransformation.md), which calculates the required transformation between the source and the target. Note that, in this example, established points are usually coplanar, so this approach is suitable for rigid, and not affine, transformations.

To find a transformation between two sets of points, apply the following:

1. Find point pairings so that you have a target set and a source set for which to find the transformation. You can store sets of points in two containers, two Aurora Imaging Library array buffers, or any combination of container and array buffer.
2. Allocate a result object, using either [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_FIND_TRANSFORMATION_RESULT`](../../Reference/3dim/M3dimAllocResult.md), or [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).
3. Find the transformation between the source and target sets of points, using [`M3dimFindTransformation`](../../Reference/3dim/M3dimFindTransformation.md).
4. If you passed a result buffer to [`M3dimFindTransformation`](../../Reference/3dim/M3dimFindTransformation.md), verify the success of the operation and the acceptable root-mean-square (RMS) error, using [`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_STATUS`](../../Reference/3dim/M3dimGetResult.md) and [`M_RMS_ERROR`](../../Reference/3dim/M3dimGetResult.md), respectively.
5. If you passed a result buffer to [`M3dimFindTransformation`](../../Reference/3dim/M3dimFindTransformation.md), and the operation was successful, allocate a transformation matrix object (using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md)) and copy the transformation into it using [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dim/M3dimCopyResult.md).
6. If you passed a transformation matrix object to [`M3dimFindTransformation`](../../Reference/3dim/M3dimFindTransformation.md), verify the success of the operation using [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_RIGID`](../../Reference/3dgeo/M3dgeoInquire.md) or [`M_AFFINE`](../../Reference/3dgeo/M3dgeoInquire.md), and ensure that the function returns [`M_TRUE`](../../Reference/3dgeo/M3dgeoInquire.md).
