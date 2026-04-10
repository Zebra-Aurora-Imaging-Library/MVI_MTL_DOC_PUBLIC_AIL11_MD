---
doctype: Reference
module: 3dblob
function: M3dblobCopyResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobCopyResult"
---

# M3dblobCopyResult

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

> Copy a group of results into an image buffer, a 3D geometry object, a transformation matrix object, or a 3D blob analysis result buffer.

## Syntax

```c
void M3dblobCopyResult(
    AIL_ID    SrcResult3dblobId,  //in
    AIL_INT64 SrcLabelOrIndex,    //in
    AIL_ID    DstAilObjectId,     //out
    AIL_INT64 CopyType,           //in
    AIL_INT64 ControlFlag         //in
)
```

## Description

This function copies a group of 3D blob analysis results into an image buffer, a 3D geometry object, a transformation matrix object, or a 3D blob analysis result buffer, according to the specified copy operation.

## Parameters

### `SrcResult3dblobId` *(in, AIL_ID)*

Specifies the identifier of the 3D blob analysis result buffer from which to copy. The result buffer must have been previously allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md) with [`M_SEGMENTATION_RESULT`](../../Reference/3dblob/M3dblobAllocResult.md).

### `SrcLabelOrIndex` *(in, AIL_INT64)*

Specifies the label or index of the blob(s) from which to copy results, or specifies that you are copying general results for all blobs for which data is stored in the source result buffer.

