---
doctype: Reference
module: 3dblob
function: M3dblobCalculate
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobCalculate"
---

# M3dblobCalculate

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

> Perform 3D blob analysis calculations.

## Syntax

```c
void M3dblobCalculate(
    AIL_ID    CalculateContext3dblobId,  //in
    AIL_ID    ContainerBufId,            //in
    AIL_ID    Result3dblobId,            //out
    AIL_INT64 LabelOrIndex,              //in
    AIL_INT64 ControlFlag                //in
)
```

## Description

This function calculates the specified features for the specified blobs. The features are calculated from the blobs in the source point cloud container, and results are added to the specified result buffer, which is the result buffer that holds the blob segmentation computed from the same source point cloud. To specify the features to calculate, pass a 3D blob analysis context that has the required features enabled for calculation. Use [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) to enable the features. Use [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) to fill the result buffer with blob identification information; the result buffer is used to identify which points in the specified container belong to the same blob. If you want to calculate features for a subset of the blobs in the point cloud, use [`M3dblobSelect`](../../Reference/3dblob/M3dblobSelect.md) and [`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md) to fill a result buffer with blob identifying information for the required blobs.

[`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) is optimized to not recalculate the same features twice. If you call the function several times without modifying the container's range component, [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) only calculates features that are enabled in the context but not yet stored in the result buffer. If the range component has changed, the function removes all feature results from the result buffer and then calculates the features enabled in the context.

## Parameters

### `CalculateContext3dblobId` *(in, AIL_ID)*

Specifies the identifier of the 3D blob analysis context that specifies the feature(s) to calculate, and other settings related to the 3D blob analysis operation. The context must have been previously allocated using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_CALCULATE_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md).

### `ContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the 3D-processable point cloud container from which the blobs originate. This must be the same container previously passed to [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md), and its confidence component must not have been modified since then. If the segmentation was performed using a label image instead, this can be any container as long as the size of its range component is the same as the label image.

### `Result3dblobId` *(out, AIL_ID)*

Specifies the identifier of the 3D blob analysis result buffer that holds the blob segmentation, and in which to store results. The result buffer must have been previously allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md).

### `LabelOrIndex` *(in, AIL_INT64)*

Specifies the label or index of the blob(s) for which to calculate features, or specifies that you are calculating features for all blobs. This parameter must be set to one of the following values:

*For specifying the blob label or index*

| Value | Description |
| --- | --- |
| `M_BLOB_INDEX` | Specifies the index of the blob. |
| `M_BLOB_LABEL` | Specifies the label of the blob. |
| `M_ALL_BLOBS` | Specifies to calculate features for all blobs. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
