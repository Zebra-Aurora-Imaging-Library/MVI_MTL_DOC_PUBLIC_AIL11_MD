---
doctype: Reference
module: 3dblob
function: M3dblobGetResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobGetResult"
---

# M3dblobGetResult

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Get calculated feature results for the specified blobs, from a 3D blob analysis result buffer.

## Syntax

```c
AIL_DOUBLE M3dblobGetResult(
    AIL_ID    Result3dblobId,  //in
    AIL_INT64 LabelOrIndex,    //in
    AIL_INT64 ResultType,      //in
    void *    ResultArrayPtr   //out
)
```

## Description

This function retrieves the results for a specified feature of one or all blobs, from the specified 3D blob analysis result buffer.

The specified feature(s) must have already been calculated using [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md). Note that [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) generates an error if you try to retrieve a result whose corresponding feature was not calculated. You can add the [`M_AVAILABLE`](../../Reference/3dblob/M3dblobGetResult.md) combination constant to a feature to determine if the result is available for retrieval.

## Parameters

### `Result3dblobId` *(in, AIL_ID)*

Specifies the identifier of the 3D blob analysis result buffer from which to get results.

### `LabelOrIndex` *(in, AIL_INT64)*

Specifies the label or index of the blob for which to get results, or specifies that you are getting general results related to all blobs. This parameter must be set to one of the following values:

*For specifying the blob label or index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BLOB_INDEX` | Specifies the index of the blob listed in the specified result buffer. |
| `M_BLOB_LABEL` | Specifies the label of the blob listed in the specified result buffer.

Note that blob labels are not consecutive; use [`M_INDEX_VALUE`](../../Reference/3dblob/M3dblobGetResult.md) to establish if a blob with a given label exists. With the exception of [`M_INDEX_VALUE`](../../Reference/3dblob/M3dblobGetResult.md), if you specify a blob label that does not exist, Aurora Imaging Library returns an error. |
| `M_ALL_BLOBS` | Specifies to retrieve results from all blobs listed in the specified result buffer. |
| `M_GENERAL` *(default)* | Specifies to retrieve general results from the 3D blob analysis result buffer. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address in which to write the results. Since the [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) function also returns the results, you can set this parameter to `M_NULL`.

## Parameter Associations

### For retrieving general results from the 3D blob analysis result buffer

To retrieve a general result, the [`ResultType`](../../Reference/3dblob/M3dblobGetResult.md) parameter should be set to one of the following values. In this case, you must set [`LabelOrIndex`](../../Reference/3dblob/M3dblobGetResult.md) to [`M_GENERAL`](../../Reference/3dblob/M3dblobGetResult.md) or [`M_DEFAULT`](../../Reference/3dblob/M3dblobGetResult.md).

---

### `M_CONTAINER_ID`

Retrieves the identifier of the container that was used for the segmentation. If [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) has not been called yet or if it was called with a label image instead of a container, [`M_NULL`](../../Reference/3dblob/M3dblobGetResult.md) is returned.  If this result is not null, post-segmentation functions that take a container (such as [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) and [`M3dblobExtract`](../../Reference/3dblob/M3dblobExtract.md)) expect this container as input.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) has not been called or that it was called with a label image instead of a container. |
| `Container identifier` | Specifies the Aurora Imaging Library identifier of the container that was used for the segmentation. |

---

### `M_INDEX_IMAGE_SIZE_BIT`

Retrieves the minimum bit size required for the index image, assuming an unsigned buffer.  Note that you can use this value to allocate an image buffer of an appropriate size for [`M3dblobCopyResult`](../../Reference/3dblob/M3dblobCopyResult.md) to create an index image.

---

### `M_LABEL_IMAGE_SIZE_BIT`

Retrieves the minimum bit size required for the label image, assuming an unsigned buffer.  Note that you can use this value to allocate an image buffer of an appropriate size for [`M3dblobCopyResult`](../../Reference/3dblob/M3dblobCopyResult.md) to create a label image.

---

### `M_MAX_DISTANCE`

Retrieves the maximum distance within which two points are considered neighbors, depending on the maximum distance mode. If [`M_MAX_DISTANCE_MODE`](../../Reference/3dblob/M3dblobControl.md) was not set to [`M_USER_DEFINED`](../../Reference/3dblob/M3dblobControl.md), this is the value that was computed during segmentation.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no maximum distance. |
| `Value >= 0` | Specifies the maximum distance value. |

---

### `M_MAX_LABEL_VALUE`

Retrieves the maximum label value of all blobs in the result buffer.  Label values for blobs are generated when a call to [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) is made. The maximum label value is not necessarily the same as the total number of blobs ([`M_NUMBER`](../../Reference/3dblob/M3dblobGetResult.md)) because some label values might not be used. You can call [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_LABEL_IMAGE_SIZE_BIT`](../../Reference/3dblob/M3dblobGetResult.md) to determine the bit depth required when creating a label image using [`M3dblobCopyResult`](../../Reference/3dblob/M3dblobCopyResult.md).  Note that if zero is returned, no blobs were found.

