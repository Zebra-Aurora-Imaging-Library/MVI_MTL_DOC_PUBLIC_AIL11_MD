---
doctype: Reference
module: 3dmod
function: M3dmodCopyResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodCopyResult"
---

# M3dmodCopyResult

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

> Copy a group of results from a 3D model finder result buffer into a 3D geometry object, an image buffer, a transformation matrix object, a model in a find surface or planar surface 3D model finder context, or a surface or planar surface 3D model finder result buffer.

## Syntax

```c
void M3dmodCopyResult(
    AIL_ID    SrcResult3dmodId,  //in
    AIL_INT64 SrcIndex,          //in
    AIL_ID    DstAilObjectId,    //out
    AIL_INT64 DstIndex,          //in
    AIL_INT64 CopyType,          //in
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function copies a group of results from a 3D model finder result buffer into a 3D geometry object, an image buffer, a transformation matrix object, a model in a find surface or planar surface 3D model finder context, or a surface or planar surface 3D model finder result buffer, according to the specified copy operation.

## Parameters

### `SrcResult3dmodId` *(in, AIL_ID)*

Specifies the identifier of the 3D model finder result buffer from which to copy. The 3D model finder result buffer must have been allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) and must contain the results of a call to [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md).

### `SrcIndex` *(in, AIL_INT64)*

Specifies the index of the model occurrence in the result buffer.

*For specifying the index of the model occurrence to copy*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to copy the result for all model occurrences. |
| `M_GENERAL` *(default)* | Specifies to copy the result relating to the entire result buffer. |
| `0 <= Value < M_NUMBER` | Specifies the index of the model occurrence for which to copy results. |

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the 3D geometry object, transformation matrix object, image buffer, find surface or planar surface 3D model finder context, or surface or planar surface 3D model finder result buffer in which to store the copied results.

### `DstIndex` *(in, AIL_INT64)*

Specifies the index of the element in the destination object in which to copy.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For copying general results from the source 3D model finder result buffer

To copy general results, set the [`CopyType`](../../Reference/3dmod/M3dmodCopyResult.md) parameter to the following value. In this case, you must set [`SrcIndex`](../../Reference/3dmod/M3dmodCopyResult.md) to [`M_GENERAL`](../../Reference/3dmod/M3dmodCopyResult.md) or [`M_DEFAULT`](../../Reference/3dmod/M3dmodCopyResult.md).  Note that any unused parameters should be set to [`M_DEFAULT`](../../Reference/3dmod/M3dmodCopyResult.md).

---

### `M_BACKGROUND_MASK`

Specifies to copy a non-zero value into an image buffer for each of the occurrence's background points and a 0 value elsewhere, creating a background mask image. Information about a point is set in the image buffer at the same location as the point's location in the range component of its point cloud container. Note that this is only available if [`M_REMOVE_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) was enabled.

#### `Image buffer identifier in which to copy`

