---
doctype: Reference
module: 3dreg
function: M3dregCopyResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregCopyResult"
---

# M3dregCopyResult

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

> Copy a group of results from a 3D registration result buffer into a transformation matrix object, an image buffer, or a container.

## Syntax

```c
void M3dregCopyResult(
    AIL_ID    SrcResult3dregId,  //in
    AIL_INT   SrcIndex,          //in
    AIL_INT   SrcReference,      //in
    AIL_ID    DstAilObjectId,    //out
    AIL_INT64 CopyType,          //in
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function copies a group of 3D registration results into a transformation matrix object, an image buffer, or a container, according to the specified copy operation. The 3D registration result buffer must contain results from a successful call to [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) with a pairwise 3D registration context.

You can copy the transformation matrix that registers a point cloud to its reference point cloud, or you can retrieve the transformation matrix that registers a point cloud to any other point cloud in the list of point clouds that were also registered when [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) was called. The destination transformation matrix object can then be used to align the working coordinate system of the point cloud to the working coordinate system of the other specified point cloud, using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md).

To retrieve the result of an intermediate iteration of the registration operation, specify the iteration index using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md)with [`M_ITERATION_INDEX`](../../Reference/3dreg/M3dregControl.md), and combine the specified copy type with the [`M_INTERMEDIATE_ITERATION`](../../Reference/3dreg/M3dregCopyResult.md) combination constant.

## Parameters

### `SrcResult3dregId` *(in, AIL_ID)*

Specifies the identifier of the pairwise 3D registration result buffer. The pairwise 3D registration result buffer must have been previously allocated using [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md).

### `SrcIndex` *(in, AIL_INT)*

Specifies the index of the registration result element associated with the source point cloud.

### `SrcReference` *(in, AIL_INT)*

Specifies relative to which coordinate system (the global coordinate system or that of a point cloud) to retrieve the copied information.

*For specifying the reference*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REGISTRATION_GLOBAL` *(default)* | Specifies the global coordinate system. |
| `0 <= Value < M3dregInquire(SrcResult3dregId, M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the registration result element associated with the point cloud to use as the reference.

This value should not equal the value specified for [`SrcIndex`](../../Reference/3dreg/M3dregCopyResult.md). |

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the destination transformation matrix, image buffer, or container in which to save the copied information.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the copy type and destination object

---

### `M_DISTANCE_IMAGE`

Specifies to copy point distances into an image buffer, creating a distance image. This operation copies the distance between paired points to a pixel location in the destination image buffer that corresponds to the reference point's location in its container. Paired points are those between the reference point cloud and the specified registration result element's point cloud (the target point cloud). Unpaired points are considered invalid and given the value of -1 in signed buffers and set to the maximum buffer value in unsigned buffers.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results.  The image buffer must be a 1-band buffer and its size must equal, in both X and Y, the size of the reference point cloud. Specifically, this is the size of the range component of the reference point cloud associated with the registration result element specified with [`SrcIndex`](../../Reference/3dreg/M3dregCopyResult.md). Note that the size will reflect any subsampling that was applied. To retrieve the size, use [`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dreg/M3dregGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dreg/M3dregGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

---

### `M_OVERLAP_MASK`

Specifies to create a mask image whose pixels are non-zero for overlapping (paired) points, and zero otherwise. Overlapping points are points in the reference point cloud that have been paired with at least one target point. This operation uses the point's location in its container (the reference point cloud) to assign a non-zero value to a corresponding pixel location in the image buffer. The non-zero value is 1 for floating-point image buffers and -1 for signed image buffers. For unsigned image buffers, the maximum buffer value is used. All other pixels in the buffer are set to 0.  > **Note:** Note that paired points at higher ranks are typically less common. You can use the overlap mask image to help you visualize which points have pairings at higher ranks.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results.  The image buffer must be a 1-band buffer and its size must equal, in both X and Y, the size of the reference point cloud. Specifically, this is the size of the range component of the reference point cloud associated with the registration result element specified with [`SrcIndex`](../../Reference/3dreg/M3dregCopyResult.md). Note that the size will reflect any subsampling that was applied. To retrieve the size, use [`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dreg/M3dregGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dreg/M3dregGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

---

### `M_PAIR_INDEX_IMAGE`