---

### `M_NORMAL_DISTANCE_MAX`

Retrieves the maximum angle between normal vectors within which two points are considered neighbors. If [`M_NORMAL_DISTANCE_MAX_MODE`](../../Reference/3dblob/M3dblobControl.md) was not set to [`M_USER_DEFINED`](../../Reference/3dblob/M3dblobControl.md), this is the value that was computed during segmentation.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies no maximum angle. |
| `Value >= 0` | Specifies the maximum angle value. |

---

### `M_NUMBER`

Retrieves the number of blobs that are listed in the result buffer.

---

### `M_RESULT_IMAGE_SIZE_X`

Retrieves the size in X of the container or image used for the blob segmentation.  Note that you can use this value to allocate an image buffer of an appropriate size for [`M3dblobCopyResult`](../../Reference/3dblob/M3dblobCopyResult.md) to create an index image, label image, or mask image.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size in X, in pixels. |

---

### `M_RESULT_IMAGE_SIZE_Y`

Retrieves the size in Y of the container or image used for the blob segmentation.  Note that you can use this value to allocate an image buffer of an appropriate size for [`M3dblobCopyResult`](../../Reference/3dblob/M3dblobCopyResult.md) to create an index image, label image, or mask image.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size in Y, in pixels. |

---

### `M_STATUS`

Retrieves the status of the segmentation operation, or why the segmentation failed.

| Value | Description |
| --- | --- |
| `M_CONTAINER_NOT_ORGANIZED` | Specifies that the segmentation failed because the source point cloud container was unorganized. |
| `M_MISSING_COMPONENT_MESH_AIL` | Specifies that the segmentation failed because the source point cloud container did not have an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component. |
| `M_MISSING_COMPONENT_NORMALS_AIL` | Specifies that the segmentation failed because the source point cloud container did not have an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component. |
| `M_NOT_INITIALIZED` | Specifies that the 3D blob analysis result buffer was not used in a call to [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md), and contains no results. |
| `M_STOPPED_BY_REQUEST` | Specifies that the blob segmentation operation was stopped from another thread using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_STOP_SEGMENT`](../../Reference/3dblob/M3dblobControl.md). |
| `M_SUCCESS` | Specifies that the blob segmentation operation completed successfully. |
| `M_TIMEOUT_REACHED` | Specifies that the blob segmentation operation took longer than the allowed value, specified using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_TIMEOUT`](../../Reference/3dblob/M3dblobControl.md), and has stopped before completion. |

---

### `M_TOTAL_NUMBER_OF_POINTS`

Retrieves the total number of points across all blobs.

### For retrieving results that are always available from a single blob or all blobs

To retrieve an always-available result of an [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) operation, the [`ResultType`](../../Reference/3dblob/M3dblobGetResult.md) parameter should be set to one of the following values. In this case, you must set [`LabelOrIndex`](../../Reference/3dblob/M3dblobGetResult.md) to **M_BLOB_INDEX()**, **M_BLOB_LABEL()**, or [`M_ALL_BLOBS`](../../Reference/3dblob/M3dblobGetResult.md). If [`LabelOrIndex`](../../Reference/3dblob/M3dblobGetResult.md) is [`M_ALL_BLOBS`](../../Reference/3dblob/M3dblobGetResult.md), the results are returned in an array instead of a single value.  > **Note:** These results are available after an [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) operation, and remain available after an [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) operation that uses the results of the blob segmentation. If [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) has not yet been called on this result, you will receive an error.

---

### `M_BLOB_TOUCHING_IMAGE_BORDERS`

