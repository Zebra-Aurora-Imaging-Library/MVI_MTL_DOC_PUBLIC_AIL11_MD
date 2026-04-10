---
doctype: Reference
module: 3dreg
function: M3dregMerge
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregMerge"
---

# M3dregMerge

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

> Merge point clouds using the results of a previous call to [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md).

## Syntax

```c
void M3dregMerge(
    AIL_ID         Result3dregId,           //in
    const AIL_ID * ContainerBufIdArrayPtr,  //in
    AIL_INT        NumContainers,           //in
    AIL_ID         DstContainerBufId,       //out
    AIL_ID         SubsampleContext3dimId,  //in
    AIL_INT64      ControlFlag              //in
)
```

## Description

This function merges all the specified point clouds into a single point cloud. Prior to merging the point clouds, it transforms them such that their working coordinate system is aligned with the global coordinate system; it does so using the results that were stored in the 3D registration result buffer after a successful call to [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md). These transformations ensure that the points are all expressed relative to the same coordinate system.

> **Note:** Note that, instead of aligning the point clouds to the global coordinate system, you can specify to align (fixture) the other point clouds to a specified point cloud's working coordinate system. To do so, use [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_MERGE_LOCATION`](../../Reference/3dreg/M3dregControl.md).

Each point cloud in the array of containers is associated with the registration result element with the same index (for example, the third container in the array is associated with the third registration result element). For the point cloud to be transformed to the appropriate location in the global coordinate system, its index in the array must match the index of the registration result element containing the point cloud's transformation.

Optionally, you can specify a subsampling context to subsample the point clouds after the merge has completed. Whether the merged point cloud is organized or unorganized depends on the subsampling mode of the subsampling context (specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md)). Note that this subsampling is unrelated to the subsampling done before calling [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) (specified using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_SUBSAMPLE`](../../Reference/3dreg/M3dregControl.md)). The latter defines which points are used in calculations for the registration operation, and does not specify which points are retained after calling this function.

Note that for applications that are, for example, merging point clouds from multiple fixed 3D sensors into a single point cloud, you can save ([`M3dregSave`](../../Reference/3dreg/M3dregSave.md)) and restore ([`M3dregRestore`](../../Reference/3dreg/M3dregRestore.md)) a 3D registration result buffer, and reuse it with [`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md).

## Parameters

### `Result3dregId` *(in, AIL_ID)*

Specifies the identifier of the pairwise 3D registration result buffer. The pairwise 3D registration result buffer must have been successfully allocated using [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md), and contain the result of a successful call to [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md).

### `ContainerBufIdArrayPtr` *(in, *AIL_ID)*

Specifies the address of the array containing the identifiers of the point cloud containers. The point clouds must be 3D-processable. You can check that the point clouds are 3D-processable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md). You can set some entries in the array to `M_NULL` to merge a subset of the point clouds.

### `NumContainers` *(in, AIL_INT)*

Specifies the size of the array passed to [`ContainerBufIdArrayPtr`](../../Reference/3dreg/M3dregMerge.md).

*For specifying the number of containers in the array*

| Value | Description |
| --- | --- |
| `2 <= Value <= M3dregInquire(Result3dregId, M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the size of the array. |

### `DstContainerBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container in which to store the transformed and merged point clouds. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container.

### `SubsampleContext3dimId` *(in, AIL_ID)*

Specifies the identifier of a 3D image processing subsampling context. This context is used to subsample the resulting point cloud after the merging is complete. If no subsampling is required, set this parameter to `M_NULL`.

### `ControlFlag` *(in, AIL_INT64)*

Specifies which components will be affected by the operation.

*For specifying whether to merge points from all components*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the operation will only affect the core components of a 3D-processable container. These core components are listed in the description of [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md).

All other components are copied to the destination container unmodified. |
| `M_APPLY_TO_ALL_COMPONENTS` | Specifies that the operation will affect the core components of a 3D-processable container, as well as any other component (including custom components) that has the same dimension as [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md). These components must not have a region of interest (ROI) associated with them. For example, using a component that is an image buffer with an ROI will cause an error.

> **Note:** Note that if a custom component has an associated calibration context, the component is merged, but its context is discarded.

To avoid ambiguity, within each source container, each component must be unique such that its data type and number of bands are not identical to any other component of the same type in the container.

[`M_APPLY_TO_ALL_COMPONENTS`](../../Reference/3dreg/M3dregMerge.md) does not support 1-bit buffers.

If the resulting operation yields an empty container with 0 valid points, the components with the same dimensions are not created and will not exist in the destination container. |