Specifies to copy point indices into an image buffer, creating a pair index image. The pixels in the resulting image reflect point pairings, given that a point's index is its numbered position in the 2D organization of its point cloud container. (Indexing begins at 0 for the upper left position and proceeds from left to right across all rows in the 2D grid.) To create the pair index image, for each point pair, the target point's index is copied to the image at a pixel position that is the same as the reference point's index. To illustrate, if a point pair consists of a reference point at index 0 and a target point at index 20 (in their respective containers), then the pixel at index 0 in the pair index image receives the value 20. Unpaired points are considered invalid and given the value of -1 in signed buffers, and set to the maximum buffer value in unsigned buffers.

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of an image buffer in which to copy results.  The image buffer must be a 1-band buffer and its size must equal, in both X and Y, the size of the reference point cloud. Specifically, this is the size of the range component of the reference point cloud associated with the registration result element specified with [`SrcIndex`](../../Reference/3dreg/M3dregCopyResult.md). Note that the size will reflect any subsampling that was applied. To retrieve the size, use [`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md) with [`M_RESULT_IMAGE_SIZE_X`](../../Reference/3dreg/M3dregGetResult.md) and [`M_RESULT_IMAGE_SIZE_Y`](../../Reference/3dreg/M3dregGetResult.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

---

### `M_REGISTRATION_MATRIX`

Specifies to copy the transformation matrix stored in the pairwise 3D registration result buffer to a transformation matrix object.

| Value | Description |
| --- | --- |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the transformation matrix object in which to copy. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `M_SUBSAMPLED_CONTAINER_REFERENCE`

Specifies to copy the subsampled version of the reference point cloud associated with the specified registration result element into a container. By default, no subsampling is applied. In general, however, subsampling is recommended (using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_SUBSAMPLE`](../../Reference/3dreg/M3dregControl.md) and/or [`M_SUBSAMPLE_REFERENCE`](../../Reference/3dreg/M3dregControl.md)). The default subsampling mode (specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md)), is [`M_SUBSAMPLE_DECIMATE`](../../Reference/3dim/M3dimControl.md) with a step size of 1 (in X and Y), which amounts to virtually no subsampling. Adjust the subsampling settings to find the best mode for your application.  Since subsampling can differ for each registration element, this copy type allows you to retrieve the specific subsampled reference point cloud used in the registration operation for that registration result element. The subsampled reference point cloud will have the same X/Y dimensions as any distance image, overlap mask, or pair index image created using this function for the respective registration result element.

| Value | Description |
| --- | --- |
| `Container identifier` | Specifies the destination container identifier.  The destination container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container. If a range component and a confidence component already exist in the container, they will be overwritten. If the container is empty, the range and confidence components are created to hold the copied information. |

---

### `M_SUBSAMPLED_CONTAINER_TARGET`

Specifies to copy the subsampled version of the target point cloud associated with the specified registration result element into a container. By default, no subsampling is applied. In general, however, subsampling is recommended (using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_SUBSAMPLE`](../../Reference/3dreg/M3dregControl.md) and/or [`M_SUBSAMPLE_TARGET`](../../Reference/3dreg/M3dregControl.md)). The default subsampling mode (specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md)), is [`M_SUBSAMPLE_DECIMATE`](../../Reference/3dim/M3dimControl.md) with a step size of 1 (in X and Y), which amounts to virtually no subsampling. Adjust the subsampling settings to find the best mode for your application.  Since subsampling can differ for each registration element, this copy type allows you to retrieve the specific subsampled target point cloud used in the registration operation for that registration result element.

| Value | Description |
| --- | --- |
| `Container identifier` | Specifies the destination container identifier.  The destination container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container. If a range component and a confidence component already exist in the container, they will be overwritten. If the container is empty, the range and confidence components are created to hold the copied information. |

### Combination Constants — For specifying to retrieve the result of an intermediate iteration

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the result for an intermediate iteration.

| Value | Description |
| --- | --- |
| `M_INTERMEDIATE_ITERATION` | Specifies to retrieve the result for an intermediate iteration of the pairwise 3D registration operation.  You can specify from which intermediate iteration to retrieve results using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_ITERATION_INDEX`](../../Reference/3dreg/M3dregControl.md). |

For reference points that are paired to multiple target points, use [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_PAIRS_RANK`](../../Reference/3dreg/M3dregControl.md) to set from which point pair to copy results.

To use this copy type, you must have enabled [`M_SAVE_PAIRS_INFO`](../../Reference/3dreg/M3dregControl.md) (using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md)) prior to performing the pairwise 3D registration operation.

Use [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_ITERATION_INDEX`](../../Reference/3dreg/M3dregControl.md) to specify from which iteration to copy the information.

> **Note:** Note, [`SrcReference`](../../Reference/3dreg/M3dregCopyResult.md) must be set to [`M_DEFAULT`](../../Reference/3dreg/M3dregCopyResult.md).
