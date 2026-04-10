---
doctype: Reference
module: 3dmod
function: M3dmodModifyResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmod / M3dmodModifyResult"
---

# M3dmodModifyResult

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

> Modify or delete surface or planar surface results from a surface or planar surface 3D model finder result buffer.

## Syntax

```c
void M3dmodModifyResult(
    AIL_ID     SrcResult3dmodId,  //in
    AIL_INT64  SrcIndex,          //in
    AIL_ID     DstResult3dmodId,  //out
    AIL_INT64  DstIndex,          //in
    AIL_INT64  ModifyType,        //in
    AIL_DOUBLE ModifyParam,       //in
    AIL_INT64  ControlFlag        //in
)
```

## Description

This function modifies or deletes surface or planar surface results and stores them in the destination result buffer. You can only modify results after calling [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md).

[`M3dmodModifyResult`](../../Reference/3dmod/M3dmodModifyResult.md) is useful when reusing surface or planar surface results to speed up the next search ([`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REUSE_RESULT`](../../Reference/3dmod/M3dmodControl.md) set to [`M_ENABLE`](../../Reference/3dmod/M3dmodControl.md)); in this case, [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) will search for occurrences at the stored positions before searching the rest of the target. [`M3dmodModifyResult`](../../Reference/3dmod/M3dmodModifyResult.md) allows you to transform results if there was a known movement between scans, or to delete results for occurrences you don't expect to find in the next scan. Note that if you want to reuse the same results for multiple searches and you want the modification to be present for each search, you should perform the modifications on the result buffer with the original results, in-place (the same result buffer for the source and the destination). Then, before each search, you should copy the results to a separate result buffer, using [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md) with [`M_RESULT`](../../Reference/3dmod/M3dmodCopyResult.md), and use the separate result buffer for the search. If you want to reuse the original (unmodified) results in a future search, you must specify different result buffers for the source and destination to avoid overwriting the original results. Note that if you only want to reuse the results once, you do not need to copy the results to a separate result buffer; you can call [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) with the original result buffer.

This function supports in-place processing; the source and destination result buffer identifiers can refer to the same result buffer.

## Parameters

### `SrcResult3dmodId` *(in, AIL_ID)*

Specifies the identifier of the source surface or planar surface 3D model finder result buffer from which to modify results. The surface or planar surface 3D model finder result buffer must have been allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md) or [`M_FIND_PLANAR_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md) and must contain the results of a call to [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md).

### `SrcIndex` *(in, AIL_INT64)*

Specifies the index of the surface occurrence in the source surface or planar surface 3D model finder result buffer.

*For specifying the index of the surface occurrence to modify*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to modify the result for all surface occurrences. |
| `0 <= Value < M_NUMBER` | Specifies the index of the surface occurrence for which to modify results. |

### `DstResult3dmodId` *(out, AIL_ID)*

Specifies the identifier of the destination surface or planar surface 3D model finder result buffer in which to store the modified results. The surface or planar surface 3D model finder result buffer must have been allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md) or [`M_FIND_PLANAR_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md) and must be the same type of result buffer as the source.

### `DstIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ModifyType` *(in, AIL_INT64)*

Specifies the type of modification to perform.

### `ModifyParam` *(in, AIL_DOUBLE)*

Specifies an attribute of the modification.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the type of modification

Note that any unused parameters should be set to [`M_DEFAULT`](../../Reference/3dmod/M3dmodModifyResult.md).

---

### `M_DELETE`

Specifies to delete results for the specified surface occurrence from the surface or planar surface 3D model finder result buffer. This is useful if you don't need to reuse the results of all of the found occurrences. You should delete results for an occurrence if you don't expect to find an occurrence at the same position in the next scene.

---

### `M_TRANSFORM`

Specifies to translate or rotate the specified surface occurrence in the surface or planar surface 3D model finder result buffer. This is useful if there was a known movement between the scans. You should transform results for an occurrence if you expect the occurrence to be displaced in the next scene.  Note that this modification type will invalidate all results related to the target point cloud, such as reserved points.

| Value | Description |
| --- | --- |
| `Transformation matrix object ID` | Specifies the identifier of the transformation matrix object used to transform the surface occurrence. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).  You can use [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) to generate the required transformation matrix. |
