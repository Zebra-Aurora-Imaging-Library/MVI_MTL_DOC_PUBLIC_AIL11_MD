---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Blob_analysis
section: Steps_to_performing_3D_blob_analysis
module_tag: 3dblob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Blob_analysis / Steps to performing 3D blob analysis"
---

# Steps to performing 3D blob analysis

The following steps provide a basic methodology for using the Aurora Imaging Library 3D Blob Analysis module:

1. Allocate a segmentation 3D blob analysis context, using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_SEGMENTATION_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md). This context is used to specify how to segment a point cloud into blobs. Note that you can skip this step and use a predefined segmentation context instead.
2. Allocate a 3D blob analysis result buffer, using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md) with [`M_SEGMENTATION_RESULT`](../../Reference/3dblob/M3dblobAllocResult.md). The result buffer will hold the segmentation and any calculated feature results.
3. If required, adjust global 3D blob analysis settings to fit your application, using successive calls to [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md).
4. If you are using a label image to define the blob segmentation, create the image (for example, using [`MblobLabel`](../../Reference/blob/MblobLabel.md)).
5. Perform the segmentation, using [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) with either a point cloud container or the label image.
6. If feature calculations are required, allocate a calculate 3D blob analysis context, using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_CALCULATE_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md).
7. If required, calculate features and analyze the results. This involves the steps listed below.
   You can repeat these steps until you obtain all required results for the blobs of interest. Note that some blob features take longer to calculate (for example, minimum/maximum Feret diameter and nearest blob). Specifying fewer blobs when calculating these features can save time. Otherwise, it is often faster to calculate all the required features (except minimum/maximum Feret diameter and nearest blob) with a single call to [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md), and then selecting for blobs of interest afterwards.
   1. Enable the features to calculate, using successive calls to [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md).
   2. Calculate results for the features enabled for calculation, using [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md).
   3. Optionally, pass the result buffer to [`M3dblobSelect`](../../Reference/3dblob/M3dblobSelect.md) and [`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md) to create a result buffer that holds information only for blobs of interest.
   4. Optionally, sort the indices assigned to blobs based on their blob features, using [`M3dblobSort`](../../Reference/3dblob/M3dblobSort.md).
8. Retrieve required results from the 3D blob analysis result buffer, using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md). Note that trying to retrieve a result that is not available generates an error.
9. If required, copy a group of 3D blob analysis results into an image buffer, a 3D geometry object, a transformation matrix object, or a 3D blob analysis result buffer, using [`M3dblobCopyResult`](../../Reference/3dblob/M3dblobCopyResult.md). For example, you can copy a blob's principal axis into a 3D geometry object to establish a unit-length 3D line geometry that marks the axis, and likewise for the second and third principal axes.
10. If required, copy specified blobs to another container, using [`M3dblobExtract`](../../Reference/3dblob/M3dblobExtract.md).
11. To draw blobs and/or blob features into a 3D graphics list, perform the following:
   1. Allocate a draw 3D blob analysis context, using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md), to hold the settings for the draw. Note that you can skip this step and use a default context instead.
   2. Specify the draw operations and options for the draw, using [`M3dblobControlDraw`](../../Reference/3dblob/M3dblobControlDraw.md).
   3. Draw the blobs and/or blob features, using [`M3dblobDraw3d`](../../Reference/3dblob/M3dblobDraw3d.md).
12. If required, save your 3D blob analysis context, using [`M3dblobSave`](../../Reference/3dblob/M3dblobSave.md) or [`M3dblobStream`](../../Reference/3dblob/M3dblobStream.md).
13. Free all your allocated objects using [`M3dblobFree`](../../Reference/3dblob/M3dblobFree.md), unless [`M_UNIQUE_ID`](../../Reference/3dblob/M3dblobAlloc.md) was specified during allocation.
