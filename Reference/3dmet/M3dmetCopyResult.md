---
doctype: Reference
module: 3dmet
function: M3dmetCopyResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetCopyResult"
---

# M3dmetCopyResult

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

> Copy a group of results from a 3D metrology result buffer into an image buffer, a 3D geometry object, a point cloud container, or a transformation matrix object.

## Syntax

```c
void M3dmetCopyResult(
    AIL_ID    SrcResult3dmetId,  //in
    AIL_ID    DstAilObjectId,    //out
    AIL_INT64 CopyType,          //in
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function copies a group of results from a 3D metrology result buffer into an image buffer, a 3D geometry object, a point cloud container, or a transformation matrix object.

When copying results of an [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) operation, there must be at least one calculated 3D geometry or matrix in the specified calculate 3D metrology result buffer. To retrieve the number of geometries or matrices in the result buffer, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_NUMBER`](../../Reference/3dmet/M3dmetGetResult.md).

When copying results of an [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md),[`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md), or [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation, the copy operations that are available depend on the resulting status of the distance, fit, or volume operation. To retrieve the status, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_STATUS_DISTANCE`](../../Reference/3dmet/M3dmetGetResult.md), [`M_STATUS_FIT`](../../Reference/3dmet/M3dmetGetResult.md), or [`M_STATUS_VOLUME`](../../Reference/3dmet/M3dmetGetResult.md), respectively.

## Parameters

### `SrcResult3dmetId` *(in, AIL_ID)*

Specifies the identifier of a 3D metrology result buffer from which to copy results. The 3D metrology result buffer must have been allocated using [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md); the result buffer must contain the results of a call to [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md), [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md), [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md), or [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md).

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the image buffer, 3D geometry object, point cloud container, or transformation matrix object into which the results will be copied.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the copy type and destination object for a 3D metrology result buffer

---

### `Calculate 3D metrology result buffer ID, for copying results of an M3dmetDistanceEx operation`

Specifies the identifier of a calculate 3D metrology result buffer from which to copy results of an [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) operation. The result buffer must have been previously allocated using [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md) with [`M_CALCULATE_RESULT`](../../Reference/3dmet/M3dmetAllocResult.md).

#### `M_DISTANCE_NORMAL_IMAGE`

Specifies to copy an image of the normal distance between the source and the reference points into an image buffer. For each source point, the operation copies the distance to the corresponding pixel location in the image buffer. Note that when dealing with a source point cloud or depth map container, the distance is set in the image buffer at the same location as the point's location in the range component of its container. Points that are not paired with a reference point are given the value of -1.  To use this copy type, [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_DISTANCE_NORMAL`](../../Reference/3dmet/M3dmetControl.md) must have been enabled prior to calling [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) and [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) must be completed.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 1-band, 32-bit float buffer. |

#### `M_DISTANCE_POINT_IMAGE`

Specifies to copy an image of the distance between the source and the reference points into an image buffer. For each source point, the operation copies the distance to the corresponding pixel location in the image buffer. Note that when dealing with a source point cloud or depth map container, the distance is set in the image buffer at the same location as the point's location in the range component of its container. Points that are not paired with a reference point are given the value of -1.  To use this copy type, [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) must be completed.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 1-band, 32-bit float buffer. |

#### `M_PAIRED_REFERENCE_FACET_IMAGE`

Specifies to copy an image of the paired reference facets to the source points.  To use this copy type, [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_SAVE_PAIRED_REFERENCE_FACET`](../../Reference/3dmet/M3dmetControl.md) must have been enabled prior to calling [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) and [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) must be completed.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 1-band, 32-bit unsigned buffer. |

#### `M_PAIRED_REFERENCE_PIXEL_X_IMAGE`

Specifies to copy an image of the paired reference pixel in the X-direction between the source and the reference points.  To use this copy type, [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_SAVE_PAIRED_REFERENCE_PIXEL`](../../Reference/3dmet/M3dmetControl.md) must have been enabled prior to calling [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) and [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) must be completed.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 1-band, 32-bit unsigned buffer. |

#### `M_PAIRED_REFERENCE_PIXEL_Y_IMAGE`