*For specifying the blob label or index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BLOB_INDEX` | Specifies the index of the blob. |
| `M_BLOB_LABEL` | Specifies the label of the blob. |
| `M_ALL_BLOBS` | Specifies to copy results from all blobs. |
| `M_GENERAL` *(default)* | Specifies to copy general results from the 3D blob analysis result buffer. |

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the image buffer, 3D geometry object, transformation matrix object, or 3D blob analysis result buffer in which to store the copied result.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For copying general results from the source 3D blob analysis result buffer

To copy general results, set the [`CopyType`](../../Reference/3dblob/M3dblobCopyResult.md) parameter to one of the following values. In this case, you must set [`SrcLabelOrIndex`](../../Reference/3dblob/M3dblobCopyResult.md) to [`M_GENERAL`](../../Reference/3dblob/M3dblobCopyResult.md) or [`M_DEFAULT`](../../Reference/3dblob/M3dblobCopyResult.md).

---

### `M_INDEX_IMAGE`

Specifies to copy blob indices into an image buffer, creating an index image. For each point in a blob, this operation copies the blob's index to a corresponding pixel location in the image buffer. This essentially sets each pixel in the index image to the index of its corresponding blob. Note that the point information is set in the image buffer at the same location as the point's location in the range component of its point cloud container. Points that are not part of any blob are given the value of -1 in signed buffers, and set to the maximum buffer value in unsigned buffers.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results.  The image buffer must be a 1-band buffer and have a bit depth deep enough to hold the largest index + 1 ([`M_NUMBER`](../../Reference/3dblob/M3dblobGetResult.md)). The image buffer size must equal the size of the container used for the segmentation, in both X and Y. To retrieve the size, use [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dblob/M3dblobGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dblob/M3dblobGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

---

### `M_LABEL_IMAGE`

Specifies to copy blob labels into an image buffer, creating a label image. For each point in a blob, this operation copies the blob's label to a corresponding pixel location in the image buffer. This essentially sets each pixel in the label image to the label of its corresponding blob. Note that the point information is set in the image buffer at the same location as the point's location in the range component of its point cloud container. Points that are not part of any blob are given the label 0.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results.  The image buffer must be a 1-band buffer and have a bit depth deep enough to hold the largest label ([`M_MAX_LABEL_VALUE`](../../Reference/3dblob/M3dblobGetResult.md)). The image buffer size must equal the size of the container used for the segmentation, in both X and Y. To retrieve the size, use [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dblob/M3dblobGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dblob/M3dblobGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

---

### `M_RESULT`

Specifies to copy the whole source 3D blob analysis result buffer into the specified destination 3D blob analysis result buffer.

| Value | Description |
| --- | --- |
| `Segmentation 3D blob analysis result buffer identifier` | Specifies the identifier of the 3D blob analysis result buffer in which to copy results. The result buffer must have been previously allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md) with [`M_SEGMENTATION_RESULT`](../../Reference/3dblob/M3dblobAllocResult.md). |

### For copying the results of a specific blob or all blobs from the source 3D blob analysis result buffer

To copy the results of a specific blob or all blobs, set the [`CopyType`](../../Reference/3dblob/M3dblobCopyResult.md) parameter to the following value. In this case, you must set [`SrcLabelOrIndex`](../../Reference/3dblob/M3dblobCopyResult.md) to a specific blob, using **M_BLOB_INDEX()** or **M_BLOB_LABEL()**, or to [`M_ALL_BLOBS`](../../Reference/3dblob/M3dblobCopyResult.md) for all blobs.

---

### `M_MASK_IMAGE`

Specifies to create a mask image whose pixels are non-zero for blob points, and zero otherwise. For each point in a blob, this operation uses the point's location in its container to assign a non-zero value to a corresponding pixel location in the image buffer. The non-zero value is 1 for floating-point image buffers and -1 for signed image buffers. For unsigned image buffers, the maximum buffer value is used. All other pixels in the buffer are set to 0.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results.  The image buffer must be a 1-band buffer and its size must equal the size of the container used for the segmentation, in both X and Y. To retrieve the size, use [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dblob/M3dblobGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dblob/M3dblobGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### For copying the results of a specific blob from the source 3D blob analysis result buffer

To copy the results of a specific blob, set the [`CopyType`](../../Reference/3dblob/M3dblobCopyResult.md) parameter to one of the following values. In this case, you must set [`SrcLabelOrIndex`](../../Reference/3dblob/M3dblobCopyResult.md) to a specific blob using **M_BLOB_INDEX()** or **M_BLOB_LABEL()**.

---

### `M_BOUNDING_BOX`

Specifies to copy the blob's axis-aligned bounding box into the specified 3D geometry object, establishing a 3D box geometry.

| Value | Description |
| --- | --- |
| `3D geometry object ID in which to copy` | Specifies the identifier of a 3D geometry object in which to copy results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `M_BOX_CENTER`

Specifies to copy the center of the blob's axis-aligned bounding box into the specified 3D geometry object, establishing a 3D point geometry.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

---

### `M_CENTROID`

Specifies to copy the centroid of the blob into the specified 3D geometry object, establishing a 3D point geometry.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

---

### `M_FERET_CONTACT_POINT_1`

Specifies to copy the start point of the blob's specified Feret diameter into the specified 3D geometry object, establishing a 3D point geometry.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

---

### `M_FERET_CONTACT_POINT_2`

Specifies to copy the end point of the blob's specified Feret diameter into the specified 3D geometry object, establishing a 3D point geometry.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

---

### `M_FIXTURING_MATRIX`

Specifies to copy, into the transformation matrix, the principal component analysis (PCA) results that can bring points along the blob's principal axes (around the centroid) to the X-, Y-, and Z-axes (around the origin). This matrix is the inverse of the PCA matrix ([`M_PCA_MATRIX`](../../Reference/3dblob/M3dblobCopyResult.md)).

| Value | Description |
| --- | --- |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the transformation matrix object in which to copy the principal component analysis (PCA) results. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `M_NEAREST_BLOB`

Specifies to copy, into the specified 3D geometry object, the point inside the current blob which is closest to the nearest neighboring blob in the point cloud (or vice versa), establishing a 3D point geometry. You must add a combination constant to specify which of the points to copy, either the point in the current blob ([`M_NEAREST_POINT_1`](../../Reference/3dblob/M3dblobCopyResult.md)), or that of the nearest neighboring blob ([`M_NEAREST_POINT_2`](../../Reference/3dblob/M3dblobCopyResult.md)).

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

---

### `M_PCA_BOX`

Specifies to copy the blob's PCA-aligned bounding box into the specified 3D geometry object, establishing a 3D box geometry.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

---

### `M_PCA_BOX_CENTER`

Specifies to copy the center of the blob's PCA-aligned bounding box into the specified 3D geometry object, establishing a 3D point geometry.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

---

### `M_PCA_MATRIX`

Specifies to copy, into the transformation matrix, the principal component analysis (PCA) results that can bring points on the X-, Y-, and Z-axes (around the origin) to the blob's principal axes (around the centroid).

| Value | Description |
| --- | --- |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the transformation matrix object in which to copy the principal component analysis (PCA) results. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `M_PRINCIPAL_AXIS_1`

Specifies to copy the blob's first principal axis into the specified 3D geometry object, establishing a 3D line geometry that starts at the blob's centroid, and has a length of 1 unit.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

---

### `M_PRINCIPAL_AXIS_2`

Specifies to copy the blob's second principal axis into the specified 3D geometry object, establishing a 3D line geometry that starts at the blob's centroid, and has a length of 1 unit.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

---

### `M_PRINCIPAL_AXIS_3`

Specifies to copy the blob's third principal axis into the specified 3D geometry object, establishing a 3D line geometry that starts at the blob's centroid, and has a length of 1 unit.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

---

### `M_SEMI_ORIENTED_BOX`

Specifies to copy the blob's semi-oriented bounding box into the specified 3D geometry object, establishing a 3D box geometry.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

---

### `M_SEMI_ORIENTED_BOX_CENTER`

Specifies to copy the center of the blob's semi-oriented bounding box into the specified 3D geometry object, establishing a 3D point geometry.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobCopyResult.md))* |  |

### Combination Constants — For specifying the point

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which of the closest points between the current blob and its nearest neighbor to copy.

| Value | Description |
| --- | --- |
| `M_NEAREST_POINT_1` | Specifies to copy the point, inside the current blob, which is closest to the nearest neighboring blob in the point cloud. |
| `M_NEAREST_POINT_2` | Specifies to copy the point, inside the nearest neighboring blob in the point cloud, which is closest to the current blob. |

### Combination Constants — For specifying the Feret

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the Feret from which to copy the contact point.

| Value | Description |
| --- | --- |
| `M_FERET_AT_PRINCIPAL_AXIS_1` | Specifies to copy the contact point of the Feret along the first principal axis. |
| `M_FERET_AT_PRINCIPAL_AXIS_2` | Specifies to copy the contact point of the Feret along the second principal axis. |
| `M_FERET_AT_PRINCIPAL_AXIS_3` | Specifies to copy the contact point of the Feret along the third principal axis. |
| `M_FERET_GENERAL` | Specifies to copy the contact point of the custom Feret. |
| `M_FERET_MAX_DIAMETER` | Specifies to copy the contact point of the longest Feret in the blob. |
| `M_FERET_MIN_DIAMETER` | Specifies to copy the contact point of the shortest Feret in the blob. |
| `M_FERET_SEMI_ORIENTED_HEIGHT` | Specifies to copy the contact point of the smallest XY-side of the semi-oriented box. |
| `M_FERET_SEMI_ORIENTED_WIDTH` | Specifies to copy the contact point of the longest XY-side of the semi-oriented box. |
| `M_FERET_X` | Specifies to copy the contact point of the X-aligned Feret. |
| `M_FERET_Y` | Specifies to copy the contact point of the Y-aligned Feret. |
| `M_FERET_Z` | Specifies to copy the contact point of the Z-aligned Feret. |
