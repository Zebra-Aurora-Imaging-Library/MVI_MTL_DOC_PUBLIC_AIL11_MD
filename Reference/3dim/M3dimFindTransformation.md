---
doctype: Reference
module: 3dim
function: M3dimFindTransformation
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimFindTransformation"
---

# M3dimFindTransformation

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

> Find the transformation between 2 sets of points.

## Syntax

```c
void M3dimFindTransformation(
    AIL_ID    FindTransformationContext3dimId,  //in
    AIL_ID    SourceContainerOrArrayBufId,      //in
    AIL_ID    TargetContainerOrArrayBufId,      //in
    AIL_ID    Result3dimOrMatrix3dgeoId,        //out
    AIL_INT64 ControlFlag                       //in
)
```

## Description

This function finds the rigid or affine transformation between the source and target sets of points. Aurora Imaging Library finds the transformation matrix A such that A * source (as per [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md)) best approximates the target. Specifically, the transformation minimizes the squared error between pairs of points. Source[x, y] is compared with Target[x, y]. Accordingly, this function works only when you know a specific pairing between the source and target points (for example, through feature extraction). For a function that finds an alignment but does not require an exact pairing, see [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md). Results are stored in the specified result buffer or transformation matrix object. To retrieve the matrix from a result buffer, use [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dim/M3dimCopyResult.md).

The source and target can be both containers or both Aurora Imaging Library array buffers ([`M_ARRAY`](../../Reference/buf/MbufAllocColor.md)), or any combination of the two. Note that only pairs with 2 valid points are considered. For an Aurora Imaging Library array buffer, all points are valid; for a container, points are valid if their corresponding confidence score is non-zero.

For this function to succeed, at least 3 non-collinear point pairs are required for a rigid transformation, and at least 4 non-coplanar point pairs are required for an affine transformation. If the operation fails, [`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_STATUS`](../../Reference/3dim/M3dimGetResult.md) returns [`M_NOT_ENOUGH_POINT_PAIRS`](../../Reference/3dim/M3dimGetResult.md), [`M_ALL_POINTS_COPLANAR`](../../Reference/3dim/M3dimGetResult.md), or [`M_NOT_INITIALIZED`](../../Reference/3dim/M3dimGetResult.md), and the zero matrix is returned. Note that attempting to use the zero matrix in functions such as [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md) will cause an error. To verify that the matrix is a valid rigid or affine transformation and not the zero matrix, call [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_RIGID`](../../Reference/3dgeo/M3dgeoInquire.md) or [`M_AFFINE`](../../Reference/3dgeo/M3dgeoInquire.md) (respectively) and ensure that the function returns [`M_TRUE`](../../Reference/3dgeo/M3dgeoInquire.md).

## Parameters

### `FindTransformationContext3dimId` *(in, AIL_ID)*

Specifies the identifier of a find transformation 3D image processing context.

*For specifying the find transformation 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `M_FIND_TRANSFORMATION_CONTEXT_AFFINE` | Specifies to find an affine transformation between the source and target sets of points. You should use this mode when one set of points (either source or target) originates from an uncalibrated source, which might contain a distortion such as a shear.

> **Note:** Note that this mode requires at least 4 non-coplanar point pairs. |
| `M_FIND_TRANSFORMATION_CONTEXT_RIGID` | Specifies to find a rigid transformation between the source and target sets of points. You should use this mode when the two sets of points (source and target) differ by a physical displacement in three dimensions (for example, a rotation and/or translation).

> **Note:** Note that this mode requires at least 3 non-collinear point pairs. |

### `SourceContainerOrArrayBufId` *(in, AIL_ID)*

Specifies the identifier of the source container containing a 3D-processable point cloud, or specifies the identifier of the source Aurora Imaging Library array buffer.

### `TargetContainerOrArrayBufId` *(in, AIL_ID)*

Specifies the identifier of the target container containing a 3D-processable point cloud, or specifies the identifier of the target Aurora Imaging Library array buffer.

### `Result3dimOrMatrix3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the find transformation 3D image processing result buffer or transformation matrix object in which to store the calculated transformation matrix.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
