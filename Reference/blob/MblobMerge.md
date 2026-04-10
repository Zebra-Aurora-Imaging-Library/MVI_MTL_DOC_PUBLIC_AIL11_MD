---
doctype: Reference
module: blob
function: MblobMerge
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / blob / MblobMerge"
---

# MblobMerge

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

> Merge the results of two blob result buffers.

## Syntax

```c
void MblobMerge(
    AIL_ID    SrcResultBlobId1,  //in
    AIL_ID    SrcResultBlobId2,  //in
    AIL_ID    DstResultBlobId,   //out
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function merges the results of two blob result buffers into a single result buffer.

The source results are merged as if they were taken from two vertically adjacent child buffers of one image. As such, the destination result buffer uses the coordinate system of the first result buffer, and positional results from the second result buffer are translated in the Y-direction to take into account this coordinate system change. Border blobs that would touch if they were in one image are grouped into one grouped blob, assigned a single label, and recalculated as if the grouped blobs were one blob. Only features that were calculated for all the individual blobs in the group are recalculated for the new grouped blob; calculations are made using the results of the individual blobs in the group. See [Merging results](../../UserGuide/C06_Blob_analysis/S15_Merging_results.md) for more detailed explanations.

Note that after merging the results, you can perform an [`MblobSelect`](../../Reference/blob/MblobSelect.md) operation to further include or exclude blobs in your destination result buffer, but if you perform an [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) operation after the merge, all the results in the destination result buffer will be discarded.

The source blob results must have been calculated by a context that:

- Has the same run information saving mode ([`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) set to [`M_ENABLE`](../../Reference/blob/MblobControl.md) or [`M_DISABLE`](../../Reference/blob/MblobControl.md)).
- Has the same pixel aspect ratio ([`M_PIXEL_ASPECT_RATIO`](../../Reference/blob/MblobControl.md)).
- Has the same lattice ([`M_CONNECTIVITY`](../../Reference/blob/MblobControl.md)).
- Has the same blob identification mode ([`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md)). If the blob result buffers have [`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md) set to [`M_LABELED`](../../Reference/blob/MblobControl.md), they cannot be merged.
- Has the same number of Ferets ([`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md)).

Note that if the source results include user-specified moments (added using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md)), or third-order moments calculated in precise mode (added using [`MblobControl`](../../Reference/blob/MblobControl.md) with[`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md) set to [`M_ENABLE`](../../Reference/blob/MblobControl.md), and [`M_MOMENT_THIRD_ORDER_MODE`](../../Reference/blob/MblobControl.md) set to [`M_PRECISE`](../../Reference/blob/MblobControl.md)), [`MblobMerge`](../../Reference/blob/MblobMerge.md) will only be able to merge the moments if they were calculated using their binary definition and [`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) was enabled. If they were calculated using their grayscale definition or [`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) was not enabled, the moments will not be merged.

Furthermore, if you specify a sorting key for result retrieval, the sorting order will not be respected after the merge.

If both source result buffers have the same camera calibration information (although their results might be from different child images), the destination result buffer will have the same camera calibration information. If either of the source result buffers is uncalibrated, or if they have different camera calibration information, the destination result buffer will be uncalibrated.

## Parameters

### `SrcResultBlobId1` *(in, AIL_ID)*

Specifies the identifier of the first source result buffer. [`SrcResultBlobId1`](../../Reference/blob/MblobMerge.md) and [`SrcResultBlobId2`](../../Reference/blob/MblobMerge.md) cannot be the same.

### `SrcResultBlobId2` *(in, AIL_ID)*

Specifies the identifier of the second source result buffer. [`SrcResultBlobId1`](../../Reference/blob/MblobMerge.md) and [`SrcResultBlobId2`](../../Reference/blob/MblobMerge.md) cannot be the same.

### `DstResultBlobId` *(out, AIL_ID)*

Specifies the identifier of the destination result buffer. [`DstResultBlobId`](../../Reference/blob/MblobMerge.md) cannot be the same as [`SrcResultBlobId1`](../../Reference/blob/MblobMerge.md) or[`SrcResultBlobId2`](../../Reference/blob/MblobMerge.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies the control settings for the merge operation. This parameter can be set to any of the following.

*For specifying the type of merge operation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default control operation.

The default value is [`M_COPY`](../../Reference/blob/MblobMerge.md), in which the source results are copied into the destination result buffer, and the orientation of the merged result will be as if the source results were taken from two vertically adjacent child buffers of one image. |
| `M_COPY` | Copies the results of [`SrcResultBlobId1`](../../Reference/blob/MblobMerge.md) and [`SrcResultBlobId2`](../../Reference/blob/MblobMerge.md) into the destination result buffer. After the merge, the original results of [`SrcResultBlobId1`](../../Reference/blob/MblobMerge.md) and [`SrcResultBlobId2`](../../Reference/blob/MblobMerge.md) remain intact. |
| `M_MOVE` | Moves the results of the source result buffers into the destination result buffer. After the merge, the original results of [`SrcResultBlobId1`](../../Reference/blob/MblobMerge.md) and [`SrcResultBlobId2`](../../Reference/blob/MblobMerge.md) are emptied. |