Specifies to copy an image of the paired reference pixel coordinate in the Y-direction between the source and reference points.  To use this copy type, [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_SAVE_PAIRED_REFERENCE_PIXEL`](../../Reference/3dmet/M3dmetControl.md) must have been enabled prior to calling [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) and [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) must be completed.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 1-band, 32-bit unsigned buffer. |

#### `M_REFERENCE_POINT_IMAGE`

Specifies to copy an image of the paired reference points' coordinates used in the distance calculation.  To use this copy type, [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_SAVE_PAIRED_REFERENCE_POINT`](../../Reference/3dmet/M3dmetControl.md) must have been enabled prior to calling [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) and [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) must be completed.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 3-band, 32-bit float buffer. |

---

### `Calculate 3D metrology result buffer ID, for copying results of an M3dmetFeatureEx operation`

Specifies the identifier of a calculate 3D metrology result buffer from which to copy results of an [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) operation. The result buffer must have been previously allocated using [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md) with [`M_CALCULATE_RESULT`](../../Reference/3dmet/M3dmetAllocResult.md).

#### `?`

| Value | Description |
| --- | --- |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object in which to copy results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |
| `3D transformation matrix object identifier` | Specifies the identifier of a 3D transformation matrix object in which to copy results. The 3D transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `Calculate 3D metrology result buffer ID, for copying results of an M3dmetVolumeEx operation`