Retrieves information about which side(s) of the container or label image that the blob touches. This is either the side(s) of an organized point cloud container's range component or a 2D label image. Note that [`M_BLOB_TOUCHING_IMAGE_BORDERS`](../../Reference/3dblob/M3dblobGetResult.md) is available only when the object used for segmentation ([`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) with [`ContainerOrLabelImageBufId`](../../Reference/3dblob/M3dblobSegment.md)) is an organized point cloud or 2D label image.  Note that a combination of the values below can be returned. For example, if the blob touches the bottom-left, the function returns [`M_BOTTOM`](../../Reference/3dblob/M3dblobGetResult.md) + [`M_LEFT`](../../Reference/3dblob/M3dblobGetResult.md). Bitwise operators must be used to verify the borders that the blob touches.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no blob touches a side. |
| `M_BOTTOM` | Specifies that the blob touches the bottom of the container or label image. |
| `M_LEFT` | Specifies that the blob touches the left of the container or label image. |
| `M_RIGHT` | Specifies that the blob touches the right of the container or label image. |
| `M_TOP` | Specifies that the blob touches the top of the container or label image. |

---

### `M_FIRST_PIXEL_X`

Retrieves the X-coordinate of the first pixel in the blob. This is the left-most pixel on the top-most line of the blob's stored location in the container's 2D grid organization, or it is the equivalent pixel in the label image. The value returned is the same as the first X-coordinate returned with [`M_PIXEL_X`](../../Reference/3dblob/M3dblobGetResult.md) or [`M_PIXEL_PACKED`](../../Reference/3dblob/M3dblobGetResult.md).

---

### `M_FIRST_PIXEL_Y`

Retrieves the Y-coordinate of the first pixel in the blob. This is the left-most pixel on the top-most line of the blob's stored location in the container's 2D grid organization, or it is the equivalent pixel in the label image. The value returned is the same as the first Y-coordinate returned with [`M_PIXEL_Y`](../../Reference/3dblob/M3dblobGetResult.md) or [`M_PIXEL_PACKED`](../../Reference/3dblob/M3dblobGetResult.md).

---

### `M_INDEX_VALUE`

Retrieves the index of the blob. You can use this result type to establish if a blob with a given label exists. If the label is valid, [`M_INDEX_VALUE`](../../Reference/3dblob/M3dblobGetResult.md) returns the corresponding index; if the label is not valid, [`M_INVALID`](../../Reference/3dblob/M3dblobGetResult.md) is returned instead of an Aurora Imaging Library error.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies an invalid index. |
| `0 <= Value < M_NUMBER` | Specifies the index of the blob. |

---

### `M_LABEL_VALUE`

Retrieves the label of the blob.

| Value | Description |
| --- | --- |
| `0 < Value <= M_MAX_LABEL_VALUE` | Specifies the label of the blob. |

---

### `M_NUMBER_OF_POINTS`

Retrieves the number of points in the blob.

---

### `M_PIXEL_MAX_X`

Retrieves the maximum X-coordinate of the blob's stored location in the container's 2D grid organization (or the equivalent coordinate in the label image), expressed in pixel units.

---

### `M_PIXEL_MAX_Y`

Retrieves the maximum Y-coordinate of the blob's stored location in the container's 2D grid organization (or the equivalent coordinate in the label image), expressed in pixel units.

---

### `M_PIXEL_MIN_X`

Retrieves the minimum X-coordinate of the blob's stored location in the container's 2D grid organization (or the equivalent coordinate in the label image), expressed in pixel units.

---

### `M_PIXEL_MIN_Y`

Retrieves the minimum Y-coordinate of the blob's stored location in the container's 2D grid organization (or the equivalent coordinate in the label image), expressed in pixel units.

---

### `M_PIXEL_PACKED`

Retrieves the packed X- and Y-coordinates of the blob's stored location in the container's 2D grid organization (or the equivalent coordinates in the label image). The coordinates are returned in an interleaved manner: `[X<sub>0</sub>, Y<sub>0</sub>, X<sub>1</sub>, Y<sub>1</sub>, ...]`. Note that the order is from left to right, top to bottom, starting with the blob's top-left stored location in the container (or equivalent location in the label image).

---

### `M_PIXEL_X`

Retrieves the X-coordinates of the blob's stored location in the container's 2D grid organization (or the equivalent coordinates in the label image).

---

### `M_PIXEL_Y`

Retrieves the Y-coordinates of the blob's stored location in the container's 2D grid organization (or the equivalent coordinates in the label image).

### For retrieving results that are sometimes available from a single blob or all blobs

