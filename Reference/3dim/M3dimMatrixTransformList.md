---
doctype: Reference
module: 3dim
function: M3dimMatrixTransformList
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimMatrixTransformList"
---

# M3dimMatrixTransformList

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

> Transform a list of 3D coordinates using the specified transformation matrix, and store the resulting coordinates in the specified destination arrays.

## Syntax

```c
void M3dimMatrixTransformList(
    AIL_ID             Matrix3dgeoId,      //in
    AIL_INT            NumPoints,          //in
    const AIL_DOUBLE * SrcCoordXArrayPtr,  //in
    const AIL_DOUBLE * SrcCoordYArrayPtr,  //in
    const AIL_DOUBLE * SrcCoordZArrayPtr,  //in
    AIL_DOUBLE *       DstCoordXArrayPtr,  //out
    AIL_DOUBLE *       DstCoordYArrayPtr,  //out
    AIL_DOUBLE *       DstCoordZArrayPtr,  //out
    AIL_INT64          ControlFlag         //in
)
```

## Description

This function transforms a list of 3D coordinates using the specified transformation matrix, and stores the resulting coordinates in the specified destination arrays.

## Parameters

### `Matrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). The transformation matrix must be any valid affine transformation matrix, where the last row is (0,0,0,1). That is, if you call [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_AFFINE`](../../Reference/3dgeo/M3dgeoInquire.md), the function returns [`M_TRUE`](../../Reference/3dgeo/M3dgeoInquire.md).

### `NumPoints` *(in, AIL_INT)*

Specifies the number of points in the arrays.

### `SrcCoordXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array that contains the source X-coordinates, or when [`ControlFlag`](../../Reference/3dim/M3dimMatrixTransformList.md) is set to [`M_PACKED`](../../Reference/3dim/M3dimMatrixTransformList.md), the address of the array that contains a packed set of source X-, Y-, and Z-coordinates.

### `SrcCoordYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array that contains the source Y-coordinates.

### `SrcCoordZArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array that contains the source Z-coordinates.

### `DstCoordXArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to store the destination X-coordinates, or when [`ControlFlag`](../../Reference/3dim/M3dimMatrixTransformList.md) is set to [`M_PACKED`](../../Reference/3dim/M3dimMatrixTransformList.md), the address of the array in which to store a packed set of destination X-, Y-, and Z-coordinates.

### `DstCoordYArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to store the destination Y-coordinates.

### `DstCoordZArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to store the destination Z-coordinates.

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether source or destination points are passed and/or returned in a packed format, or specifies to apply only rotation and scale transformations.

*For specifying the control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PACKED` | Specifies that the source points are passed in a packed format and/or that the destination points should be returned in a packed format, depending on what you pass to the [`SrcCoordYArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md), [`SrcCoordZArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md), [`DstCoordYArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md), and [`DstCoordZArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md) parameters.

If both [`SrcCoordYArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md) and [`SrcCoordZArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md) are passed [`M_NULL`](../../Reference/3dim/M3dimMatrixTransformList.md), [`SrcCoordXArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md) is assumed to be a packed set of X-, Y-, and Z-coordinates.

If both [`DstCoordYArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md) and [`DstCoordZArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md) are passed [`M_NULL`](../../Reference/3dim/M3dimMatrixTransformList.md), [`DstCoordXArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md) is filled with a packed set of X-, Y-, and Z-coordinates.

If [`SrcCoordYArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md), [`SrcCoordZArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md), [`DstCoordYArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md), and [`DstCoordZArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md) are passed [`M_NULL`](../../Reference/3dim/M3dimMatrixTransformList.md), [`SrcCoordXArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md) is assumed to be a packed set of X-, Y-, and Z-coordinates, and [`DstCoordXArrayPtr`](../../Reference/3dim/M3dimMatrixTransformList.md) is filled with a packed set of X-, Y-, and Z-coordinates. |
| `M_PLANAR` *(default)* | Specifies that the source and destination coordinates are stored in a planar format; that is, the X-, Y-, and Z-coordinate values are stored in 3 separate arrays.

> **Note:** When [`M_PLANAR`](../../Reference/3dim/M3dimMatrixTransformList.md) is specified, no source or destination array pointer parameters can be set to [`M_NULL`](../../Reference/3dim/M3dimMatrixTransformList.md). |
| `M_ROTATION_AND_SCALE` | Specifies to apply only the rotation and scale parts of the transformation to the source points. |
