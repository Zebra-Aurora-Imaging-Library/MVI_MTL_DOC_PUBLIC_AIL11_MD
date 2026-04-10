---
doctype: Reference
module: 3dreg
function: M3dregCopy
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregCopy"
---

# M3dregCopy

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

> Copy a preregistration transformation matrix from a 3D registration context into a transformation matrix object.

## Syntax

```c
void M3dregCopy(
    AIL_ID    SrcContext3dregId,  //in
    AIL_INT   SrcIndex,           //in
    AIL_ID    DstMatrix3dgeoId,   //out
    AIL_INT   DstIndex,           //in
    AIL_INT64 CopyType,           //in
    AIL_INT64 ControlFlag         //in
)
```

## Description

This function copies the preregistration transformation matrix of a registration element in a 3D registration context, into a transformation matrix object. To copy a transformation matrix that [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) has calculated, use [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) instead.

Once copied, you can use the transformation matrix with other functions like[`M3dimMatrix...`](../../Reference/3dim/M3dimMatrixTransform.md) and [`M3dgeoMatrix...`](../../Reference/3dgeo/M3dgeoMatrixGet.md).

## Parameters

### `SrcContext3dregId` *(in, AIL_ID)*

Specifies the identifier of the 3D registration context. The context must have been previously allocated using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) with [`M_PAIRWISE_REGISTRATION_CONTEXT`](../../Reference/3dreg/M3dregAlloc.md).

### `SrcIndex` *(in, AIL_INT)*

Specifies the index of the registration element in the context.

### `DstMatrix3dgeoId` *(out, AIL_ID)*

Specifies the identifier of a transformation matrix. The transformation matrix must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `DstIndex` *(in, AIL_INT)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

*Specifies the type of copy operation*

| Value | Description |
| --- | --- |
| `M_PREREGISTRATION_MATRIX` | Copies the preregistration transformation matrix found in the registration element of the 3D registration context into a transformation matrix object. This transformation matrix specifies the rough location of a point cloud relative to its reference point cloud, and is initially set using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
