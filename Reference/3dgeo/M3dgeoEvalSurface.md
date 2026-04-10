---
doctype: Reference
module: 3dgeo
function: M3dgeoEvalSurface
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoEvalSurface"
---

# M3dgeoEvalSurface

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

> Evaluate a surface and a list of partial coordinates and find the missing coordinates for points that lie on the surface.

## Syntax

```c
AIL_INT M3dgeoEvalSurface(
    AIL_ID             Geometry3dgeoId,    //in
    AIL_INT64          Operation,          //in
    AIL_INT            NumPoints,          //in
    const AIL_DOUBLE * Src1CoordArrayPtr,  //in
    const AIL_DOUBLE * Src2CoordArrayPtr,  //in
    AIL_DOUBLE *       DstCoordArrayPtr,   //out
    AIL_INT64          ControlFlag         //in
)
```

## Description

This function evaluates a surface (such as a 3D plane or sphere) and a list of partial coordinates and calculates the missing coordinates for points that lie on the surface. You must provide source coordinates in two dimensions (for example, X- and Y-coordinates). [`M3dgeoEvalSurface`](../../Reference/3dgeo/M3dgeoEvalSurface.md) calculates the coordinates for the third dimension (for example, the missing Z-coordinates). Only coordinates that lie on the given geometry's surface are calculated.

[`M3dgeoEvalSurface`](../../Reference/3dgeo/M3dgeoEvalSurface.md) supports 3D plane and 3D sphere geometries only. Note that these geometries have a dimensionality of two, which means that, given two coordinates for a point that lies on the surface, it is possible to find the third coordinate. You can inquire a 3D geometry's dimensionality using [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_DIMENSION`](../../Reference/3dgeo/M3dgeoInquire.md).

For each provided source coordinate pair, there are three possible cases:

- Exactly one point with the specified coordinates exists on the 3D geometry's surface. The missing coordinate is calculated and written to the corresponding entry in the destination array.
- Exactly two points with the specified coordinates exist on the 3D geometry's surface. This can occur with a 3D sphere geometry. In this case, you can specify to write the largest or smallest calculated coordinate to the corresponding entry in the destination array (with [`M_MAX_VALUE`](../../Reference/3dgeo/M3dgeoEvalSurface.md) or [`M_MIN_VALUE`](../../Reference/3dgeo/M3dgeoEvalSurface.md), respectively).
- No point exists. This can occur when the provided coordinates do not match the source 3D geometry (for example, when the coordinates match a sphere with a different radius, or when the coordinates match a parallel plane that does not intersect with the provided plane). If no point exists, the value **M_INVALID_POINT** (or **M_INVALID_POINT_FLOAT**, in the case of an array of type_AIL_FLOAT_) is written to the corresponding entry in the destination array.

## Parameters

### `Geometry3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the 3D geometry object used to evaluate 3D points. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and successfully defined as a plane or sphere.

### `Operation` *(in, AIL_INT64)*

Specifies the coordinate to evaluate (establish).

*For specifying the coordinate to evaluate (establish).*

| Value | Description |
| --- | --- |
| `M_EVAL_X` | Specifies to establish the X-coordinate of the source 3D geometry, for each provided pair of Y- and Z-coordinates. Results are written to the corresponding entry in the destination array. If destination coordinates are not packed, the destination array holds the X-coordinates. |
| `M_EVAL_Y` | Specifies to establish the Y-coordinate of the source 3D geometry, for each provided pair of X- and Z-coordinates. Results are written to the corresponding entry in the destination array. If destination coordinates are not packed, the destination array holds the Y-coordinates. |
| `M_EVAL_Z` | Specifies to establish the Z-coordinate of the source 3D geometry, for each provided pair of X- and Y-coordinates. Results are written to the corresponding entry in the destination array. If destination coordinates are not packed, the destination array holds the Z-coordinates. |

*For specifying which evaluated point will be returned*

| Value | Description |
| --- | --- |
| `M_MAX_VALUE` *(default)* | Stores the largest calculated coordinate for each entry in the destination array. |
| `M_MIN_VALUE` | Stores the smallest calculated coordinate for each entry in the destination array. |

*For specifying the storage format*

| Value | Description |
| --- | --- |
| `M_PACKED` | Same as specifying both[`M_PACKED_SRC`](../../Reference/3dgeo/M3dgeoEvalSurface.md) and [`M_PACKED_DST`](../../Reference/3dgeo/M3dgeoEvalSurface.md). |
| `M_PACKED_DST` | Specifies to store the point data in a packed format in the destination array; that is, the coordinates will be stored together (XYZ XYZ XYZ...).

> **Note:** Note that all three coordinates will be written to the destination array. |
| `M_PACKED_SRC` | Specifies that source points are provided in a packed format in the [`Src1CoordArrayPtr`](../../Reference/3dgeo/M3dgeoEvalSurface.md) source array; that is, the coordinates are stored together (XYZ XYZ XYZ...).

[`Src2CoordArrayPtr`](../../Reference/3dgeo/M3dgeoEvalSurface.md) must be set to [`M_NULL`](../../Reference/3dgeo/M3dgeoEvalSurface.md).

> **Note:** Note that, if packed, not all coordinates are considered. For example, if you specify an [`M_EVAL_X`](../../Reference/3dgeo/M3dgeoEvalSurface.md) operation, the function uses only the source Y- and Z-coordinates in the evaluation. |

### `NumPoints` *(in, AIL_INT)*

Specifies the number of points in the arrays.

### `Src1CoordArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array that contains the first source coordinates, or, when [`M_PACKED`](../../Reference/3dgeo/M3dgeoEvalSurface.md) or [`M_PACKED_SRC`](../../Reference/3dgeo/M3dgeoEvalSurface.md) is specified, the address of the array that contains a packed set of source X-, Y-, and Z-coordinates.

### `Src2CoordArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array that contains the second source coordinates. This parameter must be set to `M_NULL` when [`M_PACKED`](../../Reference/3dgeo/M3dgeoEvalSurface.md) or [`M_PACKED_SRC`](../../Reference/3dgeo/M3dgeoEvalSurface.md) is specified.

### `DstCoordArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to store the set of destination coordinates, or, when [`M_PACKED`](../../Reference/3dgeo/M3dgeoEvalSurface.md) or [`M_PACKED_DST`](../../Reference/3dgeo/M3dgeoEvalSurface.md) is specified, the address of the array that contains a packed set of destination X-, Y-, and Z-coordinates.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT`

The returned value is the number of valid points written to the destination array(s); that is, the number of points whose coordinates match the 3D geometry and are not set to **M_INVALID_POINT** or **M_INVALID_POINT_FLOAT**.