Specifies the identifier of a calculate 3D metrology result buffer from which to copy results of an [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation. The result buffer must have been previously allocated using [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md) with [`M_CALCULATE_RESULT`](../../Reference/3dmet/M3dmetAllocResult.md).  Some copy types require the volume operation to have completed with a specific status.

#### `M_VOLUME_ELEMENT_INDEX_IMAGE`

Specifies to create an image in which a pixel's gray value is set to the index of the volume element that was used to compute the volume. For unused volume elements, corresponding destination pixels are set to the maximum buffer value.  A volume element's index is its numbered position in the source container's mesh component or in the source depth map image buffer. For a container, the mesh component is one-dimensional, and indexing begins at 0. For a depth map, indexing begins at 0 for the upper left position and proceeds from left to right across all rows in the 2D pixel grid.  You can use the index image to determine which volume elements were used in the volume computation. To draw a single volume element, you can set its index in a draw 3D metrology context (using [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md) with [`M_VOLUME_ELEMENT_INDEX`](../../Reference/3dmet/M3dmetControlDraw.md)), and then draw the element using [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md).

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 1-band, 32-bit unsigned buffer. |

#### `M_VOLUME_ELEMENT_MASK`

Specifies to create a mask image whose pixels are non-zero for volume elements that were used for the volume computation, and zero otherwise. This operation assigns a non-zero value to the volume element's corresponding pixel location in the destination image buffer. The non-zero value is 1 for floating-point image buffers and -1 for signed image buffers. For unsigned image buffers, the maximum buffer value is used. All other pixels in the buffer are set to 0.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 1-band buffer. |

#### `M_VOLUME_ELEMENT_STATUS_IMAGE`

Specifies to create a status image whose pixels represent the status of source volume elements. A volume element's status indicates its contribution to the volume calculation, either positive, negative, a mixture of both, or not at all.  Each status value is assigned to a destination image pixel that corresponds to the volume element's index position in the source container's mesh component (or in the source depth map image buffer). For a container, the mesh component is one-dimensional, and indexing begins at 0. For a depth map, indexing begins at 0 for the upper left position and proceeds from left to right across all rows in the 2D pixel grid.  Aurora Imaging Library assigns the following values to destination image pixels to indicate the source volume element status. Note that spliced volume elements are those that intersect the reference object (only for source point clouds).  |   |   |   |   | | --- | --- | --- | --- | | Unused | Positive | Negative | Both | | 0 | 1 (0b00000001) | 2 (0b00000010) | 3 (0b00000011) | | Unused Spliced | Positive Spliced | Negative Spliced | Both Spliced | | 4 (0b00000100) | 5 (0b00000101) | 6 (0b00000110) | 7 (0b00000111) |  > **Note:** For a source point cloud, any of the eight status values are possible.  Note that for depth maps, this copy type and [`M_VOLUME_SOURCE_POINTS_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md) create identical status images.  You can map the status image through a LUT to display the volume elements' status in different colors, which can help you visualize each volume element's contribution to the volume operation, for diagnostic purposes. Typically, [`M_VOLUME_ELEMENT_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md) generates a status image that is more useful than [`M_VOLUME_SOURCE_POINTS_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md), since volume elements truly indicate the individual volumes used for the volume computation.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 1-band buffer. |

#### `M_VOLUME_REFERENCE_CONTAINER`

Specifies to copy projected source points into a container. The destination container will consist of coplanar points coincident upon the reference plane. These points result from a source point projection on the reference plane to indicate which points and triangles on the reference plane were used in the volume calculation. Note that the reference (destination) container will hold projected points from all source points, regardless of the volume mode setting. The destination container's size equals that of the source container's range component, in X and Y. The destination container's mesh component will have an X-size that equals the number of mesh triangles used to compute the volume. Note that a mesh component is automatically generated for the destination container.  You can use this container to help create a 3D drawing of volume elements. However, the index, mask, or status images that you can create using this function (for example, [`M_VOLUME_ELEMENT_MASK`](../../Reference/3dmet/M3dmetCopyResult.md)) are typically more useful as diagnostic tools.  > **Note:** This copy type is available only when a plane geometry or [`M_XY_PLANE`](../../Reference/3dmet/M3dmetVolumeEx.md) was used as the reference object for the volume computation.

| Value | Description |
| --- | --- |
| `Container identifier` | Specifies the destination container identifier.  The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container. |

#### `M_VOLUME_REFERENCE_DEPTH_MAP`

Specifies to copy volume element depths into a depth map image buffer. If the reference object was a plane, the resulting depth map's pixel values will represent the depth from the boundary of the depth map to the reference plane, for the corresponding source pixel. If the reference object was a depth map, the resulting depth map will be a copy of the reference depth map, with the following exception: any invalid pixel in the source depth map will also invalidate the corresponding pixel in the resulting depth map image buffer.  > **Note:** This copy type is available only when the source object for the volume computation was a depth map.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 1-band buffer.  The image buffer size must equal the size of the depth map used for the volume calculation, in both X and Y. To retrieve the size, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmet/M3dmetGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmet/M3dmetGetResult.md). |

#### `M_VOLUME_SOURCE_POINTS_MASK`

Specifies to create a mask image whose pixels are non-zero for source points that contributed to the volume computation and zero otherwise. When the source is a point cloud, these are the points that define the triangles of the point cloud's mesh, and for which contributing volume elements were calculated. This operation uses the source point's location in its container or depth map to assign a non-zero value to a corresponding pixel location in the image buffer. The non-zero value is 1 for floating-point image buffers and -1 for signed image buffers. For unsigned image buffers, the maximum buffer value is used. All other pixels in the buffer are set to 0.  You can use the mask image to crop out points that were not used to compute the volume. To do so, use [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md).

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 1-band buffer. |

#### `M_VOLUME_SOURCE_POINTS_STATUS_IMAGE`

Specifies to create a status image whose pixels represent the status of source points. A point's status indicates its contribution to the volume calculation, either positive, negative, a mixture of both, or not at all. This operation uses the source point's location in its container or depth map to assign a status value to a corresponding pixel location in the image buffer.  Aurora Imaging Library assigns the following values to image pixels to indicate the point status.  |   |   |   |   | | --- | --- | --- | --- | | Unused | Positive | Negative | Both | | 0 | 1 (0b00000001) | 2 (0b00000010) | 3 (0b00000011) |  Note that for depth maps, this copy type and [`M_VOLUME_ELEMENT_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md) create identical status images.  You can map the status image through a LUT to display the source points' status in different colors, which can help you visualize each point's contribution to the volume operation, for diagnostic purposes. If you want to crop points based on their status, use [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md). Note that you must appropriately process the status image before passing it to [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md). For example, to ensure that pixels indicating a positive status are kept while the rest are set to 0, use [`MimArith`](../../Reference/im/MimArith.md) with [`M_AND_CONST`](../../Reference/im/MimArith.md) to perform a bitwise AND operation; then, pass the resulting image to [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md), which will reject points that correspond to zero-valued pixels in the image.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results. The image buffer must be a 1-band buffer. |

---

### `Fit 3D metrology result buffer ID from which to copy`

Specifies the identifier of a fit 3D metrology result buffer from which to copy results of an [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) operation. The result buffer must have been previously allocated using [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md) with [`M_FIT_RESULT`](../../Reference/3dmet/M3dmetAllocResult.md).  Some copy types require the fit operation to have completed with a specific status.

#### `M_FITTED_GEOMETRY`

Specifies to copy the fitted 3D geometry object; if you specify this copy type, the copied 3D cylinder geometry object or 3D line geometry object will always be finite.  > **Note:** Note that you can perform this copy operation only if the fit completed successfully.

| Value | Description |
| --- | --- |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object in which to copy results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `M_FITTED_GEOMETRY_INFINITE`

Specifies to copy the fitted 3D geometry object; if you specify this copy type, the copied 3D cylinder geometry object or 3D line geometry object will always be infinite.  > **Note:** Note that you can perform this copy operation only if the fit completed successfully.

| Value | Description |
| --- | --- |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object in which to copy results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `M_FIXTURING_MATRIX`

Specifies to copy, into the transformation matrix, the transformation that can move the working coordinate system to the fitted 3D geometry.  If the fitted geometry is a plane, the transformation moves the working coordinate system's X- and Y-axis to the fitted plane, with the origin at the centroid of all inlier points. The plane's normal becomes the Z-axis.  If the fitted geometry is a cylinder or line, the transformation moves the working coordinate system such that its origin is at the starting point of the geometry, with the Z-axis aligned to the central axis.  If the fitted geometry is a sphere, the transformation moves the working coordinate system such that its origin is at the center of the sphere.  > **Note:** Note that you can only perform this copy operation if the fit operation completed successfully.

| Value | Description |
| --- | --- |
| `3D transformation matrix object identifier` | Specifies the identifier of a 3D transformation matrix object in which to copy results. The 3D transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `M_INLIER_MASK`

Specifies to copy the mask constructed by the points that were considered inliers during the fit. That is, points that were considered inliers will have a confidence score of 255 in the destination image buffer, and all other points will have a confidence score of 0. Information about a point is set in the image buffer at the same location as the point's location in the range component of its point cloud container.  > **Note:** Note that you can perform this copy operation whether the fit operation completed successfully or not, as long as a call to [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) was made.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results.  The image buffer must be a 1-band, 8-bit unsigned image buffer. The image buffer size must have the same size as the container or depth map used during the fit operation. To retrieve the size, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmet/M3dmetGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmet/M3dmetGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

#### `M_OUTLIER_MASK`

Specifies to copy the mask constructed by the points that were considered outliers during the fit. That is, points that were considered outliers will have a confidence score of 255 in the destination image buffer, and all other points will have a confidence score of 0. Information about a point is set in the image buffer at the same location as the point's location in the range component of its point cloud container.  > **Note:** Note that, you can perform this copy operation whether the fit operation completed successfully or not, as long as a call to [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) was made.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results.  The image buffer must be a 1-band, 8-bit unsigned image buffer. The image buffer size must have the same size of the container or depth map used during the fit operation. To retrieve the size, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmet/M3dmetGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmet/M3dmetGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

To use this copy type, [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_SAVE_VOLUME_INFO`](../../Reference/3dmet/M3dmetControl.md) must have been enabled prior to calling [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md).

When the source is a container, the image buffer must be one-dimensional with a size in X equal to the mesh component's size in X.

When the source is a depth map, the image buffer's size must match the depth map's size, in both X and Y.

To retrieve the size of the source container (mesh component) or depth map, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_RESULT_ELEMENT_IMAGE_SIZE_X`](../../Reference/3dmet/M3dmetGetResult.md) and [`M_RESULT_ELEMENT_IMAGE_SIZE_Y`](../../Reference/3dmet/M3dmetGetResult.md).

The image buffer size must equal the size of the container (range component) or depth map used for the volume calculation, in both X and Y. To retrieve the size, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmet/M3dmetGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmet/M3dmetGetResult.md).

The image buffer size must equal the size of the source container (range component) or source depth map image buffer used for the distance calculation, in both X and Y. To retrieve the size, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmet/M3dmetGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmet/M3dmetGetResult.md).

> **Note:** For a source depth map, status values are limited to unused, positive, or negative (0, 1, or 2).