To retrieve a sometimes-available result of an [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) or [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) operation, the [`ResultType`](../../Reference/3dblob/M3dblobGetResult.md) parameter should be set to one of the following values. In this case, you must set [`LabelOrIndex`](../../Reference/3dblob/M3dblobGetResult.md) to **M_BLOB_INDEX()**, **M_BLOB_LABEL()**, or [`M_ALL_BLOBS`](../../Reference/3dblob/M3dblobGetResult.md). If [`LabelOrIndex`](../../Reference/3dblob/M3dblobGetResult.md) is [`M_ALL_BLOBS`](../../Reference/3dblob/M3dblobGetResult.md), the results are returned in an array instead of a single value.  > **Note:** If [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) has not yet been called on this result, an error is thrown.

---

### `?`

---

### `?`

---

### `?`

---

### `M_AVERAGE_NORMAL_X`

Retrieves the X-component of the blob's average normal vector.

---

### `M_AVERAGE_NORMAL_Y`

Retrieves the Y-component of the blob's average normal vector.

---

### `M_AVERAGE_NORMAL_Z`

Retrieves the Z-component of the blob's average normal vector.

---

### `M_BOX_CENTER_X`

Retrieves the X-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobGetResult.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobGetResult.md).

---

### `M_BOX_CENTER_Y`

Retrieves the Y-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobGetResult.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobGetResult.md).

---

### `M_BOX_CENTER_Z`

Retrieves the Z-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobGetResult.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobGetResult.md).

---

### `M_CENTROID_X`

Retrieves the X-coordinate of the centroid of the blob.

---

### `M_CENTROID_Y`

Retrieves the Y-coordinate of the centroid of the blob.

---

### `M_CENTROID_Z`

Retrieves the Z-coordinate of the centroid of the blob.

---

### `M_COVARIANCE_MATRIX`

Retrieves the 9 entries that make up the covariance matrix, in row-major order. The covariance matrix is defined as a 3x3 matrix of second order central moments:  |   |   |   | | --- | --- | --- | | xx | xy | xz | | xy | yy | yz | | xz | yz | zz |

---

### `M_EIGENVALUE_1`

Retrieves the eigenvalue corresponding to the first principal axis. The first principal axis is the direction along which there is the most variance.

---

### `M_EIGENVALUE_2`

Retrieves the eigenvalue corresponding to the second principal axis. The second principal axis is the direction along which there is the second most variance, perpendicular to the first principal axis.

---

### `M_EIGENVALUE_3`

Retrieves the eigenvalue corresponding to the third principal axis. The third principal axis is perpendicular to the first and second principal axes.

---

### `M_FERET_AT_PRINCIPAL_AXIS_1`

