---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Blob_analysis
section: Retrieving_and_drawing_results
module_tag: 3dblob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Blob_analysis / Retrieving and drawing results"
---

# Retrieving and drawing results

After successfully segmenting your point cloud and calculating blob features, you can retrieve the required results from your 3D blob analysis result buffer using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md). To draw the blobs and/or blob features, use [`M3dblobDraw3d`](../../Reference/3dblob/M3dblobDraw3d.md).

## Possible results

The 3D Blob Analysis module produces several types of results that provide information on the make-up of the blobs in your context. You can retrieve general results such as:

- Whether the segmentation was successful, or why the segmentation failed ([`M_STATUS`](../../Reference/3dblob/M3dblobGetResult.md)).
- The number of blobs ([`M_NUMBER`](../../Reference/3dblob/M3dblobGetResult.md)).
- The total number of points across all blobs ([`M_TOTAL_NUMBER_OF_POINTS`](../../Reference/3dblob/M3dblobGetResult.md)).
- The maximum label value of the blobs ([`M_MAX_LABEL_VALUE`](../../Reference/3dblob/M3dblobGetResult.md)).

With a successful blob segmentation, you can retrieve for each blob:

- Its index ([`M_INDEX_VALUE`](../../Reference/3dblob/M3dblobGetResult.md)).
- Its label ([`M_LABEL_VALUE`](../../Reference/3dblob/M3dblobGetResult.md)).
- The number of points in the blob ([`M_NUMBER_OF_POINTS`](../../Reference/3dblob/M3dblobGetResult.md)).
- The components of the blob's average normal vector ([`M_AVERAGE_NORMAL_...`](../../Reference/3dblob/M3dblobGetResult.md)), if the vector was used during segmentation.
- Information about the blob's stored location in the container's 2D grid organization (or the equivalent information in the label image), such as the X- and Y-coordinates of the stored location ([`M_PIXEL_X`](../../Reference/3dblob/M3dblobGetResult.md), [`M_PIXEL_Y`](../../Reference/3dblob/M3dblobGetResult.md), or [`M_PIXEL_PACKED`](../../Reference/3dblob/M3dblobGetResult.md)).

Calculated feature results include for each blob (if the features were enabled for calculation):

- Its bounding box. You can retrieve the axis-aligned bounding box, the semi-oriented bounding box, or the PCA-aligned bounding box, depending on which type(s) were calculated. Related results are also available, including the bounding box's Feret diameters and its center coordinates.
- Its centroid.
- Its Feret diameters.
- Its linearity and planarity.
- Moments.
- The label of its nearest neighboring blob, and the minimum distance to that blob.
- PCA (principal component analysis) results.

Note that if you specify to retrieve results from all blobs ([`M_ALL_BLOBS`](../../Reference/3dblob/M3dblobGetResult.md)), results are returned in an array.

Also note that trying to retrieve a result that is not available generates an error.

## Drawing results

The [`M3dblobControlDraw`](../../Reference/3dblob/M3dblobControlDraw.md) function provides several operations for drawing results into a 3D graphics list; for each blob, you can draw:

- Its points.
- Its bounding box (axis-aligned, semi-oriented, or PCA-aligned).
- Its Feret diameters (maximum, minimum, or custom Feret).
- Its principal components.
- Its index.
- Its label.
- Excluded points.

By default, Aurora Imaging Library draws the points inside each blob. To draw most other blob features, you must explicitly enable the relevant draw operation (using [`M3dblobControlDraw`](../../Reference/3dblob/M3dblobControlDraw.md)).

You can also specify how to draw the blob graphics. For example, you can set the color and thickness of most features. For a bounding box, you can specify to draw it as a solid, a wireframe, a solid with wireframe, or as points. Note that each blob point's thickness is its length, in pixels, along each side when displayed. The default is one pixel. For a complete description of all possible options for the draw, refer to the description of [`M3dblobControlDraw`](../../Reference/3dblob/M3dblobControlDraw.md) in the Aurora Imaging Library Reference.

To perform a draw operation, you typically allocate a draw 3D blob analysis context to hold the settings for the draw, using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md). You then specify draw operations and options for the draw with successive calls to [`M3dblobControlDraw`](../../Reference/3dblob/M3dblobControlDraw.md). Finally, you call [`M3dblobDraw3d`](../../Reference/3dblob/M3dblobDraw3d.md) to perform the draw.

Alternatively, you can skip allocating a draw 3D blob analysis context and instead use a predefined draw 3D blob analysis context with default draw settings. To do so, directly call [`M3dblobDraw3d`](../../Reference/3dblob/M3dblobDraw3d.md) with [`M_DEFAULT`](../../Reference/3dblob/M3dblobDraw3d.md). Note that the default settings specify to draw the points of the specified blob(s), with a default thickness of one pixel.
