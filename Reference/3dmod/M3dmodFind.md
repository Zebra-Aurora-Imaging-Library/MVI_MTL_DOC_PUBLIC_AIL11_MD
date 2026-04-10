---
doctype: Reference
module: 3dmod
function: M3dmodFind
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodFind"
---

# M3dmodFind

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

> Search for the model in the specified 3D model finder context in the specified point cloud.

## Syntax

```c
void M3dmodFind(
    AIL_ID    Context3dmodId,  //in
    AIL_ID    ContainerBufId,  //in
    AIL_ID    Result3dmodId,   //out
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function searches for the model in the specified 3D model finder context, in the specified point cloud. Results are stored in the specified 3D model finder result buffer. You can retrieve results using [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md).

To consider the color information of the surface model and target point cloud, set [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_USE_COLOR`](../../Reference/3dmod/M3dmodControl.md) to [`M_ENABLE`](../../Reference/3dmod/M3dmodControl.md).

If you expect occurrences to be found in similar locations to the occurrences found in the previous search, you can use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REUSE_RESULT`](../../Reference/3dmod/M3dmodControl.md) set to [`M_ENABLE`](../../Reference/3dmod/M3dmodControl.md) to reuse the previous results to speed up the current search. [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) will search the target point cloud at the positions of the occurrences found during the previous search, before searching for occurrences in the rest of the point cloud. To reuse the same results for multiple searches, set up two result buffers: one for the initial search and another for use with subsequent searches. Before each subsequent search, copy into its result buffer the results from the initial search, using [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md) with [`M_RESULT`](../../Reference/3dmod/M3dmodCopyResult.md), and use this result buffer for the search. If there is a known movement between scans or an occurrence is not always present, you can use [`M3dmodModifyResult`](../../Reference/3dmod/M3dmodModifyResult.md) to transform or delete results for an occurrence.

A normals component ([`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md)) must exist in the specified point cloud container. Otherwise, the [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operation will not complete successfully and [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) with [`M_STATUS`](../../Reference/3dmod/M3dmodGetResult.md) will return [`M_MISSING_COMPONENT_NORMALS_AIL`](../../Reference/3dmod/M3dmodGetResult.md). You can use [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md) to generate a normals component.

If an occurrence has a score greater than or equal to its model's certainty level (specified using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_CERTAINTY`](../../Reference/3dmod/M3dmodControl.md)), it is automatically considered an occurrence (default 90%); the remaining occurrences will be the best of those greater than or equal to the model's acceptance level (specified using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_ACCEPTANCE`](../../Reference/3dmod/M3dmodControl.md)).

The maximum number of occurrences found depends on the maximum number specified for the context (specified using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_NUMBER`](../../Reference/3dmod/M3dmodControl.md)).

The 3D model finder context must be preprocessed with [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md) before calling this function.

## Parameters

### `Context3dmodId` *(in, AIL_ID)*

Specifies the identifier of the 3D model finder context to use for the search, and other settings related to the 3D model finder operation. The context must have been previously allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_BOX_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), [`M_FIND_CYLINDER_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), [`M_FIND_RECTANGULAR_PLANE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), [`M_FIND_SPHERE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), [`M_FIND_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), or [`M_FIND_PLANAR_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) and a corresponding model must have been previously defined using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md).

### `ContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the 3D-processable point cloud container in which to search for the model.

### `Result3dmodId` *(out, AIL_ID)*

Specifies the 3D model finder result buffer identifier in which to store results. The result buffer must have been previously allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
