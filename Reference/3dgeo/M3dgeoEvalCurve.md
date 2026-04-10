---
doctype: Reference
module: 3dgeo
function: M3dgeoEvalCurve
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoEvalCurve"
---

# M3dgeoEvalCurve

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

> Evaluate a curve and a list of partial coordinates and find the missing coordinates for points that lie on the curve.

## Syntax

```c
AIL_INT M3dgeoEvalCurve(
    AIL_ID             Geometry3dgeoId,    //in
    AIL_INT64          Operation,          //in
    AIL_INT            NumPoints,          //in
    const AIL_DOUBLE * SrcCoordArrayPtr,   //in
    AIL_DOUBLE *       Dst1CoordArrayPtr,  //out
    AIL_DOUBLE *       Dst2CoordArrayPtr,  //out
    AIL_INT64          ControlFlag         //in
)
```

## Description

This function evaluates a curve (such as a 3D line) and a list of partial coordinates and calculates the missing coordinates for points that lie on the curve. You must provide a source array of coordinates in one dimension (for example, X-coordinates). [`M3dgeoEvalCurve`](../../Reference/3dgeo/M3dgeoEvalCurve.md) calculates the coordinates for the other two dimensions (for example, the missing Y- and Z-coordinates). Only coordinates that lie on the given source curve are calculated.

[`M3dgeoEvalCurve`](../../Reference/3dgeo/M3dgeoEvalCurve.md) supports 3D line geometries only. Note that a 3D line has a dimensionality of one, which means that, given a single coordinate for a point that lies on the line, it is possible to find the other two coordinates. You can inquire a 3D geometry's dimensionality using [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_DIMENSION`](../../Reference/3dgeo/M3dgeoInquire.md).

For each provided source coordinate, there are two possible cases:

- Exactly one point with the specified coordinate exists on the 3D line. The missing coordinates are calculated and written to the corresponding entries in the destination arrays.
- No singular point exists. This occurs when no point with the specified coordinate exists on the 3D line, or when infinite remaining coordinate pairs are possible. If no singular point exists, or if infinite points are possible, **M_INVALID_POINT** (or **M_INVALID_POINT_FLOAT**, in the case of an array of type _AIL_FLOAT_) is written to the corresponding entries in the destination arrays. For example, no coordinates are calculated for any source coordinate that is beyond the limits of a finite line. Also note that if the source 3D line is parallel to an axis plane (for example, the XY-plane), providing any coordinate on the axis that is perpendicular to the plane returns **M_INVALID_POINT**.

## Parameters

### `Geometry3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the 3D geometry object used to evaluate 3D points. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and successfully defined as a line.

### `Operation` *(in, AIL_INT64)*

Specifies the coordinates to evaluate (establish).

*For specifying the coordinates to evaluate (establish)*

| Value | Description |
| --- | --- |
| `M_EVAL_XY` | Specifies to establish the X- and Y-coordinates of the source 3D geometry, for each provided Z-coordinate. Results are written to the corresponding entry in the destination arrays. If destination coordinates are not packed, the first and second destination arrays hold the X- and Y-coordinates, respectively. |
| `M_EVAL_XZ` | Specifies to establish the X- and Z-coordinates of the source 3D geometry, for each provided Y-coordinate. Results are written to the corresponding entry in the destination arrays. If destination coordinates are not packed, the first and second destination arrays hold the X- and Z-coordinates, respectively. |
| `M_EVAL_YZ` | Specifies to establish the Y- and Z-coordinates of the source 3D geometry, for each provided X-coordinate. Results are written to the corresponding entry in the destination arrays. If destination coordinates are not packed, the first and second destination arrays hold the Y- and Z-coordinates, respectively. |

*For specifying the storage format*

| Value | Description |
| --- | --- |
| `M_PACKED` | Same as specifying both[`M_PACKED_SRC`](../../Reference/3dgeo/M3dgeoEvalCurve.md) and [`M_PACKED_DST`](../../Reference/3dgeo/M3dgeoEvalCurve.md). |
| `M_PACKED_DST` | Specifies to store the point data in a packed format in the [`Dst1CoordArrayPtr`](../../Reference/3dgeo/M3dgeoEvalCurve.md) destination array; that is, the coordinates will be stored together (XYZ XYZ XYZ...).

[`Dst2CoordArrayPtr`](../../Reference/3dgeo/M3dgeoEvalCurve.md) must be set to [`M_NULL`](../../Reference/3dgeo/M3dgeoEvalCurve.md).

> **Note:** Note that all three coordinates will be written to the first destination array. |
| `M_PACKED_SRC` | Specifies that source points are provided in a packed format in the source array; that is, the coordinates are stored together (XYZ XYZ XYZ...).

> **Note:** Note that, if packed, not all coordinates are considered. For example, if you specify an [`M_EVAL_XY`](../../Reference/3dgeo/M3dgeoEvalCurve.md) operation, the function uses only the source Z-coordinates in the evaluation. |

### `NumPoints` *(in, AIL_INT)*

Specifies the number of points in the arrays.

### `SrcCoordArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array that contains the source coordinates, or, when [`M_PACKED`](../../Reference/3dgeo/M3dgeoEvalCurve.md) or [`M_PACKED_SRC`](../../Reference/3dgeo/M3dgeoEvalCurve.md) is specified, the address of the array that contains a packed set of source X-, Y-, and Z-coordinates.

### `Dst1CoordArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to store the first set of destination coordinates, or, when [`M_PACKED`](../../Reference/3dgeo/M3dgeoEvalCurve.md) or [`M_PACKED_DST`](../../Reference/3dgeo/M3dgeoEvalCurve.md) is specified, the address of the array that contains a packed set of destination X-, Y-, and Z-coordinates.

### `Dst2CoordArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to store the second set of destination coordinates. This parameter must be set to `M_NULL` when [`M_PACKED`](../../Reference/3dgeo/M3dgeoEvalCurve.md) or [`M_PACKED_DST`](../../Reference/3dgeo/M3dgeoEvalCurve.md) is specified.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT`

The returned value is the number of valid points written to the destination array(s); that is, the number of points whose coordinates match the 3D line geometry and are not set to **M_INVALID_POINT** or **M_INVALID_POINT_FLOAT**.