Retrieves the length of the Feret along the first principal axis. This result is the same as [`M_SIZE_X`](../../Reference/3dblob/M3dblobGetResult.md) + [`M_PCA_BOX`](../../Reference/3dblob/M3dblobGetResult.md).  To retrieve this result, you must have enabled [`M_PCA_BOX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_FERET_AT_PRINCIPAL_AXIS_2`

Retrieves the length of the Feret along the second principal axis. This result is the same as [`M_SIZE_Y`](../../Reference/3dblob/M3dblobGetResult.md) + [`M_PCA_BOX`](../../Reference/3dblob/M3dblobGetResult.md).  To retrieve this result, you must have enabled [`M_PCA_BOX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_FERET_AT_PRINCIPAL_AXIS_3`

Retrieves the length of the Feret along the third principal axis. This result is the same as [`M_SIZE_Z`](../../Reference/3dblob/M3dblobGetResult.md) + [`M_PCA_BOX`](../../Reference/3dblob/M3dblobGetResult.md).  To retrieve this result, you must have enabled [`M_PCA_BOX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_FERET_GENERAL`

Retrieves the length of the Feret in the direction specified with [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobGetResult.md) + [`M_FERET_DIRECTION_...`](../../Reference/3dblob/M3dblobGetResult.md).  To retrieve this result, you must have enabled [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_FERET_MAX_DIAMETER`

Retrieves the length of the longest Feret in the blob.  > **Note:** Note that, when you combine this result type with an [`M_FERET_DIRECTION_...`](../../Reference/3dblob/M3dblobGetResult.md) combination value, the direction is accurate up to the angle specified with [`M_FERET_PRECISION`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).  To retrieve this result, you must have enabled [`M_FERET_MAX_DIAMETER`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_FERET_MIN_DIAMETER`

Retrieves the length of the shortest Feret in the blob.  > **Note:** Note that, when you combine this result type with an [`M_FERET_DIRECTION_...`](../../Reference/3dblob/M3dblobGetResult.md) combination value, the direction is accurate up the angle specified with [`M_FERET_PRECISION`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).  To retrieve this result, you must have enabled [`M_FERET_MIN_DIAMETER`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_FERET_SEMI_ORIENTED_HEIGHT`

Retrieves the height (smallest XY side) of the semi-oriented box. This result is the same as [`M_SIZE_Y`](../../Reference/3dblob/M3dblobGetResult.md) + [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobGetResult.md).

---

### `M_FERET_SEMI_ORIENTED_WIDTH`

Retrieves the width (longest XY side) of the semi-oriented box. This result is the same as [`M_SIZE_X`](../../Reference/3dblob/M3dblobGetResult.md) + [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobGetResult.md).

---

### `M_FERET_X`

Retrieves the length of the X-aligned Feret (same as [`M_SIZE_X`](../../Reference/3dblob/M3dblobGetResult.md)).  To retrieve this result, you must have enabled [`M_BOUNDING_BOX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_FERET_Y`

Retrieves the length of the Y-aligned Feret (same as [`M_SIZE_Y`](../../Reference/3dblob/M3dblobGetResult.md)).  To retrieve this result, you must have enabled [`M_BOUNDING_BOX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_FERET_Z`

Retrieves the length of the Z-aligned Feret (same as [`M_SIZE_Z`](../../Reference/3dblob/M3dblobGetResult.md)).  To retrieve this result, you must have enabled [`M_BOUNDING_BOX`](../../Reference/3dblob/M3dblobControl.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_LINEARITY`

Retrieves the degree to which the specified blob is linear, where 1.0 is a perfect line and 0.0 is a perfect sphere. This is equal to the following equation: `1 - sqrt([`M_EIGENVALUE_2`](../../Reference/3dblob/M3dblobGetResult.md)/[`M_EIGENVALUE_1`](../../Reference/3dblob/M3dblobGetResult.md))`.  To retrieve this result, you must have enabled [`M_LINEARITY`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)). Otherwise, you must have enabled [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md) and set [`M_MOMENT_ORDER`](../../Reference/3dblob/M3dblobControl.md) to an order of at least 2.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the degree to which the blob is linear. |

---

### `M_MAX_X`

Retrieves the X-coordinate of the maximum corner of the blob's axis-aligned bounding box.

---

### `M_MAX_Y`

Retrieves the Y-coordinate of the maximum corner of the blob's axis-aligned bounding box.

---

### `M_MAX_Z`

Retrieves the Z-coordinate of the maximum corner of the blob's axis-aligned or semi-oriented bounding box.  To retrieve this result, you must have enabled [`M_BOUNDING_BOX`](../../Reference/3dblob/M3dblobControl.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_MIN_X`

Retrieves the X-coordinate of the minimum corner of the blob's axis-aligned bounding box.

---

### `M_MIN_Y`

Retrieves the Y-coordinate of the minimum corner of the blob's axis-aligned bounding box.

---

### `M_MIN_Z`

Retrieves the Z-coordinate of the minimum corner of the blob's axis-aligned or semi-oriented bounding box.  To retrieve this result, you must have enabled [`M_BOUNDING_BOX`](../../Reference/3dblob/M3dblobControl.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_NEAREST_BLOB`

Retrieves the label of the nearest blob. If there are no other blobs, or if the nearest blob is further than [`M_NEAREST_BLOB`](../../Reference/3dblob/M3dblobControl.md) + [`M_MAX_DISTANCE`](../../Reference/3dblob/M3dblobControl.md), the label is 0 and no other nearest blob results are available.  To retrieve this result, you must have enabled [`M_NEAREST_BLOB`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

---

### `M_NEAREST_BLOB_DISTANCE`

Retrieves the minimum distance to the nearest blob.  To retrieve this result, you must have enabled [`M_NEAREST_BLOB`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)) and there must be at least one blob within the specified maximum distance (that is, [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_NEAREST_BLOB`](../../Reference/3dblob/M3dblobGetResult.md) must not return 0).

---

### `M_PLANARITY`

Retrieves the degree to which the specified blob is planar, where 1.0 is a perfect plane and 0.0 is a perfect sphere. This is equal to the following equation: `1 - sqrt([`M_EIGENVALUE_3`](../../Reference/3dblob/M3dblobGetResult.md)/ [`M_EIGENVALUE_2`](../../Reference/3dblob/M3dblobGetResult.md))`.  To retrieve this result, you must have enabled [`M_PLANARITY`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)). Otherwise, you must have enabled [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md) and set [`M_MOMENT_ORDER`](../../Reference/3dblob/M3dblobControl.md) to an order of at least 2.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the degree to which the blob is planar. |

---

### `M_PRINCIPAL_AXIS_1_X`

Retrieves the X-component of the normalized vector along the first principal axis, which is the axis with the largest eigenvalue.

---

### `M_PRINCIPAL_AXIS_1_Y`

Retrieves the Y-component of the normalized vector along the first principal axis, which is the axis with the largest eigenvalue.

---

### `M_PRINCIPAL_AXIS_1_Z`

Retrieves the Z-component of the normalized vector along the first principal axis, which is the axis with the largest eigenvalue.

---

### `M_PRINCIPAL_AXIS_2_X`

Retrieves the X-component of the normalized vector along the second principal axis, which is the axis with the second largest eigenvalue.

---

### `M_PRINCIPAL_AXIS_2_Y`

Retrieves the Y-component of the normalized vector along the second principal axis, which is the axis with the second largest eigenvalue.

---

### `M_PRINCIPAL_AXIS_2_Z`

Retrieves the Z-component of the normalized vector along the second principal axis, which is the axis with the second largest eigenvalue.

---

### `M_PRINCIPAL_AXIS_3_X`

Retrieves the X-component of the normalized vector along the third principal axis, which is the axis perpendicular to the first and second principal axes.

---

### `M_PRINCIPAL_AXIS_3_Y`

Retrieves the Y-component of the normalized vector along the third principal axis, which is the axis perpendicular to the first and second principal axes.

---

### `M_PRINCIPAL_AXIS_3_Z`

Retrieves the Z-component of the normalized vector along the third principal axis, which is the axis perpendicular to the first and second principal axes.

---

### `M_SEMI_ORIENTED_BOX_ANGLE`

Retrieves the angle of the semi-oriented bounding box's rotation around the Z-axis, measured from the X-axis to the box's longest XY-side, respecting the right-hand rule.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 180.0` | Specifies the angle of the semi-oriented bounding box. |

---

### `M_SIZE_X`

Retrieves the size in X of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobGetResult.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobGetResult.md).

---

### `M_SIZE_Y`

Retrieves the size in Y of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobGetResult.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobGetResult.md).

---

### `M_SIZE_Z`

Retrieves the size in Z of the 3D blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobGetResult.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobGetResult.md).

---

### `M_STANDARD_DEVIATION_X`

Retrieves the standard deviation of points in the blob along the X-axis. This is equivalent to the following formula, where _N_ is the number of points in the blob, and _x<sub>i</sub> _ and_µ<sub>x</sub> _ are the X-coordinates of the blob's _i_ <sup>th</sup> point and centroid, respectively:  *[Image: 3dblob_StdDevFormula_X.png]*

---

### `M_STANDARD_DEVIATION_Y`

Retrieves the standard deviation of points in the blob along the Y-axis. This is equivalent to the following formula, where _N_ is the number of points in the blob, and _y<sub>i</sub> _ and_µ<sub>y</sub> _ are the Y-coordinates of the blob's _i_ <sup>th</sup> point and centroid, respectively:  *[Image: 3dblob_StdDevFormula_Y.png]*

---

### `M_STANDARD_DEVIATION_Z`

Retrieves the standard deviation of points in the blob along the Z-axis. This is equivalent to the following formula, where _N_ is the number of points in the blob, and _z<sub>i</sub> _ and_µ<sub>z</sub> _ are the Z-coordinates of the blob's _i_ <sup>th</sup> point and centroid, respectively:  *[Image: 3dblob_StdDevFormula_Z.png]*

### Combination Constants — For specifying the type of bounding box

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the type of bounding box for which to return results.

| Value | Description |
| --- | --- |
| `M_PCA_BOX` | Specifies to return results associated with the PCA bounding box. |
| `M_SEMI_ORIENTED_BOX` | Specifies to return results associated with the semi-oriented bounding box. |

### Combination Constants — For retrieving Feret contact points

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify to retrieve the contact points of the Feret.

To retrieve these results, you must have enabled [`M_FERET_CONTACT_POINTS`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

| Value | Description |
| --- | --- |
| `M_FERET_CONTACT_POINTS_X1` | Retrieves the X-coordinate of the start point of the blob's Feret diameter. |
| `M_FERET_CONTACT_POINTS_X2` | Retrieves the X-coordinate of the end point of the blob's Feret diameter. |
| `M_FERET_CONTACT_POINTS_Y1` | Retrieves the Y-coordinate of the start point of the blob's Feret diameter. |
| `M_FERET_CONTACT_POINTS_Y2` | Retrieves the Y-coordinate of the end point of the blob's Feret diameter. |
| `M_FERET_CONTACT_POINTS_Z1` | Retrieves the Z-coordinate of the start point of the blob's Feret diameter. |
| `M_FERET_CONTACT_POINTS_Z2` | Retrieves the Z-coordinate of the end point of the blob's Feret diameter. |

### Combination Constants — For retrieving the Feret direction

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify to retrieve the direction of the Feret.

| Value | Description |
| --- | --- |
| `M_FERET_DIRECTION_X` | Retrieves the X-component of the Feret's direction vector. For example, the direction of [`M_FERET_X`](../../Reference/3dblob/M3dblobGetResult.md) is (1, 0, 0). |
| `M_FERET_DIRECTION_Y` | Retrieves the Y-component of the Feret's direction vector. For example, the direction of [`M_FERET_Y`](../../Reference/3dblob/M3dblobGetResult.md) is (0, 1, 0). |
| `M_FERET_DIRECTION_Z` | Retrieves the Z-component of the Feret's direction vector. For example, the direction of [`M_FERET_Z`](../../Reference/3dblob/M3dblobGetResult.md) is (0, 0, 1). |

### Combination Constants — For retrieving the closest points between neighboring 3D blobs

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the coordinates of the closest points between the current blob and its nearest neighbor.

| Value | Description |
| --- | --- |
| `M_NEAREST_POINT_X1` | Retrieves the X-coordinate of the point inside the current blob that is closest to the nearest neighboring blob. |
| `M_NEAREST_POINT_X2` | Retrieves the X-coordinate of the point inside the nearest neighboring blob that is closest to the current blob. |
| `M_NEAREST_POINT_Y1` | Retrieves the Y-coordinate of the point inside the current blob that is closest to the nearest neighboring blob. |
| `M_NEAREST_POINT_Y2` | Retrieves the Y-coordinate of the point inside the nearest neighboring blob that is closest to the current blob. |
| `M_NEAREST_POINT_Z1` | Retrieves the Z-coordinate of the point inside the current blob that is closest to the nearest neighboring blob. |
| `M_NEAREST_POINT_Z2` | Retrieves the Z-coordinate of the point inside the nearest neighboring blob that is closest to the current blob. |

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested information to an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_DOUBLE`

The returned value is the requested information, cast to an _AIL_DOUBLE_. If the requested information does not fit into an _AIL_DOUBLE_, this function will return `M_NULL` or truncate the information.

To retrieve this result, you must have set [`M_GLOBAL_NORMAL_DISTANCE_MAX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)) to a specific angle value (not [`M_INFINITE`](../../Reference/3dblob/M3dblobControl.md)).

> **Note:** This result is internally calculated during the segmentation, provided the above condition.

To retrieve this result, you must have enabled [`M_BOUNDING_BOX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

To retrieve this result, you must have enabled [`M_BOUNDING_BOX`](../../Reference/3dblob/M3dblobControl.md), [`M_PCA_BOX`](../../Reference/3dblob/M3dblobControl.md), or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).

To retrieve this result, you must have enabled [`M_CENTROID`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)). Otherwise, you must have enabled [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md) and set [`M_MOMENT_ORDER`](../../Reference/3dblob/M3dblobControl.md) to an order of at least 1.

To retrieve this result, you must have enabled [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)) and set [`M_MOMENT_ORDER`](../../Reference/3dblob/M3dblobControl.md) to an order of at least 2.

To retrieve this result, you must have enabled [`M_PCA`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)). Otherwise, you must have enabled [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md) and set [`M_MOMENT_ORDER`](../../Reference/3dblob/M3dblobControl.md) to an order of at least 2.

To retrieve this result, you must have enabled [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobControl.md) (using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md)).