Specifies the identifier of the image buffer in which to copy results.  The image buffer must be a 1-band buffer. The image buffer size must equal the size of the container used during the 3D model finder operation, in both X and Y. To retrieve the size, use [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmod/M3dmodGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmod/M3dmodGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

---

### `M_FLOOR_MASK`

Specifies to copy a non-zero value into an image buffer for each of the floor points and a 0 value elsewhere, creating a floor mask image. Information about a point is set in the image buffer at the same location as the point's location in the range component of its point cloud container. Note that this is only available if [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodControl.md) was enabled.  > **Note:** Note that [`M_FLOOR_MASK`](../../Reference/3dmod/M3dmodCopyResult.md) is only available for surface and planar surface results.

#### `Image buffer identifier in which to copy`

Specifies the identifier of the image buffer in which to copy results.  The image buffer must be a 1-band buffer. The image buffer size must equal the size of the container used during the 3D model finder operation, in both X and Y. To retrieve the size, use [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmod/M3dmodGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmod/M3dmodGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

---

### `M_RESULT`

Specifies to copy surface or planar surface results into another surface or planar surface 3D model finder result buffer, respectively, to reuse them in subsequent searches. You should use this copy type when you want to reuse the results as an optimization to specify the approximate location for multiple searches. For example, if you expect occurrences to be found in similar locations in multiple future searches, set up two result buffers: one for the initial search and another for use with subsequent searches. Before each subsequent search, copy into its result buffer the results from the initial search (so as not to change the original results), and use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REUSE_RESULT`](../../Reference/3dmod/M3dmodControl.md) set to [`M_ENABLE`](../../Reference/3dmod/M3dmodControl.md) to reuse them at the beginning of the search to speed up the operation. [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) will search for occurrences at the stored positions before searching the rest of the target. You can use [`M3dmodModifyResult`](../../Reference/3dmod/M3dmodModifyResult.md) to modify or delete results before performing the search.  Note that if you only want to reuse the results once, you don't need to use this copy type; if no results are copied and [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REUSE_RESULT`](../../Reference/3dmod/M3dmodControl.md) is enabled, [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) will reuse the most recent results in the specified result buffer.  > **Note:** Note that [`M_RESULT`](../../Reference/3dmod/M3dmodCopyResult.md) is only available for surface and planar surface results.

#### `Planar surface 3D model finder result buffer identifier in which to copy`

Specifies the identifier of a planar surface 3D model finder result buffer in which to copy planar surface results. The result buffer must have been previously allocated using[`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_PLANAR_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md).

#### `Surface 3D model finder result buffer identifier in which to copy`

Specifies the identifier of a surface 3D model finder result buffer in which to copy surface results. The result buffer must have been previously allocated using[`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md).

### For copying the results of a specific occurrence or all occurrences from the source 3D model finder result buffer

To copy the results of a specific occurrence or all occurrences, set the [`CopyType`](../../Reference/3dmod/M3dmodCopyResult.md) parameter to one of the following values. In this case, you must set [`SrcIndex`](../../Reference/3dmod/M3dmodCopyResult.md) to [`M_ALL`](../../Reference/3dmod/M3dmodCopyResult.md) or the index of the occurrence.  Note that any unused parameters should be set to [`M_DEFAULT`](../../Reference/3dmod/M3dmodCopyResult.md).

---

### `M_INLIER_INDEX_IMAGE`

Specifies to copy the model occurrence's index into an image buffer for each of the occurrence's inlier points, creating an inlier area index image. The inlier points of the occurrence are based on [`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md). Information about a point is set in the image buffer at the same location as the point's location in the range component of its point cloud container. Points that are not part of any occurrence are given the value of -1 in signed buffers, and are set to the maximum buffer value in unsigned buffers.  Note that for surface and planar surface results, this is only available if [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) was enabled.

#### `Image buffer identifier in which to copy`

Specifies the identifier of the image buffer in which to copy results.  The image buffer must be a 1-band buffer. The image buffer size must equal the size of the container used during the 3D model finder operation, in both X and Y. To retrieve the size, use [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmod/M3dmodGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmod/M3dmodGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

---

### `M_INLIER_MASK`

Specifies to copy a non-zero value into an image buffer for each of the occurrence's inlier points and a 0 value elsewhere, creating an inlier mask image. The non-zero value is 1 for floating-point image buffers and -1 for signed image buffers. For unsigned image buffers, the maximum buffer value is used. All other pixels in the buffer are set to 0. The inlier points of the occurrence are based on [`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md). Information about a point is set in the image buffer at the same location as the point's location in the range component of its point cloud container.  Note that for surface and planar surface results, this is only available if [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) was enabled.

#### `Image buffer identifier in which to copy`

Specifies the identifier of the image buffer in which to copy results.  The image buffer must be a 1-band buffer. The image buffer size must equal the size of the container used during the 3D model finder operation, in both X and Y. To retrieve the size, use [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmod/M3dmodGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmod/M3dmodGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

---

### `M_RESERVED_INDEX_IMAGE`

Specifies to copy the model occurrence's index into the destination image buffer for each of the points in the reserved area of the occurrence, creating a reserved area index image. For cylinder and sphere results, the points in the reserved area of the occurrence are based on [`M_RESERVED_POINTS_DISTANCE`](../../Reference/3dmod/M3dmodControl.md). Information about a point is set in the image buffer at the same location as the point's location in the range component of its point cloud container. Points that are not part of any occurrence are given the value of -1 in signed buffers, and are set to the maximum buffer value in unsigned buffers.

#### `Image buffer identifier in which to copy`

Specifies the identifier of the image buffer in which to copy results.  The image buffer must be a 1-band buffer. The image buffer size must equal the size of the container used during the 3D model finder operation, in both X and Y. To retrieve the size, use [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmod/M3dmodGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmod/M3dmodGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

---

### `M_RESERVED_MASK`

Specifies to copy a non-zero value into an image buffer for each of the occurrence's reserved points and a 0 value elsewhere, creating a reserved mask image. The non-zero value is 1 for floating-point image buffers and -1 for signed image buffers. For unsigned image buffers, the maximum buffer value is used. All other pixels in the buffer are set to 0. For cylinder and sphere results, the points in the reserved area of the occurrence are based on [`M_RESERVED_POINTS_DISTANCE`](../../Reference/3dmod/M3dmodControl.md). Information about a point is set in the image buffer at the same location as the point's location in the range component of its point cloud container. Note that this copy type is useful for creating a mask image to use, for example, with [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md).

#### `Image buffer identifier in which to copy`

Specifies the identifier of the image buffer in which to copy results.  The image buffer must be a 1-band buffer. The image buffer size must equal the size of the container used during the 3D model finder operation, in both X and Y. To retrieve the size, use [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dmod/M3dmodGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dmod/M3dmodGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

### For copying the results of a specific occurrence from the source 3D model finder result buffer

To copy the results of a specific occurrence, set the [`CopyType`](../../Reference/3dmod/M3dmodCopyResult.md) parameter to one of the following values. In this case, you must set [`SrcIndex`](../../Reference/3dmod/M3dmodCopyResult.md) to the index of the occurrence.  Note that any unused parameters should be set to [`M_DEFAULT`](../../Reference/3dmod/M3dmodCopyResult.md).

---

### `M_BOUNDING_BOX`

Specifies to copy the oriented bounding box of the found model occurrence into the specified 3D geometry object, establishing a 3D box geometry.

#### `3D geometry object identifier in which to copy`

Specifies the identifier of a 3D geometry object in which to copy results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

---

### `M_CENTER`

Specifies to copy, into a 3D geometry object, the center of the specified face of the box occurrence, establishing a point geometry.  > **Note:** Note that [`M_CENTER`](../../Reference/3dmod/M3dmodCopyResult.md) is only available for box occurrences.

#### `3D geometry object identifier in which to copy`

Specifies the identifier of a 3D geometry object in which to copy results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).  Note that after calling [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md), the destination 3D geometry object will return type [`M_POINT`](../../Reference/3dgeo/M3dgeoInquire.md) if inquired using [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_GEOMETRY_TYPE`](../../Reference/3dgeo/M3dgeoInquire.md).

---

### `M_FACE`

Specifies to copy, into a 3D geometry object, the specified face of the box occurrence, establishing a plane geometry.  > **Note:** Note that [`M_FACE`](../../Reference/3dmod/M3dmodCopyResult.md) is only available for box occurrences.

#### `3D geometry object identifier in which to copy`

Specifies the identifier of a 3D geometry object in which to copy results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).  Note that after calling [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md), the destination 3D geometry object will return type [`M_PLANE`](../../Reference/3dgeo/M3dgeoInquire.md) if inquired using [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_GEOMETRY_TYPE`](../../Reference/3dgeo/M3dgeoInquire.md).

---

### `M_FIXTURING_MATRIX`

Specifies to copy, into the transformation matrix, the transformation that can move the working coordinate system to the found model occurrence.  For a box occurrence, the transformation moves the working coordinate system's origin to the box's center. The box's rotation is applied if there is one. You can add **M_FACE_INDEX()** to [`M_FIXTURING_MATRIX`](../../Reference/3dmod/M3dmodCopyResult.md) to copy the fixturing matrix for one face of a box occurrence. For a face, the transformation moves the working coordinate system's X- and Y- axis to the center of the face, and the normal becomes the Z-axis.  For a cylinder occurrence, the transformation moves the working coordinate system's origin to the cylinder's start point, and the cylinder's central axis becomes the positive Z-axis. The rotation of the X- and Y-axis is determined by the minimal rotation that must be applied to align the positive Z-axis with the cylinder's central axis.  For a rectangular plane occurrence, the transformation moves the working coordinate system's X- and Y- axis to the rectangular plane, and the normal becomes the Z-axis.  For a sphere occurrence, the transformation moves the working coordinate system's origin to the sphere's center. No rotation is applied.  For a surface occurrence, the transformation moves the working coordinate system such that it aligns with the model's reference axis transformed at the occurrence.

#### `Transformation matrix object identifier in which to copy`

Specifies the identifier of the transformation matrix object in which to copy results. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

---

### `M_GEOMETRY`

Specifies to copy the geometry of the found model occurrence, into the specified 3D geometry object, establishing a 3D geometry of the same type.  > **Note:** Note that [`M_GEOMETRY`](../../Reference/3dmod/M3dmodCopyResult.md) is only available for sphere, cylinder, and box model results.

#### `3D geometry object identifier in which to copy`

Specifies the identifier of a 3D geometry object in which to copy results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

---

### `M_OCCURRENCE_MATRIX`

Specifies to copy, into the transformation matrix, the transformation that transforms the model to the same location as the found occurrence.  > **Note:** Note that [`M_OCCURRENCE_MATRIX`](../../Reference/3dmod/M3dmodCopyResult.md) is only available for surface and planar surface results.

#### `Transformation matrix object identifier in which to copy`

Specifies the identifier of the transformation matrix object in which to copy results. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

---

### `M_PLANE`

Specifies to copy into a 3D geometry object the infinite plane on which the rectangular plane occurrence is found.  > **Note:** Note that [`M_PLANE`](../../Reference/3dmod/M3dmodCopyResult.md) is only available for rectangular plane results.

#### `3D geometry object identifier in which to copy`

Specifies the identifier of a 3D geometry object in which to copy results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

---

### `M_RESTING_PLANE`

Specifies to copy the resting plane on which the occurrence lies (set using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md)), to the model in the specified find surface or planar surface 3D model finder context or into the specified 3D geometry object.  > **Note:** Note that [`M_RESTING_PLANE`](../../Reference/3dmod/M3dmodCopyResult.md) is only available for surface and planar surface results.

#### `3D geometry object identifier in which to copy`

Specifies the identifier of a 3D geometry object in which to copy the resting plane. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

#### `Find planar surface 3D model finder context identifier in which to copy`

Specifies the identifier of a find planar surface 3D model finder context in which to copy the resting plane. The context must have been previously allocated using[`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_PLANAR_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md).

| Value | Description |
| --- | --- |
| `0 <= Value < M_NUMBER_MODELS` | Specifies the index of the model in the find planar surface 3D model finder context in which to copy the resting plane. |

#### `Find surface 3D model finder context identifier in which to copy`

Specifies the identifier of a find surface 3D model finder context in which to copy the resting plane. The context must have been previously allocated using[`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md).

| Value | Description |
| --- | --- |
| `0 <= Value < M_NUMBER_MODELS` | Specifies the index of the model in the find surface 3D model finder context in which to copy the resting plane. |

### Combination Constants — For specifying the box face's index

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify for which face of the box occurrence to copy results.

| Value | Description |
| --- | --- |
| `M_FACE_INDEX` | Specifies to copy results for a specific face of a box occurrence. |
