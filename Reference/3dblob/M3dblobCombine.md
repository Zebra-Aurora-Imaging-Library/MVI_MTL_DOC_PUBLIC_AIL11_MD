---
doctype: Reference
module: 3dblob
function: M3dblobCombine
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobCombine"
---

# M3dblobCombine

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

> Combine sets of blobs from different result buffers into one result buffer.

## Syntax

```c
void M3dblobCombine(
    AIL_ID    Src1Result3dblobId,  //in
    AIL_ID    Src2Result3dblobId,  //in
    AIL_ID    DstResult3dblobId,   //out
    AIL_INT64 Operation,           //in
    AIL_INT64 ControlFlag          //in
)
```

## Description

This function combines sets of blobs from different 3D blob analysis result buffers into one result buffer, according to the specified combine operation. The two source result buffers must hold sets of blob identifying information and feature results from two different selection operations ([`M3dblobSelect`](../../Reference/3dblob/M3dblobSelect.md)). The subsets in the source result buffers must also originate from the same blob segmentation ([`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md)).

[`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md) allows you to perform compound selections. For example, to select blobs that meet criterion 1 OR criterion 2, first call [`M3dblobSelect`](../../Reference/3dblob/M3dblobSelect.md) twice with two different 3D blob analysis result buffers: once to select for criterion 1, and then a second time to select for criterion 2. Then, combine the two sets of blob identifying information and feature results using [`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md) with [`M_OR`](../../Reference/3dblob/M3dblobCombine.md). Note that different feature results for the same blob in two source result buffers are combined and assigned to that same blob in the destination result buffer. For example, if source 1 has blob 42's feature A result and source 2 has blob 42's feature B result, then the destination result buffer will have both feature A and feature B results for blob 42 (provided the specified combine operation doesn't exclude blob 42). An error is reported if both sources contain the same feature's results, but the feature's values are different.

To merge two blobs into one blob, use [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_MERGE`](../../Reference/3dblob/M3dblobControl.md).

This function supports in-place processing; you can specify the identifier of the same result buffer for one of the sources and the destination.

## Parameters

### `Src1Result3dblobId` *(in, AIL_ID)*

Specifies the identifier of the first source 3D blob analysis result buffer, previously allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md) with [`M_SEGMENTATION_RESULT`](../../Reference/3dblob/M3dblobAllocResult.md).

### `Src2Result3dblobId` *(in, AIL_ID)*

Specifies the identifier of the second source 3D blob analysis result buffer, previously allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md) with [`M_SEGMENTATION_RESULT`](../../Reference/3dblob/M3dblobAllocResult.md).

### `DstResult3dblobId` *(out, AIL_ID)*

Specifies the identifier of the 3D blob analysis result buffer in which to store the results of the combine operation. In-place processing is supported; [`DstResult3dblobId`](../../Reference/3dblob/M3dblobCombine.md) can refer to the same result buffer as [`Src1Result3dblobId`](../../Reference/3dblob/M3dblobCombine.md) or [`Src2Result3dblobId`](../../Reference/3dblob/M3dblobCombine.md).

### `Operation` *(in, AIL_INT64)*

Specifies how to combine the sets of blobs. This parameter must be set to one of the following values:

*For specifying how to combine the sets of blobs*

| Value | Description |
| --- | --- |
| `M_AND` | Selects blobs for which selected feature results exist in both source result buffers.

This is an intersection, equivalent to `_Src1_ ∩ _Src2_` in set language notation. |
| `M_OR` | Selects blobs for which selected feature results exist in any source result buffer.

This is a union, equivalent to `_Src1_ ∪ _Src2_` in set language notation. |
| `M_SUB` | Selects blobs for which selected feature results exist in the first source result buffer, but not the second.

This is a subtraction, equivalent to `_Src1_ - _Src2_` in set language notation. |
| `M_XOR` | Selects blobs for which selected feature results exist in one source result buffer, but not both.

This is a symmetric difference, equivalent to `(_Src1_ ∪ _Src2_) - (_Src1_ ∩ _Src2_)` in set language notation. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
